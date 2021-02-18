### contextlib

Python中读写文件资源时要特别注意，使用完后要正确地关闭它们。关闭文件资源通常使用`try...finally...`：

```python
try:
    f = open('/path/to/file', 'r')
    f.read()
finally:
    if f:
        f.close()
```

使用`with`语句，可以简化`try...finally...`的写法：

```python
with open('/path/to/file', 'r') as f:
    f.read()
```

除了`open()`函数返回的对象，任何正确实现了上下文管理的对象都可以使用`with`语句。

上下文管理是通过`__enter__`和`__exit__`这两个方法实现的。比如：

```python
class Query(object):

    def __init__(self, name):
        self.name = name

    def __enter__(self):
        print('Begin')
        return self
    
    def __exit__(self, exc_type, exc_value, traceback):
        if exc_type:
            print('Error')
        else:
            print('End')
    
    def query(self):
        print('Query info about %s...' % self.name)
```

这样就可以对`Query`对象使用`with`语句：

```python
with Query('Bob') as q:
    q.query()
```

#### `@contextmanager`

编写`__enter__`和`__exit__`仍然很繁琐，因此Python的标准库`contextlib`提供了`@contextmanager`装饰器，上面的代码可以改写如下：

```python
from contextlib import contextmanager

class Query(object):

    def __init__(self, name):
        self.name = name

    def query(self):
        print('Query info about %s...' % self.name)

@contextmanager
def create_query(name):
    print('Begin')
    q = Query(name)
    yield q
    print('End')
```

`@contextmanager`这个decorator接受一个generator，用`yield`语句把`with ... as var`把变量输出出去，然后，`with`语句就可以正常地工作了：

```python
with create_query('Bob') as q:
    q.query()
```

如果希望在某段代码执行前后自动执行特定代码，也可以用`@contextmanager`实现。比如：

```python
@contextmanager
def tag(name):
    print("<%s>" % name)
    yield
    print("</%s>" % name)

with tag("h1"):
    print("hello")
    print("world")
```

代码执行结果：

```python
<h1>
hello
world
</h1>
```

代码执行顺序是：

1. `with`语句首先执行`yield`之前的语句，因此打印出`<h1>`；
2. `yield`调用会执行`with`语句内部的所有语句，因此打印出`hello`和`world`；
3. 最后执行`yield`之后的语句，打印出`</h1>`。

因此，`@contextmanager`让我们通过编写generator来简化上下文管理。

#### `@closing`

如果一个对象没有实现上下文，则不能将它用于`with`语句。但是可以使用`closing()`来将该对象变为上下文对象。比如，用`with`语句使用`urlopen()`：

```python
from contextlib import closing
from urllib.request import urlopen

with closing(urlopen('https://www.python.org')) as page:
    for line in page:
        print(line)
```

`closing`也是一个经过`@contextmanager`装饰的generator，这个generator编写起来其实非常简单：

```python
@contextmanager
def closing(thing):
    try:
        yield thing
    finally:
        thing.close()
```

它的作用就是把任意对象变为上下文对象，并支持`with`语句。

`contextlib`还有一些其他decorator，用于编写更简洁的代码。