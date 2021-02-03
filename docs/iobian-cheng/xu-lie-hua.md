### 序列化

在程序运行的过程中，所有的变量都是在内存中，一旦程序结束，变量所占用的内存就被操作系统全部回收。

把变量从内存中变成可存储或传输的过程称为**序列化**，在Python中叫**pickling**，在其他语言中也被称之为serialization，marshalling，flattening等等。序列化之后，可以把序列化后的内容写入磁盘，或者通过网络传输。把变量内容从序列化的对象重新读到内存里称为**反序列化**，即unpickling。

Python提供`pickle`模块来实现序列化。

对象的序列化，`pickle.dumps()`方法把任意对象序列化成一个`bytes`：

```python
>>> import pickle
>>> d = dict(name='Bob', age=20, score=88)
>>> pickle.dumps(d)
b'\x80\x03}q\x00(X\x03\x00\x00\x00ageq\x01K\x14X\x05\x00\x00\x00scoreq\x02KXX\x04\x00\x00\x00nameq\x03X\x03\x00\x00\x00Bobq\x04u.'
```

`pickle.dump()`直接把对象序列化后写入一个file-like Object：

```python
>>> f = open('dump.txt', 'wb')
>>> pickle.dump(d, f)
>>> f.close()
```

把对象从磁盘读到内存时，可以先把内容读到一个`bytes`，然后用`pickle.loads()`方法反序列化出对象，也可以直接用`pickle.load()`方法从一个`file-like Object`中直接反序列化出对象。

```python
>>> f = open('dump.txt', 'rb')
>>> d = pickle.load(f)
>>> f.close()
>>> d
{'age': 20, 'score': 88, 'name': 'Bob'}
```

Pickle的问题是它只能用于Python，并且可能不同版本的Python彼此都不兼容，因此，只能用Pickle保存那些不重要的数据。

#### 1、Json

JSON是更通用、更符合Web标准的序列化方式。Python内置的`json`模块提供了非常完善的Python对象到JSON格式的转换。

将Python对象转换为JSON：

```python
>>> import json
>>> d = dict(name='Bob', age=20, score=88)
>>> json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
```

`dumps()`方法返回一个`str`，内容就是标准的JSON。类似地，`dump()`方法可以直接把JSON写入一个`file-like Object`。

把JSON反序列化为Python对象，用`loads()`或者对应的`load()`方法，前者把JSON的字符串反序列化，后者从`file-like Object`中读取字符串并反序列化：

```python
>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> json.loads(json_str)
{'age': 20, 'score': 88, 'name': 'Bob'}
```

JSON标准规定JSON编码是UTF-8，所以总是能正确地在Python的`str`与JSON的字符串之间转换。

#### 2、JSON进阶

Python的`dict`对象可以直接序列化为JSON的`{}`，但通常用`class`表示对象，比如定义`Student`类，然后序列化：

```python
import json

class Student(object):
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score

s = Student('Bob', 20, 88)
print(json.dumps(s))
```

运行代码，抛出`TypeError`：

```python
Traceback (most recent call last):
  ...
TypeError: <__main__.Student object at 0x10603cc50> is not JSON serializable
```

原因是`Student`对象不是一个可序列化为JSON的对象。因为默认情况下，`dumps()`方法不知道如何将`Student`实例变为一个JSON的`{}`对象。需要使用可选参数`default`来定制JSON序列化。

为`Student`专门写一个转换函数，再把函数传递给`default`参数：

```python
def student2dict(std):
    return {
        'name': std.name,
        'age': std.age,
        'score': std.score
    }
```

就可以进行JSON序列化：

```python
>>> print(json.dumps(s, default=student2dict))
{"age": 20, "name": "Bob", "score": 88}
```

通常`class`的实例都有一个`__dict__`属性（也有少数例外，比如定义了`__slots__`的class），它就是一个`dict`，用来存储实例变量。使用`__dict__`属性可以避免为任意的类都编写转换方法：

```python
print(json.dumps(s, default=lambda obj: obj.__dict__))
```

把JSON反序列化为一个`Student`对象实例，`loads()`方法首先转换出一个`dict`对象，然后，用传入的`object_hook`函数负责把`dict`转换为`Student`实例：

```python
def dict2student(d):
    return Student(d['name'], d['age'], d['score'])
```

结果为：

```python
>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> print(json.loads(json_str, object_hook=dict2student))
<__main__.Student object at 0x10cd3c190>
```

