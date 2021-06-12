---
title: python内置函数
date: 2021-06-05 21:28:21
tags: ""
categories: ["Python"]
---

1. time
2. os
3. sys
4. classmethod 修饰符
5. JSON字典的序列化与反序列化

### time内置函数

<!--more--> 

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

### classmethod 修饰符

classmethod 修饰符对应的函数不需要实例化，不需要 self 参数，但第一个参数需要是表示自身类的 cls 参数，可以来调用类的属性，类的方法，实例化对象等。

```python
class a(object):
    bar = 1
    
    def func1(cls):
        print('foo')

    @classmethod
    def func2(cls):
        print('func2')
        print(cls.bar)

        cls().func1()   #调用foo方法


a.func2()               #不需要实例化
```

结果
```python
func2
1
foo
```

### JSON字典的序列化与反序列化

- 序列化：把python的数据类型转化为str的类型过程
- 反序列化：把str的类型转化为python的数据结构
<!--more-->
json.dumps 语法

>```
json.dumps(obj, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, indent=None, separators=None, encoding="utf-8", default=None, sort_keys=False, **kw)
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
