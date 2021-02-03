### 使用`@property`

动态绑定属性时，如果直接将属性曝露，写起来虽然简单，但是，无法检查属性，属性可能被随意更改：

```python
s = Student()
s.score = 9999
```

为了限制`score`范围，可以通过`set_score()`方法来设置，并通过`get_score()`来获取，这样，在`set_score()`方法中可以检查属性：

```python
class Student(object):

    def get_score(self):
         return self._score

    def set_score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

这样就不能随意修改`score`了：

```python
>>> s = Student()
>>> s.set_score(60) # ok!
>>> s.get_score()
60
>>> s.set_score(9999)
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

但是，这种方法又稍显复杂，不如直接使用属性简单。

装饰器可以给函数动态增加功能，Python内置了`@property`装饰器可以把方法调用转换为属性调用：

```python
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

将getter方法变为属性，只需要加上`@property`就可以。把`score`属性的setter方法变为属性赋值则要使用`@score.setter`装饰器。

```python
>>> s = Student()
>>> s.score = 60 # OK，实际转化为s.score(60)
>>> s.score # OK，实际转化为s.score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

可见属性调用，被转换为了对应的属性同名方法的调用。

本例中`_score`对外部还是可见的，将其改为`__score`也是可以的，这样对外部就是不可见的。

如果制定一getter方法，不定义setter方法，那么方法名转换的属性就是**只读属性**：

```python
class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return 2015 - self._birth
```

其中`birth`属性是可以读写的，而`age`属性是只读的。