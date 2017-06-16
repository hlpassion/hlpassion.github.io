---
layout:    post
title:    "python Challenge 答案和解题思路"
subtitle:    "心血来潮进行的，不知道能不能做下去"
date:    2017-06-11
author:    "Heron"
header-img:    "img/post-bg-2017-06-11.jpg"
header-mask:    0.1
catalog:    true
tags:
    - python
    - 游戏
---
# 0. 计算2的38次方，结果替换网址就行了(274877906944)

# 1. 字母映射问题(ocr)

每个字母向后移两位，在最后Y、Z映射到A、B。将提示下的乱码句子转换以后就是答案。

```python
import string
orginal_str = "g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj."

trans = str.maketrans(string.ascii_lowercase, string.ascii_lowercase[2:] + string.ascii_lowercase[:2])

orginal_str.translate(trans)
->"i hope you didnt translate it by hand. thats what computers are for. doing it in by hand is inefficient and that's why this text is so long. using string.maketrans() is recommended. now apply on the url."
```

# 2.识别字符问题(equality)

根据提示，字符在网页源代码中。

```html
<!--
find rare characters in the mess below:
-->
```

在下面混乱的字符中找到字符。混乱字符比较长就不贴了。

解决方法将那一段复制出来(我就是按照最方便的做法来了)，不想复制的就使用request库访问网页源代码，然后在通过额数的字符截取一下也可以。

```python
import string
for i in mess_str:
    if i in string.ascii_letters:
        print(i, end='')
```

> 如果使用复制的话，用"""  """包括字符不会报错

# 3. 字母左右被三个大写字母包围 

刚开始没审清楚题，EXACTLY，就是说左右有且仅有三个大写字母包围，最先是用最笨的遍历方式加if判断。后来采用正则表达式来完成字符的匹配问题。

```python
from urllib import request
import re

url = 'http://www.pythonchallenge.com/pc/def/equality.html'
text = request.urlopen(url)
text = text.read().decode('utf-8')
start = text.find('<!--') + len('<!--')
mass_text = text[start:-len('<!--')]

reg = re.compile('[a-z][A-Z]{3}([a-z])[A-Z]{3}[a-z]')
print(''.join(reg.findall(mass_text)))
```



