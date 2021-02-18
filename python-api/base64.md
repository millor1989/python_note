### base64

Base64是一种用64个字符来表示任意二进制数据的方法。

当用记事本打开`ext`、`jpg`、`pdf`这些文件时，会看到一大堆乱码，这是因为二进制文件包含很多无法显示和打印的字符。所以，如果要让记事本这样的文本处理软件能处理二进制数据，就需要一个二进制到字符串的转换方法。Base64是一种最常见的二进制编码方法。

Base64的原理是，用64个字符来表示二进制数据，对二进制数据每3个字节一组，即24个bit一组，划分为4段，每段6个bit，每6个bit用一个字符串表示。Base64编码把3字节的二进制数据编码为4字节的文本数据，**长度增加33%，好处是编码后的文本数据可以在邮件正文、网页等直接显示**。

Python内置了`base64`模块进行base64的编解码：

```python
>>> import base64
>>> base64.b64encode(b'binary\x00string')
b'YmluYXJ5AHN0cmluZw=='
>>> base64.b64decode(b'YmluYXJ5AHN0cmluZw==')
b'binary\x00string'
```

标准的Base64编码字符包含了`+`和`/`，而这两个字符在URL中不能直接作为参数，所以又有一种“url safe”的base64编码，将字符串`+`和`/`分别替换为了`-`和`_`：

```python
>>> base64.b64encode(b'i\xb7\x1d\xfb\xef\xff')
b'abcd++//'
>>> base64.urlsafe_b64encode(b'i\xb7\x1d\xfb\xef\xff')
b'abcd--__'
>>> base64.urlsafe_b64decode('abcd--__')
b'i\xb7\x1d\xfb\xef\xff'
```

**可以使用自定义的字符**进行Base64编码，但是通常没有必要。Base64是一种通过查表进行编码的编码方法，**不能用于加密**，即使是使用自定义的编码表也不行。

Base64适用于小段内容的编码，比如数字证书签名、Cookie内容等。

`=`字符也可能出现在Base64编码中，但`=`用在URL、Cookie里面会造成歧义，所以，很多Base64编码后会把`=`去掉：

```python
# 标准Base64:
'abcd' -> 'YWJjZA=='
# 自动去掉=:
'abcd' -> 'YWJjZA'
```

因为Base64是把3个字节变为4个字节，所以，Base64编码的长度永远是4的倍数，因此，只用加上`=`把Base64字符串的长度变为4的倍数，就可以正常解码了。

