---
title: shell变量
date: 2021-06-16 22:23:37
tags: ["shell"]
categories: ["Linux"]
---

- 变量介绍
- 变量分类
- 变量定义

****

### 一、变量介绍

> 1. 在编程中，我们总有一些数据需要临时存放在内存，以待后续使用时快速读出。内存在系统启动的时候被按照1B一个单位划分为若干个块，然后统一
> 2. 编号(16进制编号)，并对内存的使用情况做记录，保存在内存跟踪表中。

<!--more-->

1）内存占用：如果存的是一个字符则占用1个字节，如果存的是字符串则是字符串的长度加1个字节长度(\0是一个特殊字符，代表字符串结束)。

2）变量名与内存空间关系：计算机中会将对应的内存空间和变量名称绑定在一起，此时代表这段内存空间已经被程序占用，其他程序不可复用；然后将变量名对应的值存在对应内存地址的空间里。



### 二、变量分类

1. 本地变量：用户私有变量，只有本用户可以使用，保存在家目录下的.bash_profile、.bashrc文件中
2. 全局变量：所有用户都可以使用，保存在/etc/profile、/etc/bashrc文件中
3. 用户自定义变量：用户自定义，比如脚本中的变量


### 三、变量定义

变量名命名规则：

- 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头
- 中间不能有空格，可以使用下划线
- 不能使用标点符号
- 不能使用bash里的关键字（可以使用help命令查看保留关键字）



#### 临时变量

```shell
[william@William-arch ~]$ name="william"
[william@William-arch ~]$ age=18
[william@William-arch ~]$ score=98
```



#### 读取变量内容

```shell
[william@William-arch ~]$ echo $name
william
[william@William-arch ~]$ echo $age
18
[william@William-arch ~]$ echo $score
98
```



#### 取消变量 unset

```shell
[william@William-arch ~]$ unset name
[william@William-arch ~]$ echo $name

```



#### 定义全局变量 export

```shell
[william@William-arch ~]$ export name='mengxun'

# 上述设置的变量其实都是一次性变量，系统重启就会丢失。
# 如果希望本地变量或者全局变量可以永久使用，可以将需要设置的变量写入变量文件中即可。
```



#### 定义永久变量

本地变量：用户私有变量，只有本用户可以使用，保存在家目录下的.bash_profile、.bashrc文件中

全局变量：所有用户都可以使用，保存在/etc/profile、/etc/bashrc文件中