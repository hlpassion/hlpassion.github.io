---
layout:    post
title:    "python Challenge 答案和解题思路"
subtitle:    "心血来潮进行的，不知道能不能做下去"
date:    2017-06-11
author:    "Heron"
header-img:    "img/post-bg-2017-06-07.jpg"
header-mask:    0.1
catalog:    true
tags:
    - python
    - 游戏
---
# 1. 计算2的38次方，结果替换网址就行了(274877906944)

# 2. 字母映射问题(ocr)

每个字母向后移两位，在最后Y、Z映射到A、B。将提示下的乱码句子转换以后就是答案。

```python
import string
orginal_str = "g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj."

trans = str.maketrans(string.ascii_lowercase, string.ascii_lowercase[2:] + string.ascii_lowercase[:2])

orginal_str.translate(trans)
->"i hope you didnt translate it by hand. thats what computers are for. doing it in by hand is inefficient and that's why this text is so long. using string.maketrans() is recommended. now apply on the url."
```

