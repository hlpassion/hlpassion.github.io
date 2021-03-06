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

```python
scrapy genspider zhipin zhipin.com
```

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

通过树状格式来解释一下每个文件的作用：

> scrapy.cfg:项目的配置文件
>
> items.py:项目的目标文件(设置爬取字段)
>
> pipelines.py:项目的管道文件(对获取内容进行批处理)
>
> settings.py:项目的配置文件(常用配置项都在其中)
>
> spiders/zhipin.py:存储爬虫代码

### 明确目标

我们打算爬取BOSS直聘网站上数据类工作的招聘信息，通过分析请求URL地址我们可以发现最终的请求链接为https://www.zhipin.com/job_detail/?query=%s&scity=%s，query后为搜索关键字，scity后为城市代码。

- 打开items.py
- 定义结构化数据字段，保存爬取到的数据.

```python
import scrapy


class ThudatazhipinItem(scrapy.Item):
    # define the fields for your item here like:
    # name = scrapy.Field()
    job_position = scrapy.Field()   #职位名称
    job_tag = scrapy.Field()        #职位分类标签
    department_name = scrapy.Field()#职位所属部门
    job_location = scrapy.Field()   #工作地点
    
# 为了简便也可以使用 from scrapy import Field
# 使用 Field()代替scrapy.Field() 简化代码
# 这里只罗列了部分字段
```

### 编写爬虫代码(spiders/zhipin.py)

1. 确定爬取范围 

   ```python
   # 爬虫名称，必须唯一
   name = "zhipin"
   # 爬虫允许的的域名，爬虫只能爬取这个域名下的网页
   allowed_domains = ["zhipin.com"]
   def start_requests(self):
       # 搜索关键字
       search_fields = [r'大数据',r'数据分析', r'数据运营', r'数据挖掘']

       # 城市代码在网页源代码中定义 北京|上海|广州
       city_list = ['101010100', '101020100', '101280100']
       urls = []
       
       # 拼接请求地址
       for city in city_list:
           for job in search_fields:
               url = 'https://www.zhipin.com/job_detail/?query=%s&scity=%s' % (job, city)
               urls.append(url)
       # 利用parse方法解析每个请求地址，获取要爬取的数据
       for job_url in urls:
           yield scrapy.Request(url=job_url, callback=self.parse)
   ```

   每一次请求地址返回都是招聘的列表页面，我们需要获取列表页面中的所有URL，通过进一步的解析方法获取每个页面的详细内容.

   ![zhipin](/img/in-post/zhipin.png)

```python
# 解析所有结果的列表页面，获取列表页面中每个招聘信息的详细地址，通过判断下一页按钮结束请求
def parse(self, response):
    sel = Selector(response)
    url_prefix = 'https://www.zhipin.com'
    job_urls = sel.xpath('//div[@class="job-list"]/ul/li/a/@href').extract()
    for job_url in job_urls:
        url = url_prefix + job_url
        yield scrapy.Request(url=url, callback=self.parse_job_info)
	# 获取下一页按钮，如果链接地址为空，则停止请求.
    next_page_url = sel.xpath('//a[@class="next"]/href').extract()
    if len(next_page_url) != 0:
        next_page_url = url_prefix + ''.join(next_page_url)
        yield scrapy.Request(url=next_page_url, callback=self.parse)
```

2. 从网页中取数据

   以获取职位名称为例，使用[xpath](http://www.w3school.com.cn/xpath/)提取页面数据.

   ![zhipin-detail](/img/in-post/zhipin-detail.png)

```python
xpath('//div[@class="job-primary"]/div[@class="info-primary"]/div[@class="name"]/text()').extract()
```

通过使用xpath从页面中提取其他爬取字段，完成从页面取数据的过程.

3. 保存数据

   Scrapy中不借助pipelines过程直接存取结果的简单方法有四种，使用 -o指定输出文件格式.

   ```python
   # json格式
   scrapy crawl zhipin -o zhaopin.json

   # json lines格式
   scrapy crawl zhipin -o zhaopin.jsonl

   # csv格式
   scrapy crawl zhipin -o zhaopin.csv

   # xml格式
   scrapy crawl zhipin -o zhaopin.xml
   ```

   如果大量数据存取在数据库中会更好，个人常用的方式是存在Mongodb中，配置也很简单.

   在pipelines.py中输入如下代码：

   ```python
   import pymongo
   from scrapy.conf import settings
   from scrapy.exceptions import DropItem
   from scrapy import log

   class ThudatazhipinPipeline(object):

       def __init__(self):
           client = pymongo.MongoClient(
               settings['MONGODB_SERVER'],		# settings.py 中设置的服务器地址
               settings['MONGODB_PORT']		# settings.py 中设置的端口号
           )
           db = client.get_database(settings['MONGODB_DB'])    # 数据库名称
           # 数据表
           self.collention = db.get_collection(settings['MONGODB_COLLECTION'])

       def process_item(self, item, spider):
           valid = True
           for data in item:
               if not data:
                   valid = False
                   raise DropItem('Missing {0}'.format(data))
           if valid:
               self.collention.insert(dict(item))
               log.msg("userInfo added to MongoDB database!",
                       level=log.DEBUG, spider=spider)

           return item
   ```

   ### 神秘的配置文件settings.py

   ```Python
   ROBOTSTXT_OBEY = False	# 是否遵循robots协议，默认是遵守，但是为了获取内容，设置成False

   CONCURRENT_REQUESTS = 4	# 控制请求数量，Scrapy Downloader并发请求的最大值
   DOWNLOAD_DELAY = 3		# 控制下载延迟,默认情况，Scrapy在两个请求之间不等待一个固定的值，						 # 而是使用0.5到1.5之间的一个随机值
   # 通过修改以上两个变量可以控制我们爬虫的速度，防止被反爬措施封杀掉。
   USER_AGENT = []		# 设置以后可以为每次请求自动加上一个设置的USER_AGENT,也就是就常见的伪					 # 装技术，可以通过从USER_AGENT列表中，随机取值来模拟每次请求来自不同					 # 的USER_AGENT
   ```

   ​



>本文的详细代码放在[Github](https://github.com/hlpassion/THUDATAzhipin)
>
>干货可能不多，算是自己的总结吧。数据派志愿者.
>
>本人博客：[Heron](https://hlpassion.github.io/)

