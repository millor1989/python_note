### struct

准确地讲，Python并没有专门处理字节的数据类型。但是，`b'str'`可以表示字节，即字节数组=二进制`str`。而C语言中，可以很方便地使用struct、union来处理字节，以及字节和int、float的转换。

在Python中，如果要把一个32为无符号整数转换为字节（即4字节长度的`bytes`），那么直接处理，需要：

```python
>>> n = 10240099
>>> b1 = (n & 0xff000000) >> 24
>>> b2 = (n & 0xff0000) >> 16
>>> b3 = (n & 0xff00) >> 8
>>> b4 = n & 0xff
>>> bs = bytes([b1, b2, b3, b4])
>>> bs
b'\x00\x9c@c
```

很麻烦。

Python提供了一个`struct`模块来解决`bytes`和其他二进制数据类型的转换。

`struct`的`pack`函数可以把任意数据类型转换为`bytes`：

```python
>>> import struct
>>> struct.pack('>I', 10240099)
b'\x00\x9c@c'
```

`pack`函数的第一个参数是处理指令，`>I`的意思是：`>`表示字节顺序是big-endian，即网络序，`I`表示4字节无符号整数。

`unpack`函数可以将`bytes`变成相应的数据类型：

```python
>>> struct.unpack('>IH', b'\xf0\xf0\xf0\xf0\x80\x80')
(4042322160, 32896)
```

根据`>IH`的说明，后面的`bytes`依次变为`I`：4字节无符号整数和`H`：2字节无符号整数。

尽管Python不适合编写底层操作字节流的代码，但在对性能要求不高的地方，利用`struct`就方便多了。