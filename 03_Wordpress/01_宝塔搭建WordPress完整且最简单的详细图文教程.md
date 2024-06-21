# 宝塔搭建WordPress完整且最简单的详细图文教程

## 概述

wordpress是全球用户数量最大的开源网站网站程序，采用可视化模块化配置方便任何人都可以轻松的创建自己的网站。

这篇文章记录自己使用宝塔来搭建wordpress，教程十分详细且简单，个人认为目前最快、最简单、最不会踩坑的方式！

## 前提准备

- 一台[服务器](https://curl.qcloud.com/O5qrpE0j)且已安装好[宝塔](https://www.bt.cn/new/index.html)（请使用linux系统，极不推荐Windows系统）

  - **Centos安装脚本**

    - ```shell
      yum install -y wget && wget -O install.sh https://download.bt.cn/install/install_6.0.sh && sh install.sh ed8484bec
      ```

  - **Ubuntu/Deepin安装脚本**

    - ```shell
      wget -O install.sh https://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh ed8484bec
      ```

  - **Debian安装脚本**

    - ```shell
      wget -O install.sh https://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh ed8484bec
      ```

- 准好了域名，并做好了域名解析

- 下载好wordpress程序的.zip压缩文件[【点击下载】](https://saudade.lanzoub.com/iioI30yrw1vg)

## 主要流程

- 宝塔安装php、Nginx、sql等基础运行环境
- 创建网站及数据库并做网站配置
- 上传wordpress程序和主题程序
- 完成网站的基本配置

## 操作步骤

### 1、安装基础运行环境

登录到宝塔后台，进入软件商店，搜索并安装以下程序（备注：已经安装了这些的可跳过）

- Nginx 推荐版本1.20.1
- PHP 推荐版本7.4
- MySQL 推荐版本5.7
- Redis 推荐版本6.2(可选，用户缓存加速)

![image-20230612230341888](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/12/648733dfd63b6.png)

根据服务器的性能，安装时间大约5-10分钟。

### 2、创建网站

进入网站，点击创建网站，输入域名，选择创建数据库，选择PHP，提交

![image-20230613114127592](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/6487e57103e33.png)

提交后保存好以下三个资料，后面要用

![image-20230613114202685](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/6487e58c73f01.png)

### 3、设置网站参数

点击刚刚创建的网站，并进行以下配置

- 设置伪静态

  ![image-20230613224509960](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/648881e11e80f.png)

- 设置好SSL证书（自己有证书就用自己的，没有证书就先用Let’s Encrypt免费的）

  > 强烈推荐建站前就配置好SSL，因为当网站有内容了之后，再来配置SSL并开启HTTPS，会十分麻烦！！
  >
  > 阿里云和腾讯云都提供1年有效期的免费证书
  >
  > 需要注意的是，这里如果使用Let’s Encrypt的证书，那么只有一个月的有效期！
  >
  > 所以为了安全和稳定，还是建议后期更换成其他证书。

  ![](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/64888034d7a1b.png)

- 安装好PHP扩展（非必须）

  ![image-20230613224746226](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/648881e102aac.png)


### 4、上传wordpress程序

进入网站目录，上传已经下载好的wordpress的.zip压缩文件，解压

![image-20230613225105032](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/6488825b0f1cf.png)

上传wordpress程序文件

![image-20230613225156214](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/6488828e4409e.png)

解压wordpress文件，解压后会有一个wordpress文件夹，进入解压得到的wordpress文件夹，将里面的内容全部解压到当前目录中

![image-20230613225327454](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/648882e98010c.png)

![image-20230613225745541](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/648883eba22c9.png)

![image-20230613225832699](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/6488841abbe5d.png)

完成

![image-20230613225902378](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/648884386c299.png)

### 5、安装wordpress

完成以上步骤以后，我们就可以打开我们的网站了，首次打开会自动运行wordpress安装程序，我们根据流程安装即可！

> 注意：按照上方流程设置好了SSL之后，一定要用`https`打开网站！
>
> **重要提醒：**如果你的服务器要搭建多个网站，一定要设置不同的数据库表前缀，不然如果再使用Redis的话，会导致数据错乱

![image-20230613225958210](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/648884704d163.png)

![image-20230613230149628](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/648884dfaf541.png)

![image-20230613230210783](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/648884f4b837d.png)

根据自己的需求设置信息，然后安装wordpress

![image-20230613230309915](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/6488852fe64b8.png)

安装wordpress完成，即可登录

![image-20230613230420084](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/6488857613759.png)

登录账号，即可进入后台

![image-20230613230443005](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/6488858ceed25.png)

在访问域名，即可看到前端界面（初始样式）

![image-20230613230545818](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/648885cbe1618.png)

### 6、至此，结束
