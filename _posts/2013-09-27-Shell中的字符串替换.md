---
layout: post
classification: Shell
---

* SHELL中字符串的掐头去尾

```
$x=aabbaarealwwvvww 
$echo "${x%w*w}" 
aabbaarealwwvv 
$echo "${x%%w*w}" 
aabbaareal 
$echo "${x##a*a}" 
lwwvvww $echo "
${x#a*a}" 
bbaarealwwvvww
```

其中 , # 表示掐头， 因为键盘上 # 在 $ 的左面。  
其中 , % 表示去尾， 因为键盘上 % 在 $ 的右面。  
单个#或者%的表示最小匹配，双个表示最大匹配。也就是说，当匹配的有多种方案的时候，选择匹配的最大长度还是最小长度。

* 字符串的替换

代码:

$x=abcdabcd $echo ${x/a/b} # 只替换一个 bbcdabcd $echo   
${x//a/b} # 替换所有 bbcdbbcd
