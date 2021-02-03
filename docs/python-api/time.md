### time

```python
>>> import time
```

**获取时间戳**，单位：秒

```python
>>> time.time()
1611914062.303
```

乘以1000可得毫秒为单位的时间戳。

**日期转换为时间戳**，首先将日期转换为`struct_time`：

```python
>>> timearray = time.strptime('2021-01-01 00:00:00', '%Y-%m-%d %H:%M:%S')
>>> timearray
time.struct_time(tm_year=2021, tm_mon=1, tm_mday=1, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=4, tm_yday=1, tm_isdst=-1)
```

然后，使用`time.mktime()`将`struct_time`转换为时间戳：

```python
>>> time.mktime(timearray)
1609430400.0
```

**时间戳转换为时间**：

```python
>>> time.localtime(1609430400.0)
time.struct_time(tm_year=2021, tm_mon=1, tm_mday=1, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=4, tm_yday=1, tm_isdst=0)
```

**获取当前时间**：

```python
>>> time.localtime()
time.struct_time(tm_year=2021, tm_mon=2, tm_mday=1, tm_hour=11, tm_min=18, tm_sec=2, tm_wday=0, tm_yday=32, tm_isdst=0)
```

**时间格式化**：

```python
>>> print(time.strftime('%Y-%m-%d %H:%M:%S', time.localtime()))
2021-02-01 11:23:59
```

