---
title: 浏览器如何识别selenium及爬虫如何绕过反爬
date: 2021-01-29 20:53:56
tags: ["爬虫"]
categories: ["Python"]
---

各位做爬虫的对selenium应该都很熟悉了，我们经常会拿selenium进行自动登录来搭建cookie池，对于不想自己网站被爬的站主/开发人员来说，防止自动化脚本操作网站自然是反爬必须要做的工作。那么，他们究竟有哪些手段来检测用户是否是selenium呢？今天就来总结一下常见的识别selenium的方法以及各种解决之道。

<!--more-->

最广为人知的识别是否是selenium的方法就是window.navigator.webdriver，当浏览器被打开后，js就会给当前窗口一个window属性，里面存放着用户的各种"信息"。话不多说直接上图

![img](https://pic3.zhimg.com/80/v2-25a344a9bd1d04dec2f602ef741292ee_720w.jpg)正常用户访问时的webdriver为undefined

![img](https://pic4.zhimg.com/80/v2-997a5e933f19e4af17848afa9714034b_720w.png)selenium访问时为true

明白了这个属性，我们来一段简单的js来反爬

```js
<script>
if(window.navigator.webdriver == true){
    document.write("<span>看到这段就代表你是爬虫</span>")
}else{
    document.write("<span>真正的信息在这儿呢</span>")
}
</script>
```

现在把这段代码保存到HTML中分别正常打开和selenium打开

![img](https://pic4.zhimg.com/80/v2-c17c45d289ce9e3a42bd2cc546f84c43_720w.jpg)selenium打开

![img](https://pic1.zhimg.com/80/v2-12afcfa70a958a371d24bfbb49d39b7c_720w.jpg)正常打开

其实，不只是webdriver，selenium打开浏览器后，还会有这些特征码:

```text
webdriver  
__driver_evaluate  
__webdriver_evaluate  
__selenium_evaluate  
__fxdriver_evaluate  
__driver_unwrapped  
__webdriver_unwrapped  
__selenium_unwrapped  
__fxdriver_unwrapped  
_Selenium_IDE_Recorder  
_selenium  
calledSelenium  
_WEBDRIVER_ELEM_CACHE  
ChromeDriverw  
driver-evaluate  
webdriver-evaluate  
selenium-evaluate  
webdriverCommand  
webdriver-evaluate-response  
__webdriverFunc  
__webdriver_script_fn  
__$webdriverAsyncExecutor  
__lastWatirAlert  
__lastWatirConfirm  
__lastWatirPrompt  
```

只要识别到这些，那么该用户就是selenium无误了，目前针对selenium的反爬，都是从这些特征码下手的，那么该怎么反反爬呢？

1. 使用火狐浏览器

大家先别急着笑，很多时候selenium+谷歌打不开目标网站，都可以用火狐试试。因为selenium只是一个控制浏览器的工具，而chromedriver和geckodriver都不是selenium官方发布的（鬼知道谁发布的），因此在控制浏览器方面会有不同的差异，具体原理不再赘述，总之很多网站不能用selenium+chrome就可以试试firefox。

（理论上IE也可能会达到相应效果，但IE内核实在太烂了，selenium+IE=龟速爬虫）

\2. 给webdriver的options增加参数

谷歌浏览器的设置中有一个参数名为excludeSwitches，它的值是一个数组，向里面添加chrome的命令就可以在selenium打开chrome后自动执行数组内的指令，我们向里面添加一个enable-automation

```python3
from selenium import webdriver
from selenium.webdriver import ChromeOptions

option = ChromeOptions()
option.add_experimental_option('excludeSwitches', ['enable-automation'])
brower = webdriver.Chrome(options=option)
brower.get('file:///C:/Users/Administrator/Desktop/js.html')
```

此时运行这段代码，发现可以拿到正确的信息

![img](https://pic2.zhimg.com/80/v2-02e2106ce10bdab43fcf4ce1fdafb7e5_720w.jpg)右上角会跳出提示，不要管它，更不要点停用

如果你觉得右上角的提示太碍眼，可以参考这篇教程禁用此提示

[彻底禁用Chrome的“请停用以开发者模式运行的扩展程序”提示www.jianshu.com![图标](https://pic3.zhimg.com/v2-9769537726985ead0ed24f3a77d64aaa_180x120.jpg)](https://link.zhihu.com/?target=https%3A//www.jianshu.com/p/b5deac715115)

\3. 中间人代理mitmproxy

爬过app的朋友应该对这玩意儿不陌生，简单介绍一下吧。mitmproxy其实和fiddler/charles等抓包工具的原理有些类似，作为一个第三方，它会把自己伪装成你的浏览器向服务器发起请求，服务器返回的response会经由它传递给你的浏览器，你可以通过编写脚本来更改这些数据的传递，从而实现对服务器的“欺骗”和对客户端的“欺骗”。具体原理和使用见此

[使用 mitmproxy + python 做拦截代理blog.wolfogre.com![图标](https://pic4.zhimg.com/v2-736d57a8dd324d73dd46a03a064f8fdb_180x120.jpg)](https://link.zhihu.com/?target=https%3A//blog.wolfogre.com/posts/usage-of-mitmproxy/)

下面提供一个防屏蔽selenium的简单demo

```text
# my_demo.py
from mitmproxy import ctx  
    
def response(flow):  
    # 'js'字符串为目标网站的相应js名 
    if 'js' in flow.request.url:  
        for i in ['webdriver', '__driver_evaluate', '__webdriver_evaluate', '__selenium_evaluate', '__fxdriver_evaluate', '__driver_unwrapped', '__webdriver_unwrapped', '__selenium_unwrapped', '__fxdriver_unwrapped', '_Selenium_IDE_Recorder', '_selenium', 'calledSelenium', '_WEBDRIVER_ELEM_CACHE', 'ChromeDriverw', 'driver-evaluate', 'webdriver-evaluate', 'selenium-evaluate', 'webdriverCommand', 'webdriver-evaluate-response', '__webdriverFunc', '__webdriver_script_fn', '__$webdriverAsyncExecutor', '__lastWatirAlert', '__lastWatirConfirm', '__lastWatirPrompt', '$chrome_asyncScriptInfo', '$cdc_asdjflasutopfhvcZLmcfl_']:  
            ctx.log.info('Remove %s from %s.' % (i, flow.request.url))  
            flow.response.text = flow.response.text.replace('"%s"' % (i), '"NO-SUCH-ATTR"')  
        flow.response.text = flow.response.text.replace('t.webdriver', 'false')  
        flow.response.text = flow.response.text.replace('ChromeDriver', '')
```

然后我们使用如下命令行启动脚本

```text
mitmdump.exe -S my_demo.py
```

然后通过selenium就可以正常访问一些屏蔽selenium的网站了

\4. pyppeteer

先简单介绍一下puppeteer，这玩意儿是一个基于node.js的chrome官方框架，主要用于操作谷歌无头模式进行各种操作，pyppeteer则是puppeteer的python版本。

它的作用和selenium是类似的，通过脚本操作无头谷歌，但是它并不会有selenium那么多的特征字符串，可以做到完全把“自己”当作真人操作。当然，它还是有缺点的.虽然puppeteer一直在更新，但是pyppeteer已经停止更新将近一年了，所以无法保证它以后是否可用。同样因为它是基于谷歌无头的，因此它只能用于谷歌无头，不想selenium一样，编写完脚本只需改变少量代码，便可以在多种浏览器中运行。下面是pyppeteer的官方文档：

[Pyppeteer’s documentationmiyakogi.github.io](https://link.zhihu.com/?target=https%3A//miyakogi.github.io/pyppeteer/)



下面是一个简单的demo

```text
import asyncio
from pyppeteer import launch

async def main():
    browser = await launch()
    page = await browser.newPage()
    await page.goto('file:///C:/Users/Administrator/Desktop/js.html')
    print(await page.content())

asyncio.get_event_loop().run_until_complete(main())
```

如果你电脑中没有chromium，执行这段代码后会自动帮你安装，然后再运行这段代码，但是非常慢，所以建议自己网上下载chromium后再执行脚本

\5. 编译后的chromedriver

鬼知道为什么又是chrome......最近发现的一个比较有趣的chromedriver，与一般chromedriver不同的是它经过了一些底层的修改，可以直接使用它来登录一些对selenium有检测的网站（比如某宝），有兴趣的可以私聊我获取

目前个人已知的就这几种解决方法，欢迎补充更新~


https://zhuanlan.zhihu.com/p/85663187

这里提供了防淘宝检测的简单demo视频版及源码（已失效），编译后的chromedriver，大家可以参考一下，编译后的chromedriver的网盘链接也在文末，大家自行获取~

转载：[看起来是不是很凶](https://www.zhihu.com/people/z-ri-tian)

https://zhuanlan.zhihu.com/p/78368287