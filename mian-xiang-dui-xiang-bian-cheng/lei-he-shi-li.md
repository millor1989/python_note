### 类和实例

类（Class）是抽象的模板，实例（Instance）是根据类创建出来的一个个具体的“对象”，每个对象都拥有相同的方法，但各自的数据可能不同，各个实例拥有的数据都互相独立，互不影响。

Python中使用`class`关键字定义类：

```python
class Student(object):
    pass
```

`class`关键字后是类名，类名通常是大写开头的单词，随后是`(object)`，表示该类是从**`object`类**继承下来的。通常，如果没有合适的继承类，就使用`object`类，这是所有类最终都会继承的类。

定义好了`Student`类，就可以根据`Student`类创建出`Student`的实例，创建实例是通过类名加`()`实现的：

```python
>>> bart = Student()
>>> bart
<__main__.Student object at 0x10a67a590>
>>> Student
<class '__main__.Student'>
```

变量`bart`指向的就是一个`Student`的实例，后面的`0x10a67a590`是内存地址，每个object的内存地址都不一样，而`Student`本身则是一个类。

和静态语言不同，Python允许对实例变量绑定任何数据，对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的属性可以不同，**可以自由地给一个实例变量绑定属性**：

```python
>>> bart.name = 'Bart Simpson'
>>> bart.name
'Bart Simpson'
```

类可以起到模板的作用，因此，可以在创建实例的时候，把一些必须绑定的属性强制填写进去。

比如，通过定义一个特殊的**`__init__`方法**，在创建实例的时候，就把`name`，`score`等属性绑上去：

```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score
```

`__init__`方法的第一个参数永远是**`self`**，表示创建的实例本身，因此，在`__init__`方法内部，就可以把各种属性绑定到`self`，因为**`self`就指向创建的实例本身**。有了`__init__`方法，在创建实例的时候，就不能传入空的参数了，必须传入与`__init__`方法匹配的参数，但`self`不需要传，Python解释器自己会把实例变量传进去：

```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.name
'Bart Simpson'
>>> bart.score
59
```

和普通的函数相比，在类中定义的函数只有一点不同，**第一个参数永远是实例变量`self`**，并且调用时不用传递该参数。此外，类的方法和普通函数没有什么区别，所以，类的方法仍然可以用默认参数、可变参数、关键字参数和命名关键字参数。

#### 1、数据封装

可以通过定义函数来访问类的属性：

```python
>>> def print_score(std):
...     print('%s: %s' % (std.name, std.score))
...
>>> print_score(bart)
Bart Simpson: 59
```

`Student`实例本身就拥有这些属性，要访问这些属性，是没有必要从外面的函数去访问，可以直接在`Student`类的内部定义访问数据的函数，这样，就把“数据”给**封装**起来了。这些封装数据的函数是和`Student`类本身是关联起来的，称为**类的方法**：

```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))
```

定义一个方法，除了第一个参数是`self`外，其它和普通函数一样。要调用一个方法，只用实例变量上直接调用，除了`self`不用传递，其他参数正常传入：

```python
>>> bart.print_score()
Bart Simpson: 59
```

从外部看`Student`类，只需要知道，创建实例需要给出`name`和`score`，而如何打印，都是在`Student`类的内部定义的，这些数据和逻辑被“封装”起来了，调用很容易，而不用知道内部实现的细节。

另外，可以给类增加新的方法，新的方法可以在类的实例变量上直接调用。

方法就是与实例绑定的函数，和普通函数不同，方法可以直接访问实例的数据。