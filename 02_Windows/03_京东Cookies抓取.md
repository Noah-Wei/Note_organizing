# 京东Cookies抓取

## 前言

​		因为买了一个服务器，总得想做得什么，然后就发现了可以跑青龙脚本拿京豆，但是在青龙面板中需要用到京东Cookies，因此来记录一下

## 一、手机抓取——Alook浏览器

> 本操作只需要在浏览器上执行，无需理会弹窗信息，如：跳转到京东APP
>
> 手机端抓取Cookies的方式很多，本文只介绍Alook这一种，其余方式自行去[百度]([百度一下，你就知道 (baidu.com)](https://www.baidu.com/))

### 1、下载应用

手机下载`alook浏览器`，安卓免费，ios需要6元

![image-20220621221527896](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1d27f6f7da.png)

### 2、打开网址

打开京东触屏版网页 https://m.jd.com，并点击任务栏最右边的——>`未登录`

![image-20220621221600941](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1d2a05be35.png)

### 3、登录账号

登录个人账号，登录完成后如下图所示

![image-20220621221754897](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1d3124ceac.png)

### 4、打开Cookies（一）

然后依次点击浏览器任务栏的——>点击`三条杠按钮`——>右滑——>点击`工具箱按钮`

![image-20220621222140700](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1d3f41ed83.png)![image-20220621222030582](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1d3ae01f2c.png)![image-20220621222301368](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1d444b5b2d.png)

### 5、打开Cookies（二）

点击`开发者工具`——>点击`Cookies`，就能看到cookies信息了

![image-20220621222428241](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1d49bc7762.png)![image-20220621222456863](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1d4b84336e.png)

### 6、复制Cookies

点击`拷贝`按钮，然后Cookies的提取就完成了

> Cookies中包含的信息很多，一般青龙脚本挂京东所需要用的信息只需要两个。分别是**pt_pin** 和 **pt_key** 两个参数。

![image-20220621222735568](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1d55704f0d.png)

### 7、（可选操作）提取关系参数

Cookies中包含的信息很多，一般青龙脚本挂京东所需要用的信息只需要两个：分别是**pt_pin** 和 **pt_key** 两个参数，并且这两个参数是连在一起的。所以只需要将Cookie复制到文本中，并查找**pt_pin**即可

![image-20220621223607666](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1d757900bc.png)

### 8、操作完成

至此，手机端抓取完成

## 二、电脑端抓取——以Chrome浏览器为例

> 电脑端可选择的浏览器很多，如：Edge，Chrome，Firefox等等，操作方式一样，也许差距就是开发者模式中的语言不同

### 1、打开网址

用上述浏览器在 PC 端打开京东触屏版网页 https://m.jd.com

### 2、打开开发者工具（F12）

按键盘 F12 键打开开发者工具，切换到`网络（Network）`标签，然后点下图中的图标

![image-20220621224805689](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1da255455f.png)

### 3、登录账号

此时默认为未登录账号，如已登录请忽略此步骤，使用手机短信验证码登录

> 使用手机短信验证码登录(<font color="red">此方式 cookie 有效时长大概 31 天</font>，其他登录方式比较短)

### 4、获取Cookies

- 点击**Network（网络）**按钮——>**刷新界面**——>查看Name栏目中的**newhome.action**文件，文件名可能每个人会不一样，但是文件前缀部分相同——>点击**Headers**——>查看**Cookies**——>点击默认全选，并复制

- 操作如下图所示

![image-20220621225315344](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/21/62b1db5ae8f74.png)

### 5、（可选操作）

本部分内容和之前一样，详情请上划查看

## 三、警告

### <font color='red'>**cookie 是私密内容，请不要随意发送**</font>

