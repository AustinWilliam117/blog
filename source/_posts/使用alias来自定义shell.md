---
title: 使用alias来自定义shell
date: 2019-10-23 20:22:55
tags: ["terminal","zsh"]
categories: ["Linux","shell"]
---

#### alias命令格式 

- alias命令可以为命令别名，可以在bash窗口直接输入命令进行别名
- 取消命令别名格式为：unalias lm
- 直接输入 alias 命令会列出当前系统中所有已经定义的命令别名。



#### 永久保存alias命令

1. 在家目录下创建.aliases文件，输入想要使用的命令，例如：

   > alias h='history'

2. 如果装了zsh就打开**<font color ="pink">.zshrc</font>** ,默认的话打开**<font color="pink">.bashrc</font>**，在文件尾部输入

   ```shell
   if [ -f ~/.aliases ]; then
   	. ~/.aliases
   fi
   ```

3. 退出后，**<font color ="pink">source  ~/.zshrc</font>** 或者 **<font color ="pink">source ~/.bashrc</font>** 来使创建的文件生效(source后有空格，重启终端效果一样)
