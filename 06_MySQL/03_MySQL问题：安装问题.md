# MySQL问题： mysql-community-libs-compat-5.7.41-1.el7.x86_64.rpm 的公钥尚未安装

## 1、问题描述

使用yum安装mysql

```powershell
yum -y install mysql-server 
```

提示错误:

```powershell
警告：/var/cache/yum/x86_64/7/mysql57-community/packages/mysql-community-common-5.7.41-1.el7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID 3a79bd29: NOKEY
mysql-community-common-5.7.41-1.el7.x86_64.rpm 的公钥尚未安装


mysql-community-libs-compat-5.7.41-1.el7.x86_64.rpm 的公钥尚未安装

 失败的软件包是：mysql-community-libs-compat-5.7.41-1.el7.x86_64
 GPG  密钥配置为：file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

## 2、出现原因

大概意思：

​	如果使用的4.1以上版本的rpm的话，除了import mysql的公钥到个人用户的配置中，还需要import mysql的公钥到RPM的配置中

参考连接：

[MySQL :: MySQL 5.7 Reference Manual :: 2.1.4.4 Signature Checking Using RPM](https://dev.mysql.com/doc/refman/5.7/en/checking-rpm-signature.html)

[MySQL :: MySQL 5.7 Reference Manual :: 2.1.4.2 Signature Checking Using GnuPG](https://dev.mysql.com/doc/refman/5.7/en/checking-gpg-signature.html)

## 3、解决方案

- **运行命令**：`rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022`
  在重新安装

```powershell
运行命令
[root@localhost ~]# rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
重新安装
[root@localhost ~]# yum -y install mysql-server
```

![image-20230225115849687](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9877998548.png)
