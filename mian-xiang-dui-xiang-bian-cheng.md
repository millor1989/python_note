### 面向对象编程（Object Oriented Programming）

面向对象编程（OOP）是一种把对象作为程序的基本单元的程序设计思想。对象包含了数据和操作数据的函数。

**面向过程的程序设计**把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度。

面向对象的程序设计把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递。

在Python中，**所有数据类型都可以视为对象**，也可以自定义对象。自定义的对象数据类型就是面向对象中的**类（Class）**。

如果要表示学生成绩，面向过程的程序可以用dict表示：

```python
std1 = { 'name': 'Michael', 'score': 98 }
std2 = { 'name': 'Bob', 'score': 81 }
```

输出学生成绩：

```python
def print_score(std):
    print('%s: %s' % (std['name'], std['score']))
```

而面向对象的程序，首先要考虑的是学生这个对象——`Student`这种数据类型，这个对象有`name`和`score`两个**属性（Property）**。要输出学生的成绩，`Student`还要有`print_score`函数，称为对象的**方法（Method）**：

```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))
```

对象的创建和对象输出如下：

```python
>>> bart = Student('Bart Simpson', 59)
>>> lisa = Student('Lisa Simpson', 87)
>>> bart.print_score()
Bart Simpson: 59

>>> lisa.print_score()
Lisa Simpson: 87
```

类（Class）是一种抽象概念，实例（Instance）则是一个个具体的对象。

面向对象的设计思想是抽象出Class，根据Class创建Instance。

面向对象的抽象程度比函数要高，因为一个Class既包含数据，又包含操作数据的方法。