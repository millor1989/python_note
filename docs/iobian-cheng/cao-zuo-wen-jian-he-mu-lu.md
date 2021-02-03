### 操作文件和目录

Python操作目录和文件，是通过内置的`os`模块调用操作系统提供的接口函数实现的。

```python
>>> import os
>>> os.name # 操作系统类型
'posix'
```

如果`os.name`是`posix`，说明系统是`Linux`、`Unix`或`Mac OS X`，如果是`nt`，就是`Windows`系统。

还可通过`uname()`函数（Windows系统不适用），获取详细的系统信息：

```python
>>> os.uname()
posix.uname_result(sysname='Darwin', nodename='MichaelMacPro.local', release='14.3.0', version='Darwin Kernel Version 14.3.0: Mon Mar 23 11:59:05 PDT 2015; root:xnu-2782.20.48~5/RELEASE_X86_64', machine='x86_64')
```

#### 1、环境变量

通过`os.environ`可以获取操作系统中定义的环境变量：

```python
>>> os.environ
environ({'VERSIONER_PYTHON_PREFER_32_BIT': 'no', 'TERM_PROGRAM_VERSION': '326', 'LOGNAME': 'michael', 'USER': 'michael', 'PATH': '/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/mysql/bin', ...})
```

还可以通过`os.environ.get(key)`获取指定的环境变量的值：

```python
>>> os.environ.get('PATH')
'/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/mysql/bin'
>>> os.environ.get('x', 'default')
'default'
```

#### 2、操作文件和目录

操作文件和目录的函数一部分位于`os`模块，一部分位于`os.path`模块。

比如，**目录的查看、创建和删除**：

```python
# 查看当前目录的绝对路径:
>>> os.path.abspath('.')
'/Users/michael'
# 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:
>>> os.path.join('/Users/michael', 'testdir')
'/Users/michael/testdir'
# 然后创建一个目录:
>>> os.mkdir('/Users/michael/testdir')
# 删掉一个目录:
>>> os.rmdir('/Users/michael/testdir')
```

**路径的连接**不要用字符串拼接，需要通过`os.path.join()`函数，因为不同操作系统路径分隔符不同。`Linux\Unix\Mac`中是`/`，而`Windows`系统下是`\`。通用，**拆分路径**时，通过`os.path.split()`函数（将路径分为两部分，后一部分总是最后级别的目录或文件名）进行：

```python
>>> os.path.split('/Users/michael/testdir/file.txt')
('/Users/michael/testdir', 'file.txt')
```

可以用`os.path.splitext()`函数**获取文件扩展名**：

```python
>>> os.path.splitext('/path/to/file.txt')
('/path/to/file', '.txt')
```

路径的拆分、合并函数并不要求文件和目录真的存在，它们只是对字符串进行操作。

**文件的重命名和删除**：

```python
# 对文件重命名:
>>> os.rename('test.txt', 'test.py')
# 删掉文件:
>>> os.remove('test.py')
```

`os`模块没有**文件复制**函数，文件复制可以通过`读写文件`操作实现。

`shutil`模块提供了诸如`copyfile()`复制文件函数等很多实用函数，可以看作是`os`模块的补充：

```python
shutil.copyfile('./mydata.txt', './copyofmydata.txt')
```

**文件过滤**，比如，列出当前目录下的所有目录：

```python
>>> [x for x in os.listdir('.') if os.path.isdir(x)]
['.lein', '.local', '.m2', '.npm', '.ssh', '.Trash', '.vim', 'Applications', 'Desktop', ...]
```

列出所有`.py`文件：

```python
>>> [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
['apis.py', 'config.py', 'models.py', 'pymonitor.py', 'test_db.py', 'urls.py', 'wsgiapp.py']
```

