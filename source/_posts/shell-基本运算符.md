---
title: shell 基本运算符
date: 2021-01-10 15:54:20
tags: ["shell"]
categories: ["Linux"]
---


# shell 基本运算符

## 算数运算符

| 算数运算符 | 说明   |
| ---------- | ------ |
| +          | 加     |
| -          | 减     |
| *          | 乘     |
| /          | 除     |
| %          | 取余   |
| =          | 赋值   |
| ==         | 相等   |
| !=         | 不相等 |

<br>  

```shell
#!/bin/bash

a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $a / $b`
echo "a / b : $val"

val=`expr $a % $b`
echo "a % b : $val"

if [ $a == $b ]; then echo "a == b" ; fi

if [ $a != $b ]; then echo "a != b" ; fi
```

<br> 

## 关系运算符

| 运算符 | 说明                                             |
| ------ | ------------------------------------------------ |
| -eq    | 检测两个数是否相等，相等返回true                 |
| -ne    | 检测两数是否相等，不相等返回true                 |
| -gt    | 检测左边的数是否大于右边的，大于返回true         |
| -lt    | 检测左边的数是否小于右边的，小于返回true         |
| -ge    | 检测左边的数是否大于等于右边的，大于等于返回true |
| -le    | 检测左边的数是否小于等于右边的，小于等于返回true |

<br> 

## 逻辑运算符

| 运算符 | 说明        |
| ------ | ----------- |
| &&     | 逻辑与  AND |
| \|\|   | 逻辑或  OR  |

<br> 

```shell
#!/bin/bash

a=10
b=20

if [[ $a -lt 100 && $b -gt 100 ]]
then 
    echo "return ture"
else
    echo "return false"
fi

if [[ $a -lt 100 || $b -gt 100 ]]
then
    echo "return true"
else
    echo "return false"
fi
```
<br> 


## 字符串运算符

| 运算符 | 说明                                   |
| ------ | -------------------------------------- |
| =      | 检测两个字符串是否相等，相等返回true   |
| !=     | 检测两个字符串是否相等，不相等返回true |
| -z     | 检测字符串长度是否为0，为0返回true     |
| -n     | 检测字符串长度是否为0，不为0返回true   |
| str    | 检测字符串是否为空，不为空返回true     |

<br> 

```shell
#!/bin/bash

a="bac"
b="fef"

if [ $a = $b ]
then
    echo "$a = $b : a == b"
else
    echo "$a = $b : a != b"
fi

if [ -n $a ]
then
    echo "-n $a : The string length is not 0"
else
    echo "-n $a : The string length is 0"
fi

if [ $a ]
then
    echo "$a : The string length is not empty"
else
    echo "$a : The string length is empty"
fi
```

<br> 

## 文件测试运算符

| 操作符    | 说明                                                         |
| --------- | :----------------------------------------------------------- |
| -e        | 文件存在                                                     |
| -a        | 文件存在，这个选项的效果与-e相同。但它已经被“弃用”，并且不鼓励使用 |
| -f        | 表示这个文件是一个一般文件（并不是目录或设备文件）           |
| -s        | 文件大小不为零                                               |
| -d        | 表示这是一个目录                                             |
| -b        | 表示这是一个设备（光驱，软盘等）                             |
| -c        | 表示这是一个字符设备（键盘，modem，声卡等）                  |
| -p        | 这个文件是一个管道                                           |
| -h        | 这是一个符号链接                                             |
| -L        | 这是一个符号链接                                             |
| -S        | 表示这是一个socket                                           |
| -t        | 文件（描述符）被关联到一个终端设备上，这个测试选项一般被用来检测脚本中的stdin([ -t 0 ])或者stdout([ -t 1 ])是否来自一个终端 |
| -r        | 文件是否有可读读权限（指的是正在运行这个测试命令的用户是否有可读权限） |
| -w        | 文件是否有可读写权限（指的是正在运行这个测试命令的用户是否有可写权限） |
| -x        | 文件是否具有可执行权限（指的是正在运行这个测试命令的用户是否有可执行权限） |
| -g        | set-group-id(sgid)标记被设置到文件或目录上                   |
| -k        | 设置粘贴位                                                   |
| -O        | 判断你是否是文件的拥有者                                     |
| -G        | 文件的group-id是否与你相同                                   |
| -N        | 从文件上一次被读取到现在为止，文件是否被修改过               |
| f1 -nt f2 | 文件 f1 比 f2 新                                             |
| f1 -ot f2 | 文件 f1 比 f2 旧                                             |
| f1 -ef f2 | 文件 f1 和文件 f2 是相同文件的硬链接                         |
| !         | “非”，反转上边所有测试结果（如果没给出条件，那么返回真）     |

<br> 

```shell
#!/bin/bash

file="/home/william/DYJ/test/test.sh"

if [ -r $file ]
then
    echo "The file is readable"
else
    echo "The file is not readable"
fi

if [ -e $file ]
then
    echo "File exists"
else
    echo "File not exists"
fi
```

结果：

```shell
The file is readable
File exists
```

<br> 

思考：浮点运算，比如实现求圆的面积和周长。

`expr` 只能用于整数计算，可以使用 `bc` 或者 `awk` 进行浮点数运算。scale保留小数点，但对乘法无效

<br> 

```shell
#!/bin/bash

pi=3.141592

radius=2.5

area=$(echo "scale=4; $pi * $radius * $radius" | bc)

girth=$(echo "scale=4; $pi * $radius *2" | bc)

echo "area=$area"

echo "girth=$girth"
```

