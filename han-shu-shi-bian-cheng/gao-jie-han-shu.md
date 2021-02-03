### 高阶函数（High-order function）

#### 1、概念

##### 1.1、变量可以指向函数

以Python内置函数`abs()`为例，函数调用：

```python
>>> abs(-10)
10
```

而，

```python
>>> abs
<built-in function abs>
```

可见`abs`是函数本身。

函数调用结果可以赋值给变量，函数本身也可以赋值给变量（即，变量可以指向函数）：

```python
>>> x = abs(-10)
>>> x
10

>>> f = abs
>>> f
<built-in function abs>

>>> f(-10)
10
```

##### 1.2、函数名也是变量名

函数名就是指向函数的变量。

可以将函数名看成变量，它指向的是一个执行计算的函数。

既然函数名是变量，它就可以指向其它对象，比如：

```python
>>> abs = 10
>>> abs(-10)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not callable
```

`abs`指向整数后，就不能进行绝对值计算了。

实际代码是不能这样写的，应该避免变量名与函数名的混乱，如果要恢复`abs`的绝对值计算函数功能，需要重启Python交互环境。

注：由于`abs`函数实际上定义在`builtins`模块中，如果要让修改`abs`变量的指向在其它模块也生效，要用`import builtins;builtins.abs=10`。

##### 1.3、传入函数

变量可以指向函数，函数的参数能接收变量，所以也能接受函数作为参数，这种函数就称为高阶函数。比如：

```python
def add(x, y, f):
    return f(x) + f(y)
```

执行`add(-5,6,abs)`时，等价于执行`abs(-5) + abs(6)`。

#### 2、map/reduce

map/reduce的概念来源于Google的论文“[MapReduce: Simplified Data Processing on Large Clusters](http://research.google.com/archive/mapreduce.html)”。

Python内建了`map()`函数和`reduce()`函数。

##### 2.1、map

`map()`函数接收两个参数，一个是函数，一个是`Iterable`，对`Iterable`的每个元素执行传入的函数运算，将结果作为新的`Iterator`返回。

比如，函数$$f(x)=x^2$$，作用于list`[1, 2, 3, 4, 5, 6, 7, 8, 9]`，可以用`map()`函数实现如下：

```ascii
            f(x) = x * x

                  │
                  │
  ┌───┬───┬───┬───┼───┬───┬───┬───┐
  │   │   │   │   │   │   │   │   │
  ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼

[ 1   2   3   4   5   6   7   8   9 ]

  │   │   │   │   │   │   │   │   │
  │   │   │   │   │   │   │   │   │
  ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼

[ 1   4   9  16  25  36  49  64  81 ]
```

Python代码实现为：

```python
>>> def f(x):
...     return x * x
...
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

其中用`list()`函数计算出了结果迭代器`r`中的每个值，并返回一个list。

`map()`函数作为高阶函数，将运算规则抽象化了，从而可以对`Iterable`对象执行任意复杂的函数运算。

##### 2.2、reduce

`reduce(function, sequence[, initial])`函数，累积式地将**两个参数的函数**应用于序列的每一项。

比如，`reduce`实现序列的求和：

```python
>>> from functools import reduce
>>> def add(x, y):
...     return x + y
...
>>> reduce(add, [1, 3, 5, 7, 9])
25
```

再比如，将序列`[1, 3, 5, 7, 9]`变换成整数`13579`：

```python
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> reduce(fn, [1, 3, 5, 7, 9])
13579
```

结合`map`函数，可以实现将`str`转换为`int`的函数：

```python
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> def char2num(s):
...     digits = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
...     return digits[s]
...
>>> reduce(fn, map(char2num, '13579'))
13579
```

整合为一个`str2int`的函数，就是：

```python
from functools import reduce

DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}

def str2int(s):
    def fn(x, y):
        return x * 10 + y
    def char2num(s):
        return DIGITS[s]
    return reduce(fn, map(char2num, s))
```

使用lambda函数进行进一步简化：

```python
from functools import reduce

DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}

def char2num(s):
    return DIGITS[s]

def str2int(s):
    return reduce(lambda x, y: x * 10 + y, map(char2num, s))
```

#### 3、filter

Python内建的`filter()`函数用于过滤序列，接收一个函数和一个序列，把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。

需要注意的是，`filter()`函数**返回的是一个`Iteraotr`**，是惰性的！

比如，过滤掉list中的偶数，保留奇数：

```python
def is_odd(n):
    return n % 2 == 1

list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
# 结果: [1, 5, 9, 15]
```

##### 3.1、例，用`filter`函数求所有的素数

先构造一个从`3`开始的奇数序列：

```python
def _odd_iter():
    n = 1
    while True:
        n = n + 2
        yield n
```

这是一个生成器。

定义筛选函数：

```python
def _not_divisible(n):
    return lambda x: x % n > 0
```

该函数返回的是`lambda`表达式。

最后，定义返回下一个素数的生成器：

```python
def primes():
    yield 2
    it = _odd_iter() # 初始序列
    while True:
        n = next(it) # 返回序列的第一个数
        yield n
        it = filter(_not_divisible(n), it) # 构造新序列
```

打印1000以内的所有素数：

```python
# 打印1000以内的素数:
for n in primes():
    if n < 1000:
        print(n)
    else:
        break
```

#### 4、sorted

Python内置了`sorted()`函数，可以对list进行排序：

```python
>>> sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]
```

同时，`sorted()`也是一个高阶函数，可以接收一个**`key`函数**实现自定义的排序。比如按绝对值大小排序：

```python
>>> sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
```

`key`指定的函数作用于list的每个元素，`sorted()`函数根据`key`函数的返回结果进行排序。

对于字符串，`sorted()`函数默认按照ASCII的大小排序。

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'])
['Credit', 'Zoo', 'about', 'bob']
```

如果要忽略大小写进行排序，可以将`str.lower`函数作为`key`函数：

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
['about', 'bob', 'Credit', 'Zoo']
```

如果要进行**反向排序**，设置参数`reverse=True`：

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```