# BeautifulSoup解析数据

## 一、BeautifulSoup4解析数据

> 正则可以解析任意的字符串，但是bs4专门用来解析网页的

Beautiful Soup就是Python的一个HTML或XML的**解析库**，可以用它来方便地**从网页中提取数据**。官方解释如下：

- Beautiful Soup提供一些简单的、Python式的函数来处理导航、搜索、修改分析树等功能。它是一个工具箱，通过解析文档为用户提供需要抓取的数据，因为简单，所以不需要多少代码就可以写出一个完整的应用程序。
- Beautiful Soup自动将输入文档转换为Unicode编码，输出文档转换为UTF-8编码。你不需要考虑编码方式，除非文档没有指定一个编码方式，这时你仅仅需要说明一下原始编码方式就可以了。
- Beautiful Soup已成为和lxml、html6lib一样出色的Python解释器，为用户灵活地提供不同的解析策略或强劲的速度。

所以说，利用它可以省去很多烦琐的提取工作，提高了解析效率。

BeautifulSoup4是BeautifulSoup系列模块的第四个大版本

中文文档：https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html

安装BeautifulSoup，目前，BeautifulSoup的最新版本是4.x版本，之前的版本已经停止开发了

- Windows系统

```python
pip install beautifulsoup4/bs4
```

- Mac、Linux系统

```python
pip3 install beautifulsoup4/bs4
```

### 1.1 解析器

常见的解析器有以下这几种

| 解析器           | 使用方法                             | 优势                                                      | 劣势                                         |
| ---------------- | ------------------------------------ | --------------------------------------------------------- | -------------------------------------------- |
| Python标准库     | BeautifulSoup(markup, “html.parser”) | Python的内置标准库、执行速度适中 、文档容错能力强         | Python 2.7.3 or 3.2.2)前的版本中文容错能力差 |
| lxml HTML 解析器 | BeautifulSoup(markup, “lxml”)        | 速度快、文档容错能力强                                    | 需要安装C语言库<br />pip install lxml        |
| lxml XML 解析器  | BeautifulSoup(markup, “xml”)         | 速度快、唯一支持XML的解析器                               | 需要安装C语言库                              |
| html5lib         | BeautifulSoup(markup, “html5lib”)    | 最好的容错性、以浏览器的方式解析文档、生成HTML5格式的文档 | 速度慢、不依赖外部扩展                       |

如有一个html代码

```python
"""
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
```

如果没有制定解析器的

```
soup = BeautifulSoup(html)
print(soup)
```

运行之后，结果会警告

```
'''
GuessedAtParserWarning: No parser was explicitly specified, 
so I'm using the best available HTML parser for this system ("lxml"). 
This usually isn't a problem, but if you run this code on another system, 
or in a different virtual environment, 
it may use a different parser and behave differently.
'''
```

![image-20231202123433037](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/02/656ab3decaf79.png)

这是以为Python是跨平台的，考虑到不同的操作系统，不同的环境，建议指定解析器。

对于html字符串，推荐使用`“html.parser”`或者`"lxml"`

```python
# 2.指定解析器
soup = BeautifulSoup(html,'html.parser')  # lxml
print(soup)
# 格式化输出
print(soup.prettify())
```

### 1.2 四种对象

Beautiful Soup将复杂HTML文档转换成一个复杂的树形结构,每个节点都是Python对象

比如，一下html文档，被转换成下图的树形结构

![节点树](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/02/656abda6e024c.png)

对象主要分为4中

- Tag
- NavigableString
- BeautifulSoup
- Commnent：注释

```python
<html>
<head>
<meta charset="utf-8"/>
<title>
	千锋爬虫课程
</title>
</head>
<body>
<h1>
	我的第一个标题
</h1>
<p class="title" name="first">
	我的第一个段落。
</p>
<a href="https://www.1000phone.com" target="_blank">
	今天星期几？
</a>
</body>
</html>
```

###### 完整代码

```python
from bs4 import BeautifulSoup

html = """
<html>
<head>
<meta charset="utf-8"/>
<title>
	千锋爬虫课程
</title>
</head>
<body>
<h1>
	我的第一个标题
</h1>
<p class="title" name="first">
	我的第一个段落。
</p>
<a href="https://www.1000phone.com" target="_blank">
	今天星期几？
</a>
</body>
</html>
"""
soup = BeautifulSoup(html, "html.parser")
# 1.Tag：标签
# 注意：查找的是给定内容中的第一个符合要求的标签
print(soup.p)
print(soup.a)

# 可以获取name，sttrs
print(soup.p.name)  # 获取标签名称

# 以字典的形式返回，获取标签中的所有的属性
print(soup.p.attrs)  # {'class': ['title'], 'name': 'first'}
# 获取属性的值:两种方式
# 方式一
print(soup.p.attrs['class'])
# 方式二
print(soup.p['class'])

# 2.NavigableString ： String
# 获取标签的文本
print(soup.p.string)

# 3.BeautifulSoup
# soup相当于整个html文档
print(soup.name)
print(soup.attrs)
```

### 1.3 搜索文档树及css选择器【重点掌握】

#### 1.3.1 函数介绍

- 根据选择器进行定位

```python
soup.select(css选择器) : 在整个网页中获取css选择器选中的所有标签，返回的是一个列表，列表中的元素是标签
soup.select_one(css选择器) :在整个网页中获取css选择器选中的第一个标签，返回的是标签对象
标签对象.select(css选择器)  :在指定标签中获取css选择器选中的所有标签，返回的是一个列表，列表中的元素是标签
标签对象.select_one(css选择器) : 在指定标签中获取css选择器选中的第一个标签，返回的是标签对象
```

- 根据元素或内容进行定位

```python
find_all()：根据定位要求获取满足要求的所有标签，返回一个列表
find():根据定位要求获取满足要求的第一个标签
注意：有些时候，在提取标签的时候，不想提取那么多，那么可以使用limit参数。限制提取多少个
```

#### 1.3.2 CSS选择器

- 元素选择器/标签选择器
- ID选择器：结合id属性使用,注意：id属性的值都是**唯一**的
- 类选择器：结合class属性使用，注意：class属性的值**不是唯一**的，所以查找的时候可能会查找出来多个
- 包含选择器：包含先辈后辈标签
- 兄弟标签：
  - +：只匹配相邻的一个标签
  - ~：匹配相邻的所有的标签

#### 1.3.3 代码解析

以以下的HTML文档为例

```python
"""
<table class="tablelist" cellpadding="0" cellspacing="0">
 <tbody>
     <tr class="h">
         <td class="l" width="374">职位名称</td>
         <td>职位类别</td>
         <td>人数</td>
         <td>地点</td>
         <td>发布时间</td>
     </tr>
     <tr class="even">
         <td class="l square"><a target="_blank" href="position_detail.php?id=31236&keywords=python&tid=87&lid=2218">SNG16-腾讯音乐运营开发工程师（深圳）</a></td>
         <td>技术类</td>
         <td>2</td>
         <td>深圳</td>
         <td>2017-11-25</td>
     </tr>
     <tr class="odd">
         <td class="l square"><a target="_blank" href="position_detail.php?id=31235&keywords=python&tid=87&lid=2218">SNG16-腾讯音乐业务运维工程师（深圳）</a></td>
         <td>技术类</td>
         <td>1</td>
         <td>深圳</td>
         <td>2017-11-25</td>
     </tr>
     <tr class="even">
         <td class="l square"><a target="_blank" href="position_detail.php?id=34531&keywords=python&tid=87&lid=2218">TEG03-高级研发工程师（深圳）</a></td>
         <td>技术类</td>
         <td>1</td>
         <td>深圳</td>
         <td>2017-11-24</td>
     </tr>
     <tr class="odd">
         <td class="l square"><a target="_blank" href="position_detail.php?id=34532&keywords=python&tid=87&lid=2218">TEG03-高级图像算法研发工程师（深圳）</a></td>
         <td>技术类</td>
         <td>1</td>
         <td>深圳</td>
         <td>2017-11-24</td>
     </tr>
     <tr class="even">
         <td class="l square"><a target="_blank" href="position_detail.php?id=32217&keywords=python&tid=87&lid=2218">15851-后台开发工程师</a></td>
         <td>技术类</td>
         <td>1</td>
         <td>深圳</td>
         <td>2017-11-24</td>
     </tr>
     <tr class="odd">
         <td class="l square"><a id="test" class="test" target='_blank' href="position_detail.php?id=34511&keywords=python&tid=87&lid=2218">SNG11-高级业务运维工程师（深圳）</a></td>
         <td>技术类</td>
         <td>1</td>
         <td>深圳</td>
         <td>2017-11-24</td>
     </tr>
 </tbody>
</table>
"""
```

##### 1.3.3.1 select()和select_one()

###### a. 元素选择器

- 格式：`select("标签名称")`，返回的是一个列表

比如，获取所有的tr标签

```python
trs = soup.select("tr")
print(trs)
```

select()方法返回值是一个列表，如果我们想要拿到第二个标签的值，则可以

```python
tr_1 = soup.select("tr")[1]
print(tr_1)
```

再比如，获取所有a标签的href属性值。那么我们可以先先获取所有的a标签

```python
a_list = soup.select("a")
```

再筛选出href的属性值

```python
for a in a_list:
    print(a['href'])
```

###### b. ID选择器

- 格式：`select("#id属性的值")`
- 注意：id属性的值一般是唯一的，所以用ID选择器匹配到的标签对象只有一个

比如：获取idid为test的标签，有以下三种方式

```python
# 方式一：
a1 = soup.select("#test")
print(a1)
# 方式二
a2 = soup.select("a[id='test']")
print(a2)
# 方式三,明确标签的话，直接标签名
a3 = soup.select('a#test')
print(a3)
```

但是，ID选择器都是唯一的，我们使用select()查找的最后结果都是一个列表，取出其中的元素，还需要list[0]，所以，用ID选择器匹配标签，可以使用select_one()

对应的select_one()方法也有三种写法

```python
# 方式一：
a1 = soup.select_one("#test")
print(a1)
# 方式二
a2 = soup.select_one("a[id='test']")
print(a2)
# 方式三,明确标签的话，直接标签名
a3 = soup.select_one('a#test')
print(a3)
```

###### c. 类选择器

- 格式：`select(".class属性的值")`

比如：获取class属性为even的标签，类选择器和ID选择器相识，同样有三种写法

```python
# 方式一
trs = soup.select('.even')
print(trs)
# 方式二
trs2 = soup.select("tr[class='even']")
print(trs2)
# 方式三
trs3 = soup.select("tr.even")
print(trs3)
```

###### d. 包含选择器

- 包含选择器：父子选择器（使用 >）和后代选择器(使用 空格)

- 父子选择器一定要按照标签的嵌套顺序书写
- 后代选择器不一定按照标签的嵌套顺序书写，哪怕中间有隔代情况，也可以匹配到，只要是包含关系即可

比如找到td标签，如果选择父子选择器的话

```python
tds = soup.select("table > tbody > tr > td")  # 最终目的是为了找到td，前面的标签相当于条件
print(tds)
```

如果使用后代选择器的话

```python
tds = soup.select("table td")
print(tds)
```

###### e. 复合选择器

- 复合选择器，即前面的几种基础标签选择器混合使用

比如找到table标签中td标签中的ID为test的标签

```python
a = soup.select("table td > #test")
print(a)
# 同理，可以使用select_one()
a = soup.select_one("table td > #test")
print(a)
```

##### 1.3.3.2 find()和find_all()

find()和find_all()实现效果与select()和selec_onet()一致，只是写法有所不同

获取所有的tr标签

```python
trs = soup.find_all('tr')
print(trs)
```

获取所有class为even的tr标签，两种方式

```python
# 方式一
trs1 = soup.find_all('tr', class_='even')
print(trs1)
# 方式二
trs2 = soup.find_all('tr', attrs={'class': 'even'})
print(trs2)
```

将所有id属性值为test，class属性值为test的a标签匹配出来

```python
# 方式一
a_list = soup.find_all('a', class_='test', id='test')
print(a_list)
# 方式二
a_list2 = soup.find_all('a', attrs={'class': "test", 'id': 'test'})
print(a_list2)
```

获取所有a标签的href属性、标签的文本

```python
alist = soup.find_all('a')
for a in alist:
    # 方式一
    print(a['href'])
    # 方式二
    print(a.attrs['href'])
    # 获取a标签的文本
    print(a.text)
```

##### 1.3.3.3 总结

select()和find_all()：匹配所有符合条件的标签，返回一个列表，即使只有一个元素，返回的也是一个列表
select_one()和find()：匹配一个符合条件的标签对象

###### 完整代码

```python
from bs4 import BeautifulSoup

html = """
<table class="tablelist" cellpadding="0" cellspacing="0">
 <tbody>
     <tr class="h">
         <td class="l" width="374">职位名称</td>
         <td>职位类别</td>
         <td>人数</td>
         <td>地点</td>
         <td>发布时间</td>
     </tr>
     <tr class="even">
         <td class="l square"><a target="_blank" href="position_detail.php?id=31236&keywords=python&tid=87&lid=2218">SNG16-腾讯音乐运营开发工程师（深圳）</a></td>
         <td>技术类</td>
         <td>2</td>
         <td>深圳</td>
         <td>2017-11-25</td>
     </tr>
     <tr class="odd">
         <td class="l square"><a target="_blank" href="position_detail.php?id=31235&keywords=python&tid=87&lid=2218">SNG16-腾讯音乐业务运维工程师（深圳）</a></td>
         <td>技术类</td>
         <td>1</td>
         <td>深圳</td>
         <td>2017-11-25</td>
     </tr>
     <tr class="even">
         <td class="l square"><a target="_blank" href="position_detail.php?id=34531&keywords=python&tid=87&lid=2218">TEG03-高级研发工程师（深圳）</a></td>
         <td>技术类</td>
         <td>1</td>
         <td>深圳</td>
         <td>2017-11-24</td>
     </tr>
     <tr class="odd">
         <td class="l square"><a target="_blank" href="position_detail.php?id=34532&keywords=python&tid=87&lid=2218">TEG03-高级图像算法研发工程师（深圳）</a></td>
         <td>技术类</td>
         <td>1</td>
         <td>深圳</td>
         <td>2017-11-24</td>
     </tr>
     <tr class="even">
         <td class="l square"><a target="_blank" href="position_detail.php?id=32217&keywords=python&tid=87&lid=2218">15851-后台开发工程师</a></td>
         <td>技术类</td>
         <td>1</td>
         <td>深圳</td>
         <td>2017-11-24</td>
     </tr>
     <tr class="odd">
         <td class="l square"><a id="test" class="test" target='_blank' href="position_detail.php?id=34511&keywords=python&tid=87&lid=2218">SNG11-高级业务运维工程师（深圳）</a></td>
         <td>技术类</td>
         <td>1</td>
         <td>深圳</td>
         <td>2017-11-24</td>
     </tr>
 </tbody>
</table>
"""

# 创建BeautifulSoup对象
soup = BeautifulSoup(html, "html.parser")

# 1.select()和select_one()
# 1.1 元素选择器/标签选择器，格式：select("标签名称"),返回的是一个列表

# 获取所有的tr标签
trs = soup.select("tr")
print(trs)

# 拿到第二个tr标签
tr_1 = soup.select("tr")[1]
print(tr_1)

print("=" * 30)
# 获取所有a标签的href属性值
# 先获取所有的a标签
a_list = soup.select("a")
# 筛选出href的属性值
for a in a_list:
    print(a['href'])

# 1.2 ID选择器，格式：select("#id属性的值")
# 注意：id属性的值一般是唯一的，所以用ID选择器匹配到的标签对象只有一个
# 获取id为test的标签
# 方式一：
a1 = soup.select("#test")
print(a1)
# 方式二
a2 = soup.select("a[id='test']")
print(a2)
# 方式三,明确标签的话，直接标签名
a3 = soup.select('a#test')
print(a3)

"""
问题：ID选择器都是唯一的，我们使用select()查找的最后结果都是一个列表，取出其中的元素，还需要list[0]
所以，用ID选择器匹配标签，可以使用select_one()
"""
# 方式一：
a1 = soup.select_one("#test")
print(a1)
# 方式二
a2 = soup.select_one("a[id='test']")
print(a2)
# 方式三,明确标签的话，直接标签名
a3 = soup.select_one('a#test')
print(a3)

# 1.3.类选择器    格式：select(".class属性的值")
# 获取class属性为even的标签
# 方式一
trs = soup.select('.even')
print(trs)
# 方式二
trs2 = soup.select("tr[class='even']")
print(trs2)
# 方式三
trs3 = soup.select("tr.even")
print(trs3)

print("=" * 30)

# 1.4.包含选择器：父子选择器（使用 >）和后代选择器(使用 空格)
# 注意：父子选择器一定要按照标签的嵌套顺序书写
# 找到td标签
tds = soup.select("table > tbody > tr > td")  # 最终目的是为了找到td，前面的标签相当于条件
print(tds)

# 注意：后代选择器不一定按照标签的嵌套顺序书写，哪怕中间有隔代情况，也可以匹配到，只要是包含关系即可
tds = soup.select("table td")
print(tds)

# 1.5 复合选择器，即前面的几种基础标签选择器混合使用
# 找到table标签中td标签中的ID为test的标签
a = soup.select("table td > #test")
print(a)
# 同理，可以使用select_one()
a = soup.select_one("table td > #test")
print(a)

# 2.find()和find_all()
# 1.获取所有的tr标签
trs = soup.find_all('tr')
print(trs)

# 2.获取所有class为even的tr标签
# 方式一
trs1 = soup.find_all('tr', class_='even')
print(trs1)
# 方式二
trs2 = soup.find_all('tr', attrs={'class': 'even'})
print(trs2)

print("+" * 30)
# 将所有id属性值为test，class属性值为test的a标签匹配出来
# 方式一
a_list = soup.find_all('a', class_='test', id='test')
print(a_list)
# 方式二
a_list2 = soup.find_all('a', attrs={'class': "test", 'id': 'test'})
print(a_list2)

# 4.获取所有a标签的href属性
alist = soup.find_all('a')
for a in alist:
    # 方式一
    print(a['href'])
    # 方式二
    print(a.attrs['href'])
    # 获取a标签的文本
    print(a.text)


"""
select()和find_all()：匹配所有符合条件的标签，返回一个列表，即使只有一个元素，返回的也是一个列表
select_one()和find()：匹配一个符合条件的标签对象
"""
```

