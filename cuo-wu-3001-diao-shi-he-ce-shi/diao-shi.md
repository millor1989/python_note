### 调试

最简单的调试方式是使用`print`，将变量输出查看：

```python
def foo(s):
    n = int(s)
    print('>>> n = %d' % n)
    return 10 / n

def main():
    foo('0')

main()
```

执行后结果：

```shell
$ python err.py
>>> n = 0
Traceback (most recent call last):
  ...
ZeroDivisionError: integer division or modulo by zero
```

但是，正常上线的代码通常要删除`print`语句。

#### 1、断言

凡是可以用`print`来辅助调试的地方，都可以用**断言（assert）**来代替：

```python
def foo(s):
    n = int(s)
    assert n != 0, 'n is zero!'
    return 10 / n

def main():
    foo('0')
```

`assert n != 0, 'n is zero!'`表示，表达式`n != 0`应该是`True`，断言失败，`assert`语句本身就会抛出`AssertionError`：

```shell
$ python err.py
Traceback (most recent call last):
  ...
AssertionError: n is zero!
```

正常上线的程序也不该有`assert`，这种方式并不比`print`好多少。

另外，启动Python解释器时可以用`-O`参数来关闭`assert`：

```shell
$ python -O err.py
Traceback (most recent call last):
  ...
ZeroDivisionError: division by zero
```

关闭`assert`后，`assert`语句可以当作`pass`看待。

#### 2、`logging`

还可以将`print()`替换为`logging`，和`assert`比，`logging`不会抛出错误，而且可以输出到文件：

```python
import logging

logging.basicConfig(level=logging.INFO)

s = '0'
n = int(s)
logging.info('n = %d' % n)
print(10 / n)
```

`logging`允许指定记录信息的级别——`debug`，`info`，`warning`，`error`（对应有相应的方法，比如，`logging.debug`， `logging.info`，输出范围逐渐收窄），比如，指定`level=WARNING`后，`debug`和`info`就不起作用了。

另外，`logging`通过简单配置，可以输出到不同地方，比如console和文件。

#### 3、pdb

Python调试器pdb，可以让程序以单步方式运行，可以随时查看运行状态。

```python
# err.py
s = '0'
n = int(s)
print(10 / n)
```

用pdb启动：

```shell
$ python -m pdb err.py
> /Users/michael/Github/learn-python3/samples/debug/err.py(2)<module>()
-> s = '0'
```

以参数`-m pdb`启动后，pdb定位到下一步要执行的代码`-> s = '0'`。输入命令`l`来查看代码：

```shell
(Pdb) l
  1     # err.py
  2  -> s = '0'
  3     n = int(s)
  4     print(10 / n)
```

输入命令`n`可以单步执行代码：

```shell
(Pdb) n
> /Users/michael/Github/learn-python3/samples/debug/err.py(3)<module>()
-> n = int(s)
(Pdb) n
> /Users/michael/Github/learn-python3/samples/debug/err.py(4)<module>()
-> print(10 / n)
```

任何时候都可以输入命令`p 变量名`来查看变量：

```shell
(Pdb) p s
'0'
(Pdb) p n
0
```

输入命令`q`结束调试，退出程序：

```shell
(Pdb) q
```

一步步执行，虽然详细，但是这种方式太麻烦，pdb还提供了**`set_trace()`方法**，不需要一步步地逐一执行，只需`import pdb`，在可能出错的地放插入代码`pdb.set_trace()`设置断点：

```python
# err.py
import pdb

s = '0'
n = int(s)
pdb.set_trace() # 运行到这里会自动暂停
print(10 / n)
```

运行代码，程序会在`pdb.set_trace()`暂停并进入pdb调试环境，可以用命令`p`查看变量，或者用命令`c`继续运行：

```shell
$ python err.py 
> /Users/michael/Github/learn-python3/samples/debug/err.py(7)<module>()
-> print(10 / n)
(Pdb) p n
0
(Pdb) c
Traceback (most recent call last):
  File "err.py", line 7, in <module>
    print(10 / n)
ZeroDivisionError: division by zero
```

这种方式比直接pdb模式高效很多。

#### 4、IDE

最好的Python调试方式是用IDE的调试功能，很多IDE都支持设置断点、单步执行，比如Visual Studio Code安装Python插件、Eclipse安装pydev插件……

最后，IDE调试虽然方便易用，但是，经验表明，debug和调试**用好`logging`才是最重要的**。