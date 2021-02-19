### 模块（Module）

在计算机程序的开发过程中，随着程序代码越写越多，在一个文件里代码就会越来越长，越来越不容易维护。

为了使代码更具可维护性，把很多函数分组，分别放到不同的文件里，这样，每个文件包含的代码就相对较少，很多编程语言都采用这种组织代码的方式。

Python中一个`.py`文件称为一个**模块**。

使用模块最大的好处是，大大**提高了代码的可维护性**。

其次，编写代码**不必从零开始**。当一个模块编写完毕，就可以被其他地方引用。

使用模块还可以**避免函数名和变量名冲突**。相同名字的函数和变量完全可以分别存在不同的模块中，因此，在编写模块时，不必考虑名字会与其他模块冲突。但是也要注意，尽量不要与内置函数名字冲突。

此外，为了**避免模块名冲突**，Python又引入了按目录来组织模块的方法，称为**包（Package）**。

假设的`abc`和`xyz`两个模块名字与其它模块冲突了，通过包来组织模块，避免冲突。方法是选择一个顶层包名，比如`mycompany`，按照如下目录存放：

```ascii
mycompany
├─ __init__.py
├─ abc.py
└─ xyz.py
```

引入包后，只要顶层的报名不存在冲突，模块都不会与其它模块冲突。此时，`abc.py`的模块名称就变为了`mycompany.abc`。

每一个包目录下都有一个**必需的文件`__init__.py`**，只有存在这个文件时，Python才将这个目录当作包对待（而不是普通目录）。`__init__.py`可以是空文件，也可以有Python代码，`__init__.py`本身就是一个模块，而它的模块名就是`mycompany`。

包目录可以有多级。比如：

```ascii
mycompany
 ├─ web
 │  ├─ __init__.py
 │  ├─ utils.py
 │  └─ www.py
 ├─ __init__.py
 ├─ abc.py
 └─ utils.py
```

文件`www.py`的模块名就是`mycompany.web.www`，两个文件`utils.py`的模块名分别是`mycompany.utils`和`mycompany.web.utils`。

自己创建模块时要注意命名，**不能和Python自带的模块名称冲突**。例如，系统自带了sys模块，自己的模块就不可命名为sys.py，否则将无法导入系统自带的sys模块。

要注意：

- 模块名要遵循Python变量命名规范，不要使用中文、特殊字符
- 模块名不要和系统模块名冲突，最好先查看系统是否已存在该模块，检查方法是在Python交互环境执行`import abc`，若成功则说明系统存在此模块。

#### 1、使用模块

Python本身就内置了很多非常有用的模块，只要安装完毕，这些模块就可以立刻使用。以`sys`模块为例，编写一个`hello`模块：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

' a test module '

__author__ = 'Michael Liao'

import sys

def test():
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':
    test()
```

第1行和第2行是标准注释，第1行注释可以让这个`hello.py`文件直接在Unix/Linux/Mac上运行，第2行注释表示`.py`文件本身使用标准UTF-8编码；

第4行是一个字符串，表示模块的文档注释，任何模块代码的第一个字符串都被视为模块的文档注释；

第6行使用`__author__`变量把作者写进去，这样公开源代码后别人就可以看到模块的作者；

以上就是Python模块的标准文件模板，当然也可以全部删掉不写。

之后，是代码部分……

要使用`sys`模块，首先要通过`import sys`导入该模块。导入`sys`模块后，才有变量`sys`指向该模块，利用`sys`这个变量，就可以访问`sys`模块的所有功能。

`sys`模块有一个`argv`变量，它用list存储了命令行的所有参数；`argv`至少有一个元素，因为第一个元素是该`.py`文件的名称，比如：运行`python3 hello.py`获得的`sys.argv`就是`['hello.py']`；运行`python3 hello.py Michael`获得的`sys.argv`就是`['hello.py', 'Michael']`。

`hello`模块最后的两行代码：

```python
if __name__=='__main__':
    test()
```

在命令行运行`hello`模块时，Python解释器把**特殊变量`__name__`**置为`__main__`，而在其它地方导入该`hello`模块时，`if`判断将失败。这种`if`判断，可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。

命令行运行`hello.py`的效果：

```shell
$ python3 hello.py
Hello, world!
$ python hello.py Michael
Hello, Michael!
```

而启动Python环境后，再导入`hello`模块：

```shell
$ python3
Python 3.4.3 (v3.4.3:9b73f1c3e601, Feb 23 2015, 02:52:03) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import hello
>>>
```

导入时，不会执行`test()`函数，所以没有打印`Hello, world`。调用`test()`函数时，才会执行：

```python
>>> hello.test()
Hello, world!
```

##### 1.1、作用域

在一个模块中，可以定义很多变量和函数，Python使用`_`前缀来标示函数和变量的用途。Python没有限制变量和函数的访问，这种”标示“只是一种编程约定。

`__xxx__`这样的变量是**特殊变量**，可以被直接引用，但是有特殊用途，比如，`__author__`，`__name__`就是特殊变量。

类似`_xxx`和`__xxx`这样的函数或变量就是**非公开**的（private），不应该被直接引用。

而不带`_`前缀的函数和变量是**公开的**（public），可以直接引用的。

通过一个例子，看`_`标识的用途：

```python
def _private_1(name):
    return 'Hello, %s' % name

def _private_2(name):
    return 'Hi, %s' % name

def greeting(name):
    if len(name) > 3:
        return _private_1(name)
    else:
        return _private_2(name)
```

这个模块里，公开了`greeting()`函数，而把内部逻辑置为非公开的，意为调用函数`greeting()`时，不用关心内部的非公开函数实现，**是一种非常有用的代码封装和抽象的方法**。即：外部不需要引用的函数全部定义为非公开的，只有外部需要引用的函数才定义为公开的。

#### 2、安装第三方模块

Python中，使用包管理工具pip安装第三方模块。

一般来说，第三方库都会在Python官方的[pypi.org](https://pypi.org)注册，可以在官网搜索要安装的Python第三方模块名称。比如，安装Python图像处理库Pillow，命令为：

```shell
$ pip install Pillow
```

pip默认镜像源在国外，国内使用时会比较慢，可以通过配置让pip使用国内的镜像源。以清华镜像站为例，**临时使用镜像源**：

```shell
$ pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

**设置默认镜像源**（>=10.0.0）：

```shell
$ pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

还可以通过修改`pip.conf`（linux或mac系统，对应路径`$HOME/.config/pip/pip.conf`，如果是windows则是`pip.ini`文件，对应路径`%HOME%\pip\pip.ini`）文件来设置默认镜像源，如果没有就创建这个文件，修改内容为

```ascii
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

如果是非`https`的镜像源，临时使用时需要加上`--trusted-host xxxx.com`，设置默认时则要加上如下内容：

```ascii
[install]
trusted-host=xxxxxx.com
```

或者执行命令：

```shell
$ pip config set install.trusted-host xxxxxx.com
```

设置完成后，可以使用`pip config list`命令查看配置：

```shell
$ pip config list
global.index-url='http://mirrors.aliyun.com/pypi/simple/'
install.trusted-host='mirrors.aliyun.com'
```

此外，查看pip的版本和安装位置：

```shell
$ pip -V
pip 19.0.3 from c:\program files\python\lib\site-packages\pip (python 3.6)
```

升级已安装的模块：

```shell
$ pip install --upgrade scrapy
```

安装指定版本的模块：

```shell
$ pip install scipy==0.15.1
```

##### 2.2、安装常用模块

Python使用过程中，会用到很多的第三方模块，用pip逐一安装很费时费力，还需要考虑兼容性。推荐使用[Anaconda](https://www.anaconda.com)，它是一个基于Python的数据处理和科学计算平台，内置了丰富的第三方库。

##### 2.3、模块路径搜索

在试图加载一个模块时，Python会在指定的路径下搜索对应的.py文件，如果找不到，就会报错：

```python
>>> import mymodule
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named mymodule
```

默认情况下，Python解释器会搜索当前目录、所有已安装的内置模块和第三方模块，搜索路径存放在`sys`模块的`path`变量中：

```python
>>> import sys
>>> sys.path
['', '/Library/Frameworks/Python.framework/Versions/3.6/lib/python36.zip', '/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6', ..., '/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages']
```

如果要添加搜索目录，可以：

- 直接修改`sys.path`，添加目录：

  ```python
  >>> import sys
  >>> sys.path.append('/Users/michael/my_py_scripts')
  ```

  这种方法是**运行时修改**，运行结束后失效。

- 设置环境变量`PYTHONPATH`，该环境变量内容会被自动添加到模块搜索路径中。设置方法与设置`PATH`环境变量类似。此时，只需要添加对应的要添加的搜索目录，Python本身的搜索路径不受影响。