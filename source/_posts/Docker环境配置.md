---
title: Docker环境配置
date: 2021-01-21 22:31:04
tags: ["Docker"]
categories: ["Linux"]
---

目录

- [before](https://mengxun.club/2021/01/21/Docker环境配置/#before)
- [for Centos](https://mengxun.club/2021/01/21/Docker环境配置/#for-centos)
- for Ubuntu
  - [安装Ubuntu维护的版本](https://mengxun.club/2021/01/21/Docker环境配置/#安装ubuntu维护的版本)
  - [安装Docker维护的版本](https://mengxun.club/2021/01/21/Docker环境配置/#安装docker维护的版本)
- [for Mac](https://mengxun.club/2021/01/21/Docker环境配置/#for-mac)
- [for Windows](https://mengxun.club/2021/01/21/Docker环境配置/#for-windows)
- 可能遇到的问题
  - [下载镜像超时](https://mengxun.club/2021/01/21/Docker环境配置/#下载镜像超时)


<!--more-->


# [before](https://mengxun.club/2021/01/21/Docker环境配置/#before)

下面这张图展示了在不同平台docker是如何运行的，OS X和Windows都是借助于虚拟机来运行，而Linux直接运行在宿主机上。

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150234059-702119202.bmp)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150234059-702119202.bmp)

Linux无需多言，这里需要简单的介绍一下win和mac中所需的虚拟机。

在Windows和Mac中，Boot2Docker虚拟机提供一整套的docker运行环境，那它都是提供了那些组件呢？

- Boot2Docker Linux ISO，为docker定制的虚拟机镜像，其中包含了docker的运行环境。
- Virtualbox，提供虚拟机服务。
- MSYS-git，提供shell运行环境。
- 管理工具，提供Boot2Docker的管理工具。

这里附上百度云链接，当然你的网络好的话，也可以自己去[官网](https://www.docker.com/products/docker-desktop)下载最新版。

> mac and win:链接：https://pan.baidu.com/s/11IxpxTnIjq7_xF8bnYM4Wg 提取码：2xly

接下来我们来看看各平台是如何安装和运行docker的。

# [for Centos](https://mengxun.club/2021/01/21/Docker环境配置/#for-centos)

**可选的操作：查看内核版本**

目前，CentOS 仅发行版本中的内核支持 Docker。

Docker 运行在 CentOS-6.5 或更高的版本的 CentOS 上，要求系统为64位、系统内核版本为 2.6.32-431 或者更高版本。

```
uname -a 

# 示例
[root@localhost ~]# uname -a
Linux localhost 3.10.0-514.el7.x86_64 #1 SMP Tue Nov 22 16:42:41 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
```

**更新yum源**

```
yum update -y

# 示例
[root@localhost ~]# yum update -y
```

**可选的操作：卸载旧版的docker**

```
yum remove docker  docker-common docker-selinux docker-engine
```

**安装依赖包**

`yum-util` 提供`yum-config-manager`功能，另外两个是`devicemapper`驱动依赖的。

```
yum install -y yum-utils device-mapper-persistent-data lvm2

# 示例
[root@localhost ~]# yum install -y yum-utils device-mapper-persistent-data lvm2
```

**设置docker的yum源**

```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 示例
[root@bogon ~]# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
已加载插件：fastestmirror
adding repo from: https://download.docker.com/linux/centos/docker-ce.repo
grabbing file https://download.docker.com/linux/centos/docker-ce.repo to /etc/yum.repos.d/docker-ce.repo
repo saved to /etc/yum.repos.d/docker-ce.repo
```

**查询及安装docker**

```
yum list docker-ce --showduplicates | sort -r

# 示例
[root@bogon ~]# yum list docker-ce --showduplicates | sort -r
Loading mirror speeds from cached hostfile
Loaded plugins: fastestmirror
docker-ce.x86_64            3:19.03.5-3.el7                     docker-ce-stable
docker-ce.x86_64            3:19.03.4-3.el7                     docker-ce-stable
docker-ce.x86_64            3:19.03.3-3.el7                     docker-ce-stable
docker-ce.x86_64            3:19.03.2-3.el7                     docker-ce-stable
docker-ce.x86_64            3:19.03.1-3.el7                     docker-ce-stable
```

在版本列表中，选择合适的版本下载即可。

```
yum install docker-ce-17.12.1.ce -y

# 示例
[root@bogon ~]# yum install docker-ce-17.12.1.ce -y
```

**检查是否安装成功**

```
docker version

# 示例
[root@bogon ~]# docker version
Client:
 Version:	17.12.1-ce
 API version:	1.35
 Go version:	go1.9.4
 Git commit:	7390fc6
 Built:	Tue Feb 27 22:15:20 2018
 OS/Arch:	linux/amd64
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

上面示例的最后一行的意思是无法连接到docker的守护进程，你的docker守护进程启动了吗？来看如何启动。

**启动docker并加入开机启动**

```
systemctl start docker    # 启动
systemctl enable docker	  # 加入开机启动
ps -ef | grep docker
```

# [for Ubuntu](https://mengxun.club/2021/01/21/Docker环境配置/#for-ubuntu)

**安装前检查**

- 内核版本

```
$ uname -a
```

- 检查Device Mapper

```
$ ls -l /sys/class/misc/device-mapper
```

一般，高版本的Ubuntu这两项都没问题。

**Ubuntu中安装Docker**

- 安装Ubuntu维护的版本，不推荐
- 安装Docker维护的版本，推荐

## [安装Ubuntu维护的版本](https://mengxun.club/2021/01/21/Docker环境配置/#安装ubuntu维护的版本)

使用`apt-get`命令安装，使用`source`命令来更新配置：

```
$ sudo apt-get install docker.io -y 
$ source /etc/bash_completion.d/docker.io
```

查看docker安装情况：

```
$ sudo docker.io version
```

注意，这种安装方式安装的docker名字叫做`docker.io`；另外，这种方式安装的docker的版本较低，所以，我们推荐安装docker维护的版本。

## [安装Docker维护的版本](https://mengxun.club/2021/01/21/Docker环境配置/#安装docker维护的版本)

由于apt官方库里的docker版本可能比较旧，所以先卸载可能存在的旧版本：

```
$ sudo apt-get remove docker docker-engine docker-ce docker.io
```

由于这种方式安装需要4步：

```
# 1. 检查APT的HTTPS支持，查看/usr/lib/apt/methods/https文件是否存在，不存在就要下载
$ sudo apt-get update
$ sudo apt-get install apt-transport-https -y

# 2. 添加docker的APT仓库
$ echo deb https://get.docker.com/ubuntu docker main > /etc/apt/sources.list.d/docker.list

# 3. 添加仓库的key
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

# 4. 安装
$ sudo apt-get update
$ sudo apt-get install lxc-docker -y
```

上面4步是不是很麻烦？所以docker将这些命令写了个脚本，我们只需要下载这个脚本， 然后一条命令就OK了。

这里使用`curl`命令来安装docker：

```
# 如果curl不存在，使用下面命令安装
$ sudo apt-get install curl -y

# 安装docker
$ curl -sSL https://get.docker.com/ubuntu/ | sudo sh
```

查看docker安装情况：

```
$ sudo docker version
```

注意，这种方式安装的docker，就叫docker

# [for Mac](https://mengxun.club/2021/01/21/Docker环境配置/#for-mac)

由开头的图我们知道，Mac中运行docker也是要借助虚拟机的，所以，来看如何安装吧。

**下载**

去https://www.docker.com/products/docker-desktop下载或者使用开头的安装包。

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150440586-1241490978.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150440586-1241490978.png)

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150456069-1582726282.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150456069-1582726282.png)

**安装**

- 直接拖拽安装。

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150523459-2080932066.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150523459-2080932066.png)

- 然后启动它。

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150538390-508806579.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150538390-508806579.png)

启动中：

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150553739-487916499.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150553739-487916499.png)

启动后：

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150608469-404116108.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150608469-404116108.png)

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150620419-1374352363.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150620419-1374352363.png)

**测试**

可以输入下面命令进行测试。

```
docker version
docker run ubuntu echo "Hello World"
```

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150635947-2127023928.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150635947-2127023928.png)

OK，完事了。

# [for Windows](https://mengxun.club/2021/01/21/Docker环境配置/#for-windows)

由开头的图我们知道，Windows中运行docker也是要借助虚拟机的，所以，来看如何安装吧。

**系统要求**

windows7及以上系统

来看安装。

**下载boot2docker**

- 去https://www.docker.com/products/docker-desktop下载。

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150712473-984511995.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150712473-984511995.png)
[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150723636-995811009.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150723636-995811009.png)

**安装**

- 下载到本地是exe文件，双击exe文件进行安装和配置。

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150743383-1683621203.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150743383-1683621203.png)

- 重启。

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150757014-1857201498.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150757014-1857201498.png)

当你重启后，打开桌面快捷方式，会发现任务栏多了一个docker图标。

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150804491-1736837661.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150804491-1736837661.png)

**运行**

当你打开桌面快捷方式后，docker将启动在任务栏；然后你可以`win + R`打开终端，输入命令来进行测试。

```
# 查看版本
docker version
```

# [可能遇到的问题](https://mengxun.club/2021/01/21/Docker环境配置/#可能遇到的问题)

## [下载镜像超时](https://mengxun.club/2021/01/21/Docker环境配置/#下载镜像超时)

> windows环境

由于docker默认的镜像源是国外的，很可能遇到下载失败的问题，比如这样的：

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150817128-1525436794.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150817128-1525436794.png)

解决办法，点击任务栏docker，然后点击settings。

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150824019-87259280.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150824019-87259280.png)

然后在`Docker Engine`选项将`https://docker.mirrors.ustc.edu.cn/`添加到`registry-mirrors`中去。

[![img](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150831808-138678168.png)](https://img2020.cnblogs.com/blog/1168165/202004/1168165-20200402150831808-138678168.png)

下面列出国内的常用源：

| 来自              | 源                                                           |
| :---------------- | :----------------------------------------------------------- |
| 网易              | [http://hub-mirror.c.163.com](http://hub-mirror.c.163.com/)  |
| Docker 官方中国区 | [https://registry.docker-cn.com](https://registry.docker-cn.com/) |
| 中国科技大学      | [https://docker.mirrors.ustc.edu.cn](https://docker.mirrors.ustc.edu.cn/) |
| 阿里云            | [https://pee6w651.mirror.aliyuncs.com](https://pee6w651.mirror.aliyuncs.com/) |

------

see also：

[docker：CentOS安装 docker和默认安装目录](https://blog.csdn.net/weixin_38750084/article/details/90317730)
[如何进入、退出docker的container](https://blog.csdn.net/dongdong9223/article/details/52998375)
[Centos7上安装docker](https://www.cnblogs.com/yufeng218/p/8370670.html)
[Ubuntu16.04安装docker](https://www.cnblogs.com/lighten/p/6034984.html)
https://blog.csdn.net/BigData_Mining/article/details/87869147

作者： 听雨危楼

出处：https://www.cnblogs.com/Neeo/articles/11945963.html

版权：本作品采用「[署名-非商业性使用-相同方式共享 4.0 国际](https://creativecommons.org/licenses/by-nc-sa/4.0/)」许可协议进行许可。

仰望星空,脚踏实地