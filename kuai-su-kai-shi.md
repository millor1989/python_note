- 笔记来源：http://www.newthinktank.com/2019/08/learn-python-one-video/


```python
# 交换两个变量的值（很神奇的哇）
a=10
b=9
a,b=b,a
print(a,b)
```


```python
# ----- INTRO -----
# Python文件后缀是“.py”
# 打印到控制台
# Python语句由换行符终结
print("hello, world")
```

    hello, world



```python
import sys
# 获取用户输入，并保存到一个变量中
name = input("What is your name")
```

    What is your nameB.K.



```python
print("Hi ", name)
```

    Hi  B.K.



```python
# 如果要把一条语句扩展到多行，需要使用括号或者“\”
v1 = (
    1+2
    +3
)
print(v1)
```

    6



```python
v1 = 1 + 2 \
 +4
print(v1)
```

    7



```python
#多个语句放在同一行时，要使用“;”分隔语句
v1=8;v1=v1-2
print(v1)
```

    6



```python
"""
多行的
注释
"""
# ----- VARIABLES -----
# 变量名必须是字母或者下划线开头，并且只能是字母、数字、下划线的组合；变量名是大小写敏感的
V2=1
v2=2 #v2和V2是不同的

# 一次为多个变量赋值
v3 = v4 = 20
print(v3)
print(v4)
```

    20
    20



```python
# ----- DATA TYPES -----
# Python中的数据是动态类型的，并且数据类型可以改变
# 所有的数据都是一个对象
# 基本类型是integers, floats, complex numbers, strings, booleans
# Python 没有字符（char）类型

# 获取类型的方法
print(type(10))
```

    <class 'int'>



```python
# 整数没有最大值限制
# 获取可以使用的最大值（practical max size）的方法是：
print(sys.maxsize)

# Float是有精度的值
# 获取最大的float：
print(sys.float_info.max)
```

    9223372036854775807
    1.7976931348623157e+308



```python
# Float的最大精度是15个小数，超过15个小数，会出现错误
f1 = 1.1111111111111111
f2 = 1.1111111111111111
f3 = f1 + f2
print(f3)
```

    2.2222222222222223



```python
# complex numbers（复数）是[实部(real part)] + [虚部（imaginary part）]组成的
cn1 = 5 + 6j
print(cn1)
```

    (5+6j)



```python
# Boolean值是True或者False
b1 = True
# 字符串由单引号或者双引号包括
str1 = "Escape Sequence \' \t \" \\ and \n"
print(str1)

str2 = '''Triple quoted Strings can contain ' and "'''
print(str2)
```

    Escape Sequence ' 	 " \ and 
    
    Triple quoted Strings can contain ' and "



```python
# 类型转换
print("Cast ", type(int(5.4)))  # to int
print(int(5.4))
print("Cast 2 ", type(str(5.4)))  # to string
print(str(5.4))
print("Cast 3 ", type(chr(97)))  # to string
print(chr(97))
print("Cast 4 ", type(ord('a')))  # to int
print(ord('a'))
```

    Cast  <class 'int'>
    5
    Cast 2  <class 'str'>
    5.4
    Cast 3  <class 'str'>
    a
    Cast 4  <class 'int'>
    97



```python
# ----- OUTPUT -----
# 可以自定义输出分隔符
print(12, 21, 1974, sep='/')

# 消除换行符
print("No Newline", sep='')

# 字符串格式化，指数使用‘%e’
print("\n%o4d %s %.2f %c" % (1, "Derek", 1.234, 'A'))

# ----- MATH -----
print("5 + 2 =", 5 + 2)
print("5 - 2 =", 5 - 2)
print("5 * 2 =", 5 * 2)
print("5 / 2 =", 5 / 2)
print("5 % 2 =", 5 % 2)
print("5 ** 2 =", 5 ** 2)
print("5 // 2 =", 5 // 2)

# Shortcuts
i1 = 2
i1 += 1
print("i1 ", i1)

i2 = 22
i2 -= 11
print("i2 ", i2)
```

    12/21/1974
    No Newline
    
    14d Derek 1.23 A
    5 + 2 = 7
    5 - 2 = 3
    5 * 2 = 10
    5 / 2 = 2.5
    5 % 2 = 1
    5 ** 2 = 25
    5 // 2 = 2
    i1  3
    i2  11



```python
# 算术运算函数
import math
print("abs(-1) ", abs(-1)) # 绝对值
print("max(5, 4) ", max(5, 4))
print("min(5, 4) ", min(5, 4))
# 指数
print("pow(2, 2) ", pow(2, 2))
# 向上取整
print("ceil(4.5) ", math.ceil(4.5))
# 向下取整
print("floor(4.5) ", math.floor(4.5))
#取整
print("round(4.5) ", round(4.5))
# e的指数
print("exp(1) ", math.exp(1))  # e**x
# 底数为e的对数
print("log(e) ", math.log(math.exp(1)))
# 底数为10的对数
print("log(100, 10) ", math.log(100, 10))  # Base 10 Log
# 开方
print("sqrt(100) ", math.sqrt(100))
# 正弦
print("sin(0) ", math.sin(0))
# 余弦
print("cos(0) ", math.cos(0))
# 正切
print("tan(0) ", math.tan(0))
print("asin(0) ", math.asin(0))
print("acos(0) ", math.acos(0))
print("atan(0) ", math.atan(0))
print("sinh(0) ", math.sinh(0))
print("cosh(0) ", math.cosh(0))
print("tanh(0) ", math.tanh(0))
print("asinh(0) ", math.asinh(0))
print("acosh(pi) ", math.acosh(math.pi))
print("atanh(0) ", math.atanh(0))
print("hypot(0) ", math.hypot(10, 10))  # sqrt(x*x + y*y)
print("radians(0) ", math.radians(0))
# 角度数
print("degrees(pi) ", math.degrees(math.pi))
```

    abs(-1)  1
    max(5, 4)  5
    min(5, 4)  4
    pow(2, 2)  4
    ceil(4.5)  5
    floor(4.5)  4
    round(4.5)  4
    exp(1)  2.718281828459045
    log(e)  1.0
    log(100, 10)  2.0
    sqrt(100)  10.0
    sin(0)  0.0
    cos(0)  1.0
    tan(0)  0.0
    asin(0)  0.0
    acos(0)  1.5707963267948966
    atan(0)  0.0
    sinh(0)  0.0
    cosh(0)  1.0
    tanh(0)  0.0
    asinh(0)  0.0
    acosh(pi)  1.811526272460853
    atanh(0)  0.0
    hypot(0)  14.142135623730951
    radians(0)  0.0
    degrees(pi)  180.0



```python
''' 需要注意的是，Python3中的round不是四舍五入，
而是四舍六入五成偶（并且，这个规则是针对真正存储在计算机中的值而不字母值——设计计算机存储小数时的精确性问题），
如果小数点为.5，则返回结果是距离最近的偶数
'''
####所以：：：有精度要求的地方，用Decimal模块 ###
print("round4.6",round(4.6))
print("round54.5",round(54.5))
print("round5.5",round(5.5))
print("round5.45",round(5.45, 1))
print("round5.55",round(5.55, 1))
print("round4.55",round(4.55, 1))
print("round5.565",round(5.565, 1))

print("round2.675",round(2.675, 2))
# 如果要四舍五入
#((x*100+0.5)//1)/100
print("四舍五入保留两位的一种简洁方法：\n((x*100+0.5)//1)/100",((2.675*100+0.5)//1)/100)
```

    round4.6 5
    round54.5 54
    round5.5 6
    round5.45 5.5
    round5.55 5.5
    round4.55 4.5
    round5.565 5.6
    round2.675 2.67
    四舍五入保留两位的一种简洁方法：
    ((x*100+0.5)//1)/100 2.68



```python
# 产生随机数
import random
print("Random", random.randint(1,101))
```

    Random 32



```python

'''Python中浮点数进行运算时会先转换为二进制，然后运算，会有精度差异。
如果要精确计算小数，
一定要全程使用Decimal'''

from decimal import *
dc = decimal.getcontext()
print(dc.prec)
d1 = decimal.Decimal('3.1415926')
print(d1)
## 不要给Decimal传浮点数，浮点数本身可能存在无法精确存储的问题
print(Decimal(3.1415926))
print(Decimal.from_float(3.1415926))
# 设置全局精度
dc.prec = 4
print(dc.prec)
print(dc.rounding)# ROUND_HALF_EVEN :四舍六入五成偶
print(d1 + 1)
# 恢复默认
dc.prec = 28
print(dc)
d2 = Decimal('2.675')
print(d2)
# 用Decimal四舍五入保留小数的方法
d3 = d2.quantize(Decimal('0.00'), rounding = ROUND_HALF_UP)
print(d3)

```

    28
    3.1415926
    3.14159260000000006840537025709636509418487548828125
    3.14159260000000006840537025709636509418487548828125
    4
    ROUND_HALF_EVEN
    4.142
    Context(prec=28, rounding=ROUND_HALF_EVEN, Emin=-999999, Emax=999999, capitals=1, clamp=0, flags=[Inexact, FloatOperation, Rounded], traps=[InvalidOperation, DivisionByZero, Overflow])
    2.675
    2.68



```python
# ----- NaN & inf -----
# inf 指的是无穷大
print(math.inf > 0)

# NaN用来指代无法定义的数值
print(math.inf - math.inf)
```

    True
    nan



```python
# ----- CONDITIONALS -----
# 比较操作符：< > <= >= == !=

# if else & elif
age = 30
if age > 21:
    # Python用缩进来定义之前条件为True时，所有需要执行的代码
    print("You can drive a tractor trailer")
elif age >= 16:
    print("You can drive a car")
else:
    print("You can't drive")

# 逻辑操作符：and / or / not
if age < 5:
    print("Stay Home")
elif (age >= 5) and (age <= 6):
    print("Kindergarten")
elif (age > 6) and (age <= 17):
    print("Grade %d" % (age - 5))
else:
    print("College")

# Python中的三目运算
canVote = True if age >= 18 else False
print(canVote)
```

    You can drive a tractor trailer
    College
    True

Python 中变量可以两边比较：

```python
6 < age <= 17
```

而 JVM 语言不能这样比较，JVM 语言执行的是从左到右的运算，最终会由于是不同类型的比较而失败。

```python
if age < 5:
    print("Stay Home")
elif (5 <= age <= 6):
    print("Kindergarten")
elif (6 < age <= 17):
    print("Grade %d"%(age - 5))
else:
    print("College")
```



```python
# 逻辑运算符 and 优先级比 or 优先级高，not 比and优先级高
True or True or False
```




    True




```python
True and not True
```




    False




```python
# BOOL转换bool()
print(bool(1)) # all numbers are treated as true, except 0
print(bool(0))
print(bool("asf")) # all strings are treated as true, except the empty string ""
print(bool(""))
# Generally empty sequences (strings, lists, and other types we've yet to see like lists and tuples)
# are "falsey" and the rest are "truthy"

if 0:
    print(0)
elif "spam":
    print("spam")
```

    True
    False
    True
    False



```python
# Bool转化为int int()
# 使用boolean值进行数学运算时，Python会隐式的将boolean值转换为int值
a = True
b = False
c = False
a+b+c
```




    1




```python
# ----对象
# Python中一切都是对象，对象带有属性和方法
# Python中的数值是一个拥有一个属性`imag`的对象
x = 12
# x 是一个实数（real number），所以它的虚部(imaginary)是0.
print(x.imag)
# 定义一个复数（complex number）
c = 12 + 3j
print(c.imag)

print(x.bit_length)
print(x.bit_length())
```

    0
    3.0
    <built-in method bit_length of int object at 0x000000005D3F6D80>
    4



```python
# ----- STRINGS -----
# 字符串可以被当作是字符的序列，可以像操作List一样操作字符串
# 但是字符串是不可变的，不能修改其中的某个字符
# 忽略转义字符的原始字符串
# 转义字符反斜线“\”
print(r"I'll be ignored \n")

# + 字符串连接符
print("Hello " + "You")

# len 获取字符串长度
str3 = "Hello You"
print("Length ", len(str3))

# 获取字符串某个索引位置的字符
print("1st ", str3[0])

# 最后一个字符
print("Last ", str3[-1])

# 前三个字符，区间[:]为前闭后开区间
print("1st 3 ", str3[0:3])

# Get every other character
# 不懂这是嘛意思？
print("Every Other ", str3[0:-1:2]) # Last is a step

# 字符串是不可变的，不能通过 str3[0] = "a"来改变字符串
# 如果要改变字符串：
str3 = str3.replace("Hello", "Goodbye")
print(str3)

str3 = str3[:8] + "y" + str3[9:]
print(str3)


# 测试字符串是否包含字符串
print("you" in str3)
# 测试字符串是否不包含字符串
print("you" not in str3)

# 获取第一个匹配字符串的索引，不匹配则返回-1
print("You Index ", str3.find("you"))

# 去除空格 还有lstrip和rstrip
print("    Hello    ".strip())

# 用空格作为分隔符，拼接集合的内容
print(" ".join(["Some", "Words"]))

# 用分隔符或占位符把字符串切分为一个集合
print("A, string".split(", "))

# 使用f-string格式化输出
int1 = int2 = 5
print(f'{int1} + {int2} = {int1 + int2}')

# 字符串大小写转换
print("A String".lower())
print("A String".upper())

# 是字母或者数字
print("abc123".isalnum())

# 是字符
print("abc".isalpha())

# 是数字
print("abc".isdigit())
```

    I'll be ignored \n
    Hello You
    Length  9
    1st  H
    Last  u
    1st 3  Hel
    Every Other  HloY
    Goodbye You
    Goodbye you
    True
    False
    You Index  8
    Hello
    Some Words
    ['A', 'string']
    5 + 5 = 10
    a string
    A STRING
    True
    True
    False



```python
hello = "hello\nworld"
print(hello)
# 可以使用三个双引号给字符串赋值
triplequoted_hello = """hello
world"""
print(triplequoted_hello)
triplequoted_hello == hello
```

    hello
    world
    hello
    world





    True




```python
# ----- LIST -----
# 集合可以包含多种类型的数据，甚至是函数
l1 = [1, 3.14, "Derek", True]

# 集合长度
print("Length ", len(l1))

# 用索引取元素
print("1st", l1[0])
print("Last", l1[-1])

# 改变指定元素
l1[0] = 2

# 改变多个元素
l1[2:4] = ["Bob", False]

# 在某个索引位置插入元素，而且不删除元素
#  l1.insert(2, "Paul")，也可以
l1[2:2] = ["Paul", 9]

# 末尾追加元素
# l1.extend([5, 6])，作用相同
l2 = l1 + ["Egg", 4]

# 删除指定元素
l2.remove("Paul")

# 删除指定索引元素
l2.pop(0)
print("l2", l2)

# 往开头部分追加元素
# l1.append([5, 6])，也可以
l2 = ["Egg", 4] + l1

# 多维度的集合
l3 = [[1, 2], [3, 4]]
print("[1, 1]", l3[1][1])

# 元素是否存在于集合中
print("1 Exists", (1 in l1))

# 集合中的最大、最小值
print("Min ", min([1, 2, 3]))
print("Max ", max([1, 2, 3]))

# 获取集合的一部分
print(l1)
print("1st 2", l1[0:2])
print("Every Other ", l1[0:-1:2])
print("Reverse ", l1[::-1])
```

    Length  4
    1st 1
    Last True
    l2 [3.14, 9, 'Bob', False, 'Egg', 4]
    [1, 1] 4
    1 Exists False
    Min  1
    Max  3
    [2, 3.14, 'Paul', 9, 'Bob', False]
    1st 2 [2, 3.14]
    Every Other  [2, 'Paul', 'Bob']
    Reverse  [False, 'Bob', 9, 'Paul', 3.14, 2]



```python
# ----- LOOPS -----
# While : Execute while condition is True
w1 = 1
while w1 < 5:
    print(w1)
    w1 += 1

w2 = 0
while w2 <= 20:
    if w2 % 2 == 0:
        print(w2)
    elif w2 == 9:
        # Forces the loop to end all together
        break
    else:
        # Shorthand for i = i + 1
        w2 += 1
        # Skips to the next iteration of the loop
        continue
    w2 += 1

# Cycle through list
l4 = [1, 3.14, "Derek", True]
while len(l4):
    print(l4.pop(0))

# For Loop
# Allows you to perform an action a set number of times
# Range performs the action 10 times 0 - 9
# end="" eliminates newline
for x in range(0, 10):
    print(x, ' ', end="")
print('\n')
 
# Cycle through list
l4 = [1, 3.14, "Derek", True]
for x in l4:
    print(x)

# You can also define a list of numbers to
# cycle through
for x in [2, 4, 6]:
    print(x)

# You can double up for loops to cycle through lists
num_list = [[1, 2, 3], [10, 20, 30], [100, 200, 300]]
for x in num_list:
    print(x)
```

    1
    2
    3
    4
    0
    2
    4
    6
    8
    1
    3.14
    Derek
    True
    0  1  2  3  4  5  6  7  8  9  
    
    1
    3.14
    Derek
    True
    2
    4
    6
    [1, 2, 3]
    [10, 20, 30]
    [100, 200, 300]



```python
# ----- ITERATORS -----
# You can pass an object to iter() which returns
# an iterator which allows you to cycle
l5 = [6, 9, 12]
itr = iter(l5)
print(next(itr))  # Grab next value
```

    6



```python
# ----- RANGES -----
# The range() function creates integer iterables
print(list(range(0, 5)))
 
# You can define step
print(list(range(0, 10, 2)))
 
for x in range(0, 3):
    for y in range(0, 3):
        print(num_list[x][y])
```

    [0, 1, 2, 3, 4]
    [0, 2, 4, 6, 8]
    1
    2
    3
    10
    20
    30
    100
    200
    300



```python
x = 0.125
x.as_integer_ratio()
```




    (1, 8)




```python
numerator, denominator = x.as_integer_ratio()
print(numerator, denominator)
```

    1 8



```python
# ----- TUPLES -----
# Tuples与集合类似，但是，它们是不可变的，不能修改Tuples
# Tuples经常用于有多个返回值的函数比如as_integer_ratio函数
t1 = (1, 3.14, "Derek", False)
 
# Get length
print("Length ", len(t1))
 
# Get value / values
print("1st", t1[0])
print("Last", t1[-1])
print("1st 2", t1[0:2])
print("Every Other ", t1[0:-1:2])
print("Reverse ", t1[::-1])

 
# ----- DICTIONARIES -----
# Dictionaries 是键值对的集合
# 键和值可以是任何数据类型，可以混合存在
# 不允许重复的键
# 像List一样支持推导式（comprehensions）
heroes = {
    "Superman": "Clark Kent",
    "Batman": "Bruce Wayne"
}
 
villains = dict([
    ("Lex Luthor", "Lex Luthor"),
    ("Loki", "Loki")
])
 
print("Length", len(heroes))
 
# Get value by key
# Also heroes.get("Superman")
print(heroes["Superman"])
 
# 增加元素
heroes["Flash"] = "Barry Allan"
 
# 改变值
heroes["Flash"] = "Barry Allen"
 
# 获取键值组成的Tuple的集合
print(list(heroes.items()))
 
# 获取键、值的集合
print(list(heroes.keys()))
print(list(heroes.values()))
 
# 按键删除
del heroes["Flash"]
 
# 移除键并返回它的值
print(heroes.pop("Batman"))
 
# Search for key
print("Superman" in heroes)

print(heroes)

# 直接循环字典与直接循环字典的键一样
for k in heroes:
    print(k)
for k in heroes.keys():
    print(k)

# 循环字典的值
for v in heroes.values():
    print(v)

# Formatted print with dictionary mapping
d1 = {"name": "Bread", "price": .88}
print("%(name)s costs $%(price).2f" % d1)
```

    Length  4
    1st 1
    Last False
    1st 2 (1, 3.14)
    Every Other  (1, 'Derek')
    Reverse  (False, 'Derek', 3.14, 1)
    Length 2
    Clark Kent
    [('Superman', 'Clark Kent'), ('Batman', 'Bruce Wayne'), ('Flash', 'Barry Allen')]
    ['Superman', 'Batman', 'Flash']
    ['Clark Kent', 'Bruce Wayne', 'Barry Allen']
    Bruce Wayne
    True
    {'Superman': 'Clark Kent'}
    Superman
    Superman
    Clark Kent
    Bread costs $0.88



```python
keys = ['a','b','c']
values = [1,2,3]
dkv = dict(zip(keys, values))
print(dkv)
```

    {'a': 1, 'b': 2, 'c': 3}



```python
# ----- FUNCTIONS -----
# 使用函数可以重用、组织代码
# 可以通过函数注解定义函数的数据类型和默认值
# 求两个整数的和，整数的默认值是1
def get_sum(num1: int = 1, num2: int = 1):
    return num1 + num2
print(get_sum(5, 4))

# 接收可变（多个）参数
def get_sum2(*args):
    sum = 0
    for arg in args:
        sum += arg
    return sum
print(get_sum2(1, 2, 3, 4))

# 返回多个值
def next_2(num):
    return num + 1, num + 2
i1, i2 = next_2(5)
print(i1, i2)

# 函数可以返回一个函数
# 用给定的值乘以函数？其实相当于是两个参数
def mult_by(num):
    # 可以用lambda创建匿名函数
    return lambda x: x * num
print("3 * 5 =", (mult_by(3)(5)))

# 把函数作为参数
def mult_list(list, func):
    for x in list:
        print(func(x))
mult_by_4 = mult_by(4)
mult_list(list(range(0, 5)), mult_by_4)

# 创建一个函数组成的集合
power_list = [lambda x: x ** 2,
             lambda x: x ** 3,
             lambda x: x ** 4]
```

    9
    10
    6 7
    3 * 5 = 15
    0
    4
    8
    12
    16



```python
# 高阶函数（higher order functions）——对函数进行操作的函数

def mult_by_five(x):
    return 5 * x

def call(fn, arg):
    return fn(arg)

def squared_call(fn, arg):
    return fn(fn(arg))

print(
    call(mult_by_five, 1),
    squared_call(mult_by_five, 1), 
    sep='\n', 
)
```

    5
    25



```python
# ----- MAP -----
# 使用Map对集合执行一个函数
one_to_4 = range(1, 5)
times2 = lambda x: x * 2
print(list(map(times2, one_to_4)))
```

    [2, 4, 6, 8]



```python
# ----- FILTER -----
# Filter根据函数选择集合的元素
# Print out the even values from a list
print(list(filter((lambda x: x % 2 == 0), range(1, 11))))
```

    [2, 4, 6, 8, 10]



```python
from functools import reduce
# ----- REDUCE -----
# Reduce 接收一个集合返回一个结果
# Add up the values in a list
print(reduce((lambda x, y: x + y), range(1, 6)))
```

    15



```python
# ----- EXCEPTION HANDLING -----
# 可以处理可能导致程序异常的错误
# By giving the while a value of True it will
# cycle until a break is reached
while True:
 
    # If we expect an error can occur surround
    # potential error with try
    try:
        number = int(input("Please enter a number : "))
        break
 
    # The code in the except block provides
    # an error message to set things right
    # We can either target a specific error
    # like ValueError
    except ValueError:
        print("You didn't enter a number")
 
    # We can target all other errors with a
    # default
    except:
        print("An unknown error occurred")

print("Thank you for entering a number")
```

    Please enter a number : a
    You didn't enter a number
    Please enter a number : 33
    Thank you for entering a number



```python
# ----- FILE IO -----
# 可以保存和读取文件
# 使用 with 可以确保程序异常是关闭文件
# 模式 w 覆盖文件
# 模式 a 追加
with open("mydata.txt", mode="w", encoding="utf-8") \
        as myFile:
    # You can write to the file with write
    # It doesn't add a newline
    myFile.write("Some random text\nMore random text\nAnd some more")

# Open a file for reading
with open("mydata.txt", encoding="utf-8") as myFile:
    # Use read() to get everything at once
    print(myFile.read())

# Find out if the file is closed
print(myFile.closed)
```

    Some random text
    More random text
    And some more
    True



```python
# ----- CLASSES OBJECTS -----
# 对象
# attributes : height, weight
# capabilities : run, eat

# Classes是用来创建对象的模型
class Square:
    # init is used to set values for each Square
    def __init__(self, height="0", width="0"):
        self.height = height
        self.width = width
 
    # This is the getter
    # self is used to refer to an object that
    # we don't possess a name for
    @property
    def height(self):
        print("Retrieving the height")
 
        # Put a __ before this private field
        return self.__height
 
    # This is the setter
    @height.setter
    def height(self, value):
 
        # We protect the height from receiving
        # a bad value
        if value.isdigit():
 
            # Put a __ before this private field
            self.__height = value
        else:
            print("Please only enter numbers for height")
 
    # This is the getter
    @property
    def width(self):
        print("Retrieving the width")
        return self.__width
 
    # This is the setter
    @width.setter
    def width(self, value):
        if value.isdigit():
            self.__width = value
        else:
            print("Please only enter numbers for width")
 
    def get_area(self):
        return int(self.__width) * int(self.__height)

# Create a Square object
square = Square()
square.height = "10"
square.width = "10"
print("Area", square.get_area())
```

    Area 100



```python
# ----- INHERITANCE & POLYMORPHISM-----
# 继承和多态
# 当一个类继承了另一个类，它将得到另一个类的所有方法和属性，并且能够根据需要进行改变
class Animal:
    def __init__(self, name="unknown", weight=0):
        self.__name = name
        self.__weight = weight
 
    @property
    def name(self, name):
        self.__name = name
 
    def make_noise(self):
        return "Grrrrr"
 
    # Used to cast to a string type
    def __str__(self):
        return "{} is a {} and says {}".format (self.__name, type(self).__name__, self.make_noise())
 
    # Magic methods are used for operator
    # overloading
    # Here I'll define how to evaluate greater
    # than between 2 Animal objects
    def __gt__(self, animal2):
        if self.__weight > animal2.__weight:
            return True
        else:
            return False
    # Other Magic Methods
    # __eq__ : Equal
    # __ne__ : Not Equal
    # __lt__ : Less Than
    # __gt__ : Greater Than
    # __le__ : Less Than or Equal
    # __ge__ : Greater Than or Equal
    # __add__ : Addition
    # __sub__ : Subtraction
    # __mul__ : Multiplication
    # __div__ : Division
    # __mod__ : Modulus

# Dog 继承Animal
class Dog(Animal):
    def __init__(self, name="unknown", owner="unknown", weight=0):
        # 可以用父类的初始化方法
        Animal.__init__(self, name, weight)
        self.__owner = owner
 
    # Overwrite str
    def __str__(self):
        # 调用父类的方法
        return super().__str__() + " and is owned by " + \
        self.__owner

animal = Animal("Spot", 100)
print(animal)
 
dog = Dog("Bowser", "Bob", 150)
print(dog)
 
# Test the magic method
print(animal > dog)
 
# Polymorphism in Python works differently from
# other languages in that functions accept any
# object and expect that object to provide the
# needed method
 
# This isn't something to dwell on. Just know
# that if you call on a method for an object
# that the method just needs to exist for
# that object to work.
```

    Spot is a Animal and says Grrrrr
    Bowser is a Dog and says Grrrrr and is owned by Bob
    False



```python
import threading
import time
import random
# ----- THREADS -----
# 线程是轮流执行的代码块
def execute_thread(i):
    # strftime（字符串格式时间工具） 可以用来格式化时间字符串
    # You could include the date with
    # strftime("%Y-%m-%d %H:%M:%S", gmtime())
 
    # Print when the thread went to sleep
    print("Thread {} sleeps at {}".format(i,
                                          time.strftime("%H:%M:%S", time.gmtime())))
 
    # Generate a random sleep period of between 1 and
    # 5 seconds
    rand_sleep_time = random.randint(1, 5)
 
    # Pauses execution of code in this function for
    # a few seconds
    time.sleep(rand_sleep_time)
 
    # Print out info after the sleep time
    print("Thread {} stops sleeping at {}".format(i,
                                                  time.strftime("%H:%M:%S", time.gmtime())))

for i in range(10):
    # Each time through the loop a Thread object is created
    # You pass it the function to execute and any
    # arguments to pass to that method
    # The arguments passed must be a sequence which
    # is why we need the comma with 1 argument
    thread = threading.Thread(target=execute_thread, args=(i,))
    thread.start()
 
    # 输出活跃的线程数
    # 多出的一个线程是main线程中的for循环
    print("\nActive Threads :", threading.activeCount())
 
    # 返回所有的活跃线程对象
    print("Thread Objects :", threading.enumerate())
```

    Thread 0 sleeps at 09:39:36
    Active Threads : 6
    
    Thread Objects : [<_MainThread(MainThread, started 1468)>, <Thread(Thread-4, started daemon 4172)>, <Heartbeat(Thread-5, started daemon 43912)>, <HistorySavingThread(IPythonHistorySavingThread, started 42668)>, <ParentPollerWindows(Thread-3, started daemon 2240)>, <Thread(Thread-36, started 33376)>]
    Thread 1 sleeps at 09:39:36
    Active Threads :
     7
    Thread Objects : [<_MainThread(MainThread, started 1468)>, <Thread(Thread-4, started daemon 4172)>, <Heartbeat(Thread-5, started daemon 43912)>, <HistorySavingThread(IPythonHistorySavingThread, started 42668)>, <ParentPollerWindows(Thread-3, started daemon 2240)>, <Thread(Thread-36, started 33376)>, <Thread(Thread-37, started 1340)>]
    Thread 2 sleeps at 09:39:36
    Active Threads : 8
    Thread Objects : [<_MainThread(MainThread, started 1468)>, <Thread(Thread-4, started daemon 4172)>, <Heartbeat(Thread-5, started daemon 43912)>, <HistorySavingThread(IPythonHistorySavingThread, started 42668)>, <ParentPollerWindows(Thread-3, started daemon 2240)>, <Thread(Thread-36, started 33376)>, <Thread(Thread-37, started 1340)>, <Thread(Thread-38, started 37048)>]
    
    Thread 3 sleeps at 09:39:37
    
    Active Threads : 9
    Thread Objects : [<_MainThread(MainThread, started 1468)>, <Thread(Thread-4, started daemon 4172)>, <Heartbeat(Thread-5, started daemon 43912)>, <HistorySavingThread(IPythonHistorySavingThread, started 42668)>, <ParentPollerWindows(Thread-3, started daemon 2240)>, <Thread(Thread-36, started 33376)>, <Thread(Thread-37, started 1340)>, <Thread(Thread-38, started 37048)>, <Thread(Thread-39, started 43424)>]
    Thread 4 sleeps at 09:39:37
    Active Threads : 10
    Thread Objects : [<_MainThread(MainThread, started 1468)>, <Thread(Thread-4, started daemon 4172)>, <Heartbeat(Thread-5, started daemon 43912)>, <HistorySavingThread(IPythonHistorySavingThread, started 42668)>, <ParentPollerWindows(Thread-3, started daemon 2240)>, <Thread(Thread-36, started 33376)>, <Thread(Thread-37, started 1340)>, <Thread(Thread-38, started 37048)>, <Thread(Thread-39, started 43424)>, <Thread(Thread-40, started 43476)>]
    
    Thread 5 sleeps at 09:39:37
    Active Threads : 11
    Thread Objects : [<_MainThread(MainThread, started 1468)>, <Thread(Thread-4, started daemon 4172)>, <Heartbeat(Thread-5, started daemon 43912)>, <HistorySavingThread(IPythonHistorySavingThread, started 42668)>, <ParentPollerWindows(Thread-3, started daemon 2240)>, <Thread(Thread-36, started 33376)>, <Thread(Thread-37, started 1340)>, <Thread(Thread-38, started 37048)>, <Thread(Thread-39, started 43424)>, <Thread(Thread-40, started 43476)>, <Thread(Thread-41, started 41444)>]
    
    Thread 6 sleeps at 09:39:37
    Active Threads : 12
    Thread Objects : [<_MainThread(MainThread, started 1468)>, <Thread(Thread-4, started daemon 4172)>, <Heartbeat(Thread-5, started daemon 43912)>, <HistorySavingThread(IPythonHistorySavingThread, started 42668)>, <ParentPollerWindows(Thread-3, started daemon 2240)>, <Thread(Thread-36, started 33376)>, <Thread(Thread-37, started 1340)>, <Thread(Thread-38, started 37048)>, <Thread(Thread-39, started 43424)>, <Thread(Thread-40, started 43476)>, <Thread(Thread-41, started 41444)>, <Thread(Thread-42, started 41964)>]
    Thread 7 sleeps at 09:39:37



    Active Threads : 13
    Thread Objects : [<_MainThread(MainThread, started 1468)>, <Thread(Thread-4, started daemon 4172)>, <Heartbeat(Thread-5, started daemon 43912)>, <HistorySavingThread(IPythonHistorySavingThread, started 42668)>, <ParentPollerWindows(Thread-3, started daemon 2240)>, <Thread(Thread-36, started 33376)>, <Thread(Thread-37, started 1340)>, <Thread(Thread-38, started 37048)>, <Thread(Thread-39, started 43424)>, <Thread(Thread-40, started 43476)>, <Thread(Thread-41, started 41444)>, <Thread(Thread-42, started 41964)>, <Thread(Thread-43, started 33832)>]
    Thread 8 sleeps at 09:39:37
    
    Active Threads : 14
    Thread Objects : [<_MainThread(MainThread, started 1468)>, <Thread(Thread-4, started daemon 4172)>, <Heartbeat(Thread-5, started daemon 43912)>, <HistorySavingThread(IPythonHistorySavingThread, started 42668)>, <ParentPollerWindows(Thread-3, started daemon 2240)>, <Thread(Thread-36, started 33376)>, <Thread(Thread-37, started 1340)>, <Thread(Thread-38, started 37048)>, <Thread(Thread-39, started 43424)>, <Thread(Thread-40, started 43476)>, <Thread(Thread-41, started 41444)>, <Thread(Thread-42, started 41964)>, <Thread(Thread-43, started 33832)>, <Thread(Thread-44, started 42392)>]
    Thread 9 sleeps at 09:39:37
    
    Active Threads : 15
    Thread Objects : [<_MainThread(MainThread, started 1468)>, <Thread(Thread-4, started daemon 4172)>, <Heartbeat(Thread-5, started daemon 43912)>, <HistorySavingThread(IPythonHistorySavingThread, started 42668)>, <ParentPollerWindows(Thread-3, started daemon 2240)>, <Thread(Thread-36, started 33376)>, <Thread(Thread-37, started 1340)>, <Thread(Thread-38, started 37048)>, <Thread(Thread-39, started 43424)>, <Thread(Thread-40, started 43476)>, <Thread(Thread-41, started 41444)>, <Thread(Thread-42, started 41964)>, <Thread(Thread-43, started 33832)>, <Thread(Thread-44, started 42392)>, <Thread(Thread-45, started 23592)>]
    Thread 3 stops sleeping at 09:39:38
    Thread 6 stops sleeping at 09:39:38
    Thread 9 stops sleeping at 09:39:38
    Thread 2 stops sleeping at 09:39:39
    Thread 4 stops sleeping at 09:39:39
    Thread 7 stops sleeping at 09:39:39
    Thread 1 stops sleeping at 09:39:40
    Thread 5 stops sleeping at 09:39:41
    Thread 0 stops sleeping at 09:39:41
    Thread 8 stops sleeping at 09:39:42



```python
# ----- 正则表达式 -----
# Regular expressions allow you to locate and change
# strings in very powerful ways.
# They work in almost exactly the same way in every
# programming language as well.
 
# Regular Expressions (Regex) are used to
# 1. Search for a specific string in a large amount of data
# 2. Verify that a string has the proper format (Email, Phone #)
# 3. Find a string and replace it with another string
# 4. Format data into the proper form for importing for example
 
# import the Regex module
import re
 
# ---------- Was a Match Found ----------
 
# Search for ape in the string
if re.search("ape", "The ape was at the apex"):
    print("There is an ape")

# ---------- Get All Matches ----------
 
# findall() returns a list of matches
# . is used to match any 1 character or space
allApes = re.findall("ape.", "The ape was at the apex")
 
for i in allApes:
    print(i)

# finditer returns an iterator of matching objects
# You can use span to get the location
 
theStr = "The ape was at the apex"
 
for i in re.finditer("ape.", theStr):
 
    # Span 返回匹配起止位置组成的元组
    locTuple = i.span()
 
    print(locTuple)
 
    # Slice the match out using the tuple values
    print(theStr[locTuple[0]:locTuple[1]])
```

    There is an ape
    ape 
    apex
    (4, 8)
    ape 
    (19, 23)
    apex



```python
# ----- 在Python中使用SQLLite3 -----
 
# A database makes it easy for you to organize your
# data for storage and fast searching
import sqlite3
import sys
import csv
def printDB():
    # To retrieve data from a table use SELECT followed
    # by the items to retrieve and the table to
    # retrieve from
 
    try:
        result = theCursor.execute("SELECT id, FName, LName, Age, Address, Salary, HireDate FROM Employees")
 
        # You receive a list of lists that hold the result
        for row in result:
            print("ID :", row[0])
            print("FName :", row[1])
            print("LName :", row[2])
            print("Age :", row[3])
            print("Address :", row[4])
            print("Salary :", row[5])
            print("HireDate :", row[6])
 
    except sqlite3.OperationalError:
        print("The Table Doesn't Exist")
 
    except:
        print("Couldn't Retrieve Data From Database")

# ---------- END OF FUNCTIONS ----------

# connect()方法会打开一个SQLite database连接, 如果指定database不存在，就会创建指定database
# 数据库文件和本Python文件在同一个目录下
# 难道已经集成了SQLite工具？
db_conn = sqlite3.connect('test.db')
 
print("Database Created")
 
# A cursor is used to traverse the records of a result
theCursor = db_conn.cursor()
 
# execute() executes a SQL command
# We organize our data in tables by defining their
# name and the data type for the data
 
# We define the table name
# A primary key is a unique value that differentiates
# each row of data in our table
# The primary key will auto increment each time we
# add a new Employee
# If a piece of data is marked as NOT NULL, that means
# it must have a value to be valid
 
# NULL is NULL and stands in for no value
# INTEGER is an integer
# TEXT is a string of variable length
# REAL is a float
# BLOB is used to store binary data
 
# You can delete a table if it exists like this
# db_conn.execute("DROP TABLE IF EXISTS Employees")
# db_conn.commit()

try:
    db_conn.execute("CREATE TABLE Employees(ID INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, FName TEXT NOT NULL, LName TEXT NOT NULL, Age INT NOT NULL, Address TEXT, Salary REAL, HireDate TEXT);")
 
    db_conn.commit()
 
    print("Table Created")

except sqlite3.OperationalError:
    print("Table couldn't be Created")
 
 
# To insert data into a table we use INSERT INTO
# followed by the table name and the item name
# and the data to assign to those items
 
db_conn.execute("INSERT INTO Employees (FName, LName, Age, Address, Salary, HireDate)"
                "VALUES ('Derek', 'Banas', 41, '123 Main St', '500,000', date('now'))")
 
db_conn.commit()
 
print("Employee Entered")
 
# Print out all the data in the database
printDB()
 
# Closes the database connection
db_conn.close()
```

    Database Created
    Table couldn't be Created
    Employee Entered
    ID : 1
    FName : Derek
    LName : Banas
    Age : 41
    Address : 123 Main St
    Salary : 500,000
    HireDate : 2020-03-23
    ID : 2
    FName : Derek
    LName : Banas
    Age : 41
    Address : 123 Main St
    Salary : 500,000
    HireDate : 2020-03-23



```python
# ----- IMPORT ------
'''
1、import module_name——本质是将"module_name.py"中的全部代码加载到内存，并将模块赋值给当前文件中的同名变量
2、from module_name import name——本质是导入指定的引用（变量或方法等）到当前文件
3、import package_name——导入包的本质就是执行该包下的__init__.py文件
如果导入的资源比较少，第2中方式是比较好的，它减少了查找的过程。
'''
'''
import math
mt = math

与

import math as mt

等价
'''
```


```python
import math
# 导入的模块的类型是`module`

print("It's math! It has type {}".format(type(math)))
```

    It's math! It has type <class 'module'>


函数的参数：

位置参数（positional arguments）——所有参数的顺序必须一一对应，且数量一致

关键字参数——通过键值对的形式指定，关键字参数不分前后顺序；存在位置参数时，位置参数必须位于关键字参数前面。

默认参数——为参数提供默认值，调用函数时可以不传。所有位置参数必须放在默认参数前面。

**\*、\*\*用在参数前面**：

1）\*可以用在动态参数前，打包多个参数并将其转化为**元组**；也可以用在**可迭代对象前进行自动化解包**——转换为多个单变量参数。

2）\*\*可以用在动态参数前，打包多个赋值形式的参数并将其转化为**字典**；也可以用在字典参数前，解包字典中的数据项作为键值参数传给函数。


```python
def func1(*args):
    print(args)

func1(1,2,3)
```

    (1, 2, 3)



```python
def func2(a, b, c):
    print(a,b,c)

args2 = [1,2,3]
func2(*args2)
```

    1 2 3



```python
def func3(**kwargs):
    print(kwargs)

func3(a=1,b=2,c=3)
```

    {'a': 1, 'b': 2, 'c': 3}



```python
kwargs = {'a':1,'b':2,'c':3}
func2(**kwargs)
```

    1 2 3


**函数装饰器（decorator）:@**

作用：为现有的函数增加额外的功能，常用于插入日志、性能测试、事务处理等等。

使用修饰符，会改变被修饰函数的函数名属性；可以结合`functools.wraps`使用。



```python
# 被修饰函数不带参数
def log(func):
    def warpper():
        print('log start...')
        func()
        print('log end...')
    return warpper

# 使用装饰器，会改变被修饰函数的函数名属性
@log
def test():
    print('test....')
    
test()
```

    log start...
    test....
    log end...



```python
def test1():
    print('test1...')

print(test.__name__)
print(test1.__name__)
```

    warpper
    test1



```python
from functools import wraps,update_wrapper
# 使用functools的wraps函数：将修饰函数的属性设置为参数函数的属性
def log1(func):
    @wraps(func)
    def wrapper():
        print('log start...')
        func()
        print('log end...')
    return wrapper

@log1
def test2():
    print('test2...')

test()
print(test2.__name__)
```

    log start...
    test....
    log end...
    test2



```python
# 被修饰函数带参数
def log3(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print('log开始...',func.__name__)
        ret = func(*args, **kwargs)
        print('log结束...')
        return ret
    return wrapper

@log3
def test3(s):
    print('test3...', s)
    return s

@log3
def test4(s1, s2):
    print('test4...', s1, s2)
    return s1 + s2

print(test3('a'))
print(test4('a', 'bc'))
```

    log开始... test3
    test3... a
    log结束...
    a
    log开始... test4
    test4... a bc
    log结束...
    abc



```python
# 装饰器带参数
def log4(arg):
    def _log(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            print('log start...', func.__name__,arg)
            ret = func(*args, **kwargs)
            print('log结束')
            return ret
        return wrapper
    return _log

@log4('module1')
def test5(s):
    print('test5...', s)
    return s

@log4('module1')
def test6(s1, s2):
    print('test6...', s1, s2)
    return s1 + s2

print(test5('a'))
print(test6('a', 'bc'))
```

    log start... test5 module1
    test5... a
    log结束
    a
    log start... test6 module1
    test6... a bc
    log结束
    abc



```python
# ——- myfunc.py ——-
 
# ---------- RECURSIVE FUNCTIONS ----------
# 递归函数
 
# 计算阶乘（factorials）通常使用递归
# function 3! = 3 * 2 * 1
def factorial(num):
    # Every recursive function must contain a condition
    # when it ceases to call itself
    if num <= 1:
        return 1
    else:
         result = num * factorial(num - 1)
    return result

# 1st : result = 4 * factorial(3) = 4 * 6 = 24
# 2nd : result = 3 * factorial(2) = 3 * 2 = 6
# 3rd : result = 2 * factorial(1) = 2 * 1 = 2
 
# ——— MODULES ———
import myfunc
print(myfunc.factorial(4))
 
# OR
 
from myfunc import factorial
print(factorial(4))
```


```python
# ——— 用TKINTER进行GUI开发 ———
 
from tkinter import *
from tkinter import ttk
 
class Calculator:
 
    # Stores the current value to display in the entry
    calc_value = 0.0
 
    # Will define if this was the last math button clicked
    div_trigger = False
    mult_trigger = False
    add_trigger = False
    sub_trigger = False
 
    # Called anytime a number button is pressed
    def button_press(self, value):
 
        # Get the current value in the entry
        entry_val = self.number_entry.get()
 
        # Put the new value to the right of it
        # If it was 1 and 2 is pressed it is now 12
        # Otherwise the new number goes on the left
        entry_val += value
 
        # Clear the entry box
        self.number_entry.delete(0, "end")
 
        # Insert the new value going from left to right
        self.number_entry.insert(0, entry_val)
 
    # Returns True or False if the string is a float
    def isfloat(self, str_val):
        try:
 
            # If the string isn't a float float() will throw a
            # ValueError
            float(str_val)
 
            # If there is a value you want to return use return
            return True
        except ValueError:
            return False
 
    # Handles logic when math buttons are pressed
    def math_button_press(self, value):
 
        # Only do anything if entry currently contains a number
        if self.isfloat(str(self.number_entry.get())):
 
            # make false to cancel out previous math button click
            self.add_trigger = False
            self.sub_trigger = False
            self.mult_trigger = False
            self.div_trigger = False
 
            # Get the value out of the entry box for the calculation
            self.calc_value = float(self.entry_value.get())
 
            # Set the math button click so when equals is clicked
            # that function knows what calculation to use
            if value == "/":
                print("/ Pressed")
                self.div_trigger = True
            elif value == "*":
                print("* Pressed")
                self.mult_trigger = True
            elif value == "+":
                print("+ Pressed")
                self.add_trigger = True
            else:
                print("- Pressed")
                self.sub_trigger = True
 
            # Clear the entry box
            self.number_entry.delete(0, "end")
 
    # Performs a mathematical operation by taking the value before
    # the math button is clicked and the current value. Then perform
    # the right calculation by checking what math button was clicked
    # last
    def equal_button_press(self):
 
        # Make sure a math button was clicked
        if self.add_trigger or self.sub_trigger or self.mult_trigger or self.div_trigger:
 
            if self.add_trigger:
                solution = self.calc_value + float(self.entry_value.get())
            elif self.sub_trigger:
                solution = self.calc_value - float(self.entry_value.get())
            elif self.mult_trigger:
                solution = self.calc_value * float(self.entry_value.get())
            else:
                solution = self.calc_value / float(self.entry_value.get())
 
            print(self.calc_value, " ", float(self.entry_value.get()),
                                            " ", solution)
 
            # Clear the entry box
            self.number_entry.delete(0, "end")
 
            self.number_entry.insert(0, solution)
 
 
    def __init__(self, root):
        # Will hold the changing value stored in the entry
        self.entry_value = StringVar(root, value="")
 
        # Define title for the app
        root.title("Calculator")
 
        # Defines the width and height of the window
        root.geometry("430x220")
 
        # Block resizing of Window
        root.resizable(width=False, height=False)
 
        # Customize the styling for the buttons and entry
        style = ttk.Style()
        style.configure("TButton",
                        font="Serif 15",
                        padding=10)
 
        style.configure("TEntry",
                        font="Serif 18",
                        padding=10)
 
        # Create the text entry box
        self.number_entry = ttk.Entry(root,
                        textvariable=self.entry_value, width=50)
        self.number_entry.grid(row=0, columnspan=4)
 
        # ----- 1st Row -----
 
        self.button7 = ttk.Button(root, text="7", command=lambda: self.button_press('7')).grid(row=1, column=0)
 
        self.button8 = ttk.Button(root, text="8", command=lambda: self.button_press('8')).grid(row=1, column=1)
 
        self.button9 = ttk.Button(root, text="9", command=lambda: self.button_press('9')).grid(row=1, column=2)
 
        self.button_div = ttk.Button(root, text="/", command=lambda: self.math_button_press('/')).grid(row=1, column=3)
 
        # ----- 2nd Row -----
 
        self.button4 = ttk.Button(root, text="4", command=lambda: self.button_press('4')).grid(row=2, column=0)
 
        self.button5 = ttk.Button(root, text="5", command=lambda: self.button_press('5')).grid(row=2, column=1)
 
        self.button6 = ttk.Button(root, text="6", command=lambda: self.button_press('6')).grid(row=2, column=2)
 
        self.button_mult = ttk.Button(root, text="*", command=lambda: self.math_button_press('*')).grid(row=2, column=3)
 
        # ----- 3rd Row -----
 
        self.button1 = ttk.Button(root, text="1", command=lambda: self.button_press('1')).grid(row=3, column=0)
 
        self.button2 = ttk.Button(root, text="2", command=lambda: self.button_press('2')).grid(row=3, column=1)
 
        self.button3 = ttk.Button(root, text="3", command=lambda: self.button_press('3')).grid(row=3, column=2)
 
        self.button_add = ttk.Button(root, text="+", command=lambda: self.math_button_press('+')).grid(row=3, column=3)
 
        # ----- 4th Row -----
 
        self.button_clear = ttk.Button(root, text="AC", command=lambda: self.button_press('AC')).grid(row=4, column=0)
 
        self.button0 = ttk.Button(root, text="0", command=lambda: self.button_press('0')).grid(row=4, column=1)
 
        self.button_equal = ttk.Button(root, text="=", command=lambda: self.equal_button_press()).grid(row=4, column=2)
 
        self.button_sub = ttk.Button(root, text="-", command=lambda: self.math_button_press('-')).grid(row=4, column=3)

# Get the root window object
root = Tk()
 
# Create the calculator
calc = Calculator(root)
 
# Run the app until exited 
root.mainloop()
```