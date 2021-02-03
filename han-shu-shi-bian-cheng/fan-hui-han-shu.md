### 返回函数

#### 1、函数作为返回值

高阶函数除了可以接收函数作为参数，还可以把函数作为结果返回。

对于可变参数的求和函数，可以定义如下：

```python
def calc_sum(*args):
    ax = 0
    for n in args:
        ax = ax + n
    return ax
```

但是，如果不需要立刻求和，可以不返回求和的结果，而是返回求和的函数：

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum
```

此时，调用`lazy_sum()`函数，返回的不是求和结果，而是函数：

```python
>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
```

而调用函数`f()`时，才真正计算求和的结果：

```python
>>> f()
25
```

#### 2、闭包（Closure）

上一节的`lazy_sum()`函数中，定义了`sum()`函数，内部函数`sum()`可以引用外部函数`lazy_sum()`的参数和局部变量，`lazy_sum()`函数返回`sum`函数时，相关的参数和变量都保存在了返回的函数中，这种程序结构称为**闭包**。

每一次调用`lazy_sum()`函数时，**返回的都是一个新的函数**，即使参数相同：

```python
>>> f1 = lazy_sum(1, 3, 5, 7, 9)
>>> f2 = lazy_sum(1, 3, 5, 7, 9)
>>> f1==f2
False
```

其中`f1()`和`f2()`的调用结果互不影响。

需要注意，`lazy_sum()`函数在其内部定义中引用了局部变量`args`，所以，当一个函数返回一个函数后，其内部的局部变量还是被新函数引用，所以，闭包用起来简单，实现起来并不容易。

另外要注意的是，返回的函数不会立即执行，调用了`f()`才执行。

再看一个例子：

```python
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()
```

在每一次循环中，都创建了一个新的函数，然后，将创建的三个函数返回。

乍一看，调用`f1()`，`f2()`和`f3()`结果似乎应该是`1`，`4`，`9`，但实际上：

```python
>>> f1()
9
>>> f2()
9
>>> f3()
9
```

原因在于，函数定义中引用了变量`i`，而它并非立即执行的。当3个函数都返回时，它们所应用的变量`i`已经变成了`3`，所以，最终结果都是`9`。

所以，要牢记：**返回函数不要引用任何循环变量，或者后续会发生变化的变量。**

如果一定要引用循环变量，那么需要在创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：

```python
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs
```

此时，结果为：

```python
>>> f1, f2, f3 = count()
>>> f1()
1
>>> f2()
4
>>> f3()
9
```

代码比较冗长，可以通过lambda函数简化代码。

例子，利用闭包，创建一个计数器函数：

```python
def createCounter():
    def generator():
        n = 1
        while True:
            yield n
            n = n + 1

    g = generator()

    def counter():
        return next(g)
    return counter
```

