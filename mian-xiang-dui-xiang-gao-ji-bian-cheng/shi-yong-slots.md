### 使用`__slots__`

一般情况下，定义了一个类后，创建类的实例，可以给该实例绑定任何属性和方法，这是Python作为动态语言提供的灵活性。

```python
class Student(object):
    pass
```

给实例绑定属性：

```python
>>> s = Student()
>>> s.name = 'Michael' # 动态给实例绑定一个属性
>>> print(s.name)
Michael
```

**给实例绑定方法**：

```python
>>> def set_age(self, age): # 定义一个函数作为实例方法
...     self.age = age
...
>>> from types import MethodType
>>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
>>> s.set_age(25) # 调用实例方法
>>> s.age # 测试结果
25
```

与绑定到实例的属性一样，绑定到实例的方法对另一个实例也是不起作用的：

```python
>>> s2 = Student() # 创建新的实例
>>> s2.set_age(25) # 尝试调用方法
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'set_age'
```

也可以**给类动态地绑定方法**：

```python
>>> def set_score(self, score):
...     self.score = score
...
>>> Student.set_score = set_score
```

绑定到类的方法，类的所有实例都可以调用：

```python
>>> s.set_score(100)
>>> s.score
100
>>> s2.set_score(99)
>>> s2.score
99
```

#### 1、使用`__slots__`

为了限制实例的属性，比如`Student`实例只能添加`name`和`age`属性，Python允许在定义类的时候定义一个特殊变量`__slots__`：

```python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称

>>> s = Student() # 创建新的实例
>>> s.name = 'Michael' # 绑定属性'name'
>>> s.age = 25 # 绑定属性'age'
>>> s.score = 99 # 绑定属性'score'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```

`__slots__`变量中不包含的属性，在试图绑定到实例时会抛出`AttributeError`错误。

要注意，`__slots__`定义的属性仅对当前类的实例有限制作用，对类的子类是不起作用的：

```python
>>> class GraduateStudent(Student):
...     pass
...
>>> g = GraduateStudent()
>>> g.score = 9999
```

当然，也可以在子类定义中定义`__slots__`。