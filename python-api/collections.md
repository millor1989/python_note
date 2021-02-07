### collections

collection模块包含了很多有用的集合类。

#### 1、`namedtuple`

`tuple`是不变集合，可以用它表示一个点在二维坐标系中的坐标：

```python
>>> p = (1, 2)
```

但是，不够直观，不能直接看出`p`是表示坐标的。此时，可以用`namedtuple`：

```python
>>> from collections import namedtuple
>>> Point = namedtuple('Point', ['x', 'y'])
>>> p = Point(1, 2)
>>> p.x
1
>>> p.y
2
```

`namedtuple`是一个函数，可以用来创建一个自定义的`tuple`对象，并且规定了`tuple`元素的个数，并可以用属性而不是索引来引用`tuple`的某个元素，它具备tuple的不变性，又可以根据属性来引用元素，很方便。

#### 2、`deque`

使用`list`存储数据，按索引访问元素很快，但是插入和删除元素就比较慢了，因为`list`是线性存储，数据量大的时候，插入和删除效率很低。`deque`是**双向列表**可以高效地进行插入和删除，适用于栈和队列：

```python
>>> from collections import deque
>>> q = deque(['a', 'b', 'c'])
>>> q.append('x')
>>> q.appendleft('y')
>>> q
deque(['y', 'a', 'b', 'c', 'x'])
```

`deque`除了实现list的`append()`和`pop()`外，还支持`appendleft()`和`popleft()`，这样就可以非常高效地在头部添加或删除元素。

#### 3、`defaultdict`

使用`dict`时，如果引用的Key不存在，就会抛出`KeyError`。如果希望key不存在时，返回一个默认值，就可以用`defaultdict`：

```python
>>> from collections import defaultdict
>>> dd = defaultdict(lambda: 'N/A')
>>> dd['key1'] = 'abc'
>>> dd['key1'] # key1存在
'abc'
>>> dd['key2'] # key2不存在，返回默认值
'N/A'
```

注意默认值是调用函数返回的，而函数在创建`defaultdict`对象时传入。

除了在Key不存在时返回默认值，`defaultdict`的其他行为跟`dict`是完全一样的。

#### 4、`OrderedDict`

使用`dict`时，Key是无序的，如果要保持key的顺序，可以用`OrderedDict`：

```python
>>> from collections import OrderedDict
>>> d = dict([('a', 1), ('b', 2), ('c', 3)])
>>> d # dict的Key是无序的
{'a': 1, 'c': 3, 'b': 2}
>>> od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
>>> od # OrderedDict的Key是有序的
OrderedDict([('a', 1), ('b', 2), ('c', 3)])
```

`OrderedDict`的**Key顺序是插入的顺序**，不是Key本身的顺序：

```python
>>> od = OrderedDict()
>>> od['z'] = 1
>>> od['y'] = 2
>>> od['x'] = 3
>>> list(od.keys()) # 按照插入的Key的顺序返回
['z', 'y', 'x']
```

`OrderedDict`可以实现一个FIFO（先进先出）的dict，当容量超出限制时，先删除最早添加的Key：

```python
from collections import OrderedDict

class LastUpdatedOrderedDict(OrderedDict):

    def __init__(self, capacity):
        super(LastUpdatedOrderedDict, self).__init__()
        self._capacity = capacity

    def __setitem__(self, key, value):
        containsKey = 1 if key in self else 0
        if len(self) - containsKey >= self._capacity:
            last = self.popitem(last=False)
            print('remove:', last)
        if containsKey:
            del self[key]
            print('set:', (key, value))
        else:
            print('add:', (key, value))
        OrderedDict.__setitem__(self, key, value)
```

#### 5、`ChainMap`

`ChainMap`可以把一组`dict`串起来并组成一个逻辑上的`dict`。`ChainMap`本身也是一个dict，但是查找的时候，会按照顺序在内部的dict依次查找。

应用场景举例：应用程序往往都需要传入参数，参数可以通过命令行传入，可以通过环境变量传入，还可以有默认参数。可以用`ChainMap`实现参数的优先级查找，即先查命令行参数，如果没有传入，再查环境变量，如果没有，就使用默认参数。

```python
from collections import ChainMap
import os, argparse

# 构造缺省参数:
defaults = {
    'color': 'red',
    'user': 'guest'
}

# 构造命令行参数:
parser = argparse.ArgumentParser()
parser.add_argument('-u', '--user')
parser.add_argument('-c', '--color')
namespace = parser.parse_args()
command_line_args = { k: v for k, v in vars(namespace).items() if v }

# 组合成ChainMap:
combined = ChainMap(command_line_args, os.environ, defaults)

# 打印参数:
print('color=%s' % combined['color'])
print('user=%s' % combined['user'])
```

没有任何参数时，打印出默认参数：

```shell
$ python3 use_chainmap.py 
color=red
user=guest
```

当传入命令行参数时，优先使用命令行参数：

```shell
$ python3 use_chainmap.py -u bob
color=red
user=bob
```

同时传入命令行参数和环境变量，命令行参数的优先级较高：

```shell
$ user=admin color=green python3 use_chainmap.py -u bob
color=green
user=bob
```

#### 6、`Counter`

`Counter`是一个简单的计数器，可以记录字符出现的次数：

```python
>>> from collections import Counter
>>> c = Counter()
>>> for ch in 'programming':
...     c[ch] = c[ch] + 1
...
>>> c
Counter({'g': 2, 'm': 2, 'r': 2, 'a': 1, 'i': 1, 'o': 1, 'n': 1, 'p': 1})
>>> c.update('hello') # 也可以一次性update
>>> c
Counter({'r': 2, 'o': 2, 'g': 2, 'm': 2, 'l': 2, 'p': 1, 'a': 1, 'i': 1, 'n': 1, 'h': 1, 'e': 1})
```

`Counter`还可以直接统计字符串的字符出现次数：

```python
>>> help(Counter)
Help on class Counter in module collections:

class Counter(builtins.dict)
 |  Dict subclass for counting hashable items.  Sometimes called a bag
 |  or multiset.  Elements are stored as dictionary keys and their counts
 |  are stored as dictionary values.
 |  
 |  >>> c = Counter('abcdeabcdabcaba')  # count elements from a string
 ...

>>> c = Counter('abcdeabcdabcaba')
>>> c
Counter({'a': 5, 'b': 4, 'c': 3, 'd': 2, 'e': 1})

# 出现次数最多的两个字符
>>> c.most_common(2)
[('a', 5), ('b', 4)]

# 去重后的字符
>>> c.keys()
dict_keys(['a', 'b', 'c', 'd', 'e'])
```

