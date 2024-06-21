# Requests请求动态数据

## 一、静态页面和动态页面

通俗来讲：

- 静态网页：网页的内容一经发布，除非再进行人为的修改，否则页面内容不会发生改变。
- 动态页面：虽然同样页面代码不发生变化，但是其显示的内容确实可以随着时间环境或者数据操作的结果而发生变化，我们看到的数据本身不存在于网页中，网页在加载过程中数据被插入到了指定位置，这个插入过程使用了JavaScript技术。也就意味着这个数据是从其他地方引入到了网页中，原本发布的页面的源代码中不包含这个数据。

**静态页面**中，用户通过页面操作的过程就是通过浏览器使用HTTP协议向服务器发送一个请求(Request)，告诉服务器我需要展示那个页面，服务器收到请求后，直接根据用户的需求直接从文件系统中取出相应的文件，返回给浏览器，浏览器解析后为用户展示下相应的页面

![image-20231205143701870](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/05/656ec512bc5ac.png)

**动态页面**中，用户通过浏览器发送的请求到达服务器之后，服务器根据请求内容从数据库中调取相应的内容组合成一
个虚拟的文件，然后将文件发送给浏览器，用户才得以看到定制化的内容

![image-20231205145337171](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/05/656ec8ef18a42.png)

## 二、requests请求动态数据

requests使用网址模拟浏览器发送请求，获取到的数据都是静态数据，那么使用requests如何获取动态数据呢?

一般情况下动态资源返回的都是`json`数据，客户端和服务器端数据传递的时候，一般传递的都是`json`数据

### 2.1 JSON

- `json`是轻量级数据 可以被任意编程语言解析，数据类型是基于`Javascript`类型的
- 数据格式是通用的
- JSON中的类型

| JSON中的类型 | JSON类型说明 | 对于Python中的类型                  |
| ------------ | ------------ | ----------------------------------- |
| JSONObject   | json对象     | {key:value} **Python字典**          |
| JOSNArray    | JSON数组     | [1, 2, 3, 4] **Python列表**         |
| Number       | 数字类型     | 12   3.14  **Python中的int  float** |
| String       | 字符串       | 'abc'      **Python中str**          |
| Boolean      | 布尔类型     | true false  **Python中布尔bool**    |
| 空对象       | null         | **Python的None**                    |

### 2.2 requests动态请求

> 以王者荣耀英雄页面URL：https://pvp.qq.com/web201605/herolist.shtml

**需求：获取英雄的技能**

#### 2.2.1 查看URL

**查看JSON**

```
Network ——> Fetch/XH ——> xxx.json ——> Response
```

![image-20231205150051145](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/05/656ecaa29ed19.png)

**获取URL**

```
Network ——> Fetch/XH ——> xxx.json ——> Headers ——> General ——> Request URL
```

![image-20231205150311672](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/05/656ecb2e1d367.png)

#### 2.2.2  分析

> 为什么解析JSON而不是html网页？
>
> ​	因为是动态界面，有的数据是通过json传入，直接解析网页，获取的数据是不全的

使用requests请求数据，然后使用xpath解析数据，所以要导入以下两个包

```python
import requests
from lxml import etree
```

`url`前面已经获取，定义请求动态数据函数，这部分已经很熟练了，只不过要注意的是，我们请求的是`JSON`，而不是`html`，所以直接调用`resp.json()`:获取`json`数据并将`json`反序列化为Python对象，然后打印查看，为了和页面英雄数据，最后在倒序打印输出

```python
def get_data():
    url = "https://pvp.qq.com/web201605/js/herolist.json"
    # 请求json，不需要设置请求头伪装浏览器
    resp = requests.get(url)
    if resp.status_code == 200:
        # 获取json的响应数据，直接调用resp.json()：直接获取json数据并将json反序列化为Python对象
        datas = resp.json()
        # print(datas[::-1])
        # 这时候和网页已经没有任何关系了，对json进行操作
        parse_data(datas[::-1])
```

下面就负责解析数据，通过观察可以发现，英雄的技能在英雄详情页中有，并且，获取的JSON反序列化数据中，有一个字段ename，对应的值，为英雄详情页的URL最后一部分

![image-20231205154526963](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/05/656ed51abec6a.png)

![image-20231205154609606](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/05/656ed53fe8905.png)

所以我们可以先遍历获取每一位英雄的ename，从而获得每一位英雄的详情页URL

```python
for data in datas:
	detail_url = f"https://pvp.qq.com/web201605/herodetail/{data['ename']}.shtml"
```

然后我们对该页面进行请求数据

```python
detail_resp = requests.get(detail_url)
```

如果状态码正确，我们就可以打印文本查看，结果发现中文显示有问题则修改编码

```python
if detail_resp.status_code == 200:
    # print(f"{data['ename']}的数据请求成功")
    print(detail_resp.text) # 获取的是一个个的网页
    detail_resp.encoding = "GBK"
```

下面就是最后一步了，直接配合F12检查元素，定位文本所在位置，因为每一位英雄都有很多技能，所以我们可以使用json()方法拼接

```python
tree = etree.HTML(detail_resp.text)
skill_names = tree.xpath("//p[@class='skill-name']/b/text()")
skill_names = "-".join(skill_names)
```

#### 2.2.3 完整代码

```python
# 导入模块
import requests
from lxml import etree


# 1.获取数据
def get_data():
    url = "https://pvp.qq.com/web201605/js/herolist.json"
    # 请求json，不需要设置请求头伪装浏览器
    resp = requests.get(url)
    if resp.status_code == 200:
        # 获取json的响应数据，直接调用resp.json()：直接获取json数据并将json反序列化为Python对象
        datas = resp.json()
        # print(datas[::-1])
        parse_data(datas[::-1])


# 2.解析数据
def parse_data(datas):
    for data in datas:
        # 获取每个英雄的技能
        # 首先要获取英雄的详情页,通过获取每个英雄的enma,从而得到URL
        detail_url = f"https://pvp.qq.com/web201605/herodetail/{data['ename']}.shtml"
        # print(detail_url)
        
        # 发起请求
        detail_resp = requests.get(detail_url)
        if detail_resp.status_code == 200:
            # print(f"{data['ename']}的数据请求成功")
            # print(detail_resp.text) # 获取的是一个个的网页
            detail_resp.encoding = "GBK"
            tree = etree.HTML(detail_resp.text)
            skill_names = tree.xpath("//p[@class='skill-name']/b/text()")
            skill_names = "-".join(skill_names)
            print(skill_names)


if __name__ == '__main__':
    get_data()
```

### 2.3 总结

requests在解析动态网页的时候，URL最后可以是`..../xxx.json`或者`..../xxx.js`，本质都一样，操作方式完全相同

但是就是会有一点麻烦，每次都需要找到动态接口。

所以，**requests更推荐解析静态网页，解析动态网页则更推荐Selenium**



