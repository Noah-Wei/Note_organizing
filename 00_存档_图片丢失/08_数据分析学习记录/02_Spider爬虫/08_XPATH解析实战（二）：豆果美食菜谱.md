# XPATH解析实战（二）：豆果美食菜谱

## 一、需求

解析**豆果美食**的数据，保存"文章标题", "详情页链接", "作者", "点赞数", "收藏数"等信息，并实现数据持久化，保存到excel文件中

程序运行时添加一个进度条

URL：https://www.douguo.com/jingxuan/1

## 二、结果展示

![image-20231204233452911](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656df19e07c25.png)

![image-20231204233511869](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656df1afcb2b0.png)

## 三、代码分析

> 先获取一页的数据，在实现翻页效果

### 3.1 整体框架

**模块**

- 实现数据持久化，保存到excel文件中，需要用到模块：`openpyxl`
- 获取网页信息，需要用到模块：`requests`
- 使用XPath解析，需要用到：`lxml` 库中的 `etree` 模块
- 其他待定

**函数**

整体可以分为三个部分和一个`mian`代码块

- 获取数据：`get_data()`
- 解析数据：`parse_data()`
- 保存数据：`save_data()`

### 3.2 获取数据：`get_data()`

获取数据，首先要知道解析的网站URL，需求中已经给出，即

```python
url = "https://www.douguo.com/jingxuan/1"
```

然后是请求头，需要伪装程序为浏览器行为，所以`F12`从网页检查中获取请求头，在请求头中有一个参数User-Agent：作用是表标识浏览器的相关信息，足以处理大多数的网站，即

![image-20231204204343445](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656dc982d47e5.png)

```python
headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                      'Chrome/119.0.0.0 Safari/537.36'
    }
```

然后就可以获取整个网页的内容了，核心代码主要就一句

```python
resp = requests.get(url, headers=headers)
```

然后，我们进行一个判断，如果**状态码**正确的话，打印内容看看，中文是否有问题，如果有问题则修改编码格式，然后调用解析数据函数

```python
if resp.status_code == 200:
    print("获取成功")
    print(resp.text)
    parse_data(resp.text)
```

至此，第一部分完成

### 3.3 解析数据：`parse_data()`

接下来就是解析数据，本次采用的是`XPath`的方式解析数据，所以首先要把`html`内容解析为`xml文档树`

```python
tree = etree.HTML(html)
```

下面我们就可以配合`F12`网页检查代码和实际网站内容进行定位解析了

- **文章名称的获取**

![image-20231204233947021](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656df2c4c42b6.png)

从图上我们可以发现，电影名称在`h2`标签的文本中，我们可以快速直接定位

```python
titles = tree.xpath("//div[@class='relative']/a[1]/text()")
# print(titles)
```

打印结果，查看效果，返回的是一个`列表`

- **获取详情页**

根据上图，我们发现，地址在`a`标签的`href`属性的值中，但是链接只有后半段，所有我们取出`href`属性的值后，还需要遍历其中的元素为其添加上前半段

```python
links = tree.xpath("//div[@class='relative']/a[1]/@href")
links = ['https://www.douguo.com' + link for link in links]
# print(links)
```

- **获取作者**

```python
authors = tree.xpath("//div[@class='relative']/a[2]/img/@alt")
# print(authors)
```

- **获取点赞数**

```python
likes = tree.xpath("//span[@class='view']/text()")
```

- **获取收藏数**

```python
favorites = tree.xpath("//span[@class='collect']/text()")
```

至此，所有的内容信息已经全部获取完毕了。下面就是实现数据持久化了

### 3.4 保存数据：`save_data()`

本次实现数据持久化，将文件保存在`excel`文件中，在上一个函数中，我们将每一个要保存的字段都放在了一个列表中，所以我们可以利用`zip()`函数将多个列表打包成一个列表，然后再遍历循环添加到文件中

```python
def save_data(titles, links, authors, likes, favorites):
    data_list = zip(titles, links, authors, likes, favorites)
    for row_data in data_list:
        sheet.append(row_data)
```

### 3.5 `mian`代码块

> 要写入的文件一定要是关闭状态

这里就存放创建和写入文件的一些代码。首先我们要创建一张工作簿，在工作簿被创建的同时会自动创建一张工作表，然后我们获取当前活动的工作表，接着写入表单抬头信息，然后调用`get_data`函数，最后保存数据到一个路径中即可

```python
if __name__ == '__main__':
    wb = openpyxl.Workbook()
    sheet = wb.active
    sheet.append(["电影名称", "电影类型", "国家", "时间", "评分", "上映时间"])
    get_data()
    wb.save("Scrape电影数据.xlsx")
```

### 3.6 优化：获取多页数据

该网站实现解析多页数据比较简单，通过观察可以发现，多页数据只有URL不同，区别就是最后的数字不同，并且数字代表着页数，所以我们只需要修改几处地方即可

首先是将获取数据：`get_data()`函数中的代码添加一个for循环，将URL修改成拼接状态，并且，为了保证程序的稳定性，防止被检测为攻击，同时`time.sleep()`方法设置一个时间间隔，单位为秒，再优化一下使用`random.randint()`方法，设置随机数。

为了实现后台显示进度条，给for循环的`range()`套上`tqdm()`，并且去除代码中的所有`print()`语句，不然可能出错

所以我们要添加额外的模块

```python
import time
import random
from tqdm import tqdm
```

然后修改`get_data()`函数

```python
def get_data():
    for i in tqdm(range(1, 7), desc="进度条"):
        url = f"https://www.douguo.com/jingxuan/{i}"
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                          'Chrome/119.0.0.0 Safari/537.36'
        }

        # 设置时间间隔
        time.sleep(random.randint(3, 6))

        # 获取数据
        resp = requests.get(url, headers=headers)
        if resp.status_code == 200:
            # print("获取成功")
            # print(resp.apparent_encoding)
            parse_data(resp.text)
```

## 四、完整代码

```python
# -*- coding: utf-8 -*-
# @Time : 2023/12/3 20:13
# @Author : Eden Wei
# @FileName: demo4_XPath解析豆果美食.py
# @Software: PyCharm 2023.2.4 (Professional Edition)
"""
需求：用 XPath解析豆果美食【url:https://www.douguo.com/jingxuan/0】1~6页的数据，
获取标题，详情页链接，作者，点赞数，收藏数，并将结果保存到excel文件
"""
import requests
import openpyxl
import time
import random
from lxml import etree
from tqdm import tqdm


# 1.获取数据
def get_data():
    for i in tqdm(range(1, 7), desc="进度条"):
        url = f"https://www.douguo.com/jingxuan/{i}"
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                          'Chrome/119.0.0.0 Safari/537.36'
        }

        # 设置时间间隔
        time.sleep(random.randint(3, 6))

        # 获取数据
        resp = requests.get(url, headers=headers)
        if resp.status_code == 200:
            # print("获取成功")
            # print(resp.apparent_encoding)
            parse_data(resp.text)


# 2.解析数据
def parse_data(html):
    # 将html内容解析为xml文档树
    tree = etree.HTML(html)
    """
    获取标题，详情页链接，作者，点赞数，收藏数，并将结果保存到excel文件
    """
    # 获取标题
    titles = tree.xpath("//div[@class='relative']/a[1]/text()")
    # print(titles)

    # 获取详情页链接
    links = tree.xpath("//div[@class='relative']/a[1]/@href")
    links = ['https://www.douguo.com' + link for link in links]
    # print(links)

    # 获取作者
    authors = tree.xpath("//div[@class='relative']/a[2]/img/@alt")
    # print(authors)

    # 获取点赞数
    likes = tree.xpath("//span[@class='view']/text()")
    # print(likes)

    # 获取收藏数
    favorites = tree.xpath("//span[@class='collect']/text()")
    # print(favorites)

    # 保存数据
    save_data(titles, links, authors, likes, favorites)


# 3.保存数据
def save_data(titles, links, authors, likes, favorites):
    data_list = zip(titles, links, authors, likes, favorites)
    for row_data in data_list:
        sheet.append(row_data)


if __name__ == '__main__':
    wb = openpyxl.Workbook()
    sheet = wb.active
    sheet.append(["文字标题", "详情页链接", "作者", "点赞数", "收藏数"])
    get_data()
    wb.save("豆果美食菜谱.xlsx")

```





