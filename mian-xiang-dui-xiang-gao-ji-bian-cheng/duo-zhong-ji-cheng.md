### 多重继承

继承是面向对象编程的一个重要特性，通过继承子类可以扩展父类功能。

Python支持多重继承，继承多个父类的特性，并进行扩展。比如，动物可以按照哺乳类和鸟类分类：

```python
class Animal(object):
    pass

# 大类:
class Mammal(Animal):
    pass

class Bird(Animal):
    pass

# 各种动物:
class Dog(Mammal):
    pass

class Bat(Mammal):
    pass

class Parrot(Bird):
    pass

class Ostrich(Bird):
    pass
```

还可以按照会跑、会飞分类：

```python
class Runnable(object):
    def run(self):
        print('Running...')

class Flyable(object):
    def fly(self):
        print('Flying...')
```

对于狗，既是哺乳类又会跑，可以定义为：

```python
class Dog(Mammal, Runnable):
    pass
```

而蝙蝠，既是哺乳类有会飞，可以定义为：

```python
class Bat(Mammal, Flyable):
    pass
```

通过多重继承，子类可以同时获得多个父类的特性。

#### 1、MixIn

设计继承关系时，主线通常都是单一继承下来的，比如`Ostrich`继承自`Bird`。可以通过多重继承，额外加入其它的特性，比如，`Ostrich`可以加入`Runnable`特性，这种设计通常称为**MixIn**。

为了更清晰的继承关系，可以将`Runnable`和`Flyable`改为`RunnableMixIn`和`FlyableMixIn`。还可以定义出肉食动物`CarnivorousMixIn`和植食动物`HerbivoresMixIn`，让某个动物同时拥有好几个MixIn：

```python
class Dog(Mammal, RunnableMixIn, CarnivorousMixIn):
    pass
```

MixIn的目的是为一个类增加多个特性。

设计类的时候，通常优先考虑通过多重继承组合多个MixIn的特性，而不是增加继承的层次。

