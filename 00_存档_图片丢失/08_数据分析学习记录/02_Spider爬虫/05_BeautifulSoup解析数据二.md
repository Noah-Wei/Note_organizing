# BeautifulSoup解析数据二

## 一、添加进度条

在解析多页数据的时候，可能时间比较长，我们可以给程序添加一个进度条，用来观察程序运行的状态

这个就需要用到一个第三方库`tqdm`

### 1.1 tqdm说明

- `tqdm`是一个用来表示进度条的模块

- `tqdm`是一个第三方库，如果没有则需要先安装

  ```python
  pip install tqdm
  ```

- 使用该模块，运行时，代码中不要出现`print`（鱼和熊掌不可兼得）

### 1.2 使用案例

以前面的`BS4解析案例：解析中文新闻网`为例，只需要修改两处代码即可

**第一处：**

导入包

```
from tqdm import tqdm
```

**第二处：**

在原来的循环页面处的代码，使用`tqdm`

- 源代码

```python
for i in range(1, 11):
```

- 修改后代码

其中的`desc`参数为文字补充说明

```python
for i in tqdm(range(1, 11), desc="这是一个进度条"):
```

运行后，查看效果

![image-20231203190609610](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/03/656c6127dca0c.png)

### 1.3 完整代码

> 解析中国新闻网，1-10页内容，并且数据持久化，保存到.xlsx文件中

```python
import time,random,openpyxl,requests
from bs4 import BeautifulSoup
from tqdm import tqdm


# 1.获取数据
def get_data():
    for i in tqdm(range(1, 11), desc="这是一个进度条"):
        url = f"https://www.chinanews.com/scroll-news/news{i}.html"
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36'
        }
        time.sleep(random.randint(3, 6))
        resp = requests.get(url, headers=headers)
        if resp.status_code == 200:
            resp.encoding = 'utf-8'
            parser_data(resp.text)
        else:
            pass


# 2.解析数据
def parser_data(data):
    soup = BeautifulSoup(data, 'html.parser')
    li_list = soup.select('div.content_list li')
    data_list = []
    for li in li_list:
        if li.select_one('.dd_lm > a') is not None:
            news_type = li.select_one('.dd_lm > a').text
            news_title = li.select_one('.dd_bt > a').text
            news_time = li.select_one('.dd_time').text
            news_link = 'https://www.chinanews.com/' + li.select_one('.dd_bt > a').attrs['href']
            data_list.append([news_type, news_title, news_link, news_time])
    save_data(data_list)


# 3.存储数据
def save_data(data_list):
    for data in data_list:
        sheet.append(data)


if __name__ == '__main__':
    wb = openpyxl.Workbook()
    sheet = wb.active
    sheet.append(["类型", "标题", "链接", "时间"])
    get_data()
    wb.save("中国新闻网2.xlsx")
```

## 二、爬取接口数据

### 2.1 简介

​	API接口是负责传递数据的，在现今已存在的网站中，除了极个别非常古老的网站，大部分的网站都会采用API进行数据的传输。

​	那么为什么API接口这么受欢迎呢，那当然是其带来了很多的好处，最直观的便是节省了开发的大量时间、大量金钱。举个例子：一个编程团队想制作一个游戏，在这个游戏里有付费的功能，那么就需要有一个支付平台，而众所周知，支付平台是需要有很大能力去保证安全且需要一定的资金还有资质的，普通的团队压根搞不起啊，所以，这个团队，就需要用到“API接口”，他可以接入其他的支付平台API接口实现支付功能，这样就省去了很多的麻烦。所以，不例外的我们要获取的数据也是可以通过API接口传递到页面中的，那么如何利用好API接口，这便是我们要学习的了。

​	API返回的是一个JSON，而Python可以直接和JSON交互 

API接口的构造：

- API由请求地址和请求参数构成，请求地址和请求参数
- 使用`?`连接，请求参数写成`key=value`的形式（键值对形式），
- 如果有多个请求参数，使用`&`连接。

### 2.2 如何查找数据接口

> 建议使用谷歌浏览器

**步骤**

> 以：lol页面为例：https://lol.qq.com/data/info-heros.shtml

- 第一步：右键 ——> 检查 ——> Network ——> Fetch/XHR ——> 刷新网页
- 第二步：通过第一步的操作之后，没有结果则说明该网页没有API接口；如果有结果，依次点击每条，需要查看Preview的结果进行确认，如果没有需要的数据则也是无效接口
- 第三步：经过第二步的确认，如果时我们需要的接口，则点击Headers ——> General ——> Request url的值就是我们需要的接口
- 第四步：将url复制到浏览器，获取到的就是json数据，此时可以借助于json在线解析工具进行解析

### 2.3 应用

> 以lol页面为例
>
> ​	URL：https://lol.qq.com/data/info-heros.shtml
>
> ​	接口URL：https://game.gtimg.cn/images/lol/act/img/js/heroList/hero_list.js

#### 2.3.1 操作步骤

- **第一步：右键 ——> 检查 ——> Network ——> Fetch/XHR ——> 刷新网页**

![image-20231203193832782](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/03/656c68bb8c382.png)

- **第二步：通过第一步的操作之后，没有结果则说明该网页没有API接口；如果有结果，依次点击每条，需要查看Preview的结果进行确认，如果没有需要的数据则也是无效接口**

  - **点击第一个`PCSiteNavsCfg.js?_=1701603401976`，查看，没有数据并不是接口**

  ![image-20231203194116138](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/03/656c695bbcb85.png)

  - **点击第二个`hero_list.js`，查看，有数据，不是接口**

  ![image-20231203194142607](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/03/656c697739d9d.png)

- **第三步：经过第二步的确认，如果时我们需要的接口，则点击Headers ——> General ——> Request URL的值就是我们需要的接口**

URL：https://game.gtimg.cn/images/lol/act/img/js/heroList/hero_list.js

![image-20231203195334017](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/03/656c6c3d7096e.png)

- **第四步：将url复制到浏览器，获取到的就是json数据，此时可以借助于json在线解析工具进行解析**

![image-20231203195525698](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/03/656c6cae431b2.png)

#### 2.3.2 代码操作

##### a.对json接口发送请求

对json接口发送请求，首先需要获取到json接口的URL，然后就可以直接解析，并不需要伪装成浏览器

```python
url = "https://game.gtimg.cn/images/lol/act/img/js/heroList/hero_list.js"
resp = requests.get(url)
```

对json接口请求完毕之后，直接通过json()函数进行反序列化

```python
result = resp.json()
print(result)
print(type(result))
```

配合之前json在线解析工具查看，可以发现，数据是一个字典，数据都保存在`key=“hero"`中，所以借助for循环可以获取每个英雄的信息，

![image-20231203201822128](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/03/656c720d1f10d.png)

也可以根据需要获取其他字段

![image-20231203201902289](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/03/656c7235b3bf6.png)

```python
for info in result["hero"]:
    # 获取每个英雄的信息
    print(info)
    # 获取每个英雄的name，title
    print(info["name"], info["title"])
```

##### b. 下载网络图片，网络音视频

借助在线工具，可以发现`selectAudio`字段，对应的内容是一个URL，去掉转移字符之后，打开是一个音频文件，可以将其在下载下来

![image-20231203202122267](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/03/656c72c1b525a.png)

图片、语音和视频文件，在Python处理中，都属于二进制文件，所以处理方法都一致，只需要找到对应的URL和修改最后保存的文件名即可

```python
# 图片，音视频都属于二进制文件
url = "https://game.gtimg.cn/images/lol/act/img/vo/choose/1.ogg"
resp = requests.get(url)
with open("配音.ogg","wb") as f:
    f.write(resp.content)
```

##### c. 总结

在前面的使用中，我们分别使用了`resp.text:`、`resp.json()`和`resp.content`，再次对使用场景做一个说明

- `resp.text`：当resp是**html字符串**时使用
- `resp.json()`：当resp是**json字符串**时使用
- `resp.content`：当resp是**字节（二进制**）时使用

##### d. 完整代码

```python
import requests

# 1.对json接口发送请求
# 不需要伪装成浏览器
url = "https://game.gtimg.cn/images/lol/act/img/js/heroList/hero_list.js"
resp = requests.get(url)

# 对json接口请求完毕之后，直接通过json()函数进行反序列化
result = resp.json()
print(result)
print(type(result))


# 2.下载网络图片，网络音视频
# 图片，音视频都属于二进制文件
url = "https://game.gtimg.cn/images/lol/act/img/vo/choose/1.ogg"
resp = requests.get(url)
with open("配音.ogg","wb") as f:
    f.write(resp.content)

```

