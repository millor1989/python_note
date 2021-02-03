### 枚举类

定义常量时，可以用大写变量名通过整数来定义，比如，月份：

```python
JAN = 1
FEB = 2
MAR = 3
...
NOV = 11
DEC = 12
```

这样做很简单，但是变量类型是`int`，并且仍然是*变量*。

更好的方法是为枚举类型定义一个class类型，每个常量都是class的唯一实例。

Python提供了`Enum`类来实现这个功能：

```python
from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
```

这样，就拥有了`Month`类型的枚举类，可以直接使用`Month.Jan`来引用常量，或者枚举它的所有成员：

```python
>>> for name, member in Month.__members__.items():
...    print(name, '=>', member, ',', member.value)

Jan => Month.Jan , 1
Feb => Month.Feb , 2
Mar => Month.Mar , 3
Apr => Month.Apr , 4
May => Month.May , 5
Jun => Month.Jun , 6
Jul => Month.Jul , 7
Aug => Month.Aug , 8
Sep => Month.Sep , 9
Oct => Month.Oct , 10
Nov => Month.Nov , 11
Dec => Month.Dec , 12
```

`value`属性石自动赋给成员的`int`常量，默认从`1`开始计数

如果要更精确的空中枚举类型，可以从`Enum`派生自定义类：

```python
from enum import Enum, unique

@unique
class Weekday(Enum):
    Sun = 0 # Sun的value被设定为0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6
```

`@unique`装饰器用于辅助检查，保证没有重复值。

访问枚举类型的方法：

```python
>> day1 = Weekday.Mon
>>> print(day1)
Weekday.Mon
>>> print(Weekday.Tue)
Weekday.Tue
>>> print(Weekday['Tue'])
Weekday.Tue
>>> print(Weekday.Tue.value)
2
>>> print(day1 == Weekday.Mon)
True
>>> print(day1 == Weekday.Tue)
False
>>> Weekday(0) # 注意，此处是根据value值获取枚举常量，不是“顺序\索引”！！！
<Weekday.Sun: 0>
>>> print(Weekday(1))
Weekday.Mon
>>> print(day1 == Weekday(1))
True
>>> Weekday(7)
Traceback (most recent call last):
  ...
ValueError: 7 is not a valid Weekday
>>> for name, member in Weekday.__members__.items():
...     print(name, '=>', member)
...
Sun => Weekday.Sun
Mon => Weekday.Mon
Tue => Weekday.Tue
Wed => Weekday.Wed
Thu => Weekday.Thu
Fri => Weekday.Fri
Sat => Weekday.Sat
```

可以用成员名称引用枚举常量，也可以用value值获取枚举常量。