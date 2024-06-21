# Python发送邮件

## 一、概念

SMTP(Simple Mail Transfer Protocol)，即简单邮件传输协议,它是一组用于由源地址到目的地址传送邮件的规则，由它来控制信件的中转方式。 

python的smtplib提供了一种很方便的途径发送电子邮件。它对smtp协议进行了简单的封装。

Python内置对SMTP的支持，可以发送纯文本邮件、HTML邮件以及带附件的邮件。

Python对SMTP支持有smtplib和email两个模块，email负责构造邮件，smtplib负 责发送邮件。

## 二、邮件服务器设置

发送邮件之前，必需要对邮箱进行设置，邮箱需要开启SMTP服务

> 以163邮箱为例

- 选取邮箱设置

![image-20231031122919575](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/31/654082a0ad40e.png)

- 开启smtp协议

![image-20231031123412342](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/31/654083c4ed8af.png)

- 打开网易邮箱大师，扫码发送短信获取授权码

![image-20231031123511868](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/31/654084002d3a0.png)

- 获取邮箱授权码

![image-20231031123530974](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/31/65408443bf3a8.png)

## 三、发送邮件流程

### 3.1 登录邮箱

- 导入模块

```python
import smtplib 
```

- 连接邮箱服务器
  - 服务器地址:smtp.163.com(163邮箱)、smtp.qq.com(qq邮箱) 
  - 邮箱服务端口:465或者25

```python
连接对象 = smtplip.SMTP_SSL(服务器器地址, 邮箱服务端口)
```

- 登录邮箱

```python
连接对象.login(邮箱账号, 授权码)
```

### 3.2 准备数据

​	数据指的需要发送的内容。邮件内容的构建需要涉及到另外一个库email，它可以用来构建邮件主题以及各种形式的邮件内容，包括文字内容、图片内容、html内容、附件等

- 导入模块

```python
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.header import Header
```

- 创建邮件对象

```
邮件对象 = MIMEMultipart()
```

- 设置邮件主题

```
主题对象 = Header(邮件标题, 编码方式).encode()
```

- 设置邮件发送者

```python
# 设置邮件发送者
邮件对象['From'] = '用户名'
# 设置邮件接受者
邮件对象['To'] = '收件人1;收件人2;收件人3...' 
# 添加邮件内容
文字内容对象 = MIMEText(内容, 类型, 编码方式)
    # - 内容:就是文字符串串
    # - 类型:plain(简单的文字内容)、html(超文本)
邮件对象.attach(文字对象)
```

### 3.3 发送邮件

```
连接对象.sendmail(发件人, 收件人, 邮件对象.as_string()) 
连接对象.quit()
```

## 四、实现

### 4.1 发送文本

```python
# 导入包
# 发送邮件
import smtplib
# email：构建邮件，MIMEText：文本对象
from email.mime.text import MIMEText


# 构建函数
def send_data():
    # 1.准备工作：登录邮箱
    # smtp服务器地址
    mail_host = "smtp.163.com"
    # 发送方的邮箱账号
    mail_sender = "weixuqing3012@163.com"
    # 发送方的授权码，注意不是邮箱的登录密码，而是开启smtp协议时的授权码
    mail_pwd = "KDIJFPRVUVOICSCR"
    # 连接邮箱对象
    smtp = smtplib.SMTP_SSL(mail_host, 465)
    # 登录邮箱
    smtp.login(mail_sender, mail_pwd)

    # 2.准备数据：构建邮件内容
    # 正文内容
    data = MIMEText("2023/10/31,hello Python,Python发送邮件")
    # 邮件主题/标题
    data["Subject"] = "10.36Python发送邮件测试"
    # 发送方
    data["From"] = mail_sender
    # 接收方
    receivers = ["1515323189@qq.com", "weixuqing3012@outlook.com"]
    data["To"] = ";".join(receivers)

    # 3.发送邮件
    # 发送
    smtp.sendmail(mail_sender, receivers, data.as_string())
    # 退出
    smtp.quit()


if __name__ == '__main__':
    send_data()

```

### 4.2 发送html

​	有时候会涉及到发送 html邮件，html格式的邮件本质还是文本格式的邮件，所有文件的构建方式和普通文本文件的构建方式差不多

​	只需要将准备文章内容部分修改即可

```python
# -*- coding: utf-8 -*-
# @Time : 2023/10/31 10:26
# @Author : Eden Wei
# @FileName: 05_发送html邮件.py
# @Software: PyCharm 2023.2.1 (Professional Edition)
# 导入包
# 发送邮件
import smtplib
# email：构建邮件，MIMEText：文本对象
from email.mime.text import MIMEText
# 构建多个内容
from email.mime.multipart import MIMEMultipart


# 构建函数
def send_data():
    # 1.准备工作：登录邮箱
    # smtp服务器地址
    mail_host = "smtp.163.com"
    # 发送方的邮箱账号
    mail_sender = "weixuqing3012@163.com"
    # 发送方的授权码，注意不是邮箱的登录密码，而是开启smtp协议时的授权码
    mail_pwd = "KDIJFPRVUVOICSCR"
    # 连接邮箱对象
    smtp = smtplib.SMTP_SSL(mail_host, 465)
    # 登录邮箱
    smtp.login(mail_sender, mail_pwd)

    # 2.准备数据：构建邮件内容
    # 正文内容
    # 创建内容对象
    data = MIMEMultipart()
    # 内容
    html_str = """<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>邮件正文部分</h1>
<table width="500" border="1" cellspacing="0">
    <tr>
        <td>张三</td>
        <td>10</td>
        <td>male</td>
        <td>唱歌</td>
    </tr>
    <tr>
        <td>李四</td>
        <td>12</td>
        <td>female</td>
        <td>跳舞</td>
    </tr>
    <tr>
        <td>王五</td>
        <td>15</td>
        <td>male</td>
        <td>弹琴</td>
    </tr>
</table>
<ul>
    <li>人事部</li>
    <li>行政部</li>
    <li>财务部</li>
    <li>市场部</li>
</ul>
</body>
</html>"""
    # 将内容绑定到内容对象中
    data.attach(MIMEText(html_str, "html", "utf-8"))
    # 邮件主题/标题
    data["Subject"] = "10.36Python发送html内容邮件测试"
    # 发送方
    data["From"] = mail_sender
    # 接收方
    receivers = ["1515323189@qq.com", "weixuqing3012@outlook.com"]
    data["To"] = ";".join(receivers)

    # 3.发送邮件
    # 发送
    smtp.sendmail(mail_sender, receivers, data.as_string())
    # 退出
    smtp.quit()


if __name__ == '__main__':
    send_data()
```

### 4.3 发送图片

需要重写的部分为下：

新增一个导入模块

```python
# 构建图片
from email.mime.image import MIMEImage
```

```python
    # 2.准备数据：构建邮件内容
    # 正文内容
    # 创建内容对象
    data = MIMEMultipart()
    # 邮件主题/标题
    data["Subject"] = "10.36Python发送图片内容邮件测试"
    # 发送方
    data["From"] = mail_sender
    # 接收方
    receivers = ["1515323189@qq.com", "weixuqing3012@outlook.com"]
    data["To"] = ";".join(receivers)

    # 将图片构建成html
    # 读取文件内容
    with open("data/logo.png", "rb") as f:
        img_data = f.read()
    # 构建图片对象
    img = MIMEImage(img_data)
    img.add_header("Content-ID", "testimg")  # testimg用来在html中识别图片的ID
    html_str = """
    <body>
        <h2>这是一个带有图片的邮件</h2>
        <p>
         <img src="cid:testimg">
        </p>
    </body>
     """
    # 将内容绑定到内容对象中
    data.attach(MIMEText(html_str, "html", "utf-8"))
    data.attach(img)
```

### 4.4 发送文件

```python
# 发送邮件
import smtplib
# email：构建邮件，MIMEText：文本对象
from email.mime.text import MIMEText
# 构建多个内容
from email.mime.multipart import MIMEMultipart
```

```python
    # 2.准备数据：构建邮件内容
    # 正文内容
    # 创建内容对象
    data = MIMEMultipart()
    # 邮件主题/标题
    data["Subject"] = "10.36Python发送附件邮件测试"
    # 发送方
    data["From"] = mail_sender
    # 接收方
    receivers = ["1515323189@qq.com", "weixuqing3012@outlook.com"]
    data["To"] = ";".join(receivers)

    # 构建内容
    with open(r"data/致橡树.txt", 'r', encoding='gbk') as f:
        content = f.read()
    part = MIMEText(content, 'base64', 'gbk')
    # 添加到右键附件
    part['Content-Disposition'] = 'attachment;filename="zhixiangshu.txt"'
    # 将内容绑定到内容对象中
    data.attach(part)
```