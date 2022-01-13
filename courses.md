pypypy.cn

Python 的强大之处：

（1）它背后有着最庞大的免费“代码库”，给初学者足够的资源实现自己想要的功能。

（2）它是人工智能、大数据分析的重要支持语言。

（3）它被称为“胶水语言”，能很好包装和调用其他编程语言写的库。

（4）它是一个脚本语言，和其它编程语言相比更加简洁、高效。

python应用领域广泛，例如：爬虫，数据分析，科学计算，自动化办公，自动化运维，网站开发，多媒体处理，机器学习，深度学习等

例1，图像识别

```python
import requests
from aip import AipOcr

image = requests.get('https://res.pandateacher.com/python_classic.png').content

APP_ID = '16149264'
API_KEY = 'yxYg9r4OuAs4fYvfcl8tqCYd'
SECRET_KEY = 'yWg3KMds2muFsWs7MBSSFcgMQl8Wng4s'
client = AipOcr(APP_ID, API_KEY, SECRET_KEY)
res = client.basicGeneral(image)
if 'words_result' in res.keys():
    for item in res['words_result']:
        print(item['words'])

else:
    APP_ID = '11756541'
    API_KEY = '2YhkLuyQGljPUYnmi1CFgxOP'
    SECRET_KEY = '4rrHe2BF828bI8bQy6bLlx1MelXqa8Z7'
    client = AipOcr(APP_ID, API_KEY, SECRET_KEY)
    res = client.basicGeneral(image)
    if 'words_result' in res.keys():
        for item in res['words_result']:
            print(item['words'])
    else:
        print(res)
```

