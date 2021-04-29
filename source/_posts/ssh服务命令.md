---
title: ssh服务命令
date: 2021-01-07 14:25:21
tags: ["terminal"]
categories: ["Linux"]
---

虚拟机网络设置为桥接模式

> systemctl enable sshd.service 开机启动
> systemctl start sshd.service 立即启动
> systemctl restart sshd.service 立即重启

ssh命令

> ssh username@remote_ip 

之后输入密码即可

ssh连接失败

> ssh-keygen -R 服务器地址


