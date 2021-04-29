---
title: 在VirtualBox中创建共享文件夹
date: 2021-01-19 22:05:23
tags: ["VirtualBox"]
categories: ["Linux"]
---

1. 打开VirtualBox管理器，点击设置

2. 点击“共享文件夹”选项右侧的加号。

3. 填入你想要共享的文件路径

4. 共享路径选好后，会自动将文件夹名称作为共享文件夹名称，勾上“固定分配”，这样这个路径可以永久使用。

5. 现在进入虚拟机，打开Terminal，输入以下命令：

   ```shell
   sudo mkdir /pcshare
   
   sudo chmod 777 /pcshare
   
   sudo mount -t vboxsf uBuntuSharePath /pcshare
   ```

   **注意uBuntuSharePath就是之前步骤所创建的PC机上的共享文件夹名称。**

6. 之后每次启动虚拟机，都需要先在Terminal中输入命令“sudo mount -t vboxsf uBuntuSharePath /pcshare”。
