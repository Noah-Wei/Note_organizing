# MobaXterm免费版升级专业版

## 前言

**什么是MobaXterm？**

MobaXterm一款强大好用的远程终端登录利器，之前操作远端服务器一直使用的是XShell和Xftp，后来偶得一神器MobaXterm，能同时支持这二者的功能，我果断地放弃了它们而选择了MobaXterm。

但是免费版MobaXterm：会话数量限制12个，SS隧道限制2个，宏最多4个。在逛B站的时候偶然发现了这么一种方式可以完美迭代，所以记录一下使用教程。

## 使用教程

### 1、获取MobaXterm

> 个人比较推荐官网下载，并且选择解压式文件，缺点就是下载速度可能比较慢
>
> 文章基于官网v23.3版本使用

#### 1.1网盘获取

- **百度网盘**：
  - 链接：https://pan.baidu.com/s/1QagTlPbxerjf3ZjcicUmxA?pwd=klwq 
    提取码：klwq

- **蓝奏云**：
  - https://saudade.lanzoub.com/iiHu01bief7c

#### 1.2官网获取

官网地址：https://mobaxterm.mobatek.net/download.html

- **进入官网，点击菜单中的Download，跳转到下载界面，显示有两个版本：左边免费版，右边付费版。我们选择免费版本即可**
  ![image-20231011162808408](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/11/65265c99b8f3a.png)
- **然后回进入版本选择界面：左边是解压式，右边是安装式。本次选择解压式，点击下载。**
  ![image-20231011162937160](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/11/65265cf26f1b4.png)

### 2、安装并打开MobaXterm

> 选择安装式的，下载后直接安装即可

- 下载完成之后解压出来的目录是这样式的，版本的不同文件结构可能也会不同， 双击MobaXterm_Personal_9.4.exe即可打开软件
  ![image-20231011163314594](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/11/65265dcc608ca.png)

### 3、查看状态

点击**Help**图标，可见当前是免费版

![image-20231011180438694](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/11/6526733987b23.png)

### 4、破解步骤

**这里需要使用到大佬的项目：https://github.com/malaohu/MobaXterm-GenKey**

#### 4.1.自己搭建

> 采用的是Docker方式，如果服务器未安装Docker
>
> 参考文章：https://blog.csdn.net/weixin_45764765/article/details/128336180

- **MobaXterm连接上服务器，输入执行一下代码：拉去镜像，并创建一个容器运行**

> 此处用的端口是5000，需要将端口开放，端口不固定

```shell
docker pull malaohu/mobaxterm-genkey
docker run -d -p 5000:5000 malaohu/mobaxterm-genkey
```

- **通过docker images命令可以查看镜像已被拉取，docker ps命令查看已有容器运行**

```shell
docekr images
docker ps
```

![image-20231011182708393](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/11/6526787d9c79d.png)

- **访问地址：地址为ip：5000，如我的就是：http://124.223.217.165:5050/**

![image-20231011183855176](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/11/65267b4061873.png)

- **填写信息：Name随意，Version要和你当前的版本一致**

![image-20231011184006522](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/11/65267b87b1d82.png)

- **点击 Gen！会生成一个 Custom.mxtpro，将该文件复制或移动到软件的根目录中**

![image-20231011184154782](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/11/65267bf3e3853.png)

- **最后，再重新打开软件，点击Help图标，可见提示已更改，升级完成**

![image-20231011184408727](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/11/65267c7b79620.png)

#### 4.2 使用Github作者项目地址

**项目地址：http://192.99.11.204:5000/**

**后面的操作如上**

#### 4.3 直接调用文件

> 仅适用于v23.3版本

**将文件之间放到软件根目录中**

- **百度网盘：**
  - 链接：https://pan.baidu.com/s/1Vmud_wIM2T6EmmfhDHIDSg?pwd=td2c 
    提取码：td2c
- **蓝奏云：**
  - https://saudade.lanzoub.com/ih5nX1bieg9a

## 参考内容

### 1、Github项目

- https://github.com/malaohu/MobaXterm-GenKey
- https://github.com/flygon2018/MobaXterm-keygen 

### 2、文章参考

- https://51.ruyo.net/17008.html

### 3、视频参考

- https://www.bilibili.com/video/BV1gM4y1h7dL/?spm_id_from=333.337.search-card.all.click&vd_source=0fb6b656c96d10415559d2c929d12502