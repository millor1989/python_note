### 元类

#### 1、`type()`

动态语言和静态语言最大的不同是，函数和类的定义不是编译时定义的，而是运行时动态创建的。

比如，要定义一个`Hello`的类，就要写一个`hello.py`模块：

```python
class Hello(object):
    def hello(self, name='world'):
        print('Hello, %s.' % name)
```

Python解释器载入`hello`模块时，会依次执行该模块的所有语句，执行结果就是动态创建一个`Hello`类的实例，测试如下：

```python
>>> from hello import Hello
>>> h = Hello()
>>> h.hello()
Hello, world.
>>> print(type(Hello))
<class 'type'>
>>> print(type(h))
<class 'hello.Hello'>
```

`type()`函数可以查看一个类型或变量的类型，`Hello`是一个类，类型是`type`，而`h`是一个实例，它的类型是`Hello`类。

**类的定义是运行时动态创建的，创建类的方法就是使用`type()`函数**。

`type()`函数既可以返回一个对象类型，又可以创建新的类型。可以使用`type()`函数创建`Hello`类，无需通过`class`关键字进行定义：

```python
>>> def fn(self, name='world'): # 先定义函数
...     print('Hello, %s.' % name)
...
>>> Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
>>> h = Hello()
>>> h.hello()
Hello, world.
>>> print(type(Hello))
<class 'type'>
>>> print(type(h))
<class '__main__.Hello'>
```

创建类时，`type()`函数需要依次传入3个参数：

1. 类名
2. 继承的父类集合，是一个tuple，支持多重继承
3. 类的方法名称与函数的绑定，是一个dict

通过`type()`函数创建的类和`class`关键字定义的类是完全一样的。因为Python解释器遇到类定义时，也是扫描类的定义，然后调用`type()`函数创建类。

一般，都使用`class`关键字定义类，使用`type()`函数可以在运行期动态创建类。

#### 2、metaclass

metaclass，元类，定义完类后可以根据这个类创建出实例；使用metaclass可以创建类或修改类。

例，为自定义的`MyList`增加`add`方法，定义一个`ListMetaclass`，按照习惯，metaclass的类名总是以Metaclass结尾：

```python
# metaclass是类的模板，所以必须从`type`类型派生：
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        attrs['add'] = lambda self, value: self.append(value)
        return type.__new__(cls, name, bases, attrs)
```

有了`ListMetaclass`，在定义类的时候还要指定使用`ListMetaclass`来定制类，使用关键字参数`metaclass`：

```python
class MyList(list, metaclass=ListMetaclass):
    pass
```

加上`metaclass`参数后，Python解释器在创建`MyList`时，会通过`ListMetaclass.__new__()`来创建，通过修改`__new__()`方法可以修改类的定义，并返回修改后的定义。

`__new__()`方法接收的参数依次是：

1. 准备创建的类的对象
2. 类的名字
3. 累继承的父类的集合
4. 类的方法的集合

测试一下`MyList`增加的`add()`方法：

```python
>>> L = MyList()
>>> L.add(1)
>> L
[1]
```

Python的list是没有`add()`方法的。

当然可以在`MyList`定义中加入`add()`方法，一般情况下，都应该这样做。

`metaclass`的意义，可以通过ORM（Object Relational Mapping，对象-关系映射）的例子解释：

ORM框架将关系型数据库的表和类对应，编写ORM框架，所有的类只能动态定义。

比如，用户使用ORM框架时，想定一个`User`类来操作`User`表，代码可以是：

```python
class User(Model):
    # 定义类的属性到列的映射：
    id = IntegerField('id')
    name = StringField('username')
    email = StringField('email')
    password = StringField('password')

# 创建一个实例：
u = User(id=12345, name='Michael', email='test@orm.org', password='my-pwd')
# 保存到数据库：
u.save()
```

父类`Model`和属性`StringField`、`IntegerField`由ORM框架提供。魔术方法`save()`全部由metaclass自动完成。接下来，尝试实现该ORM。

首先，定义`Field`类，它用来保存数据库表的字段名和字段类型：

```python
class Field(object):

    def __init__(self, name, column_type):
        self.name = name
        self.column_type = column_type

    def __str__(self):
        return '<%s:%s>' % (self.__class__.__name__, self.name)
```

在`Field`的基础上，进一步定义各种类型的`Field`，比如`StringField`，`IntegerField`等等：

```python
class StringField(Field):

    def __init__(self, name):
        super(StringField, self).__init__(name, 'varchar(100)')

class IntegerField(Field):

    def __init__(self, name):
        super(IntegerField, self).__init__(name, 'bigint')
```

然后编写`ModelMetaclass`：

```python
class ModelMetaclass(type):

    def __new__(cls, name, bases, attrs):
        if name=='Model':
            return type.__new__(cls, name, bases, attrs)
        print('Found model: %s' % name)
        mappings = dict()
        for k, v in attrs.items():
            if isinstance(v, Field):
                print('Found mapping: %s ==> %s' % (k, v))
                mappings[k] = v
        for k in mappings.keys():
            attrs.pop(k)
        attrs['__mappings__'] = mappings # 保存属性和列的映射关系
        attrs['__table__'] = name # 假设表名和类名一致
        return type.__new__(cls, name, bases, attrs)
```

以及基类`Model`：

```python
class Model(dict, metaclass=ModelMetaclass):

    def __init__(self, **kw):
        super(Model, self).__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Model' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value

    def save(self):
        fields = []
        params = []
        args = []
        for k, v in self.__mappings__.items():
            fields.append(v.name)
            params.append('?')
            args.append(getattr(self, k, None))
        sql = 'insert into %s (%s) values (%s)' % (self.__table__, ','.join(fields), ','.join(params))
        print('SQL: %s' % sql)
        print('ARGS: %s' % str(args))
```

当用户定义一个`class User(Model)`类时，Python解释器首先在当前类`User`的定义中查找`metaclass`，如果没有找到，会继续向父类`Model`中查找`metaclass`，使用`Model`中定义的`metaclass`——`ModelMetaclass`类来创建`User`类。可见，子类是可以隐式地继承父类的`metaclass`的。

`ModelMetaclass`中做了几个事情：

- 排除掉对`Model`类的修改
- 在当前类（比如`User`）中查找定义的类的所有属性，如果找到一个`Field`属性，就把它保存到`__mappings__`的dict中，同时从类属性中删除该Field属性（不然，实例的属性会覆盖类的同名属性，会造成运行时错误）。
- 将表名保存到`__table__`中，这里简化为表名默认为类名。

在`Model`类中可以定义各种操作数据库的方法，比如`save()`、`delete`、`find()`、`update()`等等。

代码尝试：

```python
u = User(id=12345, name='Michael', email='test@orm.org', password='my-pwd')
u.save()
```

输出如下：

```python
Found model: User
Found mapping: email ==> <StringField:email>
Found mapping: password ==> <StringField:password>
Found mapping: id ==> <IntegerField:uid>
Found mapping: name ==> <StringField:username>
SQL: insert into User (password,email,username,id) values (?,?,?,?)
ARGS: ['my-pwd', 'test@orm.org', 'Michael', 12345]
```

