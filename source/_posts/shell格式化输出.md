---
title: shell格式化输出
date: 2021-06-15 23:11:39
tags: ["shell"]
categories: ["Linux"]
---

- echo 命令
- 颜色输出

****

一个程序需要有0个以上输入，一个或更多输出



### 一、 echo 命令介绍

功能：将内容输出到默认显示设备

echo 命令的功能是在显示器上显示一段文字，一般起到一个提示的作用。功能说明：显示文字

<!--more-->

语法：echo [-ne]\[字符串]

补充说明：echo 会将输入的字符串送往标准输出。输出的字符串间以空白字符间隔，并在最后加上换行号。

```shell
[william@William-arch ~]$ echo "hello world"
hello world

[william@William-arch ~]$ echo -n "hello world"
hello world[william@William-arch ~]$
```

命令选型：

\-n 不要在最后自动换行

```shell
[william@William-arch ~]$ echo -n "Login: ";read
Login: william

[william@William-arch ~]$ echo -n "date: ";date +%F
date: 2021-06-15


```

\-e 若字符串中出现一下字符，则特别加以处理，而不会将它当成一般文字输出：

转义字符

\a 发出警报声

\b 删除前一个字符

\c 最后不加上换行符号

\f 换行但光标仍旧停留在原来的位置

\n 换行且光标移至行首

\r 光标移至行首，但不换行

\t 插入tab

\v 与\f 相同

\nnn 插入nnn（八进制）所代表的 ASCII 字符

\-help 显示帮助

\-version 显示版本信息

```shell
# 删除一个字符的时候，必须要加参数n，换行之后就不在同一行了，没办法删除
[william@William-arch ~]$ echo -ne "a\b"
[william@William-arch ~]$ 

# 举例说明：倒计时
#!/bin/bash
for time in `seq 9 -1 0`; do
    echo -n $time
done

echo  

for time in `seq 9 -1 0`; do
    echo -ne "\b$time"
    sleep 1
done

echo
```

格式化输出

```shell
#!/bin/bash

echo -e '\t\t\t Fruits Shop'
echo -e "\t1) Apple"
echo -e "\t2) Orange"
echo -e "\t3) Banana"

# 结果
[william@William-arch test]$ sh fruit.sh 
                         Fruits Shop
        1) Apple
        2) Orange
        3) Banana
```



### 二、颜色代码

脚本中 echo 显示内容带颜色显示，echo 显示带颜色，需要使用参数 -e

格式如下：

`echo -e "\033[41;36m something here \033[0m"`

其中41的位置代表底色，36的位置代表字的颜色

1. 字背景颜色和文字颜色之间是英文的""
2. 文字颜色后面有个m
3. 字符串前后没有空格，如果有的话，输出也是同样有空格

```shell
下面是相应的字和背景颜色，可以自己来尝试找出不同颜色搭配
　　例
　　echo -e “\033[31m 红色字 \033[0m”
　　echo -e “\033[34m 黄色字 \033[0m”
　　echo -e “\033[41;33m 红底黄字 \033[0m”
　　echo -e “\033[41;37m 红底白字 \033[0m”
　　
字颜色：30—–37
　　echo -e “\033[30m 黑色字 \033[0m”
　　echo -e “\033[31m 红色字 \033[0m”
　　echo -e “\033[32m 绿色字 \033[0m”
　　echo -e “\033[33m 黄色字 \033[0m”
　　echo -e “\033[34m 蓝色字 \033[0m”
　　echo -e “\033[35m 紫色字 \033[0m”
　　echo -e “\033[36m 天蓝字 \033[0m”
　　echo -e “\033[37m 白色字 \033[0m”
　　
字背景颜色范围：40—–47
　　echo -e “\033[40;37m 黑底白字 \033[0m”
　　echo -e “\033[41;37m 红底白字 \033[0m”
　　echo -e “\033[42;37m 绿底白字 \033[0m”
　　echo -e “\033[43;37m 黄底白字 \033[0m”
　　echo -e “\033[44;37m 蓝底白字 \033[0m”
　　echo -e “\033[45;37m 紫底白字 \033[0m”
　　echo -e “\033[46;37m 天蓝底白字 \033[0m”
　　echo -e “\033[47;30m 白底黑字 \033[0m”
　　
最后面控制选项说明
　　\033[0m		 									关闭所有属性
　　\033[1m 										设置高亮度
　　\033[4m 										下划线
　　\033[5m 										闪烁
　　\033[7m 										反显
　　\033[8m 										消隐
　　\033[30m — \33[37m 				                设置前景色
　　\033[40m — \33[47m 				                设置背景色
　　\033[nA 										光标上移n行
　　\033[nB 										光标下移n行
　　\033[nC 										光标右移n行
　　\033[nD 										光标左移n行
　　\033[y;xH										设置光标位置
　　\033[2J 										清屏
　　\033[K 											清除从光标到行尾的内容
　　\33[s 											保存光标位置
　　\033[u 											恢复光标位置
　　\033[?25l 										隐藏光标
　　\033[?25h 									    显示光标
```