### 单元测试

在测试驱动开发（Test-Driven Development，TDD）中，单元测试很重要。

**单元测试**是对一个模块、一个函数或者一个类来进行正确性检验的测试。

如果单元测试通过，说明测试的这个函数能够正常工作。如果单元测试不通过，要么函数有bug，要么测试条件输入不正确，总之，需要修复使单元测试能够通过。

**单元测试的意义在于**：如果对单元的代码做了修改，只需要再跑一遍单元测试，如果通过，说明修改不会对单元原有的行为造成影响，如果测试不通过，说明修改与原有行为不一致，要么修改代码，要么修改测试。

这种以测试为驱动的开发模式最大的好处是，确保一个程序模块的行为符合设计的测试用例。在将来修改的时候，可以极大程度地保证该模块行为仍然是正确的。

编写一个`Dict`类，这个类的行为和`dict`一致，但是可以通过属性来访问，即：

```python
>>> d = Dict(a=1, b=2)
>>> d['a']
1
>>> d.a
1
```

`mydict.py`代码如下：

```python
class Dict(dict):

    def __init__(self, **kw):
        super().__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value
```

引入Python自带的`unittest`模块，继承`unittest.TestCase`，单元测试代码`mydict_test.py`如下：

```python
import unittest

from mydict import Dict

class TestDict(unittest.TestCase):

    def test_init(self):
        d = Dict(a=1, b='test')
        self.assertEqual(d.a, 1)
        self.assertEqual(d.b, 'test')
        self.assertTrue(isinstance(d, dict))

    def test_key(self):
        d = Dict()
        d['key'] = 'value'
        self.assertEqual(d.key, 'value')

    def test_attr(self):
        d = Dict()
        d.key = 'value'
        self.assertTrue('key' in d)
        self.assertEqual(d['key'], 'value')

    def test_keyerror(self):
        d = Dict()
        with self.assertRaises(KeyError):
            value = d['empty']

    def test_attrerror(self):
        d = Dict()
        with self.assertRaises(AttributeError):
            value = d.empty
```

其中，**`test`开头的方法才是测试方法**，不以`test`开头的方法不被认为是测试方法，测试的时候不会被执行。

对每一类测试都需要编写一个`test_xxx()`方法。由于`unittest.TestCase`提供了很多内置的条件判断，调用这些方法就可以**断言**输出是否符合预期。最常用的断言就是`assertEqual()`：

```python
self.assertEqual(abs(-1), 1) # 断言函数返回的结果与1相等
```

另一种重要的断言就是**期待抛出指定类型的Error**，比如通过`d['empty']`访问不存在的key时，断言会抛出`KeyError`：

```python
with self.assertRaises(KeyError):
    value = d['empty']
```

而通过`d.empty`访问不存在的key时，期待抛出`AttributeError`：

```python
with self.assertRaises(AttributeError):
    value = d.empty
```

#### 1、运行单元测试

运行单元测试一种方式是在`mydict_test.py`的最后加上两行代码：

```python
if __name__ == '__main__':
    unittest.main()
```

这样就可以把`mydict_test.py`当做正常的python脚本运行：

```shell
$ python mydict_test.py
```

另一种方式是在命令行**通过参数`-m unittest`直接运行单元测试**：

```shell
$ python -m unittest mydict_test
.....
----------------------------------------------------------------------
Ran 5 tests in 0.000s

OK
```

这是推荐的做法，这样可以一次批量运行很多单元测试，并且，有很多工具可以自动来运行这些单元测试。

#### 2、`setUp()`与`tearDown()`

单元测试中可以编写两个特殊的`setUp()`和`tearDown()`方法。这两个方法分别在每调用一个测试方法的前后被执行。

比如，在测试需要启动一个数据库时，就可以在`setUp()`方法中连接数据库，在`tearDown()`方法中关闭数据库，而不必在每个测试方法中重复相同的代码：

```python
class TestDict(unittest.TestCase):

    def setUp(self):
        print('setUp...')

    def tearDown(self):
        print('tearDown...')
```

每个测试方法调用前后都会打印出`setUp...`和`tearDown...`。