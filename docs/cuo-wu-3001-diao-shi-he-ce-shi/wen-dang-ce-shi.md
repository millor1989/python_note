### 文档测试

Python的官方文档有很多示例代码，比如`re`模块：

```python
>>> import re
>>> m = re.search('(?<=abc)def', 'abcdef')
>>> m.group(0)
'def'
```

可以把这些示例代码在Python的交互式环境下输入并执行，结果与文档中的示例代码显示的一致。

将示例代码与其它说明写在注释中，然后可以由一些工具自动生成文档，还可以自动执行写在注释中的这些代码。

比如：

```python
def abs(n):
    '''
    Function to get absolute value of number.
    
    Example:
    
    >>> abs(1)
    1
    >>> abs(-1)
    1
    >>> abs(0)
    0
    '''
    return n if n >= 0 else (-n)
```

Python内置的文档测试`doctest`模块可以直接提取注释中的代码并执行测试。

`doctest`严格按照Python交互式命令行的输入和输出来判断测试结果是否正确。测试异常时，会有`...`省略一些中间的输出。

比如，用`doctest`测试上节编写的`Dict`类：

```python
# mydict2.py
class Dict(dict):
    '''
    Simple dict but also support access as x.y style.

    >>> d1 = Dict()
    >>> d1['x'] = 100
    >>> d1.x
    100
    >>> d1.y = 200
    >>> d1['y']
    200
    >>> d2 = Dict(a=1, b=2, c='3')
    >>> d2.c
    '3'
    >>> d2['empty']
    Traceback (most recent call last):
        ...
    KeyError: 'empty'
    >>> d2.empty
    Traceback (most recent call last):
        ...
    AttributeError: 'Dict' object has no attribute 'empty'
    '''
    def __init__(self, **kw):
        super(Dict, self).__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value

if __name__=='__main__':
    import doctest
    doctest.testmod()
```

运行：

```shell
$ python mydict2.py
```

没有输出，说明文档测试是正常的。

如果程序有问题，比如将`__getattr__()`方法注释掉：

```shell
$ python mydict2.py
**********************************************************************
File "/Users/michael/Github/learn-python3/samples/debug/mydict2.py", line 10, in __main__.Dict
Failed example:
    d1.x
Exception raised:
    Traceback (most recent call last):
      ...
    AttributeError: 'Dict' object has no attribute 'x'
**********************************************************************
File "/Users/michael/Github/learn-python3/samples/debug/mydict2.py", line 16, in __main__.Dict
Failed example:
    d2.c
Exception raised:
    Traceback (most recent call last):
      ...
    AttributeError: 'Dict' object has no attribute 'c'
**********************************************************************
1 items had failures:
   2 of   9 in __main__.Dict
***Test Failed*** 2 failures.
```

此时，运行就报错了！

注意，`Dict`类的最后三行代码：

```python
if __name__=='__main__':
    import doctest
    doctest.testmod()
```

当模块正常导入时，`doctest`不会被执行。只有在命令行直接运行时，才执行`doctest`。所以，不必担心`doctest`会在非测试环境下执行。

