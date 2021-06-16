---
title: shell基本输入
date: 2021-06-15 23:52:03
tags: ["shell"]
categories: ["Linux"]
---

- read 命令

****

### read 命令

默认接受键盘的输入，回车符代表输入结束

read 命令选项

\-p 打印信息

\-t 限定时间

\-s 不回显

-n 输入字符个数

<!--more-->

**登录小案例**

```shell
#!/bin/bash

clear

# read 后面跟变量名，将输入的内容存入内存中
echo -ne "Login: "
read username

# read -p "Login: " username 可替代前面的echo

# 后面跟命令不换行的话加分号。
# -t 5 指定5秒不输入密码自动结束，-s 不显示输入的数据
# -n 6 密码只识别前6位输入的数据，多输入直接返回，并保留前6位
echo -ne "Password: ";read -st 5 -n 6 password
echo
# read -p 可以替换echo, 但不会结束
read -p "Username $username, Password $password"
```

结果

```shell
Login: root
Password: 123456
Username root, Password 123456
```

**Linux登录小脚本**

```shell
#!/usr/bin/bash
clear

echo "Arch Linux (Core)"
echo -e "Kernel `uname -r` an `uname -m`\n"

echo -ne "$HOSTNAME Login: ";read username
read -sp "Password: "
#echo -ne "Password: ";read -s passwd

echo
```