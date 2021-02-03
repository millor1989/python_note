### 文件读写

读写文件是最常见的IO操作。Python内置了文件读写函数（用法与C兼容）。

读写磁盘文件的功能都是操作系统提供的，操作系统不允许普通程序直接操作磁盘，所有读写文件其实是请求操作系统打开一个文件对象（通常称为文件描述符），然后，通过操作系统提供的接口从这个文件对象中读取数据（读文件）或者把数据写入这个文件对象（写文件）。

#### 1、读文件

使用Python内置的**`open()`函数**，以读文件的模式（`r`）打开文件：

```python
>>> f = open('/Users/michael/test.txt', 'r')
```

如果文件不存在，`open()`函数就会抛出一个`IOError`的错误。

```python
>>> f=open('/Users/michael/notfound.txt', 'r')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
FileNotFoundError: [Errno 2] No such file or directory: '/Users/michael/notfound.txt'
```

如果文件打开成功，可以用`read()`方法一次读取文件的全部内容。Python把内容读到内存，用一个`str`对象表示：

```python
>>> f.read()
'Hello, world!'
```

`read()`方法是一次性读取文件全部内容，但是文件太大时，可能挤爆内存。因此，为了安全可以使用`read(size)`方法读取最多`size`字节内容，或者使用`readline()`方法一次读取一行内容（`readlines()`方法一次性读取所有内容并按行返回一个`list`）。

```python
for line in f.readlines():
    print(line.strip()) # 把末尾的'\n'删掉
```

总之，使用时，要根据文件具体大小选择使用的方法。

最后调用`close()`方法**关闭文件**。文件使用完毕后**必须关闭**，文件对象会占用操作系统的资源，而操作系统同一时间能打开的文件数量也是有限的：

```python
>>> f.close()
```

文件读写都有可能产生`IOError`，为了保证总是能关闭文件对象，可以使用`try...finally...`来实现：

```python
try:
    f = open('/path/to/file', 'r')
    print(f.read())
finally:
    if f:
        f.close()
```

Python中有**`with`语句**来简化`close()`方法的调用：

```python
with open('/path/to/file', 'r') as f:
    print(f.read())
```

##### 1.1 file-like Object

有`read()`方法的对象，在Python中统称为file-like Object。内存中的字节流、网络流、自定义流等等，都是file-like Object。file-like Object不需要从特定类继承，只要写一个`read()`方法就可以。

`StringIO`就是内存中创建的file-like Object，常用作缓冲。

##### 1.2、二进制文件

读取二进制文件（图片、视频等等）要用`rb`模式打开文件：

```python
>>> f = open('/Users/michael/test.jpg', 'rb')
>>> f.read()
b'\xff\xd8\xff\xe1\x00\x18Exif\x00\x00...' # 十六进制表示的字节
```

##### 1.3、字符编码

读取GBK编码的文件，要传入`encoding`参数（默认`utf-8`编码）：

```python
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')
>>> f.read()
'测试'
```

对于编码不规范的文件，文本文件中可能夹杂了一些非法编码的字符，读取是会遇到`UnicodeDecodeError`。`open()`函数提供了`errors`参数，表示如果遇到编码错误后如何处理。最简单的方式是直接忽略：

```
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')
```

#### 2、写文件

写文件也使用`open()`函数，传入的模式标识符为`w`（写文本文件）或`wb`（写二进制文件）：

```python
>>> f = open('/Users/michael/test.txt', 'w')
>>> f.write('Hello, world!')
>>> f.close()
```

可以反复调用`write()`来写入文件，最后必需调用`f.close()`来关闭文件。写文件时，操作系统往往不会立刻把数据写入磁盘，而是先把数据放到内存**缓存**起来，只有调用`close()`方法时，操作系统才保证把没有写入的数据全部写入磁盘。为了数据安全，最好还是使用`with`语句：

```python
with open('/Users/michael/test.txt', 'w') as f:
    f.write('Hello, world!')
```

使用`encoding`参数指定编码。

在`w`模式写文件时，如果文件存在会被覆盖（删除重建）。如果要在文件存在时以追加模式写入，使用`a`模式。

