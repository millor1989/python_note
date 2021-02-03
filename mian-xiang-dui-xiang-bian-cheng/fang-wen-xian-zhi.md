### 访问限制

类内部有属性和方法，外部代码通过调用实例变量的方法可以操作数据，方法的逻辑对外部是隐藏的。从上节的`Student`类来看，外部代码可以随意的修改`name`和`score`属性：

```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.score
59
>>> bart.score = 99
>>> bart.score
99
```

如果要对外部隐藏属性，可以在属性名前加`__`。在Python中，实例的变量名如果以`__`开头，就变成了一个**私有变量（private）**，只有内部可以访问，外部不能访问：

```python
class Student(object):

    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))
```

这样，外部代码便不能访问实例的属性：

```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.__name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute '__name'
```

通过访问限制的保护，外部代码不能随意修改对象内部的状态，代码更加健壮。可以通过提供相应的方法，来限制外部代码对属性的访问：

```python
class Student(object):
    ...

    def get_name(self):
        return self.__name

    def get_score(self):
        return self.__score

    def set_score(self, score):
        self.__score = score
```

更进一步，如果要让外部代码有限地修改`score`属性，可以：

```python
class Student(object):
    ...

    def set_score(self, score):
        if 0 <= score <= 100:
            self.__score = score
        else:
            raise ValueError('bad score')
```

这样，外部代码只能设置满足`if`条件的`__score`。

要注意，Python中，类似`__xxx__`的变量名，是特殊变量，是可以直接访问的，不能用`__name__`、`__score__`这样的属性名。类似`_xxx`的变量名，外部是可以访问的，也是不能用作属性名的。这两种都只是约定，而不是限制。

其实，`__xxx`的属性，还是可以从外部访问的，不能直接访问`__name`是因为Python解释器对外把`__name`变量改成了`_Student__name`，所以，可以通过`_Student__name`来访问`__name`变量：

```python
>>> bart._Student__name
'Bart Simpson'
```

但是强烈建议不要这么做，不同版本的Python解释器可能会把`__name`改成不同的变量名。

总的来说就是，Python本身没有任何硬性的约束机制，一切全靠自觉遵守约定。

还要注意一种**错误的用法**：

```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.get_name()
'Bart Simpson'
>>> bart.__name = 'New Name' # 设置__name变量！
>>> bart.__name
'New Name'
```

表面上看，外部代码“成功”地设置了`__name`变量，但实际上这个`__name`变量和class内部的`__name`变量***不是***一个变量！内部的`__name`变量已经被Python解释器自动改成了`_Student__name`，而外部代码给`bart`新增了一个`__name`变量。所以：

```python
>>> bart.get_name() # get_name()内部返回self.__name
'Bart Simpson'
```