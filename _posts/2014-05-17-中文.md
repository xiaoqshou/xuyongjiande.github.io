---
layout: post
classification: Linux Kernel Driver
---
* Linux内核模块简单示例：

一个简单的模块示例如下：其中module_init和module_exit是必须的两个宏，用来指定init函数和exit函数。

```
#include <linux/module.h>
#include <linux/init.h>
#include <linux/kernel.h>
//other includes: kernel.h...

MODULE_LICENSE("GPL");
static int your_init(void) {
    printk(KERN_ALERT "ok!");//注意，没有逗号
    return 0;
}
static void your_exit(void) {
}

module_init(your_init);
module_exit(your_exit);
```

* Linux内核的“模块”机制：

模块装入使用的命令是insmod，  
装入模块的过程：使用sys_init_module这个系统调用给模块分配内核空间，复制模块正文到分配的内核空间，解析模块中的内核调用，调用模块的init()函数。  
模块卸载使用rmmod，操作系统会检查是否有别的模块依赖需要卸载的模块，在卸载之前会调用模块的exit()函数。  
模块可以使用的函数是内核导出的那些函数（内核符号表），而其自身导出的函数也在加载之后进入到内核符号表中，可供调用。

* 模块的编译和装载：

模块编译所使用的gcc和make等工具的版本一般不能跟kernel版本推荐的相差太远，具体版本号，参见kernel源码下的Documentation/Changes文件。  
Makefile的编写：  

```
obj-m := hello.o
```

上述Makefile的内容代表：有一个模块需要从hello.o中构造。构造的模块最终名字为hello.ko  
多文件的模块编译：  

```
obj-m := name.o
name-objs := file1.o file2.o
```

最终生成的模块名为name.ko


* 调用内核的Makefile规则：

LDD第3版的书中讲解的是，你应该已经下载了一个linux内核的源码，如2.6的源码, 放在～/目录下  
然后可以这么调用：  

```
make -C ~/kernel-2.6 M=`pwd` modules
```

另外一种简单的方法是，你针对自己系统已经安装的内核版本进行编译，不需要下载源码。  
使用的命令如下：  

```
make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
```

其中的-C选项是切换到-C参数指定的目录下，也就是可以读取那个目录下的Makefile，-M选项则可以让你在真正编译之前返回到指定的目录。这个组合也就是使用kernel的Makefile来编译当前目录的文件。  
最终我们生成的Makefile如下：  

```
obj-m := hello.o

all :
	    $(MAKE) -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
	    $(MAKE) -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

在模块所在的源码目录下，敲make命令即可生成hello.ko。  

* 装载和卸载模块：

sudo insmod hello.ko即可装载模块。模块的输出信息可以通过dmesg命令来查看。使用rmmod hello卸载模块。  
查看内核的所有模块：  

1. lsmod命令
2. 查看/proc/modules
