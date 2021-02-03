### 使用SQLite

SQLite是一个嵌入式的数据库，由C语言编写，体积很小，可以集成到各种应用程序或者移动设备应用中。它的数据库就是一个文件。

Python内置了SQLite3，Python定义了一套操作数据库接口的API，任何数据库只要提供符合Python标准的数据库驱动，就可以通过Python连接访问。

Python操作关系型数据库，首先要创建到数据的连接`Connection`;连接到数据库之后，需要打开游标`Cursor`，通过`Cursor`执行SQL语句，然后获得执行结果。

Python操作SQLite，创建数据库、创建表、插入数据：

```python
# 导入SQLite驱动:
>>> import sqlite3
# 连接到SQLite数据库
# 数据库文件是test.db
# 如果文件不存在，会自动在当前目录创建:
>>> conn = sqlite3.connect('test.db')
# 创建一个Cursor:
>>> cursor = conn.cursor()
# 执行一条SQL语句，创建user表:
>>> cursor.execute('create table user (id varchar(20) primary key, name varchar(20))')
<sqlite3.Cursor object at 0x10f8aa260>
# 继续执行一条SQL语句，插入一条记录:
>>> cursor.execute('insert into user (id, name) values (\'1\', \'Michael\')')
<sqlite3.Cursor object at 0x10f8aa260>
# 通过rowcount获得插入的行数:
>>> cursor.rowcount
1
# 关闭Cursor:
>>> cursor.close()
# 提交事务:
>>> conn.commit()
# 关闭Connection:
>>> conn.close()
```

查询数据：

```python
>>> conn = sqlite3.connect('test.db')
>>> cursor = conn.cursor()
# 执行查询语句:
>>> cursor.execute('select * from user where id=?', ('1',))
<sqlite3.Cursor object at 0x10f8aa340>
# 获得查询结果集:
>>> values = cursor.fetchall()
>>> values
[('1', 'Michael')]
>>> cursor.close()
>>> conn.close()
```

使用Python的DB-API时，`Connection`和`Cursor`对象打开后一定要记得关闭。

`Cursor`对象执行`insert`、`update`、`delete`操作时，通过`rowcount`返回操作影响的行数，即执行结果。

`Cursor`对象执行`Select`语句时，通过`fetchall()`可以拿到所有的结果集，结果集是`list`，每个元素都是一个`tuple`——对应一行记录。

SQL语句带有参数时，需要把参数按照位置传递给`execute()`方法，占位符`?`的个数对应参数的个数，比如：

```python
cursor.execute('select * from user where name=? and pwd=?', ('abc', 'password'))
```

#### 1、关于SQLite

SQLite在数据类型方面非常灵活，是弹性类型的（flexibly typed），相对于其它刚性类型（rigidly typed）数据库。

SQLite也有数据类型，但是它使用的是动态类型系统（dynamic type system）。创建表的时候，可以不指定数据类型；即使指定了数据类型，也可以保存其它数据类型的数据。

SQLite**不支持并发写入操作**。