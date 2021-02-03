### Python

编写人，荷兰的吉多·范·罗素姆（Guido Van Rossum）。

* 简洁
* 解释型语言、高级语言
* 库（内置库、第三方库）非常丰富
* 运行速度慢（相对而言）
* 代码不能加密

可用于网络应用（网站、后台服务）、日常小工具（系统管理员脚本任务等）、包装其它语言开发的程序以方便使用（胶水语言）。

#### 1、安装

记得要将python添加到环境变量`PATH`。

#### 2、解释器

Python文件为`.py`后缀，需要解释器来解释执行，Python有多种解释器，Python解释器也是开源的，甚至可以自己编写Python解释器。

##### 2.1、CPython

Python官方的C语言编写的解释器，命令行下运行`python`命令就是启动CPython解释器。

##### 2.2、IPython

基于CPython的一个交互式解释器，相比CPython交互性更强。

##### 2.3、PyPy

采用JIT技术，对Python代码进行动态编译（不是解释），可以显著提高Python代码的执行速度。PyPy和CPython有所不同，相同代码在两种解释器下运行可能会有不同结果。

##### 2.4、Jython

运行在Java平台上的Python解释器，可直接把Python代码编译为Java字节码执行。

##### 2.5、IronPython

类似于Jython，不同的是将Python代码编译为.Net的字节码，运行在.Net平台上。

#### 3、命令行模式和Python交互模式

* 命令行模式，操作系统的命令行模式。

  可以通过`python hello.py`执行Python文件。

* Python交互模式，是命令行模式下输入`python`命令后进入的交互模式，交互模式下输入`exit()`退出Python交互模式，返回命令行模式。

  可以直接运行Python指令，并输出指令结果；也可以通过`python hello.py`执行Python文件（文件必须在当前目录下，文件有输出指令，才能看到输出结果）。

  Python交互模式是为了Python代码调试，不是正式运行的Python代码环境。

Mac和Linux系统下，可以在命令行直接运行Python文件（只要文件有可执行权限）。

* 关于Python脚本第一行的 **\#!/usr/bin/python** 的解释：

  很多不熟悉 Linux 系统的同学需要普及这个知识，脚本语言的第一行，只对 Linux/Unix 用户适用，用来指定本脚本用什么解释器来执行。有这句的，加上执行权限后，可以直接用 **./** 执行，不然会出错，因为找不到 python 解释器。`#!/usr/bin/python` 是告诉操作系统执行这个脚本的时候，调用 `/usr/bin` 下的 python 解释器。`#!/usr/bin/env python` 这种用法是为了防止操作系统用户没有将 python 装在默认的`/usr/bin` 路径里。当系统看到这一行的时候，首先会到 env 设置里查找 python 的安装路径，再调用对应路径下的解释器程序完成操作。**\#!/usr/bin/python** 相当于写死了 python 路径。**\#!/usr/bin/env python** 会去环境设置寻找 python 目录，可以增强代码的可移植性，推荐这种写法。

（1）如果调用 python 脚本时，使用`python`命令（类似于`sh`命令执行shell文件）:

```
python script.py
```

`#!/usr/bin/python` 被忽略，等同于注释

（2）如果调用python脚本时，使用:

```
./script.py
```

`#!/usr/bin/python` 指定解释器的路径

#### 4、IDE

推荐使用Visual Studio Code（不是Visual Studio）。

Python文件后缀必须是`.py`，文件名必须是字母、数字、下划线的组合。

#### 5、输入和输出

* `print()`

  输出，可接收逗号分隔的多个输出内容

  如果输出内容包含表达式，会输出表达式的结果

* `input()`

  接收输入字符串，存放到变量中（字符串类型）。

  可以有输入提示信息：`input('Please enter your name:')`



