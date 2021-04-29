---
title: coc.nvim报错解决方案
date: 2019-10-22 18:28:06
tags: ["terminal","zsh"]
categories: ["Linux"]
---

#### 安装coc.nvim时 报[coc.nvim] javascript file not found 错误的解决方案

错误提示：

[coc.nvim] javascript file not found, please compile the code or use release branch.
Press ENTER or type command to continue



#### 解决方案：

1.进入coc.nvim目录

> cd ~/.vim/plugged/coc.nvim/

2.执行install.sh

> ./install.sh

3.进入.vimrc执行命令

> :PlugInstall
