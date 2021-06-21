---
title: Appium自动化框架
date: 2021-06-19 21:54:47
tags: ["自动化测试"]
categories: ["软件测试"]
---

对测试人来说，Appium 是非常重要的一个开源跨平台自动化测试工具，它允许测试人员在不同的平台（iOS、Android 等）使用同一套 API 来写自动化测试脚本，这样可大幅提升代码复用率和工作效率。

本文汇总了从 Appium 基础到自动化测试高级实战中，所涉及到的方方面面的知识点精华内容（如下所示），希望对大家快速总结和复习有所帮助。

<!--more-->

### **Appium 从基础到自动化测试框架实战**

1. Appium 基础 1(环境搭建和简介)
2. Appium 基础 2(元素定位和元素常用方法)
3. Appium 基础 3(手势操作和 uiautomator 查找元素)
4. Appium 基础 4(显式等待)
5. Appium 基础 5(toast 和参数化)
6. Appium 基础 6(webview)
7. Appium_ 企业微信练习 (非 PO，增加和删除联系人)
8. Appium_ 企业微信练习 (PO--增加联系人)

### **Appium 环境搭建**

### **JDK 的搭建**

- 下载 1.8 的 jdk
- 新建环境变量：JAVA_HOME 值为：D:\Program Files\Java\jdk1.7.0
- 新建环境变量：CLASSPATH 值为：.;%JAVA_HOME%\lib;（注意：点号表示当前目录，不能省略）
- 在系统变量 Path 的值的前面加入以下内容：%JAVA_HOME%\bin

### **SDK 的配置**

- 下载 sdk
- 打开 sdk 的 sdk manager，安装 tools 前 3 个东西和 google 的 usb 驱动
- 配置 Android home 里面的 platform-tools 和 tools

### **Appium 的搭建**

- 安装 node.js，配置 node.js 的环境变量
- npm install -g cnpm --registry=[https://registry.npm.taobao.org](https://link.zhihu.com/?target=https%3A//registry.npm.taobao.org)
- cnpm install -g appium
- cnpm install -g appium-doctor
- pip install appium-python-client

### **appium 运行的 python 代码**

- mumu 连接 adb 是：adb connect 127.0.0.1:7555

```python
from appium import webdriver

#设置 caps 的值
desire_cap= {
    #默认是 Android
    "platformName":"android",
    #adb devices 的 sn 名称
    "deviceName":"127.0.0.1:7555",
    #包名
    "appPackage":"com.xueqiu.android",
    #activity 名字
    "appActivity":".view.WelcomeActivityAlias"
}

#运行 appium，前提是要打开 appium server
driver=webdriver.Remote("http://127.0.0.1:4723/wd/hub",desire_cap)
```

### **Appium 的简介**

### **Appium 的引擎**

- Android 是 uiautomator2
- ios 是 xcuitest

### **Appium 的设计理念**

- webdriver 是基于 http 协议的，第一连接会建立一个 session 会话，并通过 post 发送一个 json 告知服务端相关测试信息
- client/server 设计模式
- 客户端通过 webdriver json wire 协议与服务器通讯
- 多语言支持
- server 可以放在任何地方
- 服务器 nodejs 开发的 http 服务
- appium 使用 appium-xcuitest-driver 来测试 iphone 设备，其中需要安装 Facebook 出的 WDA(webdriver agent) 来驱动 ios 测试

### **Appium 的生态工具**

- adb：Android 控制工具
- appium Destkop：内嵌 appium server 和 inspector 的综合工具
- appium server：appium 的核心工具，命令行工具
- appium client：各种语言的客户端封装库，用户连接 appium server，包含 python、java、ruby 等
- appcrawler 自动遍历工具

### **获取 App 的信息**

- 获取当前元素界面：adb shell dumpsys activity top
- 获取任务列表：adb shell dumpsys activity activities
- 获取 app 的 package 和 activity：adb shell；然后 logcat | grep -i displayed
- 启动应用:adb shell am start -W -n "com.xueqiu.android/.view.WelcomeActivityAlias -S

### **Capability 设置**

- 文档地址：[http://appium.io/docs/en/writing-running-appium/caps/index.html](https://link.zhihu.com/?target=http%3A//appium.io/docs/en/writing-running-appium/caps/index.html)
- platformName:android 通常都是写 android
- deviceName:127.0.0.1:7555 这个通常是 adb devices 的名称
- appPackage:com.xueqiu.android 这个是 app 的 package 包名
- appActivity:.view.WelcomeActivityAlias 这个是 app 的 activity 名
- noReset：true, false 是否重置测试的环境（例如首次打开弹框，或者登陆信息）
- unicodeKeyboard：true, false 是否需要输入非英文之外的语言并在测试完成后重置输入法，比如输入中文
- dontStopAppOnReset：true, false 首次启动的时候，不停止 app
- skipDeviceInitialization：true, false 跳过安装，权限设置等操作

### **测试用的 apk**

- [https://github.com/appium/appium/tree/master/sample-code/apps](https://link.zhihu.com/?target=https%3A//github.com/appium/appium/tree/master/sample-code/apps)

### **Android 的基础知识**

### **Android 的布局**

- Android 是通过容器的布局属性来管理子控件的位置关系，布局过程就是把界面上的所有的控件，根据他们的间距的大小，摆放在正确的位置
- 线性布局：LinearLayout
- 相对布局：RelativeLayout
- 帧布局：FrameLayout
- 绝对布局：AbsoluteLayout
- 表格布局：TableLayout
- 网格布局：GirdLayout
- 约束布局：ConstraintLayout

### **Android 四大组件**

- activity：与用户交互的可视化界面
- service：实现程序后台运行的解决方案，比如 qq 音乐的音乐在后台运行，没有界面
- content provide：内容提供者，提供程序所需要的数据，比如？提供数据库？
- broadcast receiver：广播接收器，监听外部事件的到来（比如来电）

### **Android 常用的控件**

- TextView：文本控件
- EditText：可编辑文本控件
- Button：按钮
- ImageButton：图标按钮
- ToggleButton:开关按钮
- ImageView：图片控件
- CheckBox：复选框控件
- RadioButton：单选框控件

### **控件知识**

- dom：Document Object Model 文档对象模型
- dom 应用：最早应用于 html 和 js 的交互，用户表示界的控件层级，界面的结构化描述，常见的格式为 html、xml。核心元素为节点和属性
- xpath：xml 路径语言，用于 xml 中的节点定位
- Android 的应用层级结构是定制的 xml
- app source 类似于 dom，表示 app 的层级，表示界面里面所有的控件数的结构
- 每个控件都有它的属性（resourceid、xpath、aid），没有 css 属性

### **Appium 的元素定位**

### **普通方式的定位**

- driver.find_element_by_accessibility_id() 对应 content-desc
- driver.find_element_by_id() 对应 resource-id
- driver.find_element_by_name() 对应 text
- driver.find_element_by_xpath() 对应 xpath

### **By 的定位方式**

- 首先要 from appium.webdriver.common.mobileby import MobileBy as By
- self.driver.find_element(By.ID,"") 对应 resource-id
- self.driver.find_element(By.XPATH,"") 对应 xpath
- self.driver.find_element(By.ACCESSIBILITY_ID,"") 对应 content-desc
- self.driver.find_element(By.NAME,"") 对应 text

### **Xpath 的定位方式**

- driver.find_element_by_xpath("//*[@text=' 扫一扫 ']")
- driver.find_element_by_xpath("//*[@resource-id='com.taobao.taobao:id/tv_scan_text']")
- driver.find_element_by_xpath("//*[@content-desc=' 帮助 ']")
- driver.find_element(By.XPATH,"//*[@resource-id='com.xueqiu.android:id/name' and @text=' 阿里巴巴 ']") and 的使用
- 父类和兄弟类的方法：//*[@text=' 性别 ']/..//*[@text=' 男 ']。其中 /.. 表示父类，//* 就是兄弟，孙子等类
- //*[Contains(@text,"tong")] 这是 xpath 的 text 模糊搜索的方法

### **元素的方法**

### **元素的常用方法**

- 点击方法：element.click()
- 输入操作：element.send_keys("tong")
- 设置元素的值：element.set_value("tongtong")
- 清除操作：element.clear()
- 是否可见：element.is_displayed 返回 true or false
- 是否可用：element.enabled() 返回 true or false
- 是否被选中：element.is_selected() 返回 true or false
- 获取属性值：element.get_attribute(name)

### **属性值介绍**

- get_attribute(name) 获取的属性名称和 uiautomatorviewer 的一致，但是 index 的值获取不了
- 真假获取的值是 true 和 false 的字符串，并不是 python 的 boolean 值

### **元素常用的属性**

- 获取元素文本：element.text
- 获取元素坐标：element.location
- 结果：{'y':19,'x':498}
- 获取元素尺寸（高和宽）：element.size
- 结果：{'width':500,'height':22}

### **实战小案例 1**

1. 打开雪球 app
2. 点击搜索输入框
3. 向搜索输入框输入 “阿里巴巴”
4. 在搜索的结果里选择阿里巴巴，然后点击
5. 获取这只上香港 阿里巴巴的股价，并判断这只股价的价格>200

### **代码**

```python
from time import sleep
from appium import webdriver
from appium.webdriver.common.mobileby import MobileBy as By

class TestFind():
    #设置 caps 的值
    def setup(self):
        self.desire_cap= {
            #默认是 Android
            "platformName":"android",
            #adb devices 的 sn 名称
            "deviceName":"127.0.0.1:7555",
            #包名
            "appPackage":"com.xueqiu.android",
            #activity 名字
            "appActivity":".view.WelcomeActivityAlias",
            "noReset":"true",
            "unicodeKeyboard":True
        }
        #运行 appium，前提是要打开 appium server
        self.driver=webdriver.Remote("http://127.0.0.1:4723/wd/hub",self.desire_cap)
        self.driver.implicitly_wait(5)

    def test_search(self):
        """
        1. 打开雪球 app
        2. 点击搜索输入框
        3. 向搜索输入框输入 “阿里巴巴”
        4. 在搜索的结果里选择阿里巴巴，然后点击
        5. 获取这只上香港 阿里巴巴的股价，并判断这只股价的价格>200
        :return:
        """
        sleep(3)
        #点击搜索框
        self.driver.find_element(By.ID,"com.xueqiu.android:id/tv_search").click()
        #向搜索框输入阿里巴巴
        self.driver.find_element(By.ID,"com.xueqiu.android:id/search_input_text").send_keys(" 阿里巴巴 ")
        #找到搜索框预览结果的阿里巴巴，并点击
        self.driver.find_element(By.XPATH,"//*[@resource-id='com.xueqiu.android:id/name' and @text=' 阿里巴巴 ']").click()
        #选择 HK 股价的元素
        prices=self.driver.find_elements(By.ID,"com.xueqiu.android:id/current_price")[1]
        #提取股价的 text 属性
        price=float(prices.text)

        #判断股价是否大于 200
        assert price > 200
```

### **实战小案例 2**

1. 打开雪球首页
2. 定位首页的搜索框
3. 判断搜索框是否可用，并查看搜索框 name 属性值
4. 打印搜索框这个元素的左上角坐标和它的宽高
5. 向搜索框输入：alibaba
6. 判断阿里巴巴是否可见
7. 如果可见，打印搜索成功点击，如果不可见，打印搜索失败

### **代码**

```python
from time import sleep
from appium import webdriver
from appium.webdriver.common.mobileby import MobileBy as By


class TestFind():
    #设置 caps 的值
    def setup(self):
        self.desire_cap= {
            #默认是 Android
            "platformName":"android",
            #adb devices 的 sn 名称
            "deviceName":"127.0.0.1:7555",
            #包名
            "appPackage":"com.xueqiu.android",
            #activity 名字
            "appActivity":".view.WelcomeActivityAlias",
            "noReset":"true",
            "unicodeKeyboard":True
        }
        #运行 appium，前提是要打开 appium server
        self.driver=webdriver.Remote("http://127.0.0.1:4723/wd/hub",self.desire_cap)
        self.driver.implicitly_wait(5)

    def test_element_function(self):
        """
        1. 打开雪球首页
        2. 定位首页的搜索框
        3. 判断搜索框是否可用，并查看搜索框 name 属性值
        4. 打印搜索框这个元素的左上角坐标和它的宽高
        5. 向搜索框输入：alibaba
        6. 判断阿里巴巴是否可见
        7. 如果可见，打印搜索成功点击，如果不可见，打印搜索失败
        :return:
        """
        sleep(8)
        #找到搜索框的元素
        search=self.driver.find_element(By.ID, "com.xueqiu.android:id/tv_search")
        #当搜索框是可用（类似可点击）后才进行下面的操作，is_enabled() 返回 Ture or False
        if search.is_enabled():
            #打印搜索框的 text 值
            print(search.text)
            #打印搜索框左上角的坐标
            print(search.location)
            #打印搜索框的高和宽
            print(search.size)
            #点击搜索框，才可以进行下面的操作
            search.click()
            #在搜索框中输入阿里巴巴
            self.driver.find_element(By.ID, "com.xueqiu.android:id/search_input_text").send_keys(" 阿里巴巴 ")
            #定义找到预览结果的阿里巴巴的元素
            alibaba=self.driver.find_element(By.XPATH, "//*[@resource-id='com.xueqiu.android:id/name' and @text=' 阿里巴巴 ']")
            #当 alibaba 元素可见，打开搜索成功，否则打印搜索失败
            if alibaba.is_displayed():
                print(" 搜索成功 ")
            else:
                print(" 搜索失败 ")
```

### TestCase 脚本

```python
import os
import unittest
import time
from appium import webdriver


class AndroidTests(unittest.TestCase):
    def setUp(self):
        # 定义一个空字典
        desired_caps = {}
        # 平台名称
        desired_caps['platformName'] = 'Android'
        # 平台版本（手机版本号）
        desired_caps['platformVersion'] = '5.1'
        # 设备名称固定写Android Emulator不会强行校验值
        desired_caps['deviceName'] = 'Android Emulator'
        # 不清除app数据，清除后犹如新设备安装的新软件
        desired_caps['noReset'] = 'True'
        # app包名，app之间用唯一的包名区分
        desired_caps['appPackage'] = 'cn.xiaochuankeji.tieba'
        # 一个界面就是一个activity，app中每个界面通过activity区分
        desired_caps['appActivity'] = '.ui.base.SplashActivity'
        # 初始化driver，远程连接，请求参数到appium server上
        self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)

    def tearDown(self):
        #self.driver.quit()
        pass

    def test_element_by_id(self):
        self.driver.implicitly_wait(60)
        el = self.driver.find_elements_by_id("cn.xiaochuankeji.tieba:id/title")
        print(el[2].text)
        el[2].click()



if __name__ == '__main__':
    suite = unittest.TestLoader().loadTestsFromTestCase(AndroidTests)
    unittest.TextTestRunner(verbosity=2).run(suite)

```