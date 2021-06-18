---
title: shell数组
date: 2021-06-17 23:25:41
tags: ["shell"]
categories: ["Linux"]
---

- 数组介绍
- 基本数组
- 关联数组
- 案列分享

****

### 数组介绍

一个变量只能存一个值，但是现实中又又很多值需要存储，那么变量就有些拘谨了。比如做一个学员信息表，一个班50个人，每个人6条信息，我们需要定义300个变量才能完成。恐怖恐怖，这只是一个班的学生，一个学校呢？一个市呢？……我想静静了！

仔细想想上述的案例，一个学生六个信息:ID、姓名、性别、年龄、成绩、班级。可不可以定义六个变量就能存储这六类信息呢？答案是当然可以的！变量不行，我们就用数组。

<!--more-->

### 基本数组

数组可以让用户一次赋予多个值，需要读取数据时只需通过索引调用就可以方便读出了。

#### 数组语法

`数组名称=(元素1 元素2 元素3 ...)`



### 数组读出

`${数组名称}[索引]`

#### 数组赋值

**一次赋一个值**

```shell
array[0]='tom'

array[1]='william'

array[2]='peter'
```

**一次赋多个值**

```shell
array2=(tom jack alice)

# 希望是将该文件中的每一个行作为一个元素赋值给数组array3
array3=(`cat /etc/passwd`) 

array4=(`ls /var/ftp/Shell/for*`)

 array5=(tom jack alice “bash shell”)
```

**查看数组**

`declare -a`  查看系统所有数组

**访问数组元数**

```shell
# 访问数组中第一个元素
echo ${array1[0]} 

# 访问数组中所有元素
echo ${array1[@]}
echo ${array1[*]}

# 统计数组元素的个数
echo ${#array1{@}}
echo ${#array1{*}}

# 获取数组元素的索引
echo ${!array1[@]}
echo ${!array1[*]}

# 从数组下标1开始
echo ${array1[@]:1}
echo ${array1[*]:1}

# 从数组下标1开始，访问两个元素
echo ${array1[@]:1:2}
echo ${array1[*]:1:2}
```

**遍历数组**

默认数组通过数组元素的个数进行遍历

```bash
[william@William-arch ~]$ array=('a' 'b' 'c' 'd')

[william@William-arch ~]$ echo ${array[0]}
a
[william@William-arch ~]$ echo ${array[1]}
b
[william@William-arch ~]$ echo ${array[2]}
c
[william@William-arch ~]$ echo ${array[3]}
d
```



方法二：针对关联数组可以通过数组元素的索引进行遍历



### 关联数组

关联数组可以允许用户自定义数组的索引，这样使用起来更加方便、高效



**定义关联数组**

声明关联数组变量 `declare -A array`



**关联数组赋值**

方法一：一次赋一个值

`数组名[索引]=变量名`

```bash
ass_array[index1]=pear

ass_array[index2]=apple

ass_array[index3]=orange

ass_array[index4]=peach
```

方法二：一次赋多个值

```bash
ass_array=([index1]=william [index2]=austin [index3]=peter)
```



**查看数组**

```bash
[william@William-arch ~]$ declare -A | grep ass_array
ass_array=( [1]=pear [2]=apple [3]=orange [4]=peach )
```



**访问数组元素**

```bash
echo ${ass_array[index2]} 访问数组中的第二个元数

echo ${ass_array@]} 访问数组中所有元数 等同于 echo ${array1[*]}

echo ${#ass_array[@]} 获得数组元数的个数

echo ${!ass_array[@]} 获得数组元数的索引
```



**遍历数组**

通过数组元数的索引进行遍历,针对关联数组可以通过数组元素的索引进行遍历

```bash
[william@William-arch ~]$ declare -A array

[william@William-arch ~]$ array=('a' 'b' 'c' 'd')

[william@William-arch ~]$ echo ${array[0]}
a
[william@William-arch ~]$ echo ${array[1]}
b
[william@William-arch ~]$ echo ${array[2]}
c
[william@William-arch ~]$ echo ${array[3]}
d
```



### 案例分享——学员信息系统

```shell
#!/usr/bin/bash

for ((i=0;i<3;i++))
   do
      read -p "输入第$((i + 1))个人名: " name[$i]
      read -p "输入第$[$i + 1]个年龄: " age[$i]
      read -p "输入第`expr $i + 1`个性别: " gender[$i]
done
clear
      echo -e "\t\t\t\t学员查询系统"
while :
   do
      cp=0
#      echo -e "\t\t\t\t学员查询系统"
      read -p "输入要查询的姓名: " xm
      [ $xm == "Q" ]&&exit
      for ((i=0;i<3;i++))
         do
              if [ "$xm" == "${name[$i]}" ];then
                  echo "${name[$i]} ${age[$i]} ${gender[$i]}"
                  cp=1
              fi
      done
      [ $cp -eq 0 ]&&echo "not found student"
done
```



作业：

- 使用关联数组统计文件 /etc/passwd 中用户的不同类型shell的数量
- 使用关联数组按扩展名统计指定目录中文件的数量