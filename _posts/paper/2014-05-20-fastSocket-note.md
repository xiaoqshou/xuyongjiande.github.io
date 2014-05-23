---
layout: post
title: Fastsocket note
classification: Paper
---
# Fastsocket: A scalable Kernel TCP Socket

## 0 摘要

随着多核的发展，针对短连接服务的可扩展TCP网络协议栈有着急切的性能需求。要使得现有的网络应用程序也得到好的连接性能，需要设计一个自底向上的并行网络协议栈。

针对被动连接和主动连接，fastsocket能在完成完整的本地连接。此外，通过分散的分区设计， fastsocket建立了并行的网络协议栈，并且通过提供完整的BSD socket API，fastsocket可以普遍应用在工业界。 实验表明，fastsocket可以在扩展到24核的情况下，保持85%的效率（xyj：这个85%怎么理解）。应用于Nginx和HAProxy的实验也表明了，fastsocket随着cpu数量的提升，网络性能具有直线上升的特点，是原本的linux kernel的吞吐量的2.9和6.2倍。

## 1 简介

+ 短连接非常普遍, 频繁的tcp连接的建立和销毁非常消耗资源，需要操作kernel中的全区TCB（tcp control blocks）
+ 所有的这些socket存储在两个全局的hash table中，分别是listen socket table和established socket table
+ 在多核的情况下， tcp连接的处理瓶颈就在于这两个表。
+ 挑战： 保持高可扩展性的前提下把全局table分散到每个cpu。如果处理不当，例如perCore的table缺失了listen socket，会导致那个Core上的新的连接被丢弃。
+ 底层的套接字文件是通过VFS管理的，VFS的锁空间对短连接的性能来说是一个瓶颈。许多方案绕过VFS，导致不能提供完整的BSD socket API，因此应用程序需要改写。
+ fastsocket贡献： 1）分散TCB管理，2）保持完整BSD socket API

## 2 挑战

+ Listen Socket Table

    在3.9内核之前，在连接建立阶段，一个listening socket用来处理所有core上面的TCP request。在不同core上的应用需要去同一个队列中拉取连接，所以需要锁操作来同步，也带来了性能问题。  
    
    在3.9内核中或者Megapipe上，SO_REUSEPORT会为每个core创建一份listen socket 的完整拷贝。这些拷贝被放入全局的listen table，因此listen table依然是全局共享的，当core增多的时候，需要更长的时间遍历list 来找到一个合适的拷贝。  
    
    Table层次的隔离是有必要的，但是不能简单的给每个core拷贝一份table
    
+ Established socket Table

    Linux中，establised sockets被用来跟踪连接的sockets，并且使用一个全局的hash表以及锁操作来维护这些sockets。
    
    想法是把全局table分散到per-Core table，当一个core需要访问socket的时候，只在自己的 table中搜索，所以不需要锁操纵。 
     
    关键的挑战在于获得完整的本地连接操作，一个连接一直被一个core处理。
    
+ Compatibility

    其他研究：1）全新的API和IO抽象，2）用户态的TCP栈。问题是难以应用到已存在的网络程序。
    
## 3 Fastsocket Design

To achieve : scalability扩展性, compatibility兼容性.

### 3.1 Architecture

共有4个重要设计，pre-core listen table, RFD(receive flow deliver), per-core established table, Fastsocket-aware VFS. 所有的设计合作起来达到的效果就是，从NIC中断到应用程序访问的处理操作全在一个core上完成。

![架构](/img/2.png)

### 3.2 Design Components

1. per-core listen table （作为server）

	每个core有一个listen socket table。应用程序建立连接的时候，执行过程会调用`local_listen()`函数，有两个参数，一个是socket FD，一个是core number. new socket 从原始的listen socket(global)拷贝到per-core local socket table. 这些对于应用程序来说都是透明的，提供给应用程序的socketFD是抽象过的，隐藏了底层的实现。

	当一个TCP SYN到达本机，kernel首先去local listen table中(xyj: 哪一个local listen table呢？确定的一个还是所有的挨个找？)找匹配的listen socket，如果找到，就通过RSS（xyj：什么是RSS）传递这个socket到一个core，否则就去global listen table中找。

	![preCore Listen Table](/img/3.png)

	【fault tolerance】 容错方面， 当进程崩溃的话，local listen socket会被关闭，进入的连接将会被引导到global Listen socket， 这样的话，别的process可以处理这些连接。由于local listen socket和global listen socket共享FD，所以kernel 将会把新的connet通知到相应的process。

	如果应用程序进程使用`accept()`系统调用，那么处理过程是首先去global listen table中查找和操作（因为是读操作，没有使用锁）， 如果没有找到，那么去core的local table中查找。如果找到，就返回给应用程序。由于listen的时候把socket绑定到了一个core，所以查找的时候也去这个core的local table中查找。

	【epoll compatibility】如果应用程序使用`epoll_ctl()`系统调用，来把一个listen socket添加到Epoll set中，那么local的listen socket和global的listen socket都被epoll监控。事件发生的时候，`epoll_wait()`系统调用会返回listen socket，`accept()`系统调用就会处理这个socket。保证了epoll实现的兼容性。

2. RFD(Receive Flow Deliver) （作为client）

	应用程序主动发包的时候，发出去的packets是通过正在执行本进程的那个cpu core来完成的，这个core是系统分配的； 而接收数据包的时候，core是由之前提到的RSS或者RPS来传递的。这样一来，连接可能由不同的两个core来完成。这违反了连接的locality（xyj： 为什么不能由两个core完成呢？）。RFS和Intel FlowDirector可以从软件和硬件上缓解这种情况，但是不完备。

	因此我们提出receive flow deliver（RFD），主要的思想是core主动发起连接的时候可以把core的标识和connection的source port编码到一起。 cores和ports的关系由一个关系集合来决定【cores，ports】， 对于一个port，有唯一的一个core与之对应。当一个core来建立connection的时候，RFD随机选择一个跟当前core匹配的port。接收包的时候，RFD负责决定这个包应该让哪一个core来处理，如果当前core不是被选中的core，那么就deliver到选中的core。

	然而有两种来自远程的包，server packets（远端是client）和client packets（远端是server）。针对server packets，已经由per-core listen table来解决，为了避免这些包被RFD重新deliver，所以RFD必须区分这两种包。

	【区分的方法】如果packet的source port是常用端口， 应该是client packet，因为我们假设os一般情况不会选择一个常用端口作为source port。如果destination port是常用端口，那么packet应该是一个server packet。如果通过前面两个判断无法确定包的类别，那么看这个packet是否匹配一个listen socket，如果是，证明是server packet，因为listen的端口是不允许作为source port的。如果匹配不到listen socket，那么就认为packet是client packet。

	一般情况下，前两种判断就区分了包的类别，client packet由RFD deliver， 而server packet由listen table 指定core。

	跟硬件特性共同协作，无法处理的再留给RFD处理。（xyj： 硬件特性如何工作的？）

3. pre-core established table

	由fastsocket建立的socket防盗local established table中，其他的regular sockets保存在global的table中。core首先去自己的local table中查找（不需要锁），然后去global中查找。

4. Fastsocket-aware VFS

	在类unix系统中，socket是VFS管理的。特点1） socket文件在内存中而不是存在磁盘上的。特点2） socket文件不适用路径来标识TCB（TCP control block）而是通过file 结构的private_data域。

	VFS花费了很多时间管理dentry和inode。socket却不能从中获益。所以我们设计了fastcocket-aware VFS，减少了额外的socket文件管理开销。


	Megapipe完全绕过VFS来避免dentry和inode的消耗，但是破坏了socket接口的兼容性，不能应用于很多已有网络应用。fastsocket则移除了VFS中非必要的部分来提高扩展性，同时保留必要接口提供兼容。

## implementation

基于Linux 2.6.32（centos 6）. 

### 4.1 fastsocket implementation

主要包含3方面的改动，base kernel， a new kernel module， user-level library。

+ kernel patches

	400 行代码，主要是fastsocket-aware VFS，分配per-core listen table和established table， 和RFD。

+ kernel module

	1500 LOC, 可以通过指令模块的参数来开启feature， 

+ user-level library

	可以截获并替换现有的syscall，用kernel module实现的新的接口来取代相应功能。
