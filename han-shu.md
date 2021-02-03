### 函数

函数一种基本的代码抽象方式。

#### 1、调用函数

Python内置了很多函数，可以[查看Python官方文档](https://docs.python.org/3/library/functions.html)，这些内置函数可以直接调用。通过`help(func)`可以查看`func`函数的帮助信息。

比如`abs()`、`max()`都是Python的内置函数，可以直接调用：

```python
>>> abs(100)
100
>>> abs(-20)
20
>>> abs(12.34)
12.34

>>> max(1, 2)
2
>>> max(2, 3, 1, -5)
3
```

Python内置的常用函数还包括数据类型转换函数：

```python
>>> int('123')
123
>>> int(12.34)
12
>>> float('12.34')
12.34
>>> str(1.23)
'1.23'
>>> str(100)
'100'
>>> bool(1)
True
>>> bool('')
False
```

函数名其实是指向一个函数对象的引用，可以将函数名赋值给一个变量（相当于给函数起别名）：

```python
>>> a = abs
>>> a(-1)
1
```

#### 2、定义函数

Python中，用`def`定义函数，依次出现函数名，参数（用括号包括）和`:`，在缩进块中编写函数体，用`return`返回返回值。

```python
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
```

函数执行到`return`时，函数执行完毕，并返回结果。如果没有`return`语句，函数执行完毕后也会又返回结果，只是结果为`None`。`return None`可以简写为`return`。

如果在文件中定义了函数，可以使用`from ... import ...`来导入函数。比如，从`abstest.py`文件中导入`my_abs`函数：

```python
from abstest import my_abs
```

##### 2.1、空函数

用`pass`语句定义什么也不做的空函数：

```python
def nop():
    pass
```

`pass`可以作为占位符，暂时替代还没有逻辑实现的函数体。

`pass`还可以用在其它语句中，以防止代码的语法错误：

```python
if age >= 18:
    pass
```

##### 2.2、参数检查

调用函数时，如果参数个数不对，Python解释器会抛出`TypeError`。

而，参数类型不对时，Python解释器是无法检测到的。需要函数内部进行参数类型检查。

可以使用`isinstance()`进行数据类型检查：

```python
def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
```

##### 2.3、返回多个值

函数可以有多个返回值：

```python
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny
```

事实上，函数返回多个值时，返回的是tuple类型值，本质上还是一个返回值。

语法上，返回tuple类型值可以省略括号，多个变量可以同时接收一个tuple，按位置赋给对应的值。

```python
>>> x, y = move(100, 100, 60, math.pi / 6)
>>> print(x, y)
151.96152422706632 70.0

>>> r = move(100, 100, 60, math.pi / 6)
>>> print(r)
(151.96152422706632, 70.0)
```

#### 3、函数的参数

Python函数定义非常简单、并且很灵活。可以定义必须参数、还可以使用默认参数、可变参数和关键字参数。

##### 3.1、位置参数

计算$$x^2$$：

```python
def power(x):
    return x * x
```

对于`power(x)`函数，参数`x`就是一个位置参数。

当我们调用`power`函数时，必须传入有且仅有的一个参数`x`：

```python
>>> power(5)
25
>>> power(15)
225
```

计算$$x^n$$：

```python
def power(x, n):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
```

`x`和`n`都是位置参数，传入两个参数值要按照位置依次传入：

```python
>>> power(5, 2)
25
>>> power(5, 3)
125
```

##### 3.2、默认参数

把第二个参数`n`的默认值设定为2：

```python
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
```

调用`power(5)`时，相当于调用`power(5, 2)`：

```python
>>> power(5)
25
>>> power(5, 2)
25
```

默认参数可以简化调用，但需要注意：

- **必选参数必须在前**，默认参数在后。

- 当函数有多个参数时，把变化大的参数放前面，变化小的参数放后面。变化小的参数就可以作为默认参数。

- 默认参数必须指向不可变对象。

  ```python
  def add_end(L=[]):
      L.append('END')
      return L
  ```

  正常调用时：

  ```python
  >>> add_end([1, 2, 3])
  [1, 2, 3, 'END']
  >>> add_end(['x', 'y', 'z'])
  ['x', 'y', 'z', 'END']
  ```

  使用默认调用时，第一次调用：

  ```python
  >>> add_end()
  ['END']
  ```

  后续默认调用：

  ```python
  >>> add_end()
  ['END', 'END']
  >>> add_end()
  ['END', 'END', 'END']
  ```

  **原因是**：Python函数在定义的时候，默认参数`L`的值就被计算出来了，即`[]`，因为默认参数`L`也是一个变量，它指向对象`[]`，每次调用该函数，如果改变了`L`的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的`[]`了。

  可以用`None`这个不变对象来改造这个函数：

  ```python
  def add_end(L=None):
      if L is None:
          L = []
      L.append('END')
      return L
  ```

有多个默认参数时，调用的时候，既可以按顺序提供默认参数，也可以不按顺序提供部分默认参数。

当不按顺序提供部分默认参数时，需要把参数名写上。

```python
def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)


enroll('Bob', 'M', 7)

enroll('Adam', 'M', city='Tianjin')
```

##### 3.3、可变参数

可变参数的意思是，传入的参数个数是可变的（可以是0~n个）。

在函数内部，可变参数是tuple类型的。

定义可变参数，在参数前加`*`号。

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

将list或tuple的元素作为可变参数传递给函数时，同样只需在list或tuple变量名前加`*`。`*`加在list或tuple前相当于将list或tuple转换为一个个的元素。

```python
>>> args=[1,2,3,4]
>>> print([5,6,7,*args])
[5, 6, 7, 1, 2, 3, 4]

>>> nums = [1, 2, 3]
>>> calc(*nums)
14
```

##### 3.4、关键字参数

关键字参数允许传入0个或多个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。

```python
>>>def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)


>>> person('Michael', 30)
name: Michael age: 30 other: {}

>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

关键字参数，可以扩展函数的功能。

与可变参数类似，将dict元素（key-vaue）作为关键字参数传给函数时，只用将dict变量名前加`**`。`**`作dict前缀的作用：

```python
>>> dicts={'a':1,'b':2,'c':3,'d':4}
>>> print({'e':7, **dicts})

{'e': 7, 'a': 1, 'b': 2, 'c': 3, 'd': 4}
```

##### 3.5、命名关键字参数

对于关键字参数，函数调用者可以传入任意不受限制的关键字参数。

如果要限制关键字参数的名字，需要用命名关键字参数。

```python
def person(name, age, *, city, job):
    print(name, age, city, job)
```

该函数，只接收`city`和`job`作为关键字参数。

和关键字参数`**kw`不同，命名关键字参数需要一个特殊**分隔符**`*`，`*`后面的参数被视为命名关键字参数。

如果函数定义中已经有了一个**可变参数**，后面跟着的***命名关键字参数***（非关键字参数，因为可变参数后面可以跟`**`修饰的关键字参数）就**不再需要特殊分隔符**`*`了。

```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```

命名关键字参数**必须传入参数名**，这和位置参数不同。

明明关键字参数也可以有缺省值。

```python
def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)
```

##### 3.6、参数组合

在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。但是请注意，**参数定义的顺序必须是**：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。

tuple和dict是神奇的存在，对于函数：

```python
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
```

可以有如下调用结果：

```python
>>> args = (1, 2, 3, 4)
>>> kw = {'d': 99, 'x': '#'}
>>> f1(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
>>> args = (1, 2, 3)
>>> kw = {'d': 88, 'x': '#'}
>>> f2(*args, **kw)
a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}
```

函数定义时，尽量不要使用太多的参数组合，会影响函数的可读性。

#### 4、递归函数

函数内部可以调用其它函数，如果函数内部调用自身，则称为递归调用，这个函数就是递归函数。

比如，阶乘$$n! = 1 \times2 \times3 \times ... \times n$$，用函数`fact(n)`表示时：

$$fact \left ( n \right )= n!=1 \times2 \times3 \times ... \times \left ( n-1 \right) \times n= \left (n-1 \right )! \times n=fact \left (n-1 \right ) \times n $$

所以，`fact(n)`的递归函数表示为：

```python
def fact(n):
    if n==1:
        return 1
    return n * fact(n - 1)
```

调用结果：

```python
>>> fact(1)
1
>>> fact(5)
120
```

递归函数的优点是定义简单，逻辑清晰。但是，使用递归函数需要注意防止递归调用的次数过多，而导致栈溢出。

解决递归调用栈溢出的方法是通过**尾递归**优化，事实上尾递归和循环的效果是一样的，所以，把循环看成是一种特殊的尾递归函数也是可以的。

**尾递归**是指，在函数返回的时候，调用自身本身，并且，return语句不能包含表达式（`return n * fact(n - 1)`包含了乘法表达式，不是尾递归）。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。

将上面的阶乘函数改为尾递归，需要多一些代码，主要是要把每一步的乘积传入到递归函数中：

```python
def fact(n):
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)
```

尾递归调用时，如果解释器做了优化，栈不会增长，因此，无论多少次调用也不会导致栈溢出。

**遗憾的是**，大多数编程语言没有针对尾递归做优化，Python解释器也没有做优化，即使改成尾递归方式，也会导致栈溢出。

