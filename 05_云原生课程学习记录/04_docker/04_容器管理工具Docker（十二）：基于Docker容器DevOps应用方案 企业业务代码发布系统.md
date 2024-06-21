# 容器管理工具Docker（十二）：基于Docker容器DevOps应用方案 企业业务代码发布系统

## 一、企业业务代码发布方式

### 1.1 传统方式

- 以物理机或虚拟机为颗粒度部署
- 部署环境比较复杂，需要有先进的自动化运维手段
- 出现问题后重新部署成本大，一般采用集群方式部署
- 部署后以静态方式展现

### 1.2 容器化方式

- 以容器为颗粒度部署
- 部署方式简单，启动速度快
- 一次构建可到处运行
- 出现故障后，可随时恢复
- 可同时部署多套环境（测试、预发布、生产环境等）

## 二、企业业务代码发布逻辑图

<img src="https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/09/6432c303bb622.png" alt="image-20220223152003734" style="zoom:200%;" />

## 三、企业业务代码发布工具及流程图

### 3.1 工具

| 序号 | 工具    | 工具用途                                                     |
| ---- | ------- | ------------------------------------------------------------ |
| 1    | git     | 用于提交业务代码或克隆业务代码仓库                           |
| 2    | gitlab  | 用于存储业务代码                                             |
| 3    | jenkins | 用于利用插件完成业务代码编译、构建、推送至Harbor容器镜像仓库及项目部署 |
| 4    | tomcat  | 用于运行JAVA业务代码                                         |
| 5    | maven   | 用于编译业务代码                                             |
| 6    | harbor  | 用于存储业务代码构建的容器镜像存储                           |
| 7    | docker  | 用于构建容器镜像，部署项目                                   |

### 3.2 流程图

> 本次部署Java代码包。

<img src="https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/09/6432c4785576c.png" alt="image-20220223163453076" style="zoom:200%;" />

## 四、企业业务代码发布系统环境部署

### 4.1 主机规划

| 序号 | 主机名         | 主机IP         | 主机功能                     | 软件                 |
| ---- | -------------- | -------------- | ---------------------------- | -------------------- |
| 1    | dev            | 192.168.126.20 | 开发者 项目代码 solo         | git                  |
| 2    | gitlab-server  | 192.168.126.21 | 代码仓库                     | gitlab-ce            |
| 3    | jenkins-server | 192.168.126.22 | 编译代码、打包镜像、项目发布 | jenkins、docker、git |
| 4    | harbor-server  | 192.168.126.23 | 存储容器镜像                 | harbor、docker       |
| 5    | web-server     | 192.168.126.24 | 运行容器，项目上线           | docker               |

### 4.2 主机准备

#### 4.2.1 主机名配置

~~~powershell
# hostnamectl set-hostname xxx
~~~

> 根据主机规划实施配置

#### 4.2.2 主机IP地址配

~~~powershell
# vim /etc/sysconfig/network-scripts/ifcfg-ens33
# cat /etc/sysconfig/network-scripts/ifcfg-ens33
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="none" 配置为静态IP
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="ec87533a-8151-4aa0-9d0f-1e970affcdc6"
DEVICE="ens33"
ONBOOT="yes"
IPADDR="192.168.126.2x"  把2x替换为对应的IP地址
PREFIX="24"
GATEWAY="192.168.126.2"
DNS1="119.29.29.29"
~~~

#### 4.2.3 主机名与IP地址解析配置

~~~powershell
# vim /etc/hosts
# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.126.20 dev
192.168.126.21 gitlab-server
192.168.126.22 jenkins-server
192.168.126.23 harobr-server
192.168.126.24 web-server
~~~

#### 4.2.4 主机安全设置

~~~powershell
关闭防火墙
# systemctl stop firewalld
# systemctl disable firewalld
查看防火墙状态
# firewall-cmd --state
~~~

~~~powershell
关闭SELINUX后，必须重启系统才能使其修改生效。
# sed -ri 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
查看selinux状态
# sestatus
~~~

#### 4.2.5 主机时间同步

> 如果时间不同步，主机之间时间可能有差异，导致不能正常访问
>
> 本次使用定时任务方式同步

~~~powershell
添加定时任务
# crontab -e
查看定时任务
# crotab -l
0 */1 * * * ntpdate time1.aliyun.com
~~~

### 4.3 主机中工具安装

#### 4.3.1 dev主机

>git：下载项目及上传代码至代码仓库

~~~powershell
# yum -y install git
~~~

#### 4.3.2 gitlab-server主机

> gitlab-ce

##### 4.3.2.1 获取gitlab-ce的YUM源

YUM源网站：[清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/)

![image-20230410133341006](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64339fb569726.png)

~~~powershell
编写yum源
# vim /etc/yum.repos.d/gitlab.repo
查看yum源
# cat /etc/yum.repos.d/gitlab.repo
[gitlab]
name=gitlab-ce
baseurl=https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7
enabled=1
gpgcheck=0

说明
[]                    	 容器名称，一定要放在[ ]中
name=                	 容器说明，可以自己随便写
baseurl=  				 镜像站点
gpgcheck=0               如果是1是指RPM的数字证书生效，如果是0则不生效
enabled=1                此容器是否生效--不写或写成enable = 1都是生效，enabled=0就是不生效
~~~

##### 4.3.2.2 gitlab-ce安装

~~~powershell
# yum -y install gitlab-ce
~~~

##### 4.3.2.3 gitlab-ce配置

~~~powershell
# vim /etc/gitlab/gitlab.rb
external_url 'http://192.168.126.21'
~~~

##### 4.3.2.4 启动gitlab-ce                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    

~~~powershell
重新启动gitlab-ce（更改过设置，重新启动）
# gitlab-ctl reconfigure
~~~

~~~powershell
查看状态
# gitlab-ctl status
~~~

##### 4.3.2.5 访问gitlab-ce

~~~powershell
查看初始密码，用于登录
# cat /etc/gitlab/initial_root_password
......

Password: znS4Bqlp0cfYUKg2dHzFiNCAN0GnhtnD4ENjEtEXMVE=

~~~

访问站点

![image-20220224140418176](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433b3e09325c.png)

#### 4.3.3 jenkins-server主机

> 因为最新jenkins现在不在支持jdk8

![8f5ca9bce3febccd40d7f20743ef9b83.png](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433d8ce1afd4.png)

##### 4.3.3.1 jdk安装

~~~powershell
下载jdk
# yum -y  install java-11-openjdk.x86_64
~~~

~~~powershell
更改默认使用的jdk版本
[root@centos ~]# alternatives --config java

共有 3 个提供“java”的程序。

  选项    命令
-----------------------------------------------
   1           java-1.7.0-openjdk.x86_64 (/usr/lib/jvm/java-1.7.0-openjdk-1.7.0.261-2.6.22.2.el7_8.x86_64/jre/bin/java)
*  2           java-1.8.0-openjdk.x86_64 (/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.362.b08-1.el7_9.x86_64/jre/bin/java)
 + 3           java-11-openjdk.x86_64 (/usr/lib/jvm/java-11-openjdk-11.0.18.0.10-1.el7_9.x86_64/bin/java)

按 Enter 保留当前选项[+]，或者键入选项编号：3

~~~

~~~powershell
检查版本
# java -version
java version "1.8.0_191"
Java(TM) SE Runtime Environment (build 1.8.0_191-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.191-b12, mixed mode)
~~~

##### 4.3.3.2 jenkins安装

###### 4.3.3.2.1 安装

官网：[Jenkins](https://www.jenkins.io/)

建议下载稳定版

![image-20220224141610569](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433b9ebd2a32.png)



![image-20220224141720927](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433ba07988d4.png)





~~~powershell
下载jenkins的yum源
#  wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
~~~

~~~powershell
导入key，如果不导入，安装jenkins可能会装不上
# rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
~~~

~~~powershell
获取依赖的repos的yum源（建议使用阿里云镜像，官方的太慢）
# wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
~~~

~~~powershell
安装jenkins
# yum -y install jenkins
~~~

###### 4.3.3.2.2 jenkins启动

~~~powershell
启动服务
# systemctl start jenkins
~~~

###### 4.3.3.2.3 jenkins访问

~~~powershell
获取解锁密码
# cat /var/lib/jenkins/secrets/initialAdminPassword
3363d658a1a5481bbe51a1ece1eb08ab
~~~

###### 4.3.3.2.4 jenkins初始化配置

> 默认用户名为：admin
>
> 默认密码位置：/var/lib/jenkins/secrets/initialAdminPassword

![image-20220224173833454](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433d0b6cc594.png)

![image-20220224174018298](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433d0bc3200f.png)

![image-20220224174041874](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433d0d0efa4b.png)

![image-20220224174442874](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433d0d72eb35.png)

![image-20220224174507233](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433d0e76ea46.png)

![image-20220224174541367](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433d0ed116bb.png)

![image-20220224174601389](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433d0f1d3d2b.png)

##### 4.3.3.3 git安装

~~~powershell
# yum -y install git
~~~

##### 4.3.3.4 maven安装

###### 4.3.3.4.1 获取maven安装包

官网：https://maven.apache.org/

![image-20220224174855779](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433da6f51545.png)

~~~powershell
# wget https://dlcdn.apache.org/maven/maven-3/3.9.1/binaries/apache-maven-3.9.1-bin.tar.gz
~~~

###### 4.3.3.4.2 maven安装

~~~powershell
解压并查看
# tar xf apache-maven-3.9.1-bin.tar.gz
# ls
apache-maven-3.9.1 
~~~

~~~powershell
移动文件
# mv apache-maven-3.9.1 /usr/local/mvn
~~~

~~~powershell
编辑环境变量
# vim /etc/profile
......
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.18.0.10-1.el7_9.x86_64
export MAVEN_HOME=/usr/local/mvn
export PATH=${JAVA_HOME}/bin:${MAVEN_HOME}/bin:$PATH
~~~

~~~powershell
重启配置
# source /etc/profile
~~~

~~~powershell
检查版本
# mvn -v
Maven home: /usr/local/mvn
Java version: 11.0.18, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-11-openjdk-11.0.18.0.10-1.el7_9.x86_64
Default locale: zh_CN, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-1160.83.1.el7.x86_64", arch: "amd64", family: "unix"
~~~

##### 4.3.3.5 docker安装

~~~powershell
获取yum源
# wget -O /etc/yum.repos.d/docker-ce.repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
~~~

~~~powershell
安装docker
# yum -y install docker-ce
~~~

~~~powershell
启动服务
# systemctl enable docker
# systemctl start docker
~~~

#### 4.3.4 harbor-server主机

##### 4.3.4.1 docker安装

~~~powershell
获取yum源
# wget -O /etc/yum.repos.d/docker-ce.repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
~~~

~~~powershell
安装docker
# yum -y install docker-ce
~~~

~~~powershell
启动服务
# systemctl enable docker
# systemctl start docker
~~~

##### 4.3.4.2 docker-compose安装

###### 4.3.4.2.1 获取docker-compose文件

![image-20220224180732745](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433de8a00d9c.png)

~~~powershell
# wget https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64
~~~

###### 4.3.4.2.2 docker-compose安装及测试

~~~powershell
查看文件
# ls
docker-compose-linux-x86_64
~~~

~~~powershell
移动位置
# mv docker-compose-linux-x86_64 /usr/bin/docker-compose
~~~

~~~powershell
添加执行权限
# chmod +x /usr/bin/docker-compose
~~~

~~~powershell
查看版本
# docker-compose version
Docker Compose version v2.2.3
~~~

##### 4.3.4.3 harbor部署

###### 4.3.4.3.1 harbor部署文件获取

![image-20220224181626286](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433e07c8d860.png)

![image-20220224181829179](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433e08b72bfa.png)

###### 4.3.4.3.2 harbor部署

```powershell
下载文件
# wget https://github.com/goharbor/harbor/releases/download/v2.4.1/harbor-offline-installer-v2.4.1.tgz
```

~~~powershell
查看文件
# ls
harbor-offline-installer-v2.4.1.tgz
~~~

~~~powershell
解压文件
# tar xf harbor-offline-installer-v2.4.1.tgz
~~~

~~~powershell
查看文件
[root@harbor-server ~]# cd harbor/
[root@harbor-server harbor]# ls
common.sh  harbor.v2.4.1.tar.gz  harbor.yml.tmpl  install.sh  LICENSE  prepare
~~~

~~~powershell
修改配置文件
[root@harbor-server harbor]# mv harbor.yml.tmpl harbor.yml
[root@harbor-server harbor]# vim harbor.yml
[root@harbor-server harbor]# cat harbor.yml
# Configuration file of Harbor

# The IP address or hostname to access admin UI and registry service.
# DO NOT use localhost or 127.0.0.1, because Harbor needs to be accessed by external clients.
hostname: 192.168.126.23 修改

# http related config
http:
  # port for http, default is 80. If https enabled, this port will redirect to https port
  port: 80

# https related config
#https: 注释
  # https port for harbor, default is 443
#  port: 443 注释
  # The path of cert and key files for nginx
#  certificate: /your/certificate/path 注释
#  private_key: /your/private/key/path 注释

~~~

~~~powershell
[root@harbor-server harbor]# ./prepare
~~~

~~~powershell
[root@harbor-server harbor]# ./install.sh
~~~

~~~powershell
[root@harbor-server harbor]# docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED              STATUS                        PORTS                                   NAMES
12605eae32bb   goharbor/harbor-jobservice:v2.4.1    "/harbor/entrypoint.…"   About a minute ago   Up About a minute (healthy)                                           harbor-jobservice
85849b46d56d   goharbor/nginx-photon:v2.4.1         "nginx -g 'daemon of…"   About a minute ago   Up About a minute (healthy)   0.0.0.0:80->8080/tcp, :::80->8080/tcp   nginx
6a18e370354f   goharbor/harbor-core:v2.4.1          "/harbor/entrypoint.…"   About a minute ago   Up About a minute (healthy)                                           harbor-core
d115229ef49d   goharbor/harbor-portal:v2.4.1        "nginx -g 'daemon of…"   About a minute ago   Up About a minute (healthy)                                           harbor-portal
f5436556dd32   goharbor/harbor-db:v2.4.1            "/docker-entrypoint.…"   About a minute ago   Up About a minute (healthy)                                           harbor-db
7fb8c4945abe   goharbor/harbor-registryctl:v2.4.1   "/home/harbor/start.…"   About a minute ago   Up About a minute (healthy)                                           registryctl
d073e5da1399   goharbor/redis-photon:v2.4.1         "redis-server /etc/r…"   About a minute ago   Up About a minute (healthy)                                           redis
7c09362c986b   goharbor/registry-photon:v2.4.1      "/home/harbor/entryp…"   About a minute ago   Up About a minute (healthy)                                           registry
55d7f39909e3   goharbor/harbor-log:v2.4.1           "/bin/sh -c /usr/loc…"   About a minute ago   Up About a minute (healthy)   127.0.0.1:1514->10514/tcp               harbor-log
~~~

#### 4.3.5 web-server

> docker安装

~~~powershell
# wget -O /etc/yum.repos.d/docker-ce.repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
~~~

~~~powershell
# yum -y install docker-ce
~~~

~~~powershell
# systemctl enable docker
# systemctl start docker
~~~

### 4.4 工具集成配置

#### 4.4.1 配置docker主机使用harbor

##### 4.4.1.1 jenkins-server

~~~powershell
配置json访问harbor
[root@jenkins-server ~]# vim /etc/docker/daemon.json
[root@jenkins-server ~]# cat /etc/docker/daemon.json
{
        "insecure-registries": ["http://192.168.126.23"]
}
~~~

~~~powershell
重启服务
[root@jenkins-server ~]# systemctl restart docker
~~~

~~~powershell
登录harbor
[root@jenkins-server ~]# docker login 192.168.126.23
Username: admin
Password:
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
~~~

##### 4.4.1.2 harbor-server

~~~powershell
配置json访问harbor
[root@harbor-server harbor]# vim /etc/docker/daemon.json
[root@harbor-server harbor]# cat /etc/docker/daemon.json
{
        "insecure-registries": ["http://192.168.126.23"]
}
~~~

~~~powershell
停止容器（否则重启服务时可能会导致容器不会全部启动）
[root@harbor-server harbor]# docker-compose down
~~~

~~~powershell
重启服务
[root@harbor-server harbor]# systemctl restart docker
~~~

~~~powershell
重启容器
[root@harbor-server harbor]# docker-compose up -d
~~~

##### 4.4.1.3 web-server

~~~powershell
配置json访问harbor
[root@web-server ~]# vim /etc/docker/daemon.json
[root@web-server ~]# cat /etc/docker/daemon.json
{
        "insecure-registries": ["http://192.168.126.23"]
}
~~~

~~~powershell
重启服务
[root@web-server ~]# systemctl restart docker
~~~

~~~powershell
登录测试
[root@web-server ~]# docker login 192.168.126.23
Username: admin
Password:
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
~~~

#### 4.4.2 配置jenkins使用docker

> 在jenkins-server主机上配置

~~~powershell
验证系统中是否有jenkins用户（默认jenkins用户不能使用docker命令）
[root@jenkins-server ~]# grep jenkins /etc/passwd
jenkins:x:997:995:Jenkins Automation Server:/var/lib/jenkins:/bin/false
~~~

~~~powershell
验证系统中是否有docker用户及用户组
[root@jenkins-server ~]# grep docker /etc/group
docker:x:993:
~~~

~~~powershell
添加jenkins用户到docker用户组
[root@jenkins-server ~]# usermod -G docker jenkins
[root@jenkins-server ~]# grep docker /etc/group
docker:x:993:jenkins
~~~

~~~powershell
重启jenkins服务
[root@jenkins-server ~]# systemctl restart jenkins
~~~

#### 4.4.3 密钥配置

![image-20230410194917972](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433f7be80a91.png)

##### 4.4.3.1 dev主机至gitlab-ce

> 开发者（dev主机）生成密钥对，找到公钥，把公钥放置到Gitlab中

###### 4.4.3.1.1 dev主机生成密钥对

~~~powershell
生成密钥对
[root@dev ~]# ssh-keygen
~~~

###### 4.4.3.1.2 添加公钥至gitlab-ce

~~~powershell
查看公钥
[root@dev ~]# cat /root/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCy2PdvT9qX55CLZzzaaEf06x8gl3yHGfdJSmAp9L1Fdtcbd3yz3U0lgdOwWpB8fQ/A3HoUUTWCb1iC5WJBOvqkoD8rJ2xC3HJ62zjOjmqcn2fEs09CzJj3bCfahuqPzaPkIOoH42/Y2QdImQ7xZOqqjS7aIc5T2FjDLG3bMhaYFyvx18b1qiPACuh67iniPQnL667MFZ/0QGGVnQKwxop+SezhP9QqV1bvPk94eTdkERIBiY1CNcNmVryk6PzSKY8gfW++3TGN9F+knhMXcswFOu6FzqxcA3G+hYg+Io2HJaDrsfHGZ6CP5T9QiOlIWlNxz05BOK3OFQ5BPeomA+jv root@dev
~~~

登录gitlab设置

![image-20220224210606310](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433fb119868e.png)



![image-20220224210748207](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433fb1552b58.png)



![image-20220224210823231](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433fb191ed39.png)

##### 4.4.3.2 jenkins-server主机至gitlab-ce

> jenkins生成密钥对，找到公钥，把公钥放置到Gitlab中
>
> 将私钥配置到jenkins中

###### 4.4.3.2.1 在jenkins-server生成密钥对

~~~powershell
生成密钥对
[root@jenkins-server ~]# ssh-keygen
~~~

###### 4.4.3.2.2 添加公钥至gitlab-ce

~~~powershell
添加公钥
[root@jenkins-server ~]# cat /root/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCyg3WaEm5yH9yva8Jm5wfTPwN3ROGMNPpAex8zYj+M1GesoMtE6gkiKHWydAJBiLuu/1fBx6HlgzzxghVj9oK4DmTRZQh2IZY4+zZIGBRaDBuBO1f7+SdVE/jZoLd1a+yZ3FQmy37AlXUcIKxbrDBtefvJ31faziWyZKvT4BGFJCznRU6AOxOg1pe4bWbWI+dGnMIIq7IhtK+6tY/w3OlF7xcWmrJP1oucpq33BYOrnRCL9EO5Zp2jcejDeG5UvXONG7CggT7FDhjwcCRZvX+AutDGAtgBckNXZjV9SDKWgDifCSDtDfV4Be4zb8b3hxtSMsbEY8YHxsThsmHrUkbz root@jenkins-server
~~~

登录gitlab设置

![image-20220224211329307](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433fbb988cfa.png)

##### 4.4.3.3 配置jenkins-sever主机的私钥到凭据列表

~~~powershell
查看私钥
[root@jenkins-server ~]# cat /root/.ssh/id_rsa
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAsoN1mhJuch/cr2vCZucH0z8Dd0ThjDT6QHsfM2I/jNRnrKDL
ROoJIih1snQCQYi7rv9Xwceh5YM88YIVY/aCuA5k0WUIdiGWOPs2SBgUWgwbgTtX
+/knVRP42aC3dWvsmdxUJst+wJV1HCCsW6wwbXn7yd9X2s4lsmSr0+ARhSQs50VO
gDsToNaXuG1m1iPnRpzCCKuyIbSvurWP8NzpRe8XFpqyT9aLnKat9wWDq50Qi/RD
uWado3How3huVL1zjRuwoIE+xQ4Y8HAkWb1/gLrQxgLYAXJDV2Y1fUgyloA4nwkg
7Q31eAXuM2/G94cbUjLGxGPGB8bE4bJh61JG8wIDAQABAoIBAEOwy6BPyuelo1Y1
g3LnujTlWRgZ23kCAb7/sPYYFEb/qAxysIGCSVJVi0PO76gQBDM4iftmCsLv/+UI
UbolGK5Ybuxj5lB9LeyPfaba0qTOoINhkFxwvvRo7V0Ar3BsKzywqoxHb9nxEoZG
8XSVl4t7zPlgonzK3MqHmAxwk9QrIB/rnjolHGN6HvfK2Cwq5WN1crspwQ+XDbRS
J5qoAtv6PJzrU6QhJl/zSMCb0MytlIhZi+V+1yY/QhAYrWJgWypEwGlAXlVC90r4
twX1W/sl63xzFF396WjM1478yqpttvID06dKTC9T3y/k8lLmRNXwqmTCIm7C/jxP
9wjXJUECgYEA4r1N4AML7JpvE7hBRkreSdIIwoppRkBwl6gt/AJj8yt2ydQ8PY4P
X3s5ZkCfPzHX2XsVUUcQpsBFcU2PyG29+qzt3XOmlGYgJG11xPQbwi95/u9VSd5u
AuaNNa2YPw2teuM0hKVAl5knfy0+YHcOCdU14gHCCWsD4uOz5Zg9jVMCgYEAyYzv
SBvCqbZ4d5agTn+ZiOkmgKVT4UVnmZPivFXnCWiIbX2fi3ok7jU1hZAs6lf6VlTU
EPV8T1LwjO9yhFmicepzSl9lJCMbMXHt20OsqN0oUQFpoTQ07pbBE2K8c1IuQUEi
B2SoLHqv7Ym9jHQqvT3DVhTiC+H2LwsgVRvvi+ECgYAxaID0xJUvnMOBr5ABykTA
H1WrVs/z8AzY71v942N2VM1Q07/AxhkRfF+YqZJKCgl4KbsOeAbn31QCiZ1AVrGk
U1SOAiqVgd+VMIkOPwdhfEkARZT3QNIGLcktnkNj0g4wjhwen4gAwO37Z5eFG8xi
ViSkuC9ZMAmrwmSsLk2TYwKBgHQh0tYXuMiVLUCq99+DQnJS9S53FKfel900Cxc9
4AvZwZJlKgLx9EmVOyukcVzuKH6KDk9fQ6tpPNXYOoHsK9+7mYanBN4XpFmPLeCD
U/9QvyQ9ziFmtYEsOD/1SmSgW6qZ3wOnigdnAeu6zA8b+GxmJCF7kuwJ3RIqNQ0V
NafBAoGAXyynoTT2zugFq8jYRubxkMk7NdnTRAnGh+mlyrGGMsNLmPvfAw+6yKph
1fVHKXHtSrgtK0CVOIcmaH3r+LfG4Mfrjlq+8qiKcepBFvO9cZLNKn11vqQtzs7m
y+ydl4xTcCPoAMDsVeamJ3fv+9nyXe5KqYtw+BJMjpP+PnNN2YQ=
-----END RSA PRIVATE KEY-----
~~~

登录jenkins

![image-20220224212308684](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433fd77f0774.png)

![image-20220224212411414](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433fe22aade2.png)



![image-20220224212543257](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433fe26105b1.png)



![image-20220224212622017](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433fe322d7a9.png)



![image-20220224212928853](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433fe3603b7f.png)

![image-20220224213022249](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433fe3a224d8.png)





### 4.5 jenkins插件安装

#### 4.5.1 maven integration

> 用于编译JAVA项目

![image-20230410202103978](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433ff309b547.png)





![image-20230410202247348](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433ff97e115d.png)



![image-20230410202309366](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6433ffadcc5a7.png)

#### 4.5.2 git parameter

> 用于基于git版本提交进行参数构建项目

![image-20230410202438379](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64340006e0fbf.png)

#### 4.5.3 gitlab

> 用于jenkins-server拉取项目

![image-20230410202530049](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6434003a7ac12.png)

#### 4.5.4 Generic Webhook Trigger

> 用于项目自动化构建

![image-20230410202548025](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6434004c835c5.png)

#### 4.5.5 ssh

> 用于jenkins-server对web-server实施项目部署

![image-20230410202723070](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643400ab941a7.png)

### 4.6 jenkins全局工具配置

![image-20230410204612418](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643405151b57b.png)

#### 4.6.1 JDK配置

~~~powershell
查看jdk
[root@jenkins-server ~]# java -version
openjdk version "11.0.18" 2023-01-17 LTS
OpenJDK Runtime Environment (Red_Hat-11.0.18.0.10-1.el7_9) (build 11.0.18+10-LTS)
OpenJDK 64-Bit Server VM (Red_Hat-11.0.18.0.10-1.el7_9) (build 11.0.18+10-LTS, mixed mode, sharing)
[root@jenkins-server ~]# echo $JAVA_HOME
/usr/lib/jvm/java-11-openjdk-11.0.18.0.10-1.el7_9.x86_64
~~~

**配置jdk**

![image-20230410204942162](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643405e6a944e.png)

#### 4.6.2 Git配置

~~~powershell
查看git版本
[root@jenkins-server ~]# git version
git version 1.8.3.1
~~~

![image-20230410205054868](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6434062f3ed8b.png)

#### 4.6.3 Maven配置

~~~powershell
查看版本和路径
[root@jenkins-server ~]# mvn --version
Apache Maven 3.8.4 (9b656c72d54e5bacbed989b64718c159fe39b537)
Maven home: /usr/local/mvn
Java version: 1.8.0_191, vendor: Oracle Corporation, runtime: /usr/local/jdk/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-1160.49.1.el7.x86_64", arch: "amd64", family: "unix"
[root@jenkins-server ~]# echo $MAVEN_HOME
/usr/local/mvn
~~~

![image-20230410205256977](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643406a97011a.png)

### 4.7 jenkins系统配置

> 主要配置jenkins-server通过ssh协议连接web-server

#### 4.7.1 添加jenkins-server访问web-server凭据

![image-20230410205452487](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6434071d18fa4.png)

![image-20230410205535672](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643407484d566.png)

![image-20230410205557118](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6434075db29be.png)

![image-20230410205830821](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643407f77967c.png)

![image-20230410205859375](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64340813d5346.png)

#### 4.7.2 配置ssh协议连接主机

![image-20230410205943614](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64340840268e9.png)

![image-20230410210219842](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643408dc5d404.png)

## 五、企业业务代码项目发布

### 5.1 数据库管理系统部署 mariadb及创建项目数据库

~~~powershell
安装数据库
[root@web-server ~]# yum -y install mariadb mariadb-server
~~~

~~~powershell
开机自启动
[root@web-server ~]# systemctl enable mariadb
[root@web-server ~]# systemctl start mariadb
~~~

~~~powershell
设置密码
[root@web-server ~]# mysqladmin -uroot password 'abc123'
~~~

~~~powershell
登录数据库
[root@web-server ~]# mysql -uroot -pabc123
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3
Server version: 5.5.68-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
~~~

~~~powershell
创建数据库
MariaDB [(none)]> create database if not exists solo default charset utf8 collate utf8_general_ci;
~~~

~~~powershell
授权访问
MariaDB [(none)]> grant all on solo.* to 'root'@'%' identified by "123456";
Query OK, 0 rows affected (0.00 sec)
授权中会不包含localhost，所以localhost需要单独授权
MariaDB [(none)]> grant all on solo.* to 'root'@'localhost' identified by "123456";
Query OK, 0 rows affected (0.00 sec)
~~~

### 5.2 项目代码获取

![image-20220224223418318](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64340bd39c8ed.png)



~~~powershell
# git clone --recurse-submodules https://gitee.com/dl88250/solo.git
~~~

### 5.3 项目代码修改

~~~powershell
[root@dev ~]# ls
solo
~~~

~~~powershell
修改数据库连接配置信息
[root@dev ~]# vim solo/src/main/resources/local.properties
[root@dev ~]# cat solo/src/main/resources/local.properties
。。。。

#### MySQL runtime ####
runtimeDatabase=MYSQL
jdbc.username=root
jdbc.password=123456
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.URL=jdbc:mysql://192.168.126.24:3306/solo?useUnicode=yes&characterEncoding=UTF-8&useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
 。。。。
~~~

### 5.4 项目代码上传到gitlab

在gitlab中创建代码库

![image-20230410212006968](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64340d077e41c.png)

![image-20230410212020978](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64340d158e143.png)



![image-20230410212203609](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64340d7c422c1.png)

![image-20230410212249140](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64340da9ee4d9.png)

~~~powershell
用户名邮箱配置
# git config --global user.name "dev"
# git config --global user.email "dev@edenwei.com"
~~~

~~~powershell
[root@dev solo]# git remote remove origin
~~~

~~~powershell
[root@dev solo]# git remote add origin git@192.168.126.21:root/solo.git
~~~

~~~powershell
添加当前目录下的所有文件到暂存区
[root@dev solo]# git add -A .
提交暂存区到本地仓库
[root@dev solo]# git commit -m "new"
[master 3e39b0a] new
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~

~~~powershell
给当前版本设置tag为1.0.1
[root@dev solo]# git tag 1.0.0
~~~

~~~powershell
[root@dev solo]# git push origin 1.0.0
~~~

~~~powershell
[root@dev solo]# git push -u origin --all
~~~

![image-20230410213707973](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643411049d49d.png)

### 5.5 构建项目运行基础应用容器镜像

> 在harbor-server主机上操作

#### 5.5.1 创建项目目录

~~~powershell
[root@harbor-server ~]# mkdir tomcatdir
[root@harbor-server ~]# cd tomcatdir
~~~

#### 5.5.2 生成Dockerfile文件

~~~powershell
[root@harbor-server tomcatdir]# echo "tomcat is running" >> index.html
~~~

~~~powershell
[root@harbor-server tomcatdir]# vim Dockerfile
[root@harbor-server tomcatdir]# cat Dockerfile
FROM centos:centos7

MAINTAINER "www.edenwei.com"

ENV VERSION=8.5.87
ENV JAVA_HOME=/usr/local/jdk
ENV TOMCAT_HOME=/usr/local/tomcat

RUN yum -y install wget

RUN wget https://dlcdn.apache.org/tomcat/tomcat-8/v${VERSION}/bin/apache-tomcat-${VERSION}.tar.gz --no-check-certificate

RUN tar xf apache-tomcat-${VERSION}.tar.gz

RUN mv apache-tomcat-${VERSION} /usr/local/tomcat

RUN rm -rf apache-tomcat-${VERSION}.tar.gz /usr/local/tomcat/webapps/*

RUN mkdir /usr/local/tomcat/webapps/ROOT

ADD ./index.html /usr/local/tomcat/webapps/ROOT/

ADD ./jdk /usr/local/jdk


RUN echo "export TOMCAT_HOME=/usr/local/tomcat" >> /etc/profile

RUN echo "export JAVA_HOME=/usr/local/jdk" >> /etc/profile

RUN echo "export PATH=${TOMCAT_HOME}/bin:${JAVA_HOME}/bin:$PATH" >> /etc/profile

RUN echo "export CLASSPATH=.:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar" >> /etc/profile


RUN source /etc/profile

EXPOSE 8080

CMD ["/usr/local/tomcat/bin/catalina.sh","run"]
~~~

~~~powershell
[root@harbor-server tomcatdir]# ls
Dockerfile  index.html  jdk
~~~

#### 5.5.3 使用docker build构建容器镜像

~~~powershell
构建容器镜像
[root@harbor-server tomcatdir]# docker build -t 192.168.126.23/library/tomcat:8587 .
~~~

#### 5.5.4 推送容器镜像至harbor容器镜像仓库

~~~powershell
查看镜像
[root@harbor-server tomcatdir]# docker images
REPOSITORY                      TAG       IMAGE ID       CREATED              SIZE
192.168.126.23/library/tomcat    8587      01c433f8562d   About a minute ago   796MB
~~~

~~~powershell
登录harbor仓库
[root@harbor-server tomcatdir]# docker login 192.168.126.23
Username: admin
Password:
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
~~~

~~~powershell
推送到harbor仓库
[root@harbor-server tomcatdir]# docker push 192.168.126.23/library/tomcat:8587
~~~

![image-20230410220620747](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643417dd5a76f.png)

#### 5.5.5 验证容器镜像可用性

~~~powershell
[root@harbor-server ~]# docker run -d 192.168.126.23/library/tomcat:8587
d5443961ca65311ca0d68d53d44be997f5d6fde2d78772173ac6927112f34579
~~~

~~~powershell
[root@harbor-server ~]# docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED         STATUS                 PORTS                                   NAMES
d5443961ca65   192.168.126.23/library/tomcat:8587    "/usr/local/tomcat/b…"   3 seconds ago   Up 2 seconds           8080/tcp                                nifty_tesla
~~~

~~~powershell
[root@harbor-server tomcatdir]# docker inspect 9f9966 | grep IPA
            "SecondaryIPAddresses": null,
            "IPAddress": "172.17.0.2",
                    "IPAMConfig": null,
                    "IPAddress": "172.17.0.2",
~~~

~~~powershell
[root@harbor-server ~]# curl http://172.17.0.2:8080
tomcat is running
~~~

### 5.6 项目构建及发布

#### 5.6.1 项目构建及发布步骤

第一步：jenkins获取项目代码

第二步：jenkins对项目代码编译，由maven完成

第三步：jenkins使用docker对编译完成的项目代码进行打包，打包成容器应用镜像

第四步：jenkins把打包的容器应用镜像上传到harbor

第五步：jenkins通过ssh插件完成对web-server进行运行容器应用镜像的操作

#### 5.6.2 创建项目任务

##### 5.6.2.1 jenkins配置

![image-20230410221101874](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643418f667ce0.png)

![image-20230410221134957](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64341917a05f6.png)

![image-20230410221349554](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6434199e45556.png)

![image-20230410221454838](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643419df5a256.png)

源码地址

![image-20230410221619211](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64341a33c59f9.png)

![image-20230410222213097](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64341b95af349.png)

![image-20230410222235467](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64341babf351a.png)

![image-20230410222256772](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64341bc159ecf.png)



![image-20230410222655957](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64341cb087d21.png)

~~~powershell
Dockerfile：
REPOSITORY=192.168.126.23/library/solo:${Tag}
# 构建镜像
cat > Dockerfile << EOF
FROM 192.168.126.23/library/tomcat:8587
RUN rm -rf /usr/local/tomcat/webapps/ROOT
COPY target/*.war /usr/local/tomcat/webapps/ROOT.war
CMD ["/usr/local/tomcat/bin/catalina.sh", "run"]
EOF
docker build -t $REPOSITORY .

# 上传镜像
docker login 192.168.126.23 -u admin -p Harbor12345
docker push $REPOSITORY
docker logout 192.168.126.23
~~~

~~~powershell
shell script:
REPOSITORY=192.168.126.23/library/solo:${Tag}
# 部署
docker rm -f blog-solo |true
docker image rm $REPOSITORY |true
docker container run -d --name blog-solo -p 80:8080 $REPOSITORY
~~~

![image-20230410222827993](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64341d0c6bf76.png)

![image-20230410222845850](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64341d1e49f6d.png)

![image-20230410222930289](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/64341d4ad0721.png)

![image-20230410224919671](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/643421f054c86.png)

##### 5.6.2.2 访问harbor仓库

![image-20230410224949610](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6434220e3bbee.png)

##### 5.6.2.3 访问应用

![image-20230410225140457](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/04/10/6434227cf2b4f.png)
