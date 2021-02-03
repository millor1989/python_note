### StringIO和BytesIO

数据读写也可以在内存中进行，`StringIO`和`BytesIO`就是内存读写数据的工具。

#### 1、StringIO

`StringIO`用于在内存中读写`str`。

将`str`写入`StringIO`：

```python
>>> from io import StringIO
>>> f = StringIO()
>>> f.write('hello')
5
>>> f.write(' ')
1
>>> f.write('world!')
6
>>> print(f.getvalue())
hello world!
```

`getvalue()`方法用于获取写入后的`str`。

从`StringIO`读取`str`：

```python
>>> from io import StringIO
>>> f = StringIO('Hello!\nHi!\nGoodbye!')
>>> while True:
...     s = f.readline()
...     if s == '':
...         break
...     print(s.strip())
...
Hello!
Hi!
Goodbye!
```

#### 2、BytesIO

`BytesIO`用于在内存中读写二进制数据。

在`BytesIO`中写入`bytes`：

```python
>>> from io import BytesIO
>>> f = BytesIO()
>>> f.write('中文'.encode('utf-8'))
6
>>> print(f.getvalue())
b'\xe4\xb8\xad\xe6\x96\x87'
```

读取`bytes`：

```python
>>> from io import BytesIO
>>> f = BytesIO(b'\xe4\xb8\xad\xe6\x96\x87')
>>> f.read()
b'\xe4\xb8\xad\xe6\x96\x87'
```

