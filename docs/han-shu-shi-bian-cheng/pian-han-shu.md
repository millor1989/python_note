### 偏函数(Partial function)

Python的`functools`模块提供了偏函数的功能。这里的偏函数和数学意义上的偏函数不一样。

使用默认参数，可以降低函数的调用难道，而偏函数也可以做到这点。比如`int()`函数，仅传入一个字符串参数时，它可以将字符串转换为十进制整数：

```python
>>> int('12345')
12345
```

但是，`int()`函数还提供了额外的`base`参数，默认值是`10`。如果传入`base`参数，则可以根据`base`指定进制进行转换：

```python
>>> int('12345', base=8)
5349
>>> int('12345', 16)
74565
```

如果想要进行二进制转换，但是又不想每次都传入`base`参数——`int(x, base=2)`，那么一种做法是定义一个新的函数：

```python
def int2(x, base=2):
    return int(x, base)
```

如果**使用`functools.partial`创建偏函数**，那么就不要自定义`int2()`了：

```python
>>> import functools
>>> int2 = functools.partial(int, base=2)
>>> int2('1000000')
64
>>> int2('1010101')
85
```

`functools.partial`的作用就是，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单。

创建偏函数时，实际上可以接收函数对象、`*args`和`**kw`这3个参数。

`int2 = functools.partial(int, base=2)`固定了`int()`函数的关键字参数`base`，也相当于：

```python
kw = { 'base': 2 }
int('10010', **kw)
```

而，`max2 = functools.partial(max, 10)`，实际上会把`10`作为`*args`的一部分自动加到左边，此时`max2(5,6,7)`相当于：

```python
args = (10, 5, 6, 7)
max(*args)
```

执行结果为`10`。