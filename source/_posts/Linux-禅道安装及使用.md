---
title: Linux 禅道安装及使用
date: 2021-01-09 14:24:51
tags: ["禅道"]
categories: ["测试软件"]
---

Linux一键安装包内置了apache， php， mysql这些应用程序，只需要下载解压缩即可运行禅道企业版。

Linux一键安装包分为32位和64位两个包，请大家根据操作系统的情况下载相应的包。


## 安装

1. 将禅道企业版Linux一键安装包直接解压到/opt目录下， 不要 解压到别的目录再拷贝到/opt/，因为这样会导致文件的所有者和读写权限改变， 也不要解压后把整个目录777权限 。

    可以使用命令： tar -zxvf  ZenTaoPMS.Biz1.0.zbox_64.tar.gz -C /opt
<!--more-->    

2. 启动程序(建议使用root启动)
```shell
执行/opt/zbox/zbox start 命令开启Apache和Mysql。

执行/opt/zbox/zbox stop 命令停止Apache和Mysql。

执行/opt/zbox/zbox restart 命令重启Apache和Mysql。

可以使用/opt/zbox/zbox -h命令来获取关于zbox命令的帮助。

其中 -ap参数 可以修改Apache的端口，-mp参数 可以修改Mysql的端口（比如：/opt/zbox/zbox -ap 8080）
```



3. **如果MySQL或Apache启动错误，可以查看是否程序已安装过或已启动,停止相关程序服务，再次执行`/opt/zbox/zbox restart`**

    > sudo systemctl stop mysqld.service 关闭mysql service

    浏览器访问 http://localhost (默认帐号 admin，密码 123456)


## 参考文档

1. [禅道企业版使用帮助-Linux一键安装包安装](https://www.zentao.net/book/zentaobizhelp/281.html)

