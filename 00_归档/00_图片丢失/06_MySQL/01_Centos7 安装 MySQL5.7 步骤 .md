# Centos7 安装 MySQL5.7 步骤

> 之前一直是在window上安装mysql，但是实际应用mysql都是安装在服务器上，所以记录一下
>
> 本文记录了两种方式来安装mysql
>
> 一、使用yum源方式安装
>
> 二、使用本地tar文件方式安装

## 一、使用yum源方式安装

### 1、卸载系统自带 mariadb

> MariaDB Server 是最流行的开源关系型数据库之一。它由 MySQL 的原始开发者制作，并保证保持开源。
>
> 在 CentOS 7 中默认安装有 MariaDB

> 可忽略，安装完成之后可以直接覆盖掉MariaDB。

- **查看并卸载系统自带的 Mariadb**

```powershell
[root@localhost /]# rpm -qa|grep mariadb
mariadb-libs-5.5.68-1.el7.x86_64
[root@localhost /]# rpm -e --nodeps mariadb-libs-5.5.68-1.el7.x86_64
[root@localhost /]# rpm -qa|grep mariadb
```

![image-20230223221502781](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f774e6e1591.png)

### 2、下载并安装MySQL官方的 Yum 

> 由于CentOS 的yum源中没有mysql，需要到mysql的官网下载yum repo配置文件

#### 2.1 下载mysql的yum源配置

```powershell
[root@localhost ~]#  wget http://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm

--2023-02-25 06:24:11--  https://repo.mysql.com//mysql57-community-release-el7-11.noarch.rpm
正在解析主机 repo.mysql.com (repo.mysql.com)... 23.212.157.5
...
...
100%[=========================================================================>] 25,680      --.-K/s 用时 0s      

2023-02-25 06:24:19 (180 MB/s) - 已保存 “mysql57-community-release-el7-11.noarch.rpm” [25680/25680])

[root@localhost ~]# ls
anaconda-ks.cfg       mysql57-community-release-el7-11.noarch.rpm  模板  图片  下载  桌面
initial-setup-ks.cfg  公共                                         视频  文档  音乐
```

![image-20230224222609584](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/24/63f8c90114a08.png)

#### 2.2 安装mysql的yum源

> 命令执行完成后会在 /etc/yum.repos.d/ 目录下生成两个repo文件
>
> mysql-community.repo
>
> mysql-community-source.repo

```powershell
[root@localhost ~]# yum -y install mysql57-community-release-el7-11.noarch.rpm

已加载插件：fastestmirror, langpacks
正在检查 mysql57-community-release-el7-11.noarch.rpm: mysql57-community-release-el7-11.noarch
mysql57-community-release-el7-11.noarch.rpm 将被安装
...
...
已安装:
  mysql57-community-release.noarch 0:el7-11                                                                        

完毕！
[root@localhost ~]# ls /etc/yum.repos.d/
CentOS-Base.repo       CentOS-fasttrack.repo  CentOS-Vault.repo          mysql-community-source.repo
CentOS-CR.repo         CentOS-Media.repo      CentOS-x86_64-kernel.repo
CentOS-Debuginfo.repo  CentOS-Sources.repo    mysql-community.repo
```

![image-20230224223012995](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/24/63f8c9f4a3b49.png)

#### 2.3 使用yum方式安装mysql

```powershell
[root@localhost ~]# yum -y install mysql-server
```

![image-20230225115849687](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9877998548.png)

##### 2.3.1 安装过程中报错解决

> 如果没有提示错误可忽略

- 问题描述

```powershell
警告：/var/cache/yum/x86_64/7/mysql57-community/packages/mysql-community-common-5.7.41-1.el7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID 3a79bd29: NOKEY
mysql-community-common-5.7.41-1.el7.x86_64.rpm 的公钥尚未安装


mysql-community-libs-compat-5.7.41-1.el7.x86_64.rpm 的公钥尚未安装

 失败的软件包是：mysql-community-libs-compat-5.7.41-1.el7.x86_64
 GPG  密钥配置为：file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

- 解决方案
  **运行命令**：`rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022`
  在重新安装

```powershell
运行命令
[root@localhost ~]# rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
重新安装
[root@localhost ~]# yum -y install mysql-server
```

### 3、使用并设置mysql

#### 3.1 启动mysql并查看状态

```powershell
[root@localhost ~]# systemctl start mysqld.service
[root@localhost ~]# systemctl status mysqld.service
```

![image-20230225120205049](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9883cef8d7.png)

#### 3.2 获取临时密码

> 在第一次登录时需要，登录后可修改密码

```powershell
[root@localhost ~]# cat /var/log/mysqld.log | grep password
2023-02-25T12:00:58.723624Z 1 [Note] A temporary password is generated for root@localhost: 1!L#qo3?d6i=
```

![image-20230225120509439](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f988f548c8e.png)

#### 3.3 登录mysql

> 密码为刚才获取的临时密码，即`1!L#qo3?d6i=`

```powershell
[root@localhost ~]# mysql -u root -p
Enter password: 
```

![image-20230225120615882](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f98937b56b5.png)

#### 3.4 修改登录密码

> 如果密码设置太简单，会提示错误：
>
> `ERROR 1819 (HY000): Your password does not satisfy the current policy requirements`

```mysql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Wxq3012@';
Query OK, 0 rows affected (0.00 sec)
```

![image-20230225121314302](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f98ada84b70.png)

### 4、设置远程访问

#### 4.1 开启mysql的远程访问权限

> 在mysql命令行中输入
>
> 命令中的用%代表所有IP，如有需要，可换成指定IP

```mysql
mysql> grant all privileges on *.* to 'root'@'%' identified by 'Wxq3012@' with grant option;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

![image-20230225122209269](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f98cf11c5d3.png)

#### 4.2 为firewalld添加开放端口3306

```powershell
[root@localhost ~]# firewall-cmd --zone=public --add-port=3306/tcp --permanent
success
重启配置
[root@localhost ~]# firewall-cmd --reload
success
```

![image-20230225122519087](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f98daf035d8.png)

#### 4.3 远程连接测试

![image-20230225122749043](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f98e44e5fe3.png)

### 5、至此，mysql安装结束

完结，撒花

## 二、本地 tar 文件方式安装

### 1、获取tar安装包文件

#### 1.1 下载mysql5.7安装包

- **MySQL安装包官方下载地址：https://dev.mysql.com/downloads/mysql/5.7.html**

![image-20230223213022003](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f76a755a422.png)

#### 1.2 包上传到 Linux 服务器

> 可以通过XFTP软件将安装包上传到服务器

- **在 Linux 服务器根目录下创建两个文件夹：**
  - tools 文件夹，存放软件安装包
  - az 文件夹，存放安装后的软件

```powershell
[root@localhost /]# cd /
[root@localhost /]# mkdir tools
[root@localhost /]# mkdir az
```

- **将下载好的 MySQL 安装包上传至 tools 文件夹下：**

```powershell
[root@localhost /]# ls /tools/
mysql-5.7.41-linux-glibc2.12-x86_64.tar
```

![image-20230223215230855](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f76f9f34137.png)

### 2、安装前查看设置

#### 2.1 卸载 CentOS7 系统自带 mariadb

> MariaDB Server 是最流行的开源关系型数据库之一。它由 MySQL 的原始开发者制作，并保证保持开源。
>
> 在 CentOS 7 中默认安装有 MariaDB

- **查看并卸载系统自带的 Mariadb**

```powershell
[root@localhost /]# rpm -qa|grep mariadb
mariadb-libs-5.5.68-1.el7.x86_64
[root@localhost /]# rpm -e --nodeps mariadb-libs-5.5.68-1.el7.x86_64
[root@localhost /]# rpm -qa|grep mariadb
```

![image-20230223221502781](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f774e6e1591.png)

#### 2.2 检查系统是否安装过 MySQL

```powershell
[root@localhost /]# rpm -qa | grep mysql
```

![image-20230223215548430](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f7706488242.png)

- **如果系统中 MySQL ，查询所有 MySQL 对应的文件夹，全部删除**

```powershell
 [root@localhost /]# whereis mysql
 [root@localhost /]# find / -name mysql
```

#### 2.3 检查有无 MySQL 用户组

> 检查有无 MySQL 用户组，没有则创建

- **检查 mysql 用户组是否存在**

```powershell
[root@localhost /]# cat /etc/group | grep mysql
[root@localhost /]# cat /etc/passwd | grep mysql
```

- **创建 mysql 用户组和用户**

```powershell
[root@localhost /]# groupadd mysql
[root@localhost /]# useradd -r -g mysql mysql
```

![image-20230223222407933](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f77708038bd.png)

### 3、安装 MySQL5.7 

#### 3.1 解压下载的tar文件

- **解压下载的 mysql-5.7.41-linux-glibc2.12-x86_64.tar 文件后**
  **得到 mysql-5.7.41-linux-glibc2.12-x86_64.tar.gz 文件**

```powershell
[root@localhost /]# mysql-5.7.41-linux-glibc2.12-x86_64.tar.gz
```

![image-20230223223051863](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f7789c02c2f.png)

#### 3.2 解压tar.gz文件

- **解压 mysql-5.7.41-linux-glibc2.12-x86_64.tar.gz 文件到 /az/ 文件夹**

![image-20230223223109321](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f778ad617e1.png)

![image-20230223223320682](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f77930ba12c.png)

#### 3.3 修改文件夹名称

- **修改文件夹名称为 mysql5.7**

```powershell
[root@localhost az]# mv mysql-5.7.41-linux-glibc2.12-x86_64/ mysql5.7
[root@localhost az]# ls
mysql5.7
```

![image-20230223223722187](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f77a223f0fd.png)

#### 3.4 更改文件夹权限

- **为了避免权限问题，更改 mysql5.7 目录下所有文件夹所属的用户组、用户以及权限**

```powershell
[root@localhost az]# chown -R mysql:mysql /az/mysql5.7/
[root@localhost az]# chmod -R 755 /az/mysql5.7/
```

![image-20230223223928554](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f77aa097171.png)

#### 3.5 初始化mysql和获取密码

- **进入 /az/mysql5.7/bin/ 目录，编译安装并初始化 mysql **
  **务必记住数据库管理员临时密码**

```powershell
[root@localhost bin]# ./mysqld --initialize --user=mysql --datadir=/az/mysql5.7/data --basedir=/az/mysql5.7
2023-02-23T22:43:52.131986Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
...
...
2023-02-23T22:43:52.554378Z 1 [Note] A temporary password is generated for root@localhost: Pyq#VB8mieDS
```

![image-20230223224433026](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f77bd12cffb.png)

#### 3.6 编译my.cnf 配置文件

##### 3.6.1 修改 my.cnf 配置文件

```powershell
[root@localhost bin]# vim /etc/my.cnf

[mysqld]
datadir=/az/mysql5.7/data
port = 3306
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
symbolic-links=0
max_connections=400
innodb_file_per_table=1
表名存储在磁盘是小写的，但是比较的时候是不区分大小写
lower_case_table_names=1
```

![image-20230223224829399](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f77cbd70d0c.png)

##### 3.6.2 修改 my.cnf 权限

```powershell
[root@localhost bin]# chmod -R 755 /etc/my.cnf 
```

#### 3.7 编译mysql.server 文件

> 因为没有安装下/usr/local/mysq目录下，所以需要修改成安装的/az/mysql5.7目录。

```powershell
[root@localhost bin]# vim /az/mysql5.7/support-files/mysql.server 
```

![image-20230223225449572](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f77e39abfc8.png)

### 4、设置mysql服务

#### 4.1 查询服务

```powershell
[root@localhost ~]# ps -ef | grep mysql
root       2031   1778  0 06:27 pts/0    00:00:00 tar -x mysql-5.7.41-linux-glibc2.12-x86_64.tar
root       2396   1778  0 06:55 pts/0    00:00:00 grep --color=auto mysql
[root@localhost ~]# ps -ef | grep mysqld
root       2398   1778  0 06:55 pts/0    00:00:00 grep --color=auto mysqld
```

![image-20230223225626365](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f77e9a79302.png)

#### 4.2 启动服务

```powershell
[root@localhost ~]# /az/mysql5.7/support-files/mysql.server start
Starting MySQL.Logging to '/az/mysql5.7/data/localhost.localdomain.err'.
 SUCCESS! 
```

#### 4.3 添加软连接，并重启服务

```powershell
[root@localhost ~]# ln -s /az/mysql5.7/support-files/mysql.server /etc/init.d/mysql
[root@localhost ~]# ln -s /az/mysql5.7/bin/mysql /usr/bin/mysql


[root@localhost ~]# service mysql restart
Shutting down MySQL.. SUCCESS! 
Starting MySQL. SUCCESS! 
[root@localhost ~]# 
```

![image-20230223230456686](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f78098c18b8.png)

### 5、使用并设置mysql

#### 5.1 登录mysql 

> 密码就是初始化时生成的临时密码

```powershell
[root@localhost ~]# mysql -u root -p
Enter password: 
```

![image-20230223230622415](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f780ee7ece1.png)

#### 5.2 修改密码

```powershell
mysql> set password for root@localhost = password('root');
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> 
```

![image-20230223230928316](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f781a86b46f.png)

### 6、设置远程访问

#### 6.1 开启mysql的远程访问权限

```shell
mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> update user set user.Host='%' where user.User='root';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

![image-20230223231130370](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f7822273b66.png)

#### 6.2 设置开机自启

```powershell
将服务文件拷贝到init.d下，并重命名为mysql
[root@localhost ~]# cp /az/mysql5.7/support-files/mysql.server /etc/init.d/mysqld

赋予可执行权限
[root@localhost ~]# chmod +x /etc/init.d/mysqld

添加服务
[root@localhost ~]# chkconfig --add mysqld

显示服务列表
[root@localhost ~]# chkconfig --list
```

![image-20230223231402517](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f782ba9c195.png)

#### 6.3 开放3306端口

```powershell
开放3306端口命令
[root@localhost ~]# firewall-cmd --zone=public --add-port=3306/tcp --permanent
success

重启防火墙
[root@localhost ~]# firewall-cmd --reload
success
```

![image-20230223232311576](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/23/63f784dfa337d.png)

#### 6.4 远程连接测试

![image-20230225122749043](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f98e44e5fe3.png)

### 7、至此，mysql安装结束

完结，撒花