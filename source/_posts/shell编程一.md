---
title: shell编程一
date: 2021-05-30 21:11:14
tags: ["shell"]
categories: ["Linux"]
---

Shell 语法

- 如何书写一个shell脚本
- shell脚本运行
- shell中的特殊符号
- 管道
- shell中数学运算
- 脚本退出

****

shell脚本就是将完成一个任务的所有命令按照执行的先后顺序，自上而下写入到一个文本文件中，然后给予执行权限



### 一、如何书写一个shell脚本

shell脚本的命名：

名只要有意义，最好不要用 a、b、c、1、2 这种命名方式；虽然 linux 系统中，文件没有扩展名的概念，依然建议你用.sh结尾；名字不要太长，最好能在30个字以内解决。例如：check_memory.sh

<!--more-->

shell 脚本格式：

shell 脚本开头必须指定脚本运行环境，以`#!`这个特殊符号为组成。如：#!/bin/bash 指定该脚本是运行解析由`/bin/bash`来完成的

shell中的注释使用#号

```shell
shell 脚本中，最好加入脚本说明字段

#!/usr/bin/bash
#Author: Meng xun
#Created Time: 2021/05/28 18:41
#Script Description: first shell study script
```



### 二、如何运行一个 shell 脚本

脚本运行需要执行权限，当我们给一个文件赋予执行权限后，该脚本就可以运行

`chomd u+x filename`

如果不希望赋予执行权限，那么可以使用bash命令来运行未给予执行权限的脚本bash filename

`bash filename`

### 三、shell中的特殊符号

```text
不要和正则表达式中的符号含义搞混淆了

~										家目录
!										执行历史命令
$										变量中取内容符
+ - * \ %				 对应数学运算		加 减 乘 除 取余数
&										后台执行
*										星号是shell中的通配符，匹配所有
?										问号是shell中的通配符，匹配除回车以外的一个字符
;										分号可以在shell中一行执行多个命令，命令之间用分号分割
|										管道符，上一个命令的输出作为下一个命令的输入 `cat filename | grep "abc"`
\										转义字符
``									反引号，命令中执行命令 echo "today is \`date +%F`"
''									单引号，脚本中字符串要用单引号引起来，但是不同于双引号的是，但引号不解释变量
""									双引号，脚本中出现的字符串可以用双引号引起来
```

### 四、Shell 中管道符的运用

`|` 管道符在shell中使用的最多的，很多组合命令都需要通过组合命令来完成

### 五、shell 重定向

```tex
>				重定向输入		覆盖原有数据
>>				重定向追加输入，在原数据的末尾添加
<				重定向输出	wc -l < /etc/passwd
<<				重定向追加输出		fdisk /dev/sdb <<EOF 	...... 	EOF
```

### 六、shell 数学运算

```shell
expr 命令： 只能做整数运算，格式比较古板，注意空格
$ expr 1 + 1
2
$ expr 5 - 2
3
$ expr 5 \* 2				#注意*号出现为转义，否则为通配符
10
$ expr 5 / 2
2
$ expr 5 % 2
1

使用bc计算器处理浮点运算，scale=2代表小数点保留两位(不过arch上不知道为什么不是)
$ echo "scale=2;3.764+100"|bc
103.764
$ echo "scale=2;100.237-3"|bc
97.237
$ echo "scale=2;100/3"|bc
33.33
$ echo "scale=2;100*3.6754"|bc
367.5400

双小圆括号运算，在shell中(())也可以用来做数学运算
$ echo $((100.12+3))
103.12
$ echo $((100-3))
97
$ echo $((100*3))
300
$ echo $((100/3))
33
$ echo $((100**3))
1000000
```

### 七、退出脚本

`exit` NUM 退出脚本，释放系统资源，NUM代表一个整数，代表返回值
