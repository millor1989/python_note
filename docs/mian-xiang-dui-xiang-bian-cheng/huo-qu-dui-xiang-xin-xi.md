### 获取对象信息

#### 1、`type()`

`type()`函数返回传入对象对应的类型。

使用`type()`函数可以判断对象类型，基本类型都可以用`type()`判断：

```python
>>> type(123)
<class 'int'>
>>> type('str')
<class 'str'>
>>> type(None)
<type(None) 'NoneType'>
```

函数或者类，也可以用`type()`函数判断：

```python
>>> type(abs)
<class 'builtin_function_or_method'>
>>> type(a)
<class '__main__.Animal'>
```

可以在`if`语句中使用`type()`函数来判断对象的类型：

```python
>>> type(123)==type(456)
True
>>> type(123)==int
True
>>> type('abc')==type('123')
True
>>> type('abc')==str
True
>>> type('abc')==type(123)
False
```

对于基本类型可以直接写`int`、`str`等，要判断一个对象是否是函数，可以使用`types`模块定义的常量：

```python
>>> import types
>>> def fn():
...     pass
...
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True
>>> type((x for x in range(10)))==types.GeneratorType
True
```

#### 2、`isinstance()`

`type()`函数不能判断类型的继承关系，对于继承关系的判断，可以使用`isinstance()`函数，比如对于继承关系：

```ascii
object -> Animal -> Dog -> Husky
```

应用`isinstance()`函数：

```python
>>> a = Animal()
>>> d = Dog()
>>> h = Husky()

>>> isinstance(h, Husky)
True

>>> isinstance(h, Dog)
True

>>> isinstance(h, Animal)
True

>>> isinstance(d, Dog) and isinstance(d, Animal)
True

>>> isinstance(d, Husky)
False
```

`isinstance()`函数也可以用于基本类型的判断：

```python
>>> isinstance('a', str)
True
>>> isinstance(123, int)
True
>>> isinstance(b'a', bytes)
True
```

还可以判断一个对象是某种类型中的一种：

```python
>>> isinstance([1, 2, 3], (list, tuple))
True
>>> isinstance((1, 2, 3), (list, tuple))
True
```

Python中，一切皆对象，基本类型、函数等等都是`object`类型的。

#### 3、`dir()`

`dir()`函数可以获得一个对象**所有的属性和方法**。它返回一个包含对象所有属性和方法的list：

```python
>>> dir('ABC')
['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']
```

类似`__xxx__`的属性和方法在Python中都是有特殊用途的，其它的都是普通属性或方法。

比如，`__len__`方法返回长度，在Python中，调用`len()`函数获取一个对象的长度时，实际上在`len()`函数内部它会自动去调用该对象的`__len__()`方法：

```python
>>> len('ABC')
3
>>> 'ABC'.__len__()
3
```

自定义类的时候，想用`len(myObj)`则需要写一个`__len__()`方法：

```python
>>> class MyDog(object):
...     def __len__(self):
...         return 100
...
>>> dog = MyDog()
>>> len(dog)
100
```

使用`dir()`查看类的属性和方法，配合`getattr()`、`setattr()`、`hasattr()`，可以直接操作一个对象的状态：

```python
>>> class MyObject(object):
...     def __init__(self):
...         self.x = 9
...     def power(self):
...         return self.x * self.x
...
>>> obj = MyObject()
```

测试该对象的属性：

```python
>>> hasattr(obj, 'x') # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19
```

如果试图获取不存在的属性，会抛出`AttributeError`错误：

```python
>>> getattr(obj, 'z') # 获取属性'z'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'MyObject' object has no attribute 'z'
```

使用`getattr()`时，可以传入默认参数，如果属性不存在，则返回默认值：

```python
>>> getattr(obj, 'z', 404) # 获取属性'z'，如果不存在，返回默认值404
404
```

`getattr()`还可以获取对象的方法：

```python
>>> hasattr(obj, 'power') # 有属性'power'吗？
True
>>> getattr(obj, 'power') # 获取属性'power'
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn = getattr(obj, 'power') # 获取属性'power'并赋值到变量fn
>>> fn # fn指向obj.power
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn() # 调用fn()与调用obj.power()是一样的
81
```

要注意的是，只有在不知道对象信息的时候，才会去获取对象信息。非必要，不要使用`getattr()`。

比如，从文件流读取图像的函数：

```python
def readImage(fp):
    if hasattr(fp, 'read'):
        return readData(fp)
    return None
```

首先，要判断该`fp`对象是否存在read方法，如果存在，则该对象是一个流，如果不存在，则无法读取。这种场合才需要`hasattr()`。另外，在Python这类动态语言中，根据鸭子类型，有`read()`方法，不代表该`fp`对象就是一个文件流，它也可能是网络流，也可能是内存中的一个字节流，但只要`read()`方法返回的是有效的图像数据，就不影响读取图像的功能。