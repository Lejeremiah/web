#### python爬虫笔记



##### 1.存为html文件

```python
from urllib.request import urlopen
url = 'httpp://www.baidu.com'

resp = urlopen(url)
with open("mybaidu.html", mode = "w", encoding = "utf-8") as f:
    f.write(resp.read().encode("utf-8"))
```



##### 2.批量抓取网站图片

```python
import requests
import re
import time
headers = {
    'User-Agent': 'asd', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'
}
target = 'https://www.vmgirls.com/18132.html'

resp = requests.get(target, headers = headers)
html = resp.text
urls = re.findall('<a href="(.*?)" alt=".*?" title=".*?">', html)

i = 0
for url in urls：
	time.sleep(1)
    i = i + 1
    url = 'http:' + url
    resp = requests.get(url, headers = headers)
    file_name = str(i) + '.jpg'
    with open(file_name, 'wb') as f:
        f.write(resp.content)
```

```python

import requests
import re
import time
headers = {
    'User-Agent': 'asd', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'
}
target = 'https://www.vmgirls.com/18132.html'

resp = requests.get(target, headers = headers)
html = resp.text
urls = re.findall('<a href="(.*?)" alt=".*?" title=".*?">', html)
print(urls)
i = 0
for url in urls :
    i = i+1
    time.sleep(1)
    url = 'http:' + url
    print(url)
    resp = requests.get(url, headers = headers)
    file_name = str(i) + '.jpg'
    with open(file_name, 'wb') as f:
        f.write(resp.conten
```

