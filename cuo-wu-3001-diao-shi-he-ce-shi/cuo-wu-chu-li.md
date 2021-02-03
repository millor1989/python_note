### 错误处理

在程序运行的过程中，如果发生了错误，可以事先约定返回一个错误代码，这样，就可以知道是否有错，以及出错的原因。在操作系统提供的调用中，返回错误码非常常见。比如打开文件的函数`open()`，成功时返回文件描述符（就是一个整数），出错时返回`-1`。

用**错误码来表示是否出错十分不便**，因为函数本身应该返回的正常结果和错误码混在一起，造成调用者必须用大量的代码来判断是否出错：

```python
def foo():
    r = some_function()
    if r==(-1):
        return (-1)
    # do something
    return r

def bar():
    r = foo()
    if r==(-1):
        print('Error')
    else:
        pass
```

一旦出错，要一级级上报，直到某个函数可以处理该错误。

为了避免这种情况，Python提供了`try...except...finally...`的错误处理机制。

#### 1、`try`

```python
try:
    print('try...')
    r = 10 / 0
    print('result:', r)
except ZeroDivisionError as e:
    print('except:', e)
finally:
    print('finally...')
print('END')
```

当认为某些代码可能会出错时，用`try`来运行这段代码，如果执行出错，后续代码将不会执行，而是跳转到错误处理的`except`代码块，执行完错误处理，如果有`finally`代码块，则执行`finally`代码块，然后结束。

上面代码执行结果：

```ascii
try...
except: division by zero
finally...
END
```

即使没有错误发生，`finally`代码块还是会执行，比如上面代码将`10 / 0`改为`10 / 2`：

```ascii
try...
result: 5
finally...
END
```

另外，错误可能有多种，所以可以有多个`except`语句块，比如：

```python
try:
    print('try...')
    r = 10 / int('a')
    print('result:', r)
except ValueError as e:
    print('ValueError:', e)
except ZeroDivisionError as e:
    print('ZeroDivisionError:', e)
finally:
    print('finally...')
print('END')
```

`int()`函数可能会抛出`ValueError`。

Python的错误也是类，所有的错误都继承自`BaseException`。使用`except`时要注意，错误类型会涵盖它的子类，比如：

```python
try:
    foo()
except ValueError as e:
    print('ValueError')
except UnicodeError as e:
    print('UnicodeError')
```

因为`UnicodeError`是`ValueError`的子类，第二个`except`块永远没有机会执行。

`try...except`的另一个好处是，可以跨越多层次调用，比如函数`main()`调用`bar()`，`bar()`调用`foo()`，结果`foo()`出错了，这时，只要`main()`捕获到了，就可以处理：

```python
def foo(s):
    return 10 / int(s)

def bar(s):
    return foo(s) * 2

def main():
    try:
        bar('0')
    except Exception as e:
        print('Error:', e)
    finally:
        print('finally...')
```

所以，只要在适当的地方捕捉错误即可。

#### 2、调用栈

没有被捕获处理的错误，会一直网上抛出，最后被Python解释器捕获，打印一个错误信息，然后程序退出。比如：

```python
# err.py:
def foo(s):
    return 10 / int(s)

def bar(s):
    return foo(s) * 2

def main():
    bar('0')

main()
```

执行结果：

```shell
$ python3 err.py
Traceback (most recent call last):
  File "err.py", line 11, in <module>
    main()
  File "err.py", line 9, in main
    bar('0')
  File "err.py", line 6, in bar
    return foo(s) * 2
  File "err.py", line 3, in foo
    return 10 / int(s)
ZeroDivisionError: division by zero
```

可见，错误信息反映了整个错误的调用函数链，通过分析错误的调用栈信息，可以定位错误发生的位置。

#### 3、记录错误

让错误抛出到Python解释器，程序就退出了；还可以捕获错误，把错误堆栈打印出来，然后分析错误原因，这样，程序可以继续执行下去。

Python内置了`logging`模块可以非常容易地记录错误信息：

```python
# err_logging.py

import logging

def foo(s):
    return 10 / int(s)

def bar(s):
    return foo(s) * 2

def main():
    try:
        bar('0')
    except Exception as e:
        logging.exception(e)

main()
print('END')
```

此时，程序打印完错误，可以继续执行：

```python
$ python3 err_logging.py
ERROR:root:division by zero
Traceback (most recent call last):
  File "err_logging.py", line 13, in main
    bar('0')
  File "err_logging.py", line 9, in bar
    return foo(s) * 2
  File "err_logging.py", line 6, in foo
    return 10 / int(s)
ZeroDivisionError: division by zero
END
```

通过配置，`logging`还可以把错误记录到日志文件里，用于事后分析。

#### 4、抛出错误

错误是class，捕获一个错误就是捕获到该class的一个实例。错误并不是凭空产生的，而是有意创建并抛出的。Python的内置函数会抛出很多类型的错误，自定义函数也可以抛出错误。

比如，定义一个错误类，选好继承关系，使用`raise`语句抛出：

```python
# err_raise.py
class FooError(ValueError):
    pass

def foo(s):
    n = int(s)
    if n==0:
        raise FooError('invalid value: %s' % s)
    return 10 / n

foo('0')
```

执行时，可以追踪到自定义的错误：

```python
$ python3 err_raise.py 
Traceback (most recent call last):
  File "err_throw.py", line 11, in <module>
    foo('0')
  File "err_throw.py", line 8, in foo
    raise FooError('invalid value: %s' % s)
__main__.FooError: invalid value: 0
```

只有在必要的时候才自定义错误类型，一般优先考虑使用Python内置的错误类型。

另一种错误处理方式：

```python
# err_reraise.py

def foo(s):
    n = int(s)
    if n==0:
        raise ValueError('invalid value: %s' % s)
    return 10 / n

def bar():
    try:
        foo('0')
    except ValueError as e:
        print('ValueError!')
        raise

bar()
```

`bar()`函数中，捕获错误，仅仅输出记录一下，然后向上抛出；其中`raise`语句不带参数，表示将当前捕获错误直接抛出。

另外，在`except`中`raise`一个错误，可以将一种类型的错误转化为另一种类型：

```python
try:
    10 / 0
except ZeroDivisionError:
    raise ValueError('input error!')
```

