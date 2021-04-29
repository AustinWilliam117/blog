---
title: 你的下一个输入法何必是搜狗，拥抱fcitx5
date: 2020-12-28 13:28:21
tags: ["fcitx5","输入法"]
categories: ["Linux"]
---

## fcitx5的安装

```shell
sudo pacman -S fcitx5-git fcitx5-chinese-addons fcitx5-chewing fcitx5-pinyin-zhwiki fcitx5-pinyin-moegirl fcitx5-config-qt fcitx5-gtk-git fcitx5-qt4-git fcitx5-qt5-git fcitx5-material-color
```

<!-- more -->

- [fcitx5-chinese-addons](https://archlinux.org/packages/?name=fcitx5-chinese-addons) 包含了大量中文输入方式：拼音、双拼、五笔拼音、自然码、仓颉、冰蟾全息、二笔等
- [fcitx5-chewing](https://archlinux.org/packages/?name=fcitx5-chewing) 对注音输入法 [libchewing](https://archlinux.org/packages/?name=libchewing) 的包装

- [fcitx5-qt](https://archlinux.org/packages/?name=fcitx5-qt)：对 Qt5 程序的支持
- [fcitx5-gtk](https://archlinux.org/packages/?name=fcitx5-gtk)：对 GTK 程序的支持
- [fcitx5-qt4-git](https://aur.archlinux.org/packages/fcitx5-qt4-git/)AUR：对 Qt4 程序的支持
- [fcitx5](https://archlinux.org/packages/?name=fcitx5) 的配置文件位于 `~/.local/share/fcitx5`，尽管您可以使用文本编辑器编辑配置文件，但是使用 GUI 配置显然更方便。安装 [fcitx5-configtool](https://archlinux.org/packages/?name=fcitx5-configtool) 软件包。

- [fcitx5-pinyin-zhwiki](https://aur.archlinux.org/packages/fcitx5-pinyin-zhwiki/)AUR：felixonmars 根据中文维基百科创建的词库。适用于 **拼音输入法**
- `fcitx5-pinyin-moegirl`（在 ArchLinux CN 源中）：[outloudvi](https://github.com/outloudvi/fcitx5-pinyin-moegirl)根据萌娘百科创建的词库。适用于 **拼音输入法**
- [fcitx5-material-color](https://archlinux.org/packages/?name=fcitx5-material-color)：提供了类似微软拼音的外观，其仓库位于：[GitHub:Fcitx5-Material-Color](https://github.com/hosxy/Fcitx5-Material-Color)，而且官方文档里提供了更加美观的单行模式的设置方式。

kde 桌面可安装这个配置工具，非kde不用安装。

```shell
kcm-fcitx5-git
```



## 配置环境变量 

1. vim ~/.pam_environment

```shell
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=\@im=fcitx
SDL_IM_MODULE DEFAULT=fcitx
```

2. vim  ~/.xporfile

```shell
export QT_IM_MODULE=fcitx5
```



## 使用美丽的单行模式(inline_preedit)

**编辑时请退出fcitx5,否则文件会覆盖**

对于fcitx5自带pinyin 请修改 `~/.config/fcitx5/conf/pinyin.conf`

对于fcitx5-rime，请新建/修改 `~/.config/fcitx5/conf/rime.conf`

加入/修改以下内容：

```shell
# 可用时在应用程序中显示预编辑文本
PreeditInApplication=True
```

kde用户可直接在 系统设置-区域设置-输入法-（选择你的输入法）设置-在程序中显示预编辑文本



## Fcitx5 无法输入中文
该问题在国内版 wps-office-cnAUR 11.1.0.9604-1 版本更新后部分用户出现，于 wps-office-cnAUR 11.1.0.9615-1 版本修复，但是部分用户仍然需要修改环境变量（例如 .xprofile 文件）：

```shell
export QT_IM_MODULE=fcitx5
```



## 参考文献

1. Fcitx5-Material-Color https://github.com/hosxy/Fcitx5-Material-Color

2. 在Manjaro上优雅地使用Fcitx5 [DotIN13](https://www.wannaexpresso.com/) https://www.wannaexpresso.com/2020/03/26/fcitx5/

3. wiki.archlinux Fcitx5 https://wiki.archlinux.org/index.php/Fcitx5



