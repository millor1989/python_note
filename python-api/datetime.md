### datetime

```python
>>> import datetime
```

获取**当前时间**：

```python
>>> print(datetime.datetime.now())
2021-01-29 14:07:54.194000
```

**时间格式化**：

```python
>>> print(datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S'))
2021-01-29 14:09:02
```

**解析字符串时间**：

```python
>>> datetime.datetime.strptime('2020-12-31 22:22:22', '%Y-%m-%d %H:%M:%S')
datetime.datetime(2020, 12, 31, 22, 22, 22)
```

**日期加减**，使用`datetime.timedelta()`实现：

```python
>>> now = datetime.datetime.now()

>>> def timeformate(tt):
...     return tt.strftime('%Y-%m-%d %H:%M:%S')

>>> print(timeformate(now))
2021-01-29 14:14:11

# 加一天
>>> timeformate(now + datetime.timedelta(days=1))
'2021-01-30 14:16:31'

# 减一天
>>> timeformate(now - datetime.timedelta(days=1))
'2021-01-28 14:16:31'

# 减一天
>>> timeformate(now + datetime.timedelta(days=-1))
'2021-01-28 14:16:31'

# 加一小时
>>> timeformate(now + datetime.timedelta(hours=1))
'2021-01-29 15:16:31'

# 加一分钟
>>> timeformate(now + datetime.timedelta(minutes=1))
'2021-01-29 14:17:31'
```

直接**构造时间**：

```python
>>> from datetime import datetime
>>> dt = datetime(2015, 4, 19, 12, 20) # 用指定日期时间创建datetime
>>> print(dt)
2015-04-19 12:20:00
```

#### 1、`datetime`与timestamp

**epoch time**指的是1970年1月1日 00:00:00 UTC+00:00时区的时刻，时间相对于epoch time的秒数称为**时间戳（timestamp）**，epoch time对应的时间戳为`0`，epoch time之前的时间戳为负值。

timestamp是统一规定的，与时区无关。

`datetime`可以使用`timestamp()`方法转换为timestamp：

```python
>>> from datetime import datetime
>>> dt = datetime(2015, 4, 19, 12, 20) # 用指定日期时间创建datetime
>>> dt.timestamp() # 把datetime转换为timestamp
1429417200.0
```

注意Python的timestamp是一个浮点数，整数位表示秒。

使用`datetime`的`fromtimestamp()`方法，可以将时间戳转换为`datetime`：

```python
>>> from datetime import datetime
>>> t = 1429417200.0
>>> print(datetime.fromtimestamp(t))
2015-04-19 12:20:00
```

timestamp是一个浮点数，它没有时区的概念，但是`datetime`是有时区的，`datetime`默认的时区是当前操作系统设定的时区（比如，北京时间，即UTC+8:00时区）。

使用`datetime`的`utcfromtimestamp()`方法，可以将时间戳转换为UTC标准时区时间：

```python
>>> from datetime import datetime
>>> t = 1429417200.0
>>> print(datetime.fromtimestamp(t)) # 本地时间
2015-04-19 12:20:00
>>> print(datetime.utcfromtimestamp(t)) # UTC时间
2015-04-19 04:20:00
```

#### 2、时区转换

`datetime`类有一个时区属性`tzinfo`，默认为`None`，无法用来区分`datetime`的时区，除非为`datetime`设置了时区：

```python
>>> from datetime import datetime, timedelta, timezone
>>> tz_utc_8 = timezone(timedelta(hours=8)) # 创建时区UTC+8:00
>>> now = datetime.now()
>>> now
datetime.datetime(2015, 5, 18, 17, 2, 10, 871012)
>>> dt = now.replace(tzinfo=tz_utc_8) # 强制设置为UTC+8:00
>>> dt
datetime.datetime(2015, 5, 18, 17, 2, 10, 871012, tzinfo=datetime.timezone(datetime.timedelta(0, 28800)))
```

通过`utcnow()`可以得到当前的UTC时间，然后可以进行任意的时区转换：

```python
# 拿到UTC时间，并强制设置时区为UTC+0:00:
>>> utc_dt = datetime.utcnow().replace(tzinfo=timezone.utc)
>>> print(utc_dt)
2015-05-18 09:05:12.377316+00:00
# astimezone()将转换时区为北京时间:
>>> bj_dt = utc_dt.astimezone(timezone(timedelta(hours=8)))
>>> print(bj_dt)
2015-05-18 17:05:12.377316+08:00
# astimezone()将转换时区为东京时间:
>>> tokyo_dt = utc_dt.astimezone(timezone(timedelta(hours=9)))
>>> print(tokyo_dt)
2015-05-18 18:05:12.377316+09:00
# astimezone()将bj_dt转换时区为东京时间:
>>> tokyo_dt2 = bj_dt.astimezone(timezone(timedelta(hours=9)))
>>> print(tokyo_dt2)
2015-05-18 18:05:12.377316+09:00
```

时区转换的关键在于，拿到一个`datetime`时，要获知其正确的时区，然后强制设置时区，作为基准时间。

利用带时区的`datetime`，通过`astimezone()`方法，可以转换到任意时区。

注：不是必须从UTC+0:00时区转换到其他时区，**任何带时区的`datetime`都可以正确转换**。`datetime`表示的时间需要时区信息才能确定一个特定的时间，否则只能视为本地时间。存储`datetime`，最佳方法是将其转换为timestamp再存储，因为timestamp的值与时区完全无关。

