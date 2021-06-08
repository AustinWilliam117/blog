---
title: Python文件
date: 2021-06-07 21:01:45
tags: ""
categories: ["Python"]
---

### open 方法

Python open() 方法用于打开一个文件，并返回文件对象，在对文件进行处理过程中都需要使用这个函数，如果文件无法被打开，会抛出OSError

<!--more-->

**使用 open() 方法一定要保证关闭文件对象，即调用 close() 方法**

open() 函数常用形式是接收两个参数：文件名(file)和模式(mode)

`opne(file,momde='r')`

完整的语法格式为：

`open(file,mode='r',buffering=-1,encoding=None,errors=None,newline=None, closefd=True, opener=None)`

参数说明

- file: 必需，文件路径（相对或者绝对路径）。
- mode: 可选，文件打开模式
- buffering: 设置缓冲
- encoding: 一般使用utf8
- errors: 报错级别
- newline: 区分换行符
- closefd: 传入的file参数类型
- opener: 设置自定义开启器，开启器的返回值必须是一个打开的文件描述符。

mode参数有：

| t    | 文本模式 (默认)。                                            |
| ---- | ------------------------------------------------------------ |
| x    | 写模式，新建一个文件，如果该文件已存在则会报错。             |
| b    | 二进制模式。                                                 |
| +    | 打开一个文件进行更新(可读可写)。                             |
| U    | 通用换行模式（**Python 3 不支持**）。                        |
| r    | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。 |
| rb   | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。一般用于非文本文件如图片等。 |
| r+   | 打开一个文件用于读写。文件指针将会放在文件的开头。           |
| rb+  | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。一般用于非文本文件如图片等。 |
| w    | 打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| wb   | 以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。 |
| w+   | 打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。 |
| a    | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| ab   | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| a+   | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
| ab+  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |

默认为文本模式，如果要以二进制模式打开，加上*b*

**file 对象**

file 对象使用 open 函数来创建，下表列出了 file 对象常用的函数：

| 1    | [file.close()](https://www.runoob.com/python3/python3-file-close.html)关闭文件。关闭后文件不能再进行读写操作。 |
| ---- | ------------------------------------------------------------ |
| 2    | [file.flush()](https://www.runoob.com/python3/python3-file-flush.html)刷新文件内部缓冲，直接把内部缓冲区的数据立刻写入文件, 而不是被动的等待输出缓冲区写入。 |
| 3    | [file.fileno()](https://www.runoob.com/python3/python3-file-fileno.html)返回一个整型的文件描述符(file descriptor FD 整型), 可以用在如os模块的read方法等一些底层操作上。 |
| 4    | [file.isatty()](https://www.runoob.com/python3/python3-file-isatty.html)如果文件连接到一个终端设备返回 True，否则返回 False。 |
| 5    | [file.next()](https://www.runoob.com/python3/python3-file-next.html)**Python 3 中的 File 对象不支持 next() 方法。**返回文件下一行。 |
| 6    | [file.read([size\])](https://www.runoob.com/python3/python3-file-read.html)从文件读取指定的字节数，如果未给定或为负则读取所有。 |
| 7    | [file.readline([size\])](https://www.runoob.com/python3/python3-file-readline.html)读取整行，包括 "\n" 字符。 |
| 8    | [file.readlines([sizeint\])](https://www.runoob.com/python3/python3-file-readlines.html)读取所有行并返回列表，若给定sizeint>0，返回总和大约为sizeint字节的行, 实际读取值可能比 sizeint 较大, 因为需要填充缓冲区。 |
| 9    | [file.seek(offset[, whence\])](https://www.runoob.com/python3/python3-file-seek.html)移动文件读取指针到指定位置 |
| 10   | [file.tell()](https://www.runoob.com/python3/python3-file-tell.html)返回文件当前位置。 |
| 11   | [file.truncate([size\])](https://www.runoob.com/python3/python3-file-truncate.html)从文件的首行首字符开始截断，截断文件为 size 个字符，无 size 表示从当前位置截断；截断之后后面的所有字符被删除，其中 windows 系统下的换行代表2个字符大小。 |
| 12   | [file.write(str)](https://www.runoob.com/python3/python3-file-write.html)将字符串写入文件，返回的是写入的字符长度。 |
| 13   | [file.writelines(sequence)](https://www.runoob.com/python3/python3-file-writelines.html)向文件写入一个序列字符串列表，如果需要换行则要自己加入每行的换行符。 |

### with open()

#### 读取整个文件

要读取整个文件，需要包含几行文本文件。下面创建一个文件**pi_digits.txt**

```txt
3.1415926535
8979323846
2643383279
```

下面程序打开并读取这个文件，在将其内容显示在屏幕上，**file_reader.py**

```python
with open('pi_digits.txt') as file_object:
    contents = file_object.read()
    print(contents)
```

函数open()接受一个参数，要打开的文件名称。Python在当前执行的文件所在目录中查找指定的文件。函数open()返回一个表示文件的对象。在这里，open('pi_digits.txt') 返回一个表示文件 pi_digits.txt 的对象；Python将这个对象存储在我们将在后面使用的变量中。

关键字with在不需要访问文件后将其关闭。在这个程序中，我们调用了open()，但没有调用close()；通过with open可以让Python去确定，在合适的时候将文件自动关闭。

在有了文件对象后，我们使用read()方法来读取这个文件的全部内容，并将其作为一个长长的字符串存储在变量contents中，再通过打印contents显示文本内容

```python
[william@William-arch Desktop]$ python file_reader.py
3.1415926
897479203
937743020

[william@William-arch Desktop]$ 

```

相比原始文件，该输出唯一不同的地方是末尾多了一行空行。为何多出一行空行呢？因为read()到达文件末尾时返回一个空字符串，而将这个空字符串显示出来就是一个空行。要删除多出来的空行，可在print语句中使用rstrip()：

```python
with open('pi_digits.txt') as file_object:
    contents = file_object.read()
    print(contents.rstrip())
```

```shell
[william@William-arch Desktop]$ python file_reader.py
3.1415926
897479203
937743020
[william@William-arch Desktop]$ 

```

#### 文件路径

注意Windows系统中，在文件路径中使用反斜杠（ \ ）而不是斜杠 ( / )

#### 逐行读取

读取文件时，常常需要检查其中的每一行：你可能要在文件中查找特定的信息，或者要以某种方式修改文件中的文本。

要以每次一行的方式检查文件，可对文件对象使用for循环**file_reader.py**

```python
filename = 'pi_digits.txt'

with open(filename) as file_object:
    for line in file_object:
        print(line)
```

```shell
[william@William-arch Desktop]$ python file_reader.py
3.1415926

897479203

937743020

[william@William-arch Desktop]$
```

当然我们可以在print语句中使用rstrip()去除空格

```python
filename = 'pi_digits.txt'

with open(filename) as file_object:
    for line in file_object:
        print(line.rstrip())
```

```shell
[william@William-arch Desktop]$ python test.py
3.1415926
897479203
937743020
[william@William-arch Desktop]$ 
```

#### 创建一个文件包含各行内容的列表

使用关键字with时，open()返回的文件对象只在with代码快内可用。如果要在with代码块外访问文件的内容，可在with代码块将文件的各行存储在一个列表中，并在with代码块外使用该列表：你可以立即处理文件的各个部分，也可推迟到程序后面再处理。

下面的是with代码块中将文件pi_digits.txt的各行存储在一个列表中，再在with代码块外打印它们：

```python
filename = 'pi_digits.txt'

with open(filename) as file_object:
    lines = file_object.readlines()
    
for line in lines:
    print(line.rstrip())
```

我自己写了一个方法也可以实现上述操作

```python
filename  = 'pi_digits.txt'
list1 = []

with open(filename) as file_object:
    for line in file_object:
        list1.append(line.rstrip())
    print(list1)
```

#### 使用文件的内容

在读取文件到内存后，就可以以任何方式使用这些数据了。下面以简单的方式使用圆周率的值。首先，我们将创建一个字符串，它包含文件中存储的所有数字，且没有任何空格。

```python
filename = 'pi_string.py'

with open(filename) as file_object:
    lines = file_object.readlines()
    
pi_string = ''
for line in lines:
    pi_strinng += line.rstrip()
    
print(pi_string)
print(len(pi_string))
```

#### 包含一百万的大型文件

前面我们分析的都是一个只有三行的文本文件，但这些代码也可处理大得多的文件。如果我们有一个文本文件，其中包含精确到小数点后1 000 000位而不是30位的圆周率值，也可创建一个包含所有这些数字的字符串。在这里我们只打印到小数点后50位，以免终端显示全部是1 000 000位而不断地翻滚：

**pi_string.py**

```python
filename = 'pi_million_digits.txt'

with open(filename) as file_object:
    lines = file_object.readlines()
    
pi_string = ''

for line in lines:
    pi_string += line.strip()
    
print(pi_string[:52] + "...")
print(len(pi_string))
```

对于你可处理的数据量，Python没有任何限制；只要系统的内存足够多，想处理多少数据都可以

#### 圆周率中包含你的生日吗

可将生日表示为一个由数字组成的字符串，再检查这个字符串是否包含在pi_string中:

```python
filename = 'pi_million_digits.txt'

with open(filename) as file_object:
    lines = file_object.readlines()
    
pi_string = ''
for line in lines:
    pi_string += line.rstrip()
    
birthday = input("Enter your birthday, in the form mmddyy: ")
if birthday in pi_string:
    print("Your birthday appears in the first million digits of pi!")
else:
    print("Your birthday does not appear in the first million digits of pi.")
```

#### 练习

10-1 Python 学习笔记：在文本编辑器中新建一个文件，写几句话来总结一下你至此学到的 Python 知识，其中每一行都以“In Python you can”打头。将这个文件命名为learning_python.txt，并将其存储到为完成本章练习而编写的程序所在的目录中。

编写一个程序，它读取这个文件，并将你所写的内容打印三次：

第一次打印时读取整个文件；

第二次打印时遍历文件对象；

第三次打印时将各行存储在一个列表中，再在 with 代码块外打印它们。

```python
filename='第10章\learning_python.txt'

with open(filename) as file_object:
    contents=file_object.read()
    print("第一次打印读取整个文件：")
    print(contents.strip())

with open(filename) as file_object:
    print("第二次打印遍历文件对象：")
    for line in file_object:
        print(line.strip())

with open(filename) as file_object:  
    lines=file_object.readlines()
print("第三次打印将各行存储在一个列表中，再在with代码外打印它们：")
for line in lines:
    print(line.strip())
```

**这里本来想只写一遍with open的，但是发现，运行正常，但是第二三次没有打印，思考了一下，觉得应该是文件关闭，后面无法进行文件读取，试用了一下open()和close()发现也是一样，所以写了三遍with open用来进行文件读取，希望以后能遇到更好的解决办法**

10-2 C语言学习笔记：可使用方法 replace()将字符串中的特定单词都替换为另一个单词。下面是一个简单的示例，演示了如何将句子中的'dog'替换为'cat'：

```python
>>> message = "I really like dogs." 
>>> message.replace('dog', 'cat') 
'I really like cats.' 
```

读取你刚创建的文件 learning_python.txt 中的每一行，将其中的 Python 都替换为另一门语言的名称，如 C。将修改后的各行都打印到屏幕上。

```python
filename='第10章\learning_python.txt'

with open(filename) as file_object:
    lines=file_object.readlines()

for line in lines:
    line=line.replace('Python','C')
    print(line.strip())
```



### 写入文件

保存数据的最简单的方式之一是将其写入到文件中。通过将输出写入文件，即便关闭包含程序输出的终端窗口，这些输出也依然存在：你可以在程序结束运行后查看这些输出，可与别人分享输出文件，还可编写程序来将这些输出读取到内存中并进行处理。

#### 写入空文件

要将文本写入文件，在调用open()时需要应该另一个实参，告诉Python你要写入打开的文件。

**wite_message.py**

```python
filename = 'programming.txt'

with open(filename,'w') as file_object:
    file_object.write('I lone programming!')
```

上述程序在调用open()时提供了两个实参。第一个实参是要打开的文件名称；第二个实参('w')告诉Python，要以写入模式打开这个文件。打开文件时，可指定读取模式('r')、写入模式('w')、附加模式('a')或让你能够读取和写入文件的模式('r+')。如果你省略了模式实参，Python将以默认的只读模式打开文件

如果要写入的文件不存在，函数open()将自动创建它。

以写入('w')模式打开文件时千万要小心，因为如果指定的文件已经存在，Python将在返回文件对象前清空该文件。

**Python只能将字符串写入文本文件。要将数值数据存储到文本文件中，必须系那是用函数str()将其转换为字符串格式。**

#### 写入多行

函数write()不会在你写入的文本末尾添加换行符，因此写入多行时没有指定换行符。文件看起来可能不是你希望的那样：

```python
filename = 'programming.txt'

with open(filename,'w') as file_object:
    file_object.write("I love programming.")
    file_object.write("I love creating new games.")
```

如果你打开programming.txt，将发现两行内容挤在一起：

```txt
 I love programming.I love creating new games.
```

要让每个字符串都独占一行，需要在write()语句中包含换行符：

```python
filename = 'programming.txt'

with open(filename,'w') as file_object:
    file_object.write("I love programming.\n")
    file_object.write("I love creating new games.\n")
```

```txt
I love programming.
I love creating new games.
```

#### 附加到文件

如果你要给文件添加内容，而不是覆盖原有的内容，可以附加模式打开文件。你以附加模式打开文件时，Python不会在返回文件对象前清空文件，而你写入到文件的行都将添加到文件末尾。如果指定的文件不存在，Python将为你创建一个空文件。

**write_message.py**

```python
filename = 'programming.txt'

with open(filename,'a') as file_object:
    file_object.write("I also love findding meaning in large datasets.\n")
    file_object.write("I love creating apps that can run in a browser.\n")
```

```txt
I love programming.
I love creating new games.
I also love findding meaning in large datasets.
I love creating apps that can run in a browser.
```

动手试一试10-3 访客：编写一个程序，提示用户输入其名字；用户作出响应后，将其名字写入到文件guest.txt中。

10-4 访客名单：编写一个while循环，提示用户输入其名字。用户输入其名字后，在屏幕上打印一句问候语，并将一条访问记录添加到文件guest_book.txt中。确保这个文件中的每条记录都独占一行。

10-5 关于编程的调查：编写一个while循环，询问用户为何喜欢编程。每当用户输入一个原因后，都将其添加到一个存储所有原因的文件中。

> 本文部分内容来自于《Python编程：从入门到实践》
>
> 参考Python菜鸟教程