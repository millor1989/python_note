### 高级特性

简化代码，提高开发效率。

#### 1、切片（Slice）

Python提供了切片（slice）操作符，用以简便的获取一个list或吐了的部分元素。

获取list的前3个元素：

```python
>>> L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
>>> L[0:3]
['Michael', 'Sarah', 'Tracy']
```

`L[0:3]`表示，从索引`0`开始取，直到索引`3`为止，但不包括索引`3`。其中第一个索引为`0`时可以省略：

```python
>>> L[:3]
['Michael', 'Sarah', 'Tracy']

>>> L[1:3]
['Sarah', 'Tracy']
```

Python支持倒序切片，倒数第一个元素的索引是`-1`：

```python
>>> L[-2:]
['Bob', 'Jack']
>>> L[-2:-1]
['Bob']
```

切片还可以按**步长**获取元素，比如前10个数，每两个取一个：

```python
>>> L = list(range(100))

>>> L[:10:2]
[0, 2, 4, 6, 8]
```

所有数据，每5个取一个：

```python
>>> L[::5]
[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]
```

还可以用来**复制list**：

```python
>>> L[:]
[0, 1, 2, 3, ..., 99]
```

默认的步长是`1`，步长也可以为负数——表示从后往前的步长：

```python
>>> ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack'][-1:-4:-2]
['Jack', 'Tracy']
```

将list或字符串**反转**：

```python
>>> ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack'][::-1]
['Jack', 'Bob', 'Tracy', 'Sarah', 'Michael']

>>> '12345'[::-1]
'54321'
```

tuple也是一种list，区别是tuple是不可变的。tuple也可以使用切片操作，结果仍然是tuple：

```python
>>> (0, 1, 2, 3, 4, 5)[:3]
(0, 1, 2)
```

字符串也可以看作list，元素是每个字符，它也可以用切片操作，结果仍然是字符串：

```python
>>> 'ABCDEFG'[:3]
'ABC'
>>> 'ABCDEFG'[::2]
'ACEG'
```

Python的切片操作非常简单，不需要截取函数（比如，substring之类）。

#### 2、迭代

遍历list或者tuple，也称迭代（iteration）。

Python `for...in...`不仅可以迭代list或tuple，还可以用于迭代其它的可迭代对象。

迭代dict：

```python
>>> d = {'a': 1, 'b': 2, 'c': 3}
>>> for key in d:
...     print(key)
...
a
c
b
```

由于dict是无序的，迭代结果顺序可能会不同。

默认迭代dict时，迭代的是key。如果要迭代value，可以用`for value in d.values()`，如果要同时迭代key和value，可以用`for k,v in d.items()`。

字符串也是可迭代对象：

```python
>>> for ch in 'ABC':
...     print(ch)
...
A
B
C
```

通过Python `collections`模块的Iterable类型，可以**判断一个对象是否是可迭代**的：

```python
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```

如果要通过索引来迭代一个list，可以使用Python内置的`enumerate`函数将list转换为索引-元素对：

```python
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print(i, value)
...
0 A
1 B
2 C
```

Python `for`循环中，同时引用两个变量是很常见的，比如：

```python
>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
...     print(x, y)
...
1 1
2 4
3 9
```

#### 3、列表生成式（List Comprehensions）

列表生成式，是Python内置的非常简单却强大的创建list的生成式。

比如，生成list`[1,2,3,4,5,6,7,8,9,10]`可以用`list(range(1,11))`生成。

而要生成list`[1×1,2×2,3×3,4×4,5×5,6×6,7×7,8×8,9×9,10×10]`，按照之前掌握的知识，要通过迭代：

```python
>>> L = []
>>> for x in range(1, 11):
...    L.append(x * x)
...
>>> L
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

使用列表生成式，可以将其大大简化：

```python
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

只用将要生成的元素放在循环的开始。

for循环的后面还可以**加上`if`判断**，比如，筛选偶数的平方：

```python
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```

还可以**使用两层循环**，比如，生成全排列：

```python
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```

三层及三层以上的循环很少用到。

**使用多个变量**，比如：

```python
>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> [k + '=' + v for k, v in d.items()]
['y=B', 'x=A', 'z=C']
```

3.1、if...else与for循环

用`if ... else ...`结合`for`循环时，`if ... else ...`要放在`for`循环的前面：

```python
>>> [x if x % 2 == 0 else -x for x in range(1, 11)]
[-1, 2, -3, 4, -5, 6, -7, 8, -9, 10]
```

与`if`语句放在`for`语句后的生成式不同的是，`if`在后表示的是过滤条件（不能带`else`），而`if...else...`在`for`语句前，则是根据`for`语句返回结果，进一步通过`if ... else ...`进行运算。

可以理解为`for`语句有返回值，`if...else...`语句也有返回值。其中的`x if x % 2 == 0 else -x`是**`if  else`语句有返回值（单行）的形式**，带`:`的`if else`多行形式（即使每个分支都加了`return`）是没有返回值的。

#### 4、生成器（Generator）

通过列表生成式可以直接创建列表。但是，受内存限制，列表容量也是有限制的。

此外，创建一个100万个元素的列表，不仅占用空间大，如果仅访问前面的几个元素，后面的绝大多数元素占用的空间都是白白浪费的。

Python提供了一边进行循环，一边进行计算的生成器（generator）。

```python
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
```

可见，列表生成式代码和生成器代码区别仅仅是最外层的`[]`和`()`。

就结果而言，列表生成式结果是list，生成器结果是`generator`，它**对应的是算法，而不是结果列表**，要获取生成器的生成的每个元素，可以通过`next()`函数获得生成器的下一个返回值：

```python
>>> next(g)
0
>>> next(g)
1
>>> next(g)
4
>>> next(g)
9
>>> next(g)
16
>>> next(g)
25
>>> next(g)
36
>>> next(g)
49
>>> next(g)
64
>>> next(g)
81
>>> next(g)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

每次调用`next(g)`，则计算`g`的下一个元素的值，直到计算到最后一个元素，没有下一个元素时，调用`next(g)`会抛出`StopIteration`错误。

生成器是**可迭代对象**，所以，迭代它的正确方式应该是循环，而不是`next`函数：

```python
>>> g = (x * x for x in range(10))
>>> for n in g:
...     print(n)
... 
0
1
4
9
16
25
36
49
64
81
```

##### 4.1、yield关键字

一个打印斐波那契数列（Fibonacci）的函数：

```python
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        print(b)
        a, b = b, a + b
        n = n + 1
    return 'done'
```

执行效果：

```python
>>> fib(6)
1
1
2
3
5
8
'done'
```

将上面函数的`print(b)`替换为`yield b`：

```python
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
```

函数就变身为一个生成器：

```python
>>> f = fib(6)
>>> f
<generator object fib at 0x104feaaa0>
```

函数执行遇到`yield`关键字时，返回`yield`语句的值（相当于`return`），而再次执行函数时，是接着上次`yield`语句返回的地方继续执行。

```python
>>> for n in fib(6):
...     print(n)
...
1
1
2
3
5
8
```



