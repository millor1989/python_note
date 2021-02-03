### 匿名函数

传入函数时，有时候不需要显式地定义函数，直接传入匿名函数更方便。

Python中，对匿名函数提供了有限的支持。

```python
>>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

其中，`lambda x: x * x`实际上等价于：

```python
def f(x):
    return x * x
```

关键字`lambda`表示匿名函数，冒号前的`x`表示函数参数。

Python的匿名函数**只能有一个表达式**。

匿名函数**不用写`return`**，返回值是该表达式的结果。

匿名函数没有名字，所以，不用担心函数名的冲突。

匿名函数也是一个函数对象，可以被赋值给一个变量，再利用变量来调用该函数：

```python
>>> f = lambda x: x * x
>>> f
<function <lambda> at 0x101c6ef28>
>>> f(5)
25
```

也可以将匿名函数作为返回值返回：

```python
def build(x, y):
    return lambda: x * x + y * y
```

