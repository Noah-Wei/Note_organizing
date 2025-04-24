# 爬虫简介&requests使用&正则解析数据

## 一、爬虫简介

### 1.1 什么是爬虫 

爬虫，即网络数据采集，**是数据分析的第一步：获取数据**

简言之，爬虫可以帮助我们把网站上的信息快速、批量的提取并保存下来。

爬虫(crawler)也经常被称为网络蜘蛛(spider)，是**按照一定的规则自动浏览网站并获取所需信息的机器人程序(自动化脚本代码)**，被广泛的应用于互联网搜索引擎和数据采集。使用过互联网和浏览器的人都知道，网页中除了供用户阅读的文字信息之 外，还包含一些超链接，网络爬虫正是通过网页中的超链接信息，不断获得网络上其他页面的地址，然后持续的进行数据采集。

正因如此，网络数据采集的过程就像一个爬虫或者蜘蛛在网络上漫游， 所以才被形象的称为爬虫或者网络蜘蛛 

### 1.2 爬虫有什么用

比如，我们在网上看到很多精美的图片，想要保存下来，但是一次次的右键另存为九显得非常的费时费力，那么我们就可以利用爬虫将这些图片快速的抓取下来，极大地节省时间和精力。

比如，我们像收集一些新闻，看一下每天都发生了哪些事情，我们可以写个爬虫把新闻爬取下来，每天运行一次或者设置定时任务定时运行，这样我们就可以不用进入网页就能看到新闻，也可以根据关键词进行热点分析。

再比如，大家抢过的火车票，演唱会门票，茅台等等都可以利用爬虫来实现

### 1.3  爬虫的应用领域

对于大多数的公司而言，即使的获取行业数据和竞对数据是企业生存的重要环节之一，然而对大部分企业来说，数据都是其与生俱来的短板。在这种情况下，合理的利用爬虫来获取数据，并从中提取出有商业价值的信息，对这些企业来说就显得至关重要。

- 新闻引擎
- 新闻聚合
- 社交应用
- 舆情监控
- 行业数据

### 1.4 爬虫的合法性探讨

经常听人说起“爬虫写得好，牢饭吃到饱”，那么编程爬虫程序是否违法呢?关于这个问题，我们可以从以下几个⻆度进行解读。

1. 网络爬虫这个领域目前还属于拓荒阶段，虽然互联网世界已经通过自己的游戏规 则建立起了一定的道德规范，即 `Robots协议`（全称是“网络爬虫排除标准”），但法律部分还在建立和完善中，也就是说，现在这个领域暂时还是灰色地带。
2. “法不禁止即为许可”，如果爬虫就像浏览器一样获取的是前端显示的数据（网页上的公开信息)而不是网站后台的私密敏感信息，就不太担心法律法规的约束，因为目前大数据产业链的发展速度远远超过了法律的完善程度。
3. 在爬取网站的时候，需要限制自己的爬虫遵守 Robots 协议，同时控制网络爬虫程序的抓取数据的速度;在使用数据的时候，必须要尊重网站的知识产权（从Web 2.0时代开始，虽然Web上的数据很多都是由用户提供的，但是网站平台是投入了运营成本的，当用户在注册和发布内容时，平台通常就已经获得了对数据的所有权、使用权和分发权）。如果违反了这些规定，在打官司的时候败诉几率相当高。
4. 适当的隐匿自己的身份在编写爬虫程序时必要的，而且最好不要被对方举证你的爬虫有破坏别人动产(例如服务器)的行为
5. 不要在公网(如代码托管平台)上去开源或者展示你的爬虫代码，这些行为通常会给自己带来不必要的麻烦。

### 1.5 Robots协议

大多数网站都会定义 `robots.txt` 文件，这是一个君子协议，并不是所有爬虫都必须遵守的游戏规则。

可以通过**`https://域名/robots.txt`**查看一个网站的robots协议

如：

- 淘宝的：https://www.taobao.com/robots.txt

- 豆瓣的：https://www.douban.com/robots.txt

![image-20231201134003583](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/01/656971b4d6952.png)

## 二、术语解读

### 2.1 客户端和服务端

本质上，客户端和服务器端都是计算机

客户端(client) ：是面向用户的，但是，客户端存储条件有限

服务器端(server) ：是给面向用户的一端进行服务的，可以存储相应数据

当客户端需要数据的时候，客户端向服务器端发送请求，服务器端（一台计算机）接收到请求，做出响应，把相应数据返回给客户端

![客户端和服务端](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/01/6569701814bd9.png)

### 2.2 请求和响应

发送请求或者响应是两台计算机进行通信的操作，进行通信时要遵守通信三要素: 

1. 通信协议：进行通信交流时要遵守的规则 ——**OSI网络传输七层模型**

   ![4](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/01/6569716f34154.png)

   我们使用到的为应用层，这一层的协议有HTTP协议（超文本传输协议）、FTP协议 （文件传输协议）等

   **注意：现在常见到的HTTPS协议是在HTTP的基础上进行了SSL加密，保证数据传输更加安全一些**

2. IP地址：IP地址是定位计算机的身份标识，常用的是域名（对ip地址进行封装，相比于 ip 地址，方便大家记忆）

3. 端口号：定位到相应服务的标识 （因为一台计算机上是有很多服务的）

#### 2.2.1 HTTP请求

​	HTTP 请求通常是由**请求行、请求头、空行、消息体（请求体）**四个部分构成，如果没有数据发给服务器，消息体就不是必须的部分

**1、请求行**

- 格式

```
请求方式 请求资源路径 协议及其版本号 
```

- 举例

```
GET / HTTP/1.1
```

- 常见的请求方式

![5](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/01/65697d2a4f0cb.png)

常见的一般只有`GET`和`POST`

GET: 获取，一般要在服务器中获取数据，则使用GET，注意：get最大的特点是：传递给服务器的数据在url中显示

​	如：https://www.baidu.com/s?wd=%E4%B8%AD%E5%9B%BD

POST: 上传， 一般向服务器传递数据，如果需要保存或者校验则使用POST

**2、请求头**

- 请求头由若干键值对构成，包含了浏览器、编码方式、首选语言、缓存策略等信息

![image-20231201143051361](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/01/65697d9cf3c19.png)

- 内容说明

```python
# 请求行
GET / HTTP/1.1
# 允许接受的文件类型
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
# 允许接受的文件压缩格式    
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cache-Control: max-age=0
Connection: keep-alive
# 向服务器传递的标识信息，如：用户登录
Cookie: uuid_n_v=v1; uuid=D9049750900F11EE8F3E256A4A5D3F840E4691B568D84D0697A35544BC932820; _csrf=f207e5eee56b08e958ba0d7c48b3ea291cad11dd55324054ac934cc6497be238; _lxsdk_cuid=18c23fcdfdac8-028da29bedf405-26031051-19d826-18c23fcdfdac8; _lxsdk=D9049750900F11EE8F3E256A4A5D3F840E4691B568D84D0697A35544BC932820; Hm_lvt_703e94591e87be68cc8da0da7cbd0be2=1701410824; Hm_lpvt_703e94591e87be68cc8da0da7cbd0be2=1701410835; __mta=245791035.1701410824319.1701410824319.1701410835396.2; _lxsdk_s=18c23fcdfda-cb0-206-eac%7C%7C6
# 请求的服务器的主机域名
Host: www.maoyan.com
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Upgrade-Insecure-Requests: 1
# 发送请求的设备信息（浏览器信息）
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36
sec-ch-ua: "Google Chrome";v="119", "Chromium";v="119", "Not?A_Brand";v="24"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
```

**3、空行**

- 用来分割请求头和请求体的，因为这两个数据都是键值对形式展示的

**4、请求体**

- 如果没有向服务器传递信息的话，这部分是可以为空的
- 这部分存放的是客户输入的信息数据
  - 比如百度搜索时，输入的搜索内容就是我们想要给百度服务器传递的信息，那这个内容就是请求体
  - 比如我们登录的时候输入的账号密码就是我们要向服务器传递的信息，这个也是请求体

#### 2.2.2 HTTP响应

​	HTTP 响应通常是由**响应行、响应头、空行、消息体（响应体）**四个部分构成，其中消息体是服务响应的数据，可能是 HTML页面，也有可能是JSON或二进制数据等

![image-20231201143716450](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/01/65697f1db89db.png)

**1、响应行**

- 响应行中包含了协议版本和响应状态码，比如`HTTP/1.1 200 OK`，响应状态码有很多种，常⻅的如下表所示：
- 响应状态码大类：

| 状态码分类 | 说明                                                         |
| :--------: | :----------------------------------------------------------- |
|    1XX     | 响应中--临时状态码，表示请求已经接受，告诉客户端应该继续请求或者如果它已经完成则忽略它 |
|    2XX     | 成功--表示请求已经被成功接受，处理已完成                     |
|    3XX     | 重定向--重定向到其他地方（让客户端再发起一个请求以完成整个处理） |
|    4XX     | 客户端错误--处理发生错误，责任在客户端，如：客户端的请求一个不存在的资源，客户端未被授权，禁止访问等 |
|    5XX     | 服务器端错误--处理发生错误，责任在服务器端，如：服务端抛出异常，路由出错，HTTP版本不支持等 |

- 常见的响应状态码：

| 状态码 |            英文描述             | 解释                                                         |
| :----: | :-----------------------------: | :----------------------------------------------------------- |
|  200   |               OK                | 客户端**请求成功**，即处理成功                               |
|  302   |              Found              | 只是所请求的资源已移动到由Location响应头给定的URL，浏览器会自动重新访问到这个页面 |
|  304   |          Not Modified           | 告诉客户端，你请求的资源至上次取得后，服务端并未更改，你直接用你本地缓存吧。隐式重定向 |
|  400   |           Bad Request           | 客户端请求有**语法错误**，不能被服务器所理解                 |
|  403   |            Forbidden            | 服务器收到请求，但是**拒绝提供服务**，比如：没有权限访问相关资源 |
|  404   |            Not Found            | **请求资源不存在**，一般是URL输入有误，或者网站资源被删除了  |
|  428   |      Precondition Required      | **服务器要求有条件的请求**，告诉客户端要想访问资源，必须携带特定的请求头 |
|  429   |        Too Many Requests        | **太多请求**，可以限制客户端请求某个资源的数量，配合Retry-After（多长时间后可以请求）响应头一起使用 |
|  431   | Request Header Fields Too Large | **请求头太大**，服务器不愿意处理请求，因为它的头部字段太大。请求可以在减少请求头域的大小后提交 |
|  405   |       Method Not Allowed        | **请求方式有误**，比如：应该用GET请求方式的资源，而用了POST  |
|  500   |      Internal Server Error      | **服务器发生不可预期的错误**，表示服务器出异常了             |
|  503   |       Service Unavailable       | **服务器尚未准备好处理请求**，比如：服务器刚刚启动，还未初始化好 |
|  511   | Network Authentication Required | **客户端需要进行身份验证才能获得网络访问权限**               |

**2、响应头**

- 响应头类似于请求头，是以键值对形式展示数据的，返回内容的格式、编码、传递到客户端的数据标识等
- 内容说明

```python
# 响应行
HTTP/1.1 200 OK
# 内容类型以及编码
Date: Fri, 01 Dec 2023 06:14:44 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 10277
Connection: keep-alive
Strict-Transport-Security: max-age=1800
# 传递给客户端的身份标识
Set-Cookie: token=; path=/; expires=Thu, 01 Jan 1970 00:00:00 GMT; httponly
Set-Cookie: token.sig=aB29lesEJiqdFjuwdj_IQ-OaJxg; path=/; expires=Thu, 01 Jan 1970 00:00:00 GMT; httponly
Set-Cookie: uid=; path=/; expires=Thu, 01 Jan 1970 00:00:00 GMT; httponly
Set-Cookie: uid.sig=hlqKwItixnwRJMmu6X-sP8X3qa8; path=/; expires=Thu, 01 Jan 1970 00:00:00 GMT; httponly
# 内容压缩方式
content-encoding: gzip
```

**3、空行**

- 用来分割响应头和响应体的

**4、响应体**

- 服务器端根据客户的需求返回的数据，可能是HTML文档、也可能是JSON数据或者二进制数据

#### 2.2.3 URL

- URL：统一资源定位符

- 格式为: `协议://域名:端口号/资源路径?key1=value1&key2=value2`

- `?key1=value1&key2=value2 `是GET请求时客户向服务器传递的信息
- value就是传递的数据，key就是服务器端接受数据的字段名
  - 例如：百度搜索python： https://www.baidu.com/s?ie=utf-8&tn=baidu&wd=python

## 三、爬虫的工作流程

Python爬虫的基本流程非常简单，主要可以分为三部分：

1. 获取网络公开数据：requests、selenium、scrapy
2. 解析网页（提取数据）：正则表达式、bs4（基于css选择器的解析器）、lxml（基于xpath语法的解析器）
3. 存储数据：csv文件，Excel文件，数据库等

当然更为高级的爬虫是在数据采集和处理时会使用并发编程和分布式技术，这就需要有调度器(安排线程或者进程执行对应的任务)、后台管理程序(监控爬虫的工作状态 以及检查数据抓取的结果)等的参与

![6](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/01/65697179e6e84.png)

## 四、requests库初识

### 4.1 requests爬取猫眼电影

**主要流程：**

1. 导入包：`requests`

   - requests是一个第三方库
   - Windows安装方式：`pip install requests`
   - Mac 安装方式：`pip3 install requests`

2. 模拟浏览器向服务器发起请求，服务器会返回响应（核心）

   ```python
   url = 'https://www.maoyan.com/films?showType=3'
   resp = requests.get(url)
   print(resp)
   ```

3. 获取响应中包含的信息

   - 实际情况中：返回的响应体中，中文为乱码

   ```python
   print(f"响应状态码：{resp.status_code}")
   print(f"响应头：{resp.headers}")  # 返回一个字典
   
   print(f"响应体：{resp.text}")  # 返回的是一个字符串
   ```

4. 解决方案

   - 方式一：直接修改编码方式

   ```
   resp.encoding = "utf-8"
   print(f"响应头的编码格式：{resp.encoding}")  # utf-8
   print(f"响应体：{resp.text}")
   ```

   - 方式二：保存请求体，然后修改保存数据的解码方式

   ```
   data = resp.content
   print(f"响应体：{data.decode('utf-8')}")
   ```

**分析问题：**

- 问题：网页（中文）发生乱码的原因是什么
  -  中文的编码格式：
    - BGK：相当于国家标准，一个中文是2个字节
    - UTF-8：相当于国际标准，一个中文是3个字节
  - 网页的编码格式：
    -  ISO-8859-1：单字节的编码格式，中文无法识别
  - 综上所诉：原因是编码和解码的方式不同，因为中文是多字节的，用单字节方式解析多字节，则乱码

- 如何解决网页发生乱码问题
  - 重新设置网页的编码格式
  - 网页源代码中一般都规定了charset，则可以同步修改

###### 完整代码

```python
# 1.导入包
import requests

# 2.模拟浏览器向服务器发起请求，服务器会返回响应
# requests.get(url)
url = "https://www.maoyan.com/films?showType=3"
resp = requests.get(url)  # 核心代码
print(resp)  # <Response [200]>

# 3. 获取响应中包含的信息
print(f"响应状态码：{resp.status_code}")
print(f"响应头：{resp.headers}")  # 返回一个字典

#  查看网页的源代码，类型是字符串，默认中文显示不出，乱码
# print(f"响应体：{resp.text}")  # 返回的是一个字符串

# 4.问题及解决方案
"""
a.网页（中文）发生乱码的原因是什么？
    中文的编码格式：
        BGK：相当于国家标准，一个中文是2个字节
        UTF-8：相当于国际标准，一个中文是3个字节
    ISO-8859-1：单字节的编码格式，中文无法识别
    综上所诉：原因是编码和解码的方式不同，因为中文是多字节的，用单字节方式解析多字节，则乱码
"""
print(f"响应头的编码格式：{resp.encoding}")  # ISO-8859-1
print(f"响应体的编码格式：{resp.apparent_encoding}")  # utf-8

"""
b.如何解决网页发生乱码问题？
    重新设置网页的编码格式
    网页源代码中一般都规定了charset，则可以同步修改
"""

# 方式一：直接修改编码方式
# resp.encoding = "utf-8"
# print(f"响应头的编码格式：{resp.encoding}")  # utf-8
# print(f"响应体：{resp.text}")

# 方式二：保存请求体，然后修改保存数据的解码方式
# resp.text：类型是字符串
# resp.content：类型是字节
data = resp.content
print(f"响应体：{data.decode('utf-8')}")
```

### 4.2 设置请求头解决反爬策略

然而，在实际的网站爬取中，有的网站程序一运行，就会被检出这是一个程序，从而被禁止，反爬，比如百度。下面一百度为例

爬取百度网站信息

```python
import requests

url = "https://www.baidu.com"

resp = requests.get(url)

print(f"响应状态码：{resp.status_code}")  # 200
print(f"响应体：{resp.text}") # 内容只有很少的一部分，被反爬了
```

程序运行后，查看结果

![image-20231201171016926](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/01/6569a2fc1290e.png)

可以看到，只有很少的一部分样式内容，网站上的内容并没有被爬取到。所以我们需要设置请求头解决反爬策略

在`requests.get()`方法中有一个参数为`headers`，`header`s的作用是：请求头，伪装爬虫，似的爬虫被伪装成浏览器传参，需要注意的是：必须使用`key=value`的方式传参。
在实际浏览器请求中，查看请求头中，有一个参数为`User-Agent`：作用是表标识浏览器的相关信息，足以处理大多数的网站
注意：书写的时候一定要写成`key:value`的形式，即给两部分内容加上引号

![image-20231201171319705](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/01/6569a3b07f6a4.png)

所以代码需要修改

```
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36"
}
resp = requests.get(url, headers=headers)
```

再次运行，查看结果，可以看到，所有的内容都已经能显示出来。

###### 完整代码

```python
import requests

url = "https://www.baidu.com"
"""
requests.get()方法中有一个参数为headers,传参必须使用key=value的方式传参
headers的作用是：请求头，伪装爬虫，似的爬虫被伪装成浏览器
在实际浏览器请求中，查看请求头中，有一个参数为User-Agent：作用是表标识浏览器的相关信息，足以处理大多数的网站
注意：书写的时候一定要写成key:value的形式，即给两部分内容加上引号
"""
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36"
}
resp = requests.get(url, headers=headers)

print(f"响应状态码：{resp.status_code}")  # 200
# print(f"响应体：{resp.text}")  # 内容只有很少的一部分，被反爬了

resp.encoding = "utf-8"
print(f"响应体：{resp.text}")
```

### 4.3 使用正则解析网页数据

通过上面的方式，我们能够正常的爬取网站，但是，爬取的内容很多，包括了很多无用的信息，比如css样式，所以我们需要对内容进行筛选，筛选出对我们有用的部分。

所以使用正则表达式解析网页数据。比如爬取百度首页的热搜部分

![image-20231201184439030](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/01/6569b91950123.png)

通过前面的方式，我们可以爬取网站的全部内容，通过查看百度热搜关键字，我们可以发现，每一条消息都被包裹在已在`<span>`标签里面，即

![image-20231201184741784](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/01/6569b9ce73341.png)

所以我们只需要定位到该标签，然后提取其中的内容即可。

所以我们使用一个变量`regex_str`来定义这串字符串，并且我们要提取其中的内容，所以我们对内容部分使用正则表达式捕获组（`.*?`），`(.*?)`: 这是一个非贪婪匹配的捕获组，它匹配任意字符零次或多次，直到下一个部分的结束标签 `</span>`。非贪婪匹配使用 `*?` 来表示，它会尽量匹配尽可能少的字符。，所以完整的代码为：

```
regex_str = r'<span class="title-content-title">(.*?)</span>'
```

然后调用`re`模块中的`findall`方法，`findall`方法用于查找字符串中所有匹配给定模式的子字符串。在 `resp.text` 中查找所有匹配 `regex_str` 正则表达式的子字符串，并将它们存储在 `result` 变量中。即

```
result = re.findall(regex_str, resp.text)
```

最后打印`result`变量即可，结果返回一个列表

###### 完整代码

```python
import re

import requests

url = "https://www.baidu.com"

headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36"
}
resp = requests.get(url, headers=headers)

print(f"响应状态码：{resp.status_code}")  # 200
# print(f"响应体：{resp.text}")

# 使用正则进行制定数据的提取
regex_str = r'<span class="title-content-title">(.*?)</span>'
# 注意，在findall中，正则表达式中，如果有分组()，则最终的结果只显示()中匹配到的内容
result = re.findall(regex_str, resp.text)
print(result)
```

### 4.4 爬取并解析豆瓣电影Top250

需求：提取豆瓣电影TOP250第一页的电影数据，并将数据保存到CSV文件中

###### 完整代码

```python
# 导入库
import requests, re, csv


# 1.获取网页数据
def get_html():
    url = 'https://movie.douban.com/top250'
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36'}
    resp = requests.get(url, headers=headers)
    if resp.status_code == 200:
        print(resp.text)
        strip_data(resp.text)
    else:
        print(f"请求失败,{resp.status_code}")


# 2.解析网页数据
def strip_data(html):
    # 电影名称
    names = re.findall(r'<img width="100" alt="(.+?)"', html)
    print(names)
    # 电影评分
    scores = re.findall(r'<span class="rating_num" property="v:average">(\d\.\d)</span>', html)
    print(scores)
    # 评语
    intro = re.findall(r'<span class="inq">(.+?)</span>', html)
    print(intro)

    # 数据的整合
    result = list(map(lambda ele1, ele2, ele3: [ele1, ele2, ele3], names, scores, intro))
    print(result)

    write_csv(result)


# 3.写入csv文件
def write_csv(data):
    with open(r'豆瓣电影Top250.csv', 'w', encoding='utf-8', newline="") as f:
        writer = csv.writer(f)
        writer.writerows(data)
    print("数据保存成功")


if __name__ == '__main__':
    get_html()
```



> ```Python
> # 需求：提取豆瓣电影Top250的电影数据，并将数据保存到csv文件
> 
> import requests,re,csv
> 
> # 1.获取网页数据
> def get_html():
>     url = 'https://movie.douban.com/top250'
>     headers = {
>         'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36'
>     }
>     resp = requests.get(url,headers=headers)
>     if resp.status_code == 200:
>         print(resp.text)
>         strip_data(resp.text)
>     else:
>         print(f'请求失败{resp.status_code}')
> 
> # 2.解析网页数据
> def strip_data(html):
>     # 电影名称
>     names = re.findall(r'<img width="100" alt="(.+?)"',html)
>     print(names)
>     # 评分
>     scores = re.findall(r'<span class="rating_num" property="v:average">(\d\.\d)</span>',html)
>     print(scores)
>     # 评语
>     intro = re.findall(r'<span class="inq">(.+?)</span>',html)
>     print(intro)
>     # 整合
>     result = list(map(lambda ele1,ele2,ele3:[ele1,ele2,ele3],names,scores,intro))
>     write_csv(result)
> 
> # 3.写入csv文件
> def write_csv(data):
>     with open(r'豆瓣电影Top250.csv','w',encoding='utf-8',newline='') as f:
>         writer = csv.writer(f)
>         writer.writerows(data)
>     print('数据保存成功')
> 
> if __name__ == '__main__':
>     get_html()
> ```