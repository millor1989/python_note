### hashlib

#### 1、摘要算法简介

摘要算法又称哈希算法、散列算法。将任意长度的数据转换为一个长度固定的数据串（通常为16进制的字符串）。

摘要算法的的目的是查看原始数据是否被篡改。摘要算法是单向的，计算出摘要容易，通过摘要反推原始数据很难，对原始数据做一个bit的修改都会导致摘要不同。

Python的hashlib模块提供了常见的摘要算法，比如MD5、SHA1等等。

以MD5为例，MD5是最常见的摘要算法，速度很快，生成结果是固定的128 bit字节，通常用一个32位的16进制字符串表示。：

```python
import hashlib

md5 = hashlib.md5()
md5.update('how to use md5 in python hashlib?'.encode('utf-8'))
print(md5.hexdigest())
```

结果为：

```ascii
d26a53750bc40b38b65a520292f69306
```

如果数据量很大，可以分多次调用`update()`，结果是一样的：

```python
import hashlib

md5 = hashlib.md5()
md5.update('how to use md5 in '.encode('utf-8'))
md5.update('python hashlib?'.encode('utf-8'))
print(md5.hexdigest())
```

SHA1摘要算法结果是160bit字节，通常用一个40位的16进制字符串表示

```python
import hashlib

sha1 = hashlib.sha1()
sha1.update('how to use sha1 in '.encode('utf-8'))
sha1.update('python hashlib?'.encode('utf-8'))
print(sha1.hexdigest())
```

比SHA1更安全的算法是SHA256和SHA512，不过越安全的算法不仅越慢，而且摘要长度更长。

因为任何摘要算法都是把无限多的数据集合映射到一个有限的集合中，有可能两个不同的数据通过某个摘要算法得到了相同的摘要，这种情况称为**碰撞**。碰撞的情况很少见。

#### 2、摘要算法应用

存储密码时，保存摘要而不是明文密码以防止密码数据泄露。

另外，如果用`123456`，`888888`，`password`这些简单的口令作为密码，黑客可以通过这些简单密码的MD5摘要反推密码。为了确保用户口令不能简单地通过这种方式被反推，可以对原始口令加一个复杂字符串，俗称**“加盐”**。

```python
def calc_md5(password):
    return get_md5(password + 'the-Salt')
```

通过加盐，只要黑客不知道“盐”字符串，即使用户使用简单口令，黑客也很难通过MD5摘要进行反推。

此外，为了防止两个使用相同简单口令的用户的摘要相同，还可以将用户的用户名等信息作为”盐“的一部分来计算摘要。

### hmac

加盐的哈希，实际就是Hmac算法：Keyed-Hashing for Message Aucthentication。它通过一个标准算法，在计算哈希过程中，把`key`混入计算过程中。

与自定义的加盐哈希算法不同的是，Hmac算法针对所有哈希算法都通用，无论是MD5还是SHA-1。采用Hmac替代自定义加盐算法，可以使程序算法更标准化，也更安全。

Python自带的hmac模块实现了标准的Hmac算法：

```python
>>> import hmac
>>> message = b'Hello, world!'
>>> key = b'secret'
>>> h = hmac.new(key, message, digestmod='MD5')
>>> # 如果消息很长，可以多次调用h.update(msg)
>>> h.hexdigest()
'fa4ee7d173f2d97ee79022d1a7355bcf'
```

与普通hash算法类似。hmac输出的长度和原始哈希算法的长度一致。需要注意的是，传入的key和message都是`bytes`类型，`str`类型需要首先编码为`bytes`。