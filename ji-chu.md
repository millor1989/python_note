### 基础

- 采用缩进形式组织代码（复制+粘贴代码时可能会有问题），一般使用4个空格缩进。

- `#`为单行注释，`'''`或`"""`包括的是多行注释，当注释作为字符串一部分出现时，不再被当做注释，而是代码的一部分。多行注释的方式可以用来表示多行内容。

- 大小写敏感。

#### 1、数据类型

##### 1.1、整数

Python可以表示任意大小的整数（而Java的数值有大小限制），包括负整数。

支持十六进制表示整数，`0xff00`。

可以用`_`格式化整数，`10_000_000_000`（`10000000000`）、`0xa1b2_c3d4`（`0xa1b2c3d4`）。

##### 1.2、浮点数

浮点数可以直接书写，比如`1.23`，`-9.01`。

对于比较大或比较小的浮点数，必须用科学计数法表示，用`e`代替10，$$1.23\times10^9$$可以表示为`1.23e9`或`12.3e8`，`0.000012`可以表示为`1.2e-5`。

**由于整数和浮点数在计算机内部存储方式的不同，整数运算是精确地，而浮点数运算可能有误差。**

Python中的两种除法`/`（即使是整数相除，并且整除，结果也是浮点数）和`//`（地板除，相除结果即使是小数或无法除尽，结果依然是整数，相当于取整，与之对应的是取余`%`）。

```python
>>> 9 / 3
3.0
>>> 10 // 3
3
>>> 10 % 3
1
```

##### 1.3、字符串

字符串用`'`或者`"`包括的文本表示。对于`'`或者`"`的表示要通过转义字符`\`来标识。

Python支持使用`r`来表示字符串直接显示，不作转义。

```python
>>> print('I\'m ok.')
I'm ok.
>>> print('I\'m learning\nPython.')
I'm learning
Python.
>>> print('\\\n\\')
\
\

>>> print('\\\t\\')
\       \
>>> print(r'\\\t\\')
\\\t\\
```

`str.upper()`：转换为大写

`str.lower()`：转换为小写

`str.capitalize()`：转换为首字母大写

##### 1.4、布尔值

`True`或`False`（注意大小写）。

布尔运算返回结果也是布尔值。

逻辑运算`and`、`or`、`not`。

布尔值经常用于条件判断：

```python
if age >= 18:
    print('adult')
else:
    print('teenager')
```

##### 1.5、空值

`None`

#### 2、变量

变量，用变量名表示，变量名必须为字母、数字和下划线的组合，不能以数字开头。

变量可以是任意数据类型。

`=`为赋值语句，可以将任意数据类型赋值给变量，同一个变量可以被反复赋值，并且可以被赋予不同类型的值。与Java不同，Python是动态语言，更加灵活。

- 变量在计算机内存中的表示，写`a='ABC'`时，Python做了两件事：

  1、内存中创建`'ABC'`字符串

  2、内存中创建变量`a`，并将其指向`'ABC'`

- 常量

  Python中常量的变量名通常全部大写。Python中常量也是变量，是一种约定，是可变得，可以改变常量的值。

#### 3、字符串和编码

计算机只能处理数字，处理字符串就要进行编码。

最早的编码是`ASCII`，只有127个字符编码，英文大小写、一些符号和数字，`A`编码为64，`z`编码122。

各国都有不同编码标准，Unicode包含了所有语言的字符。Unicode编码中最常用的是`UCS-16`编码，用两个字节表示一个字符（生僻字符，要4字节）。Unicode编码的问题是，占用存储空间可能比较多（比如，对于全英文文本），在存储和传输上不划算，所以出现了可变长Unicode编码`UTF-8`编码。

UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。如果你要传输的文本包含大量英文字符，用UTF-8编码就能节省空间。UTF-8编码有一个额外的好处，就是ASCII编码实际上可以被看成是UTF-8编码的一部分，所以，大量只支持ASCII编码的历史遗留软件可以在UTF-8编码下继续工作。

**计算机系统通用的字符编码方式**是：在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。

##### 3.1、Python字符串

Python 3中，字符串是以Unicode编码的。

Python字符串类型`str`，是不可变类型。

Python提供了`ord()`函数获取字符的整数表示，`chr()`函数将编码转换为对应字符。

```python
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```

也可以用字符对应的十六进制写字符串，两者是等价的：

```python
>>> '\u4e2d\u6587'
'中文'
```

Python的`bytes`类型数据使用`b`为前缀的单引号或双引号表示：

```python
x = b'ABC'
```

`encode()`和`decode()`方法可以用来进行`bytes`类型和`str`类型的转换：

```python
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'

>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```

`len()`函数，对于`str`计算的是字符数，对于`bytes`计算的是字节数：

```python
>>> len('ABC')
3
>>> len('中文')
2


>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```

为避免乱码，应当始终坚持使用UTF-8编码对`str`和`bytes`进行转换。

Python源代码也是文本文件，保存源码时尽量**申明了UTF-8编码**（尤其是使用中文或其他字符时）保存为UTF-8编码：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

这样，Python解释器在读取源代码时，会按照UTF-8编码读取。

第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；

第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。

申明了UTF-8编码并不意味着`.py`文件就是UTF-8编码的，必须并且要确保文本编辑器正在**使用UTF-8 without BOM编码**。

##### 3.2、字符串格式化

Python字符串格式化方式与C语言一致，通过`%`运算实现：

```
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
```

常见占位符：

| 占位符 | 替换内容     |
| ------ | ------------ |
| %d     | 整数         |
| %f     | 浮点数       |
| %s     | 字符串       |
| %x     | 十六进制整数 |

格式化整数和浮点数，还可以指定是否补0和整数与小数的位数。

`%s`可以用于任意类型，他会将任何数据类型转换为字符串。

表示`%`时，用`%%`表示。



还可以使用字符串的`format()`方法格式化字符串，此时用`{0}`、`{1}`……的形式表示占位符。

```python
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```



最后一种格式化字符串方法——`f`开头的字符串，`f-string`。用对应变量替换占位符`{var}`

```python
>>> r = 2.5
>>> s = 3.14 * r ** 2
>>> print(f'The area of a circle with radius {r} is {s:.2f}')
The area of a circle with radius 2.5 is 19.62
```

其中`:`后面的`.2f`指定了格式化参数（保留两位小数）。

#### 4、list和tuple

##### 4.1、list

list是有序集合，可随时添加或者删除其中的元素。

list中的元素可以是不同类型的。

可以用`len()`函数获取list元素的个数。

可以通过从0开始的索引访问list元素。

可以通过从-1开始的索引从后往前的访问list元素。

```python
>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']

>>> len(classmates)
3

>>> classmates[0]
'Michael'
>>> classmates[1]
'Bob'
>>> classmates[2]
'Tracy'

>>> classmates[-1]
'Tracy'
```

###### **※**`[idx]`是下标操作（subscript），前提是对象是可下标的（subscriptable）。

`append`，往list末尾追加元素。

`insert`往指定索引位置插入元素。

`pop()`删除并返回末尾元素，`pop(i)`删除并返回指定索引`i`对应的元素。

通过对应索引赋值，可以替换指定索引对应元素。

```python
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']

>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']

>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']

>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']

>>> classmates[1] = 'Sarah'
>>> classmates
['Michael', 'Sarah', 'Tracy']
```

##### 4.2、tuple

元组，tuple。与list类似，但是，**tuple是不可变的**，一经创建就不能修改。

可以通过索引访问，方式与list相同。

```python
>>> (123,'2')[0]
123

>>> (123,'2')[1]
'2'
```

tuple可以是空的。

定义只有一个元素的tuple是要加`,`。

```python
>>> classmates = ('Michael', 'Bob', 'Tracy')

>>> t = (1, 2)
>>> t
(1, 2)

>>> t = ()
>>> t
()

>>> t = (1,)
>>> t
(1,)
```

tuple元素可以是任何类型。

tuple是不可变的，但是可以修改tuple的list类型元素。

```python
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```

#### 5、条件判断

Python的`if`语句不能省略`:`。

```python
age = 20
if age >= 18:
    print('your age is', age)
    print('adult')


age = 3
if age >= 18:
    print('your age is', age)
    print('adult')
else:
    print('your age is', age)
    print('teenager')


age = 3
if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
```

`if`判断条件可以**简写**

```python
if x:
    print('True')
```

只要`x`是非零数值、非空字符串、非空list等，就判断为`True`，否则为`False`。

#### 6、循环

##### 6.1、for

可以迭代list或tuple的元素。

```python
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)

sum = 0
for x in range(101):
    sum = sum + x
print(sum)
```

其中`range(101)`生成的是一个0~100的序列。

##### 6.2、while

```python
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)
```

##### 6.3、break和continue

`break`结束循环

`continue`跳过本次循环

#### 7、dict和set

##### 7.1、dict

Python字典（dictionary）dict，类似于Java的map。

具有极快的查找速度，不会随字典变大而变慢（list则不同），但是dict占用内存空间大（list则较小）。

dict的key必须是不可变对象。

dict的key不能重复，key-value一一对应。多次对同一个key赋值，只保留最后一次赋值结果。

dict内部顺序与key放入的先后顺序无关。

通过`dict[key]`形式读取不存在的key会报错。可以通过`in`来判断是否存在key，也可以通过`get()`方法——key不存在返回`None`，或指定默认值。

使用`pop(key)`方法删除key，并返回key对应的value。

```python
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95

>>> d['Adam'] = 67
>>> d['Adam']
67

>>> d['Jack'] = 90
>>> d['Jack']
90
>>> d['Jack'] = 88
>>> d['Jack']
88


>>> 'Thomas' in d
False


>>> d.get('Thomas')
None
>>> d.get('Thomas', -1)
-1


>>> d.pop('Bob')
75
>>> d
{'Michael': 95, 'Tracy': 85}
```

##### 7.2、set

set与dict类似，但是只存储key，不存储value。

set中没有重复key，set会自动过滤重复key。

set是无序的，set的显示顺序不表示set是有序的。

可以通过`add(key)`方法增加set元素，通过`remove(key)`方法删除set元素。

set可以看作无序且无重复元素的集合，可以做数学意义上的交集（`&`），并集（`|`），差集（`-`）。

```
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}

>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.add(4)
>>> s
{1, 2, 3, 4}

>>> s.remove(4)
>>> s
{1, 2, 3}

>>>s1= {1,2,3,4,4}
>>>s2={2,3,4,5}
>>>print(s1)
{1, 2, 3, 4}


>>>print(s1 | s2)
{1, 2, 3, 4, 5}
>>>print(s1 & s2)
{2, 3, 4}
>>>print(s1 - s2)
{1}
```

