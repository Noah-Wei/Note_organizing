# Navicat连接数据库报错：The server key has changed. Either you are under attack or the administrator changed the key

## 1、问题描述

今天在用数据库可视化软件Navicat使用SSH连接远程数据库报错如下

- 错误截图

  ![image-20240304165545304](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/03/04/65e58da4be3f2.png)

- 中文翻译

  服务器密钥已更改。 您受到攻击或管理员更改了密钥。 新服务器密钥哈希：。。。。

## 2、解决方案

删除`C:\Users\Tiramisu\.ssh\known_hosts`文件，其中**用户名根据自己的电脑来**

![image-20240304165917097](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/03/04/65e58d689a34b.png)

