### datetime

```python
>>> import datetime
```

获取当前时间：

```python
>>> print(datetime.datetime.now())
2021-01-29 14:07:54.194000
```

时间格式化：

```python
>>> print(datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S'))
2021-01-29 14:09:02
```

解析字符串时间：

```python
>>> datetime.datetime.strptime('2020-12-31 22:22:22', '%Y-%m-%d %H:%M:%S')
datetime.datetime(2020, 12, 31, 22, 22, 22)
```

日期加减，使用`datetime.timedelta()`实现：

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



