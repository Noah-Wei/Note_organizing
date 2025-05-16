# XPATH解析实战（一）：解析Scrape电影网数据

## 一、需求

解析**Scrape电影网**的数据，保存"电影名称", "电影类型", "国家", "时间", "评分", "上映时间"等信息，并实现数据持久化，保存到excel文件中

URL：https://ssr1.scrape.center/page/1

## 二、结果展示

![image-20231204213003376](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656dd45b6e0ed.png)

## 三、代码分析

### 3.1 整体框架

**模块**

- 实现数据持久化，保存到excel文件中，需要用到模块：`openpyxl`
- 获取网页信息，需要用到模块：`requests`
- 使用XPath解析，需要用到：`lxml` 库中的 `etree` 模块
- 其他待定

**函数**

整体可以分为三个部分和一个`mian`代码块

- 获取数据：`get_html()`
- 解析数据：`parse_html()`
- 保存数据：`save_data()`

### 3.2 获取数据：`get_html()`

获取数据，首先要知道解析的网站URL，需求中已经给出，即

```python
url = "https://ssr1.scrape.center/page/1"
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

然后，我们进行一个判断，如果**状态码**正确的话，则修改编码格式（有的网站默认编码对中文不太适合），然后调用解析数据函数

```python
if resp.status_code == 200:
    print("获取成功")
    resp.encoding = "UTF-8"
    parse_html(resp.text)
```

至此，第一部分完成

### 3.3 解析数据：`parse_html()`

接下来就是解析数据，本次采用的是`XPath`的方式解析数据，所以首先要把`html`内容解析为`xml文档树`

```python
tree = etree.HTML(html)
```

下面我们就可以配合`F12`网页检查代码和实际网站内容进行定位解析了

- **电影名称的获取**

![image-20231204205115279](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656dcb4455d0b.png)

从图上我们可以发现，电影名称在`h2`标签的文本中，并且这个`h2`标签具有唯一的`class`属性的值`m-b-sm`，我们可以快速直接定位

```python
names = tree.xpath("//h2[@class='m-b-sm']/text()")
print(names)
```

打印结果，查看效果，返回的是一个`列表`

- **获取电影类型**

![image-20231204205432519](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656dcc09c21cf.png)

根据图，我们发现，当有多个类型时，每一个类型都被放在`button`标签下的`span`标签的文本中，所以我们不能直接定位，因而我们先定位到上级的`div`标签，然后利用`for`循环，对每一个`div`进行XPath解析，但是这样获取到的文本都是一个个列表，为了同意格式，使用`join()`方法拼接，然后用一个空列表来接收，即

```python
# b.获取电影类型
divs = tree.xpath("//div[@class='categories']")
types = []
for div in divs:
    text = div.xpath("./button/span/text()")
    text_str = "|".join(text)
    types.append(text_str)
print(types)
```

**注：**上面的for循环，可以简写成列表生成式，即

```python
types2 = ["-".join(div.xpath("./button/span/text()")) for div in divs]
```

- 获取上映国家

![image-20231204210312220](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656dce11ed320.png)

我们可以发现，上映国家信息文本保存的位置，但是尴尬的是，下面上映时间的文本与其一直，所以我们直接找上一级标签，然后配合谓语定位，进行彻底的定位

```python
counties_1 = tree.xpath(
    "//div[@class='p-h el-col el-col-24 el-col-xs-9 el-col-sm-13 el-col-md-16']/div[2]/span[1]/text()")
```

**注：**这里我们在观察源代码，该`div`标签的`class`属性的值，很长，但是没有和他相似的，所有我们又可以利用`starts-with()`方法进行简写

```python
counties = tree.xpath("//div[starts-with(@class,'p-h')]/div[2]/span[1]/text()")
```

- 获取电影时长、上映时间

这两部分的获取方式和上面一样，即

电影时长

```python
times = tree.xpath("//div[starts-with(@class,'p-h')]/div[2]/span[3]/text()")
```

上映时间

```python
release_data = tree.xpath("//div[starts-with(@class,'p-h')]/div[3]/span/text()")
```

- 获取电影评分![image-20231204211157066](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656dd01e5af88.png)

获取电影评分，同样很简单，需要使用谓语定位

```python
scores = tree.xpath("//p[starts-with(@class,'score')]/text()")
```

但是，当我们打印输出查看效果的时候

![image-20231204211327592](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656dd07758352.png)

效果不尽如意，每个值的前面有一个`\n`和一串空字符，我们直接调用`strip()`方法去除，简写成列表生成式，即

```python
scores = [score.strip() for score in scores]
print(scores)
```

这时候在打印检查的时候，已经变得正常的了。

至此，所有的内容信息已经全部获取完毕了。下面就是实现数据持久化了

### 3.4 保存数据：`save_data()`

本次实现数据持久化，将文件保存在`excel`文件中，在上一个函数中，我们将每一个要保存的字段都放在了一个列表中，所以我们可以利用`zip()`函数将多个列表打包成一个列表，然后再遍历循环添加到文件中

```python
3.保存数据
def save_data(names, types, counties, times, scores):
    zip_data = zip(names, types, counties, times, scores)
    for row_data in zip_data:
        sheet.append(row_data)
```

### 3.5 `mian`代码块

> 要写入的文件一定要是关闭状态

这里就存放创建和写入文件的一些代码。首先我们要创建一张工作簿，在工作簿被创建的同时会自动创建一张工作表，然后我们获取当前活动的工作表，接着写入表单抬头信息，然后调用`get_html`函数，最后保存数据到一个路径中即可

```python
if __name__ == '__main__':
    wb = openpyxl.Workbook()
    sheet = wb.active
    sheet.append(["电影名称", "电影类型", "国家", "时间", "评分", "上映时间"])
    get_html()
    wb.save("Scrape电影数据.xlsx")
```

## 四、完整代码

```python
# -*- coding: utf-8 -*-
# @Time : 2023/12/3 20:12
# @Author : Eden Wei
# @FileName: demo2_XPath解析电影数据.py
# @Software: PyCharm 2023.2.4 (Professional Edition)
import openpyxl
import requests
from lxml import etree


# 1.获取数据
def get_html():
    url = "https://ssr1.scrape.center/page/1"
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                      'Chrome/119.0.0.0 Safari/537.36'
    }
    resp = requests.get(url, headers=headers)
    if resp.status_code == 200:
        print("获取成功")
        resp.encoding = "UTF-8"
        parse_html(resp.text)


# 2.解析数据
def parse_html(html):
    # 将html内容解析为xml文档数
    tree = etree.HTML(html)

    # a.获取电影名称
    names = tree.xpath("//h2[@class='m-b-sm']/text()")
    print(names)

    # b.获取电影类型
    types = []
    divs = tree.xpath("//div[@class='categories']")
    for div in divs:
        text = div.xpath("./button/span/text()")
        text_str = "|".join(text)
        types.append(text_str)
    print(types)

    # 方式二：
    types2 = ["-".join(div.xpath("./button/span/text()")) for div in divs]
    # print(types2)

    # c.上映的国家
    counties_1 = tree.xpath(
        "//div[@class='p-h el-col el-col-24 el-col-xs-9 el-col-sm-13 el-col-md-16']/div[2]/span[1]/text()")
    # print(counties_1)

    # 方式二
    counties = tree.xpath("//div[starts-with(@class,'p-h')]/div[2]/span[1]/text()")
    print(counties)

    # d.时长
    times = tree.xpath("//div[starts-with(@class,'p-h')]/div[2]/span[3]/text()")
    print(times)

    # e.评分
    scores = tree.xpath("//p[starts-with(@class,'score')]/text()")
    scores = [score.strip() for score in scores]
    print(scores)

    # f.上映时间
    release_data = tree.xpath("//div[starts-with(@class,'p-h')]/div[3]/span/text()")
    print(release_data)

    # 保存数据
    save_data(names, types, counties, times, scores)


# 3.保存数据
def save_data(names, types, counties, times, scores):
    zip_data = zip(names, types, counties, times, scores)
    for row_data in zip_data:
        sheet.append(row_data)


if __name__ == '__main__':
    wb = openpyxl.Workbook()
    sheet = wb.active
    sheet.append(["电影名称", "电影类型", "国家", "时间", "评分", "上映时间"])
    get_html()
    wb.save("Scrape电影数据.xlsx")
```

## 五、优化：获取多页数据

比如获取1-10页的数据

### 5.1 分析

该网站实现解析多页数据比较简单，通过观察可以发现，多页数据只有URL不同，比如：

第一页的URL

![image-20231204213401396](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656dd54911334.png)

第二页的URL

![image-20231204213429704](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656dd5655ce23.png)

第十页的URL

![image-20231204213448931](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656dd578989e6.png)

区别就是最后的数字不同，并且数字代表着页数，所以我们只需要修改几处地方即可

首先是将获取数据：`get_html()`函数中的代码添加一个for循环，将URL修改成拼接状态，并且，为了保证程序的稳定性，防止被检测为攻击，同时`time.sleep()`方法设置一个时间间隔，单位为秒，再优化一下使用`random.randint()`方法，设置随机数。

### 5.2 代码修改

所以我们要添加额外的模块

```python
import time
import random
```

然后修改`get_html()`函数

```python
def get_html():
    for i in range(1, 11):
        url = f"https://ssr1.scrape.center/page/{i}"
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                          'Chrome/119.0.0.0 Safari/537.36'
        }
        time.sleep(random.randint(3, 6))
        resp = requests.get(url, headers=headers)
        if resp.status_code == 200:
            print("获取成功")
            resp.encoding = "UTF-8"
            parse_html(resp.text)
```

### 5.3 结果展示

可见，已经能够获取1-10页的数据了

![image-20231204214218696](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/04/656dd73b02ca7.png)