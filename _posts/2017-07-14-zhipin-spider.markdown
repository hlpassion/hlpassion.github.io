---
layout:    post
title:    "BOSS直聘网站数据类职位爬虫"
subtitle:    "手把手教"
date:    2017-07-14
author:    "Heron"
header-img:    "img/post-bg-2017-07-14.jpg"
header-mask:    0.1
catalog:    true
tags:
    - python
    - 数据职位
    - 爬虫
    - Scrapy
---
## BOSS直聘网站数据类职位爬虫

> 学习了Scrapy框架以后，刚好赶上数据派需要各招聘网站的数据类职位信息，刚好借此机会验证自己的学习效果。并通过这篇较详细的笔记为其他新手提供一些帮助。

### 配置环境

本人使用Anaconda管理Pythond的各种包，使用Anaconda Python3.6版本，翻墙下载速度快，不方便的同学可以到我的网盘下载。[链接](http://pan.baidu.com/s/1pLE1tUj ) 密码:w58k.

### Scrapy框架

Scrapy框架[官方地址](https://scrapy.org/)

安装方式使用pip安装即可

```python
pip install scrapy
```

开始项目前先使用Scrapy命令生成新的Scrapy项目。

```
scrapy startproject THUDATAzhipin
```

cd THUDATAzhipin 利用Scrapy命令生成spider模板.

```
├── __init__.py
├── __pycache__
│   ├── __init__.cpython-36.pyc
│   └── settings.cpython-36.pyc
├── items.py
├── middlewares.py
├── pipelines.py
├── settings.py
└── spiders
    ├── __init__.py
    ├── __pycache__
    │   └── __init__.cpython-36.pyc
    └── zhipin.py
```









>干货可能不多，算是自己的总结吧。数据派志愿者.
>
>本人博客：[Heron](https://hlpassion.github.io/)

