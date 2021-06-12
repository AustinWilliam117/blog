---
title: Appium UI自动化测试
date: 2021-06-10 23:31:23
tags: ["自动化测试"]
categories: ["软件测试"]
---

### App自动化测试背景

随着移动终端的普及，手机应用越来越多，也越来越重要。App的回归测试用例数量也越来越多，全量回归也越来越消耗时间。另外移动端碎片化严重，尤其是Android端碎片化（不同的机型、不同的处理器架构、不同的系统版本、不同的厂商）严重性更为突出，市面上Android机型甚至有几万，几十万款，所以我们也需要通过这种自动化测试帮助我们减少兼容性的测试工作。总之为了减少这种重复的、大量回归到测试任务，我们迫切需要引进一些自动化测试来协助。

<!--more-->

### Appium自动化测试简介

Appium是一个开源（大量社区人员维护源码）的，适用于原生（native）（纯粹使用安卓自带的开发组件和应用，开发的产品）或混合（原生应用中嵌入html5页面）移动应用( hybrid mobile apps )的自动化测试框架。Appium应用 WebDriver （继承了WebDriver协议，在协议中封装和拓展）: JSON wire protocol 驱动安卓和iOS移动应用。

纯原生应用时效性差，例如：开发好的应用上传到应用商店，需要商店的审核。审核可能1-3天左右，IOS需要2周左右。618前一天开发好，急着上线。

混合移动应用：在应用中嵌入一些html5页面，在自己的应用中启用web端，嵌入html5。不需要对已有应用再次审核。但是体验性差，自适应差，响应时间久。

混合移动应用运用场景：商城中活动页面（快的更新频率）

#### App自动化测试工具对比

**iOS**

官方:

- Uiautomation/XCUITest: 白盒, UI测试, JS
  其他:
- FastMonkey: 性能(仿Monkey), 张钊

**Andorid**

官方:

- Uiautomator/Uiautomtor2: UI测试, Java
- Monkey: app性能/稳定性测试, 随机操作
- MonkeyRunner: UI测试, Jpython, 只能通过坐标定位
- Robotium: 白盒, UI测试, Java, 支持Webview/Toast/menu/Dialog等, 无法跨进程
- Espresso: 官方推荐扩展测试包, 白盒,ui, 一般开发自测使用
- CTS: 兼容性测试, Java

其他:

- Python-Uiautomotor2: UI测试, 使用简单, 支持无线连接设备及使用weditor查看元素定位
- Adb-For-Test/adb-For-Robotium: 个人, 基于adb命令的封装

**多平台支持**

- Calabash: iOS/Andriod/混合app, Ruby, BDD模式, Api丰富
- Appium: iOS/Andriod/混合app/H5, Java/Python/Ruby/JS..
- Macaco: 阿里基于Appium进行的精简封装的一套框架, 支持Electron应用, 包含app-inspector和ui-recorder, 统一了iOS/Android操作的Api, 目前坑比较多, 环境搭建较麻烦
- **Airtest(ATS)**: 网易推出的一款基于截图对比的App自动化测试工具, 可用于App游戏UI测试, 支持iOS/Android

**云平台**

- **Sauce Labs**: Appium官方推荐, 应用最广的云测平台, 收费
- Testin/腾讯云测等: 国内云平台, 收费
- OpenSTF: 开源手机集群管理平台, 免费

#### Appium的特点

- 支持多平台(Android、 iOS等)
- 支持多语言(python、 java、 ruby、 javascript、 c#等)
- Appium是跨平台的，可以用在OSX, Windows以及Linux 桌面系统上运行。（IOS应用只能在MAC上测试）
- Appium选择了Client/Server的设计模式。只要client能够发送http请求给server，那么的话client用什么语言来实现都是可以的，这就是如何做到支持多语言的原因:
- Appium扩 展了WebDriver的协议，没有自己重新去实现一套。这样的好处是以前的WebDriver API能够直接被继承过来，以前的Selenium (WebDriver) 各种语言的binding都可以拿来就用，省去了为每种语言开发一个client的工作量。

#### Appium底层处理流程

**Andorid(uiautomator)**

appium为C/S架构(C是指Client，S是指Server，C/S模式就是指客户端/服务器模式)，appium会暴露给我们一些API，我们调用API（Client）就会发起请求到Server端。Server会push一个bootstrap.jar包到手机里面。bootstrap.jar会分析、监听和转发，我们请求的命令，到底层调用我们Android底层的Uiautomator框架操作APP控件。

![](Appium实现原理.jpg)

1. 调用Android adb完成基本的系统操作
2. 向Android上部署bootstrap.jar
3. bootstrap.jar Forward Android的端口到PC机器上
4. PC上监听端口接收请求，使用webdriver协议
5. 分析命令并通过forward 端口发给bootstrap.jar
6. bootstrap.jar接收请求并把命令发给uiautomator
7. ui automator执行命令

**iOS**

1. client端 依然是 test script是我们的webdriver测试脚本。
2. 中间是起的Appium的服务，Appium在服务端起了一个Server（4723端口），跟selenium Webdriver测试框架类似， Appium⽀持标准的WebDriver JSONWireProtocol。在这里提供它提供了一套REST的接口,Appium Server接收web driver client标准rest请求，解析请求内容，调⽤用对应的框架响应操作。
3. appium server调用instruments.js 启动⼀一个socket server，同时分出一个⼦子进程运⾏instruments.app，将bootstrap.js（一个UIAutomation脚本）注⼊入到device⽤于和外界进行交互
4. 最后Bootstrap.js将执行的结果返回给appium server
5. appium server再将结果返回给 appium client

#### Appium的哲学

- 开源免费（大量社区人员维护源码）

- 不需要重新编译或者修改应用：不需要修改app代码就可以做自动化测试

  1. monkeyruner较早之前的app测试框架，只能用python写。不能通用于所有手机

  - 基于坐标去点击的，不同手机分辨率，坐标不相同，就会导致用例失败。点击空间的位置，一直在加载，然后又点下一个页面的控件，实际上还是点击本页面的控件，导致后面测试都挂了。因为他不能识别控件是否加载完毕

  2. monkeytalk基于控件定位测试，解决了monkeyrunner的弊端。但只能用JavaScript去写

  - 需要拿到源码，在其中插入agent代理，才能进行操作应用，可能出现问题，导致崩溃。对app有负面影响

  3. robotium基于控件定位，也能应用原生和混合。需要用java去实现自动化脚本，而且需要签名。对于app来说，最后打成包后缀是apk，要删掉重新签名，对文件有改变，有多余工作量。

  4. macaca阿里开发的测试框架，底层还是appium，只是写法更简单

- 不被一种语言或者框架约束

- 不重复造轮子

### [Appium自动化测试环境搭建](Appium App自动化测试环境搭建http://www.bcbxhome.com/bcbxxy/forum.php?mod=viewthread&tid=9)

- Python环境搭建
- 安装JDK, 配置环境变量
  - 新建系统变量
    - 变量名：JAVA_HOME
    - 变量值：java的安装路径
  - 新建path
    - %JAVA_HOME%\bin（bin里面包含java.exe，为路径下可执行程序）
- 安装Android SDK, 配置环境变量
  - 新建系统变量
    - 变量名：ADNDRIOD_HOME
    - 变量值：SDK的安装路径
  - 新建path：
    - %ANDROID_HOME%\platform-tools
    - %ANDROID_HOME%\tools
- 安装Appium-Windows-Desktop
- 安装Appium-Python-Client
  - `pip install selenium`
  - `pip install Appium-Python-Clinet`
- 安装模拟器

### 连接设备

**怎么校验手机连接上了**

命令行输入 `adb devices` 显示127.0.0.1:21503 device即可

**设备没连接上的几种情况**

- USB调试没打开，设置——开发者选项——USB调试
- 关掉模拟器，重新以管理员权限打开
- 版本不匹配：只要将sdk路径下的platform-tools路径中的如下三个文件：adb、AdbWinApi.dll、AdbWinUsbApi.dll 复制到逍遥模拟器安装目录下
- 以上都调式了，还是连接不上：可能手机没安装驱动，下载91助手，自动下载手机的驱动
- offline：数据线重新连接一下
- unauthorized：未经授权，授权即可

### Andorid sdk介绍

- add-ons: 附加库
- build-tools: 编译工具
- platform: 各版本sdk
- platforms-tools: 平台通用工具, 如adb
- tools: 常用工具

### Adb介绍

Adb(Android Debug Bridge): Andoid设备调试桥梁, 可以再PC端通过命令调试Android设备, 如获取设备状态, 安装/卸载app, 上传/下载文件等操作

### Adb常用命令

**开启/关闭服务**

- adb start-server: 开启服务
- adb kill-server: 关闭服务

**连接设备/获取连接状态(自动开启服务)**

- adb connect/disconnect 设备名或uuid: 连接/断开连接设备
- adb devices: 查看连接的设备

**安装/卸载app**

- adb install 安装包路径.apk
- adb uninstall apk包名

> 通过uiautomatorviewer可以获取获取apk包名

**上传/下载文件**

- 上传: adb push 本地文件 设备目录
- 下载: adb pull 设备文件 本地目录

```
Copyadb push 1.txt /sdcard/
adb pull sdcard/1.txt .
```

> adb shell: 可用于查看设备中的文件, exit退出

**强大的adb shell**

- pm: 应用及权限管理 `adb shell pm list packages`

- am: Activity操作 `adb shell am start -n 包名/包名.主Activity名`

- input: 模拟按键/输入

  - 点击(触控)指定坐标: `adb shell input tap 50 250`
  - 输入文字: `adb shell input text hello`
  - 按键: `adb shell input keyevent 3`
  - 滑动: `adb shell input swipe 300 1000 300 500`

- logcat: 日志查看及过滤(问题定位)

- monkey: 性能/稳定性测试

- dumpsys: 性能分析

- screencap: 截图 `adb shell screencap -p /sdcard/01.png`

- screenrecord: 录屏 `adb shell screenrecord --time-limit 10 /sdcard/demo.mp4`

- **获取包名** `adb shell dumpsys activity top |findstr "ACTIVITY"`

  ACTIVITY cn.xiaochuankeji.tieba（包名）/.ui.home.page.PageMainActivity（ACTIVITY名） 30649d5e pid=2339

  如果没有登录就可以拉取activity会很不安全，所以安卓默认拉取首个activity

  想要拉取启动的activity，需要在软件启动的一瞬间获取包名

**示例**:

> 配合uiautomatorviewer查看元素坐标, 使用bounds中x,y的平均值, 屏幕分辨率1280*760, 滑动时可取平均值

- 安装高仿微信app
- 启动app
- 点击登录按钮
- 输入18010181267
- 按TAB键
- 输入123456

```
Copyadb install app-debug.apk
adb am start -n com.lqr.wechat/com.lqr.wechat.com.lqr.wechat.ui.activity.SplashActivity
adb shell input tap 170 1197
adb shell input text 18010181267
adb shell input keyevent KEYCODE_TAB
adb adb shell input tap 360 498
adb shell input swipe 700 540 10 540  # 滑动时离开一定边界
adb shell screencap -p /sdcard/01.png
adb shell input keyevent 3 # 按HOME键
adb pull /sdcard/01.png .  # 下载图片
```

> 支持的KEYCODE

- 0 --> "KEYCODE_UNKNOWN"
- 1 --> "KEYCODE_MENU"
- 2 --> "KEYCODE_SOFT_RIGHT"
- 3 --> "KEYCODE_HOME"
- 4 --> "KEYCODE_BACK"
- 5 --> "KEYCODE_CALL"
- 6 --> "KEYCODE_ENDCALL"
- 7 --> "KEYCODE_0"
- 8 --> "KEYCODE_1"
- 9 --> "KEYCODE_2"
- 10 --> "KEYCODE_3"
- 11 --> "KEYCODE_4"
- 12 --> "KEYCODE_5"
- 13 --> "KEYCODE_6"
- 14 --> "KEYCODE_7"
- 15 --> "KEYCODE_8"
- 16 --> "KEYCODE_9"
- 17 --> "KEYCODE_STAR"
- 18 --> "KEYCODE_POUND"
- 19 --> "KEYCODE_DPAD_UP"
- 20 --> "KEYCODE_DPAD_DOWN"
- 21 --> "KEYCODE_DPAD_LEFT"
- 22 --> "KEYCODE_DPAD_RIGHT"
- 23 --> "KEYCODE_DPAD_CENTER"
- 24 --> "KEYCODE_VOLUME_UP"
- 25 --> "KEYCODE_VOLUME_DOWN"
- 26 --> "KEYCODE_POWER"
- 27 --> "KEYCODE_CAMERA"
- 28 --> "KEYCODE_CLEAR"
- 29 --> "KEYCODE_A"
- 30 --> "KEYCODE_B"
- 31 --> "KEYCODE_C"
- 32 --> "KEYCODE_D"
- 33 --> "KEYCODE_E"
- 34 --> "KEYCODE_F"
- 35 --> "KEYCODE_G"
- 36 --> "KEYCODE_H"
- 37 --> "KEYCODE_I"
- 38 --> "KEYCODE_J"
- 39 --> "KEYCODE_K"
- 40 --> "KEYCODE_L"
- 41 --> "KEYCODE_M"
- 42 --> "KEYCODE_N"
- 43 --> "KEYCODE_O"
- 44 --> "KEYCODE_P"
- 45 --> "KEYCODE_Q"
- 46 --> "KEYCODE_R"
- 47 --> "KEYCODE_S"
- 48 --> "KEYCODE_T"
- 49 --> "KEYCODE_U"
- 50 --> "KEYCODE_V"
- 51 --> "KEYCODE_W"
- 52 --> "KEYCODE_X"
- 53 --> "KEYCODE_Y"
- 54 --> "KEYCODE_Z"
- 55 --> "KEYCODE_COMMA"
- 56 --> "KEYCODE_PERIOD"
- 57 --> "KEYCODE_ALT_LEFT"
- 58 --> "KEYCODE_ALT_RIGHT"
- 59 --> "KEYCODE_SHIFT_LEFT"
- 60 --> "KEYCODE_SHIFT_RIGHT"
- 61 --> "KEYCODE_TAB"
- 62 --> "KEYCODE_SPACE"
- 63 --> "KEYCODE_SYM"
- 64 --> "KEYCODE_EXPLORER"
- 65 --> "KEYCODE_ENVELOPE"
- 66 --> "KEYCODE_ENTER"
- 67 --> "KEYCODE_DEL"
- 68 --> "KEYCODE_GRAVE"
- 69 --> "KEYCODE_MINUS"
- 70 --> "KEYCODE_EQUALS"
- 71 --> "KEYCODE_LEFT_BRACKET"
- 72 --> "KEYCODE_RIGHT_BRACKET"
- 73 --> "KEYCODE_BACKSLASH"
- 74 --> "KEYCODE_SEMICOLON"
- 75 --> "KEYCODE_APOSTROPHE"
- 76 --> "KEYCODE_SLASH"
- 77 --> "KEYCODE_AT"
- 78 --> "KEYCODE_NUM"
- 79 --> "KEYCODE_HEADSETHOOK"
- 80 --> "KEYCODE_FOCUS"
- 81 --> "KEYCODE_PLUS"
- 82 --> "KEYCODE_MENU"
- 83 --> "KEYCODE_NOTIFICATION"
- 84 --> "KEYCODE_SEARCH"
- 85 --> "TAG_LAST_KEYCODE"

### Appium API详细介绍

如何去定位元素，sdk/tools下uiautomatorviewer.bat双击打开

- `driver.find_element_by_id`

  ```python
  # 找到最右app推荐列表下第一个人昵称
  def test_element_by_id():
      driver.implicitly_wait(60)
      element = driver.find_element_by_id("cn.xiaochuankeji.tieba:id/simple_member_tv_name")
  	print(element.text)    
  ```

- `driver.find_elements_by_id`（app中id基本不唯一，平行结构id可能一样）

  ```python
  # 找到最右app上方菜单栏，打印并点击
  def test_element_by_id():
      driver.implicitly_wait(60)
      element = driver.find_elements_by_id("cn.xiaochuankeji.tieba:id/title")
  	print(element[2].text) 
      element[2].click()
  ```

- `driver.find_element_by_class_name`

- `driver.find_elements_by_class_name`

- `driver.find_element_by_xpath`

- `driver.find_elements_by_xpath`（一般不用）

