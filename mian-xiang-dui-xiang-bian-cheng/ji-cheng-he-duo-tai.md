### 继承和多态

面向对象的程序设计中，定义一个类的时候，可以继承某个现有的类，新的类称为子类（Subclass），而被继承的类称为基类、父类或超类（Base Class，Super Class）。

比如，类`Animal`：

```python
class Animal(object):
    def run(self):
        print('Animal is running...')
```

编写`Dog`和`Cat`类时，可以从`Animal`类继承：

```python
class Dog(Animal):
    pass

class Cat(Animal):
    pass
```

对于`Dog`来说，`Animal`就是它的父类，对于`Animal`来说，`Dog`就是它的子类。

继承的一个好处是，**子类获得了父类的全部功能**。由于`Animial`实现了`run()`方法，因此，`Dog`和`Cat`作为它的子类，也拥有了`run()`方法：

```python
>>> dog = Dog()
>>> dog.run()
Animal is running...

>>> cat = Cat()
>>> cat.run()
Animal is running...
```

子类可以增加自己特有的方法：

```python
class Dog(Animal):

    def run(self):
        print('Dog is running...')

    def eat(self):
        print('Eating meat...')
```

子类还可以重写父类的方法：

```python
class Dog(Animal):

    def run(self):
        print('Dog is running...')

class Cat(Animal):

    def run(self):
        print('Cat is running...')
```

此时，再运行：

```python
>>> dog = Dog()
>>> dog.run()
Dog is running...

>>> cat = Cat()
>>> cat.run()
Cat is running...
```

即，当子类和父类都存在相同的`run()`方法时，我们说，子类的`run()`覆盖了父类的`run()`，在代码运行的时候，总是会调用子类的`run()`。这就是继承的另一个好处：**多态**。

定义类的时候，实际上就定义了数据类型。比如：

```python
>>> a = list()
>>> b = Animal()
>>> c = Dog()

>>> isinstance(a, list)
True
>>> isinstance(b, Animal)
True
>>> isinstance(c, Dog)
True
```

但是，当运行如下代码：

```python
>>> isinstance(c, Animal)
True
```

`Dog`也是`Animal`类型的，因为`Dog`是从`Animal`继承下来的。在继承关系中，如果一个实例的数据类型是某个类的子类，那它的数据类型也可以被看做是父类。但是，反过来是不行的：

```python
>>> b = Animal()
>>> isinstance(b, Dog)
False
```

为了进一步理解多态的好处，再看一个函数：

```python
def run_twice(animal):
    animal.run()
    animal.run()
```

传入`Animal`实例时：

```python
>>> run_twice(Animal())
Animal is running...
Animal is running...
```

而传入`Dog`实例时：

```python
>>> run_twice(Dog())
Dog is running...
Dog is running...
```

如果再定义一个`Tortoise`类型，也继承`Animal`：

```python
class Tortoise(Animal):
    def run(self):
        print('Tortoise is running slowly...')
```

传入`Tortoise`实例到`run_twice`函数：

```python
>>> run_twice(Tortoise())
Tortoise is running slowly...
Tortoise is running slowly...
```

只要是`Animal`类或者其子类，就会自动调用实际类型的`run()`方法。对于一个变量，只需要知道它是`Animal`类型，无需确切地知道它的子类型，就可以放心地调用`run()`方法，而具体调用的`run()`方法是作用在`Animal`、`Dog`、`Cat`还是`Tortoise`对象上，由运行时该对象的确切类型决定，这就是多态好处。

调用方只管调用，不管细节，而当新增一种`Animal`的子类时，只要确保`run()`方法编写正确，不用管原来的代码是如何调用的。这就是著名的“开闭”原则：

- 对扩展开放：允许新增`Animal`子类；
- 对修改封闭：不需要修改依赖`Animal`类型的`run_twice()`等函数。

可以多重继承，任何类，最终都可以追溯到根类`object`，比如：

```ascii
                ┌───────────────┐
                │    object     │
                └───────────────┘
                        │
           ┌────────────┴────────────┐
           │                         │
           ▼                         ▼
    ┌─────────────┐           ┌─────────────┐
    │   Animal    │           │    Plant    │
    └─────────────┘           └─────────────┘
           │                         │
     ┌─────┴──────┐            ┌─────┴──────┐
     │            │            │            │
     ▼            ▼            ▼            ▼
┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐
│   Dog   │  │   Cat   │  │  Tree   │  │ Flower  │
└─────────┘  └─────────┘  └─────────┘  └─────────┘
```

#### 1、静态语言 VS 动态语言

对于静态语言（例如Java）来说，如果需要传入`Animal`类型，则传入的对象必须是`Animal`类型或者它的子类，否则，将无法调用`run()`方法。

对于Python这样的动态语言来说，则不一定非要传入`Animal`类型。只要保证传入的对象有一个`run()`方法就可以了：

```python
class Timer(object):
    def run(self):
        print('Start...')
```

这就是**动态语言的“鸭子类型”**，它并**不要求严格的继承体系**，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。

Python的“file-like object“就是一种鸭子类型。对真正的文件对象，它有一个`read()`方法，返回其内容。但是，许多对象，只要有`read()`方法，都被视为“file-like object“。许多函数接收的参数就是“file-like object“，不一定要传入真正的文件对象，完全可以传入任何实现了`read()`方法的对象。