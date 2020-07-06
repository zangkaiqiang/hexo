---
title: Read Image From Url
date: 2020-07-06 16:16:00
tags:
---
## 使用图床获取图像Url地址
```
http://10.1.2.37:7777/images/2020/07/06/d5-24a.jpg
```
![alt](/images/d5-24a.jpg)

```
from urllib.request import urlopen
import numpy as np
import matplotlib.pyplot as plt

def read_url(url):
    req = urlopen(url)
    arr = np.asarray(bytearray(req.read()), dtype=np.uint8)
    img = cv2.imdecode(arr, -1)
    img = cv2.cvtColor(img,cv2.COLOR_BGR2RGB)
    return img
    
url = 'http://10.1.2.37:7777/images/2020/07/06/d5-24a.jpg'
img = read_url(url)
plt.imshow(img)
plt.show()
```
![alt](/images/d5-24a.jpg)