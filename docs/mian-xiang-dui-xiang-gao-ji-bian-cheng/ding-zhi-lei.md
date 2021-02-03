### 定制类

Python提供了很多的特殊用途的函数，可以用来定制类。

#### 1、`__str__`

定义了`Student`类，打印一个实例：

```python
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...
>>> print(Student('Michael'))
<__main__.Student object at 0x109afb190>
```

输出的`<__main__.Student object at 0x109afb190>`很不友好。可以定义`__str__()`方法（类似Java的`toString()`方法），返回一个友好的字符串：

```python
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...     def __str__(self):
...         return 'Student object (name: %s)' % self.name
...
>>> print(Student('Michael'))
Student object (name: Michael)
```

但是，此时直接输出变量，而不用`print`，输出还是不友好：

```python
>>> s = Student('Michael')
>>> s
<__main__.Student object at 0x109afb310>
```

因为，直接输出调用的不是`__str__()`方法，而是`__repr__()`方法，两者的区别是`__str__()`返回用户看到的字符串，而`__repr__()`返回程序开发者看到的字符串，也就是说，`__repr__()`是为调试服务的。

所以，再定义一个`__repr__()`。但是通常`__str__()`和`__repr__()`代码都是一样的，那么可以：

```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name=%s)' % self.name

    __repr__ = __str__
```

#### 2、`__iter__`

如果想让类可以像list或tuple那样用于`for...in`循环，可以实现一个`__iter__()`方法，该方法返回一个可迭代对象；Python的for循环会不断调用该迭代对象的`__next__()`方法拿到循环的下一个值，直到遇到`StopIteration`错误时退出循环。

以斐波那契数列为例，写一个Fib类：

```
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b

    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100000: # 退出循环的条件
            raise StopIteration()
        return self.a # 返回下一个值
```

将其用于`for`循环：

```python
>>> for n in Fib():
...     print(n)
...
1
1
2
3
5
...
46368
75025
```

#### 3、`__getitem__`

虽然`Fib()`可以用于`for`循环，但是，不能像list那样获取它的某一个索引对应的元素：

```python
>>> Fib()[5]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'Fib' object does not support indexing
```

可以实现`__getitem__()`方法：

```python
class Fib(object):
    def __getitem__(self, n):
        a, b = 1, 1
        for x in range(n):
            a, b = b, a + b
        return a
```

此时，就可以按索引访问`Fib()`的元素：

```python
>>> f = Fib()
>>> f[0]
1
>>> f[1]
1
>>> f[2]
2
>>> f[3]
3
>>> f[10]
89
>>> f[100]
573147844013817084101
```

但，如果对`Fib()`使用list切片，还是报错。因为，`__getitem__()`传入的参数可能是一个`int`，也可能是一个切片对象`slice`，需要加入判断：

```python
class Fib(object):
    def __getitem__(self, n):
        if isinstance(n, int): # n是索引
            a, b = 1, 1
            for x in range(n):
                a, b = b, a + b
            return a
        if isinstance(n, slice): # n是切片
            start = n.start
            stop = n.stop
            if start is None:
                start = 0
            a, b = 1, 1
            L = []
            for x in range(stop):
                if x >= start:
                    L.append(a)
                a, b = b, a + b
            return L
```

切片效果：

```python
>>> f = Fib()
>>> f[0:5]
[1, 1, 2, 3, 5]
>>> f[:10]
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```

这里没有对负数切片和切片步长（step）进行处理，要实现标准的`__getitem__()`还有很多工作要做。

此外，如果把对象看成是`dict`，`__getitem__()`的参数也可能是可以作为`key`的`object`，比如`str`。

与`__getitem__()`对应的还有`__setitem__()`和`__delitem__()`，前者将对象看作list或dict来对集合赋值，后者用于删除集合中的某个元素。

通过上面的方法，自定义的类可以表现得和Python自带的list、tuple、dict类似，这完全依赖于动态语言的“鸭子类型”，不需要强制继承某个接口。

#### 4、`__getattr__`

一般情况下，如果调用类不存在的方法或属性会报错：

```python
>>> class Student(object):
    
...    def __init__(self):
...        self.name = 'Michael'
...

>>> s = Student()
>>> print(s.name)
Michael
>>> print(s.score)
Traceback (most recent call last):
  ...
AttributeError: 'Student' object has no attribute 'score'
```

为了避免这个错误，除了加上`score`属性外，Python还提供了另一个种解决方案，通过`__getattr__()`动态返回一个属性：

```python
class Student(object):

    def __init__(self):
        self.name = 'Michael'

    def __getattr__(self, attr):
        if attr=='score':
            return 99
```

这样，在**调用不存在的属性时**，Python解释器会是同通过`__getattr__(self, 'attr')`尝试获得属性：

```python
>>> s = Student()
>>> s.name
'Michael'
>>> s.score
99
```

`__getattr__()`还可以返回函数：

```python
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
```

只是，要用调用方法的方式：

```python
>>> s.age()
25
```

注意，只有在调用不存在的属性时，才会调用`__getattr__`！！！

此外，任意的调用，比如`s.abc`都会返回`None`，是因为定义的`__getattr__`默认返回是`None`。可以让类只响应特定的某些属性，并按照约定抛出`AttributeError`错误：

```python
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
        raise AttributeError('\'Student\' object has no attribute \'%s\'' % attr)
```

实际上，可以将一个类的所有属性和方法调用全部动态化处理。

##### 动态调用的意义：

比如，很多网站做REST API开发，调用API的URL类似：

- http://api.server/user/friends
- http://api.server/user/timeline/list

如果要为每个URL对应的API逐一的编写SDK，工作量会非常大。

而使用动态地`__getattr__`，可以写出一个**链式调用**：

```python
class Chain(object):

    def __init__(self, path=''):
        self._path = path

    def __getattr__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __str__(self):
        return self._path

    __repr__ = __str__
```

尝试一下：

```python
>>> Chain().status.user.timeline.list
'/status/user/timeline/list'
```

这样，随着API的变化，SDK可以根据URL实现动态的调用，而且不随API的增加而改变。

某些REST API会把参数放到URL中，比如Github的API：

```python
GET /users/:user/repos
```

调用时，将`:user`替换为实际用户名。如果使用链式调用，会很简单：

```python
Chain().users('michael').repos
```

#### 5、`__call__`

一个实例可以有自己的属性和方法，调用实例的方法时，用`instance.method()`调用。如果要使用实例本身调用实例的方法，需要在类中定义一个`__call__()`方法：

```python
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)
```

调用方式：

```python
>>> s = Student('Michael')
>>> s() # self参数不要传入
My name is Michael.
```

`__call__()`方法可以有参数。对实例直接调用就如同调用函数一样，**可以将对象看成函数，也可以把函数看成对象**，Python中这两者没有根本的区别。

如果把对象看作函数，函数本身也可以在运行期间动态创建出来，类的实例也都是运行期创建出来的，这就模糊了对象和函数的界限。

通过`callabel()`函数，可以判断一个对象**是否是可以被调用**的：

```python
>>> callable(Student())
True
>>> callable(max)
True
>>> callable([1, 2, 3])
False
>>> callable(None)
False
>>> callable('str')
False
```

