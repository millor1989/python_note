### 装饰器

由于函数也是一个对象，而函数对象也可以被赋给变量，所以，通过变量也能调用函数。

```python
>>> def now():
...     print('2015-3-25')
...
>>> f = now
>>> f()
2015-3-25
```

函数对象有一个`__name__`属性，是函数的名字：

```python
>>> now.__name__
'now'
>>> f.__name__
'now'
```

假如，要增强`now()`函数的功能，比如，在函数调用前后自动打印日志，但是同时不希望改变`now()`函数的定义，就需要用到装饰器。

在代码运行期间，动态地增加功能的方式，称为**装饰器（Decorator）**。

本质上，装饰器是一个返回函数的高阶函数。如果要定义一个可以打印日志的装饰器，可以定义如下：

```python
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```

`log()`函数，接收一个函数作为参数，并返回一个函数，是一个装饰器。通过Python的`@`语法，将装饰器置于函数的定义处：

```python
@log
def now():
    print('2015-3-25')
```

此时，调用`now()`函数，不仅会运行`now()`函数本身，还会在运行`now()`函数前打印日志：

```python
>>> now()
call now():
2015-3-25
```

执行`now()`函数，等价于执行了`log(now)`。

将`@log`放到`now()`函数的定义处，相当于执行了：

```python
now = log(now)
```

由于`log()`是一个装饰器，返回一个函数，所以，原来的`now()`函数仍然存在，只是现在同名的`now`变量指向了新的函数，于是调用`now()`将执行新的函数，即执行在`log()`函数中返回的`wrapper()`函数。

`wrapper()`函数的参数定义是`(*args, **kw)`，因此，`wrapper()`函数可以**接受任意参数的调用**。在`wrapper()`函数内，首先打印日志，再紧接着调用原始函数。

如果装饰器需要传入参数，那么需要编写一个返回装饰器的高阶函数，函数代码会更复杂。比如，需要自定义日志文本的日志装饰器：

```python
def log(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```

其用法如下：

```python
@log('execute')
def now():
    print('2015-3-25')
```

执行结果如下：

```python
>>> now()
execute now():
2015-3-25
```

与两层嵌套的装饰器相比，三层嵌套的效果是：

```python
>>> now = log('execute')(now)
```

首先执行`log('execute')`，返回的是`decorator`函数，再调用返回的函数，参数是`now`函数，返回值最终是`wrapper`函数。

函数也是对象，它有`__name__`等属性，对于经过装饰器修饰的`now`函数，它的`__name__`属性已经从`now`变为了`wrapper`：

```python
>>> now.__name__
'wrapper'
```

因为返回的`wrapper`函数的名字本身就是`wrapper`，所以，需要将原始函数的`__name__`等属性复制到`wrapper()`函数中，否则有些依赖函数签名的代码执行就会出错。

**Python内部提供了`functools.wraps`**，不需要编写`wrapper.__name__=func.__name__`这样的代码。完整的装饰器代码如下：

```python
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```

对于带参数的装饰器：

```python
import functools

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```

**例 1**，在函数执行前后打印日志的装饰器：

```python
>>> import functools

>>> def logwrap(fn):
...    @functools.wraps(fn)
...    def wrapper(*args, **kw):
...        print('before func')
...        inner = fn(*args, **kw)
...        print('after func')
...        return inner # 此处注意，不能是`inner()`，不能带括号
...    return wrapper

@logwrap
>>> def f(x, y):
...    print('executing',x ,'+', y)
...    return x+y

>>> f(1,2)
before func
executing 1 + 2
after func
3
```

**例 2**、一个同时支持带参数和不带参数的装饰器的定义：

```python
import functools
def log(test):
    if isinstance(test,str):
        def decorator(fn):
            @functools.wraps(fn)
            def wrapper(*args,**kw):
                print('log of %s:%s'%(fn.__name__,test))
                return fn(*args,**kw)     
            return wrapper
        return decorator
    else:
        @functools.wraps(test)
        def wrapper(*args,**kw):
            print('log of %s:%s'%(test.__name__,'defult'))
            return test(*args,**kw)
        return wrapper
```

其中，当装饰器是没有参数的装饰器时，`log`函数的参数是要装饰的函数本身；而当装饰器是有参数的装饰器时，`log`函数的参数是`str`。

装饰器的使用：

```python
@log
def f(x, y):
    pass

@log('arguments')
def f1(x, y):
    pass
```

执行结果：

```python
>>> f(1,2)
log of f : default

>>> f1(1,2)
log of f1 : arguments
```

