---
title: python内置函数
date: 2021-06-05 21:28:21
tags: ""
categories: ["Python"]
---

\1. time

\2. os

\3. sys

\4. classmethod 修饰符

\5. JSON字典的序列化与反序列化

如果你从Python解释器退出再进入，那么你定义的所有的方法和变量就都消失了。为此Python提供了一个办法，把这些定义存放在文件中，为一些脚本或者交互式的解释器实例使用，这个文件被成为模块

<!--more-->

### time内置函数

```python
# 获取时间戳（1970年开始计时）

import time

# 获取当前时间的时间戳
print(int(time.time()))
"""
1622864228
"""

#获取当前时间
print(time.localtime(time.time()))

"""
time.struct_time(tm_year=2021, tm_mon=6, tm_mday=5, tm_hour=11, tm_min=35, tm_sec=42, tm_wday=5, tm_yday=156, tm_isdst=0)
"""

# 格式化输出当前时间
print(time.strftime('%y-%m-%d %H:%M:%S', time.localtime()))

"""
21-06-05 11:39:11
"""

# time.sleep(2) 休眠2秒
```

### os内置函数

在python中，os模块提供了对操作系统进行操作的接口。查看os模块使用的方法为dir()，查看该模块的帮助方法为help()

```python
import os

# 在D盘创建文件夹log
os.mkdir('d:/log')

# 重命名文件夹
os.rename('d:/log','d:newLog')

# 删除文件夹
os.rmdir('d:/newLog')

"""对目录文件的处理"""

# 获取当前文件的目录 `os.path.dirname(__file__)`
print('当前文件的目录为：', os.path.dirname(__file__))

# 获取当前文件的目录的上一级目录
print('当前文件的上一级目录为：',os.path.dirname(os.path.dirname(__file__)))

# 实现对当前文件下其他文件的拼接
base_dir = os.path.dirname(__file__)
# 将本文件目录下的login文件与当前目录地址拼接
print(os.path.join(base_dir,'login'))
# 读取login文件下内容
f = open(os.path.join(base_dir,'login'),'r')
print(f.read())
f.close()

"""请求参数不确定，有可能一个，也可能N个参数"""

def func(*args,**kwargs):
    return kwargs

print(func(name='mengxun',age='18',address='beijing'))
```

### sys模块

sys 提供对解释器使用或维护的一些变量以及与解释器强烈交互的函数的访问

- import sys 引入 python 标准库中的 sys.py 模块
- sys.argv 是一个包含命令行参数的列表
- sys.path 包含了一个 Python 解释器自动查找所需模块的路径的列表

sys.argv[]说白了就是一个从程序外部获取参数的桥梁，这个“外部”很关键，所以那些试图从代码来说明它作用的解释一直没看明白。因为我们从外部取得的参数可以是多个，所以获得的是一个列表（list)，也就是说sys.argv其实可以看作是一个列表，所以才能用[]提取其中的元素。其第一个元素是程序本身，随后才依次是外部给予的参数。

下面我们通过一个极简单的test.py程序的运行结果来说明它的用法。将其保存在~/Desktop/test.py

```python
import sys

a = sys.argv[0]
print(a)
```

得到的结果为

```python
test.py
```

然后我们将代码中的0改为1：

```python
import sys

a = sys.argv[1]
print(a)
```

然后在shell中输入一个参数

```shell
$ python test.py what

what
```

得到的结果为我们输入的参数what，看到这里你是不是还不明白呢，那我们在把代码改一下

```python
import sys

a = sys.argv[2:]
print(a)
```

这次我们在shell中多添加几个参数，以空格隔开：

```shell
$ python test.py a b c d e f g

['b', 'c', 'd', 'e', 'f', 'g']
```

我们再稍微更改一下代码：

```python
import sys

a = sys.argv[:2]
print(a)
```

我们在shell中输入和上次一样的值

```shell
$ python test.py a b c d e f g

['test.py', 'a']
```

应该大彻大悟了吧。sys.argv[ ]其实就是一个列表，里边的项为用户输入的参数，关键就是要明白这参数是从程序外部输入的，而非代码本身的什么地方，要想看到它的效果就应该将程序保存了，从外部来运行程序并给出参数



**变量**

```python
import sys

if sys.argv[1] == 'sleep':
    print('sleep')
else:
    print('end')
    
    
$ python file.py sleep
sleep

$ python file.py s
end
```

**常用的方法**

```python
import sys

# 查看版本号
>>> print(sys.version)
3.9.5 (tags/v3.9.5:0a7dcbd, May  3 2021, 17:27:52) [MSC v.1928 64 bit (AMD64)]

# 查看平台
>>> print(sys.platform)
win32
```

**python包下的模块明显存在，而就是提示不存在**

```python
# 查看python的包

import sys

for item in sys.path:
    print(item)
    
...

C:\Python39\python39.zip
C:\Python39\DLLs
C:\Python39\lib
C:\Python39
C:\Python39\lib\site-packages
```

1. 标准库：C:\Python39\lib
2. 第三方库：C:\Python39\lib\site-packages
3. 自定义的库：

如果自己写的包，无法调用。可以通过`sys.path.append()`加入到

```python
import sys 

sys.path.append('包的地址')

# 比如 sys.path.append('D:\git\GITHUB\stack\day3')

from login import *

index() # 调用login文件下的函数

# 可以再次查看sys.path路径
for item in sys.path
	print(item)  
```

**IDE代码可以正常执行，当把代码上传到Linux中或CI之后，提示模块不存在**

```python
# 可以在文件之前添加这样的代码

file_path = os.path.join(os.path.dirname(os.path.dirname(__file__)),'包名')
sys.path.append(file_path)

from 包下的文件名 import 函数
from 包下的文件名 import *
```



### JSON字典的序列化与反序列化

- 序列化：把python的数据类型转化为str的类型过程
- 反序列化：把str的类型转化为python的数据结构

json.dumps 语法

>```
>json.dumps(obj, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, indent=None, separators=None, encoding="utf-8", default=None, sort_keys=False, **kw)
>```

```python
# 序列化 json.dumps()	将 Python 对象编码成 JSON 字符串

>>> import json

>>> dict1 = {"name":"mengxun","age":18}

# 序列化：dict-->str
>>> dict_str = json.dumps(dict1)
>>> print('dict_str',type(dict_str))

{"name": "mengxun", "age": 18} <class 'str'>
```

`sort_keys=True` 告诉编码器按照字典排序(a到z)输出，如果是字典类型的python对象，就把关键字按照字典排序。

`indent`参数根据数据格式缩进显示，读起来更加清晰

`separators`分隔符的意思，参数意思分别为不同dict项之间的分隔符和dict项内key和value之间的分隔符，把：和，后面的空格都除去了。

```python
# 使用参数让JSON数据格式化输出

>>> import json

>>> dict1 = {"name":"mengxun","age":18}
>>> dict_str = json.dumps(dict1,sort_keys=True,indent=4,separators=(',',':'))

'{\n    "age":18,\n    "name":"mengxun"\n}'
```



json.loads

json.loads 用于解码 JSON 数据。该函数返回 Python 字段的数据类型

语法

> ```
> json.loads(s[, encoding[, cls[, object_hook[, parse_float[, parse_int[, parse_constant[, object_pairs_hook[, **kw]]]]]]]])
> ```

```python
# 反序列化 json.loads() 将已编码的 JSON 字符串解码为 Python 对象

>>> import json

>>> jsonData = '{"name":"mengxun","age":18}'

>>> text = json.loads(jsonData)
>>> print(text,type(text))

{'name': 'mengxun', 'age': 18} <class 'dict'>
```



**列表的序列化与反序列化**

```python
# 列表的序列化
>>> import json

>>> list1 = ['admin','mengxun',18]

>>> list_str = json.dumps(list1)
>>> print(list_str,type(list_str))

["admin", "mengxun", 18] <class 'str'>


# 列表的反序列化

>>> import json

>>> str_list = json.loads(list_str)
>>> print(str_list,type(str_list))

['admin', 'mengxun', 18] <class 'list'>
```



**元组的序列化与反序列化**

```python
>>> import json

>>> tuple1=(1,2,3)

# 序列化
>>> tuple_str = json.dumps(tuple1)
>>> print(tuple_str,type(tuple_str))

[1, 2, 3] <class 'str'>

# 反序列化

>>> str_tuple = json.loads(tuple_str)
>>> print(str_tuple,type(str_tuple))

[1, 2, 3] <class 'list'>
```



## 附录

### Python os.path() 模块

os.path 模块主要用于获取文件的属性

以下是os.path 模块的几种常用方法

| os.path.abspath(path)               | 返回绝对路径                                                 |
| ----------------------------------- | ------------------------------------------------------------ |
| os.path.basename(path)              | 返回文件名                                                   |
| os.path.commonprefix(list)          | 返回list(多个路径)中，所有path共有的最长的路径               |
| os.path.dirname(path)               | 返回文件路径                                                 |
| os.path.exists(path)                | 如果路径 path 存在，返回 True；如果路径 path 不存在，返回 False。 |
| os.path.lexists                     | 路径存在则返回True,路径损坏也返回True                        |
| os.path.expanduser(path)            | 把path中包含的"~"和"~user"转换成用户目录                     |
| os.path.expandvars(path)            | 根据环境变量的值替换path中包含的"$name"和"${name}"           |
| os.path.getatime(path)              | 返回最近访问时间（浮点型秒数）                               |
| os.path.getmtime(path)              | 返回最近文件修改时间                                         |
| os.path.getctime(path)              | 返回文件 path 创建时间                                       |
| os.path.getsize(path)               | 返回文件大小，如果文件不存在就返回错误                       |
| os.path.isabs(path)                 | 判断是否为绝对路径                                           |
| os.path.isfile(path)                | 判断路径是否为文件                                           |
| os.path.isdir(path)                 | 判断路径是否为目录                                           |
| os.path.islink(path)                | 判断路径是否为链接                                           |
| os.path.ismount(path)               | 判断路径是否为挂载点                                         |
| os.path.join(path1[, path2[, ...]]) | 把目录和文件名合成一个路径                                   |
| os.path.normcase(path)              | 转换path的大小写和斜杠                                       |
| os.path.normpath(path)              | 规范path字符串形式                                           |
| os.path.realpath(path)              | 返回path的真实路径                                           |
| os.path.relpath(path[, start])      | 从start开始计算相对路径                                      |
| os.path.samefile(path1, path2)      | 判断目录或文件是否相同                                       |
| os.path.sameopenfile(fp1, fp2)      | 判断fp1和fp2是否指向同一文件                                 |
| os.path.samestat(stat1, stat2)      | 判断stat tuple stat1和stat2是否指向同一个文件                |
| os.path.split(path)                 | 把路径分割成 dirname 和 basename，返回一个元组               |
| os.path.splitdrive(path)            | 一般用在 windows 下，返回驱动器名和路径组成的元组            |
| os.path.splitext(path)              | 分割路径，返回路径名和文件扩展名的元组                       |
| os.path.splitunc(path)              | 把路径分割为加载点与文件                                     |
| os.path.walk(path, visit, arg)      | 遍历path，进入每个目录都调用visit函数，visit函数必须有3个参数(arg, dirname, names)，dirname表示当前目录的目录名，names代表当前目录下的所有文件名，args则为walk的第三个参数 |
| os.path.supports_unicode_filenames  | 设置是否支持unicode路径名                                    |

实例

```python
import os

print(os.path.basename('~/Desktop/runboot.txt'))		# 返回文件名
print(os.path.dirname('~/Desktop/runboot.txt'))			# 返回目录路径
print(os.path.split('~/Desktop/runboot.txt'))				# 分割文件名和路径
print(os.path.join('~','test','runboot.txt'))				# 将目录和文件名合成一个路径
```

执行以上程序输出结果为：

```python
runboot.txt
~/Desktop
~/test/runboot.txt
~/test/runboot.txt
```

以下实例输出文件的相关信息

```python
import os
import time
 
file='/root/runoob.txt' # 文件路径
 
print( os.path.getatime(file) )   # 输出最近访问时间
print( os.path.getctime(file) )   # 输出文件创建时间
print( os.path.getmtime(file) )   # 输出最近修改时间
print( time.gmtime(os.path.getmtime(file)) )  # 以struct_time形式输出最近修改时间
print( os.path.getsize(file) )   # 输出文件大小（字节为单位）
print( os.path.abspath(file) )   # 输出绝对路径
print( os.path.normpath(file) )  # 规范path字符串形式
```

```python
1623576450.1366549
1623576450.1366549
1623576450.1366549
time.struct_time(tm_year=2021, tm_mon=6, tm_mday=13, tm_hour=9, tm_min=27, tm_sec=30, tm_wday=6, tm_yday=164, tm_isdst=0)
531
/root/runoob.txt
/root/runoob.txt
```