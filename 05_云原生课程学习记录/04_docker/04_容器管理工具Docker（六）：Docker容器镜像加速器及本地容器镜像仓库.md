# 容器管理工具Docker（六）：Docker容器镜像加速器及本地容器镜像仓库

## 一、容器镜像加速器

> 由于国内访问国外的容器镜像仓库速度比较慢，因此国内企业创建了容器镜像加速器，以方便国内用户使用容器镜像。

### 1.1 获取阿里云容器镜像加速地址

![image-20230221215459213](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/21/63f4cd3b00308.png)

![image-20230221215529021](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/21/63f4cd51b0fd4.png)

![image-20230221215854850](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/21/63f4ce1f86d39.png)

### 1.2 配置docker daemon使用加速器

- 添加daemon.json配置文件


~~~powershell
[root@localhost ~]# vim /etc/docker/daemon.json

{
        "registry-mirrors": ["https://s27w6kze.mirror.aliyuncs.com"]
}
~~~

- 重启docker


~~~powershell
[root@localhost ~]# systemctl daemon-reload
[root@localhost ~]# systemctl restart docker
~~~

- 尝试下载容器镜像


~~~powershell
[root@localhost ~]# docker pull centos
~~~

- 可以发现，速度较之前有了很大的提升

## 二、容器镜像仓库

### 2.1 docker hub（公有）

#### 2.1.1 注册并登录

> 准备邮箱及用户ID

网址：[Docker](https://www.docker.com/)

![image-20220125224745376](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/21/63f4d10599c39.png)

![image-20220125225914283](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/21/63f4d2f77f48a.png)

#### 2.1.3 创建容器镜像仓库

![image-20220125230046456](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/21/63f4d2fc4895a.png)

![image-20230221222220471](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/21/63f4d39d3815f.png)

![image-20230221222301872](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/21/63f4d3c68817e.png)

#### 2.1.4 在本地登录 Docker Hub

> 默认可以不添加 docker hub 容器镜像仓库地址

- 在本地登录 Docker Hub

~~~powershell
[root@localhost ~]# docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: edenwei
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
~~~

- 退出登录 Docker Hub


~~~powershell
[root@localhost ~]# docker logout
Removing login credentials for https://index.docker.io/v1/
~~~

#### 2.1.5 上传容器镜像到 docker hub

> 在登录Docker Hub主机上传容器镜像,向全球用户共享容器镜像。
>
> 上传容器镜像需要为容器镜像重新打标记

- 查看原始容器镜像

```powershell
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
centos       latest    5d0da3dc9764   4 months ago   231MB
```

- 重新为容器镜像打标记

```powershell
[root@localhost ~]# docker tag centos:latest edenwei/centos:v1
```

- 再次查看容器镜像

~~~powershell

[root@localhost ~]# docker images
REPOSITORY              TAG       IMAGE ID       CREATED        SIZE
edenwei/centos   v1        5d0da3dc9764   4 months ago   231MB
centos                  latest    5d0da3dc9764   4 months ago   231MB
~~~

- 上传容器镜像至 docker hub


~~~powershell
[root@localhost ~]# docker push edenwei/centos:v1
The push refers to repository [docker.io/edenwei/centos]
74ddd0ec08fa: Mounted from library/centos 
v1: digest: sha256:a1801b843b1bfaf77c501e7a6d3f709401a1e0c83863037fa3aab063a7fdb9dc size: 529
~~~

- 在网页中查看仓库


![image-20230221223414616](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/21/63f4d66754444.png)

#### 2.1.6 从 docker hub 下载容器镜像

> 在其它主机上下载

![image-20230221223911956](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/21/63f4d790a4c5b.png)

- 下载容器镜像并查看

~~~powershell
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE

[root@localhost ~]# docker pull edenwei/centos:v1
v1: Pulling from edenwei/centos
a1d0c7532777: Pull complete 
Digest: sha256:a1801b843b1bfaf77c501e7a6d3f709401a1e0c83863037fa3aab063a7fdb9dc
Status: Downloaded newer image for edenwei/centos:v1
docker.io/edenwei/centos:v1

[root@localhost ~]# docker images
REPOSITORY       TAG       IMAGE ID       CREATED         SIZE
edenwei/centos   v1        5d0da3dc9764   17 months ago   231MB
~~~

### 2.2 harbor（私有）

> 配置要求：CPU 2核，内存 4G，硬盘 100G

#### 2.2.1 文件准备

##### 2.2.1.1 获取 docker compose二进制文件

- 下载 docker-compose 二进制文件

~~~powershell
dock[root@localhost ~]# wget https://github.com/docker/compose/releases/download/1.25.0/docker-compose-Linux-x86_64
~~~

- 查看已下载二进制文件


~~~powershell
[root@localhost ~]# ls
docker-compose-Linux-x86_64
~~~

- 移动二进制文件到 /usr/bin 目录，并更名为 docker-compose


~~~powershell
[root@localhost ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin

[root@localhost ~]# mv docker-compose-Linux-x86_64 /usr/bin/docker-compose
~~~

- 为二进制文件添加可执行权限


~~~powershell
[root@localhost ~]# chmod +x /usr/bin/docker-compose
~~~

- 安装完成后，查看docker-compse版本


~~~powershell
[root@localhost ~]# docker-compose version
docker-compose version 1.25.0, build 0a186604
docker-py version: 4.1.0
CPython version: 3.7.4
OpenSSL version: OpenSSL 1.1.0l  10 Sep 2019
~~~

##### 2.2.1.2 获取harbor安装文件

![image-20220125232445910](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/22/63f613a0d1f20.png)

![image-20220125232519365](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/22/63f613a67b0ed.png)



![image-20220125233602760](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/22/63f613e6748ca.png)

![image-20220125233652604](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/22/63f61442e4d40.png)





![image-20220125233739356](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/22/63f61448c4b7c.png)



- 下载harbor离线安装包


~~~powershell
[root@localhost ~]# wget https://github.com/goharbor/harbor/releases/download/v2.4.1/harbor-offline-installer-v2.4.1.tgz
~~~

- 查看已下载的离线安装包


~~~powershell
[root@localhost ~]# ls
harbor-offline-installer-v2.4.1.tgz

~~~

##### 2.2.1.3 获取TLS文件

- 查看准备好的证书

~~~powershell
[root@localhost ~]# ls
kubemsb.com_nginx.zip
~~~

- 解压证书压缩包文件


~~~powershell
[root@localhost ~]# unzip kubemsb.com_nginx.zip
Archive:  kubemsb.com_nginx.zip
Aliyun Certificate Download
  inflating: 6864844_kubemsb.com.pem
  inflating: 6864844_kubemsb.com.key
~~~

- 查看解压出的文件


~~~powershell
[root@localhost ~]# ls
6864844_kubemsb.com.key
6864844_kubemsb.com.pem
~~~

#### 2.2.2 修改配置文件

- 解压harbor离线安装包


~~~powershell
[root@localhost ~]# tar xf harbor-offline-installer-v2.4.1.tgz
~~~

- 查看解压出来的目录


~~~powershell
[root@localhost ~]# ls
harbor 
~~~

- 移动证书到harbor目录


```powershell
[root@localhost ~]# mv 6864844_kubemsb.com.* harbor
```

- 查看harbor目录

~~~powershell
[root@localhost ~]# ls harbor
6864844_kubemsb.com.key  6864844_kubemsb.com.pem  common.sh  harbor.v2.4.1.tar.gz  harbor.yml.tmpl  install.sh  LICENSE  prepare
~~~

- 创建配置文件


~~~powershell
[root@localhost ~]# cd harbor/
[root@localhost ~]# mv harbor.yml.tmpl harbor.yml
~~~

- 修改配置文件内容


~~~powershell
[root@localhost ~]# vim harbor.yml

harbor.yml 文件内容

# Configuration file of Harbor

# The IP address or hostname to access admin UI and registry service.
# DO NOT use localhost or 127.0.0.1, because Harbor needs to be accessed by external clients.
hostname: www.kubemsb.com 修改为域名，而且一定是证书签发的域名

# http related config
http:
  # port for http, default is 80. If https enabled, this port will redirect to https port
  port: 80

# https related config
https:
  # https port for harbor, default is 443
  port: 443
  # The path of cert and key files for nginx
  certificate: /home/harbor/6864844_kubemsb.com.pem 证书（绝对入径）
  private_key: /home/harbor/6864844_kubemsb.com.key 密钥（绝对入径）

# # Uncomment following will enable tls communication between all harbor components
# internal_tls:
#   # set enabled to true means internal tls is enabled
#   enabled: true
#   # put your cert and key files on dir
#   dir: /etc/harbor/tls/internal

# Uncomment external_url if you want to enable external proxy
# And when it enabled the hostname will no longer used
# external_url: https://reg.mydomain.com:8433

# The initial password of Harbor admin
# It only works in first time to install harbor
# Remember Change the admin password from UI after launching Harbor.
harbor_admin_password: 123456 访问密码
......
~~~

#### 2.2.3 执行预备脚本

~~~powershell
[root@localhost ~]# ./prepare

输出
prepare base dir is set to /root/harbor
Clearing the configuration file: /config/portal/nginx.conf
Clearing the configuration file: /config/log/logrotate.conf
Clearing the configuration file: /config/log/rsyslog_docker.conf
Generated configuration file: /config/portal/nginx.conf
Generated configuration file: /config/log/logrotate.conf
Generated configuration file: /config/log/rsyslog_docker.conf
Generated configuration file: /config/nginx/nginx.conf
Generated configuration file: /config/core/env
Generated configuration file: /config/core/app.conf
Generated configuration file: /config/registry/config.yml
Generated configuration file: /config/registryctl/env
Generated configuration file: /config/registryctl/config.yml
Generated configuration file: /config/db/env
Generated configuration file: /config/jobservice/env
Generated configuration file: /config/jobservice/config.yml
Generated and saved secret to file: /data/secret/keys/secretkey
Successfully called func: create_root_cert
Generated configuration file: /compose_location/docker-compose.yml
Clean up the input dir
~~~

#### 2.2.4 执行安装脚本

~~~powershell
[root@localhost ~]# ./install.sh

输出
[Step 0]: checking if docker is installed ...

Note: docker version: 20.10.12

[Step 1]: checking docker-compose is installed ...

Note: docker-compose version: 1.25.0

[Step 2]: loading Harbor images ...

[Step 3]: preparing environment ...

[Step 4]: preparing harbor configs ...
prepare base dir is set to /root/harbor

[Step 5]: starting Harbor ...
Creating network "harbor_harbor" with the default driver
Creating harbor-log ... done
Creating harbor-db     ... done
Creating registry      ... done
Creating registryctl   ... done
Creating redis         ... done
Creating harbor-portal ... done
Creating harbor-core   ... done
Creating harbor-jobservice ... done
Creating nginx             ... done
✔ ----Harbor has been installed and started successfully.----
~~~

#### 2.2.5 验证运行情况

~~~powershell
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED              STATUS                        PORTS                                                                            NAMES
71c0db683e4a   goharbor/nginx-photon:v2.4.1         "nginx -g 'daemon of…"   About a minute ago   Up About a minute (healthy)   0.0.0.0:80->8080/tcp, :::80->8080/tcp, 0.0.0.0:443->8443/tcp, :::443->8443/tcp   nginx
4e3b53a86f01   goharbor/harbor-jobservice:v2.4.1    "/harbor/entrypoint.…"   About a minute ago   Up About a minute (healthy)                                                                                    harbor-jobservice
df76e1eabbf7   goharbor/harbor-core:v2.4.1          "/harbor/entrypoint.…"   About a minute ago   Up About a minute (healthy)                                                                                    harbor-core
eeb4d224dfc4   goharbor/harbor-portal:v2.4.1        "nginx -g 'daemon of…"   About a minute ago   Up About a minute (healthy)                                                                                    harbor-portal
70e162c38b59   goharbor/redis-photon:v2.4.1         "redis-server /etc/r…"   About a minute ago   Up About a minute (healthy)                                                                                    redis
8bcc0e9b06ec   goharbor/harbor-registryctl:v2.4.1   "/home/harbor/start.…"   About a minute ago   Up About a minute (healthy)                                                                                    registryctl
d88196398df7   goharbor/registry-photon:v2.4.1      "/home/harbor/entryp…"   About a minute ago   Up About a minute (healthy)                                                                                    registry
ed5ba2ba9c82   goharbor/harbor-db:v2.4.1            "/docker-entrypoint.…"   About a minute ago   Up About a minute (healthy)                                                                                    harbor-db
dcb4b57c7542   goharbor/harbor-log:v2.4.1           "/bin/sh -c /usr/loc…"   About a minute ago   Up About a minute (healthy)   127.0.0.1:1514->10514/tcp                                                        harbor-log

~~~

#### 2.2.6 访问harbor UI界面

##### 2.2.6.1 在物理机通过浏览器访问

![image-20220126000804490](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/22/63f61a88e6423.png)









![image-20220126000840905](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/22/63f61a8db886e.png)



##### 2.2.6.2 在Docker Host主机通过域名访问

- 添加域名解析


~~~powershell

[root@localhost ~]# vim /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.126.132 www.kubemsb.com
~~~



![image-20220126001253192](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/22/63f623315cf89.png)

![image-20220126001510880](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/22/63f62336875d5.png)



## 三、docker镜像上传至Harbor及从harbor下载

> 在其他主机上使用 harbor

### 3.1  修改docker daemon使用harbor

- 添加 /etc/docker/daemon.json 文件，默认不存在，需要手动添加


~~~powershell

[root@localhost ~]# vim /etc/docker/daemon.json
{
		如果没有域名解析，用IP地址
        "insecure-registries": ["www.kubemsb.com"] 
}
~~~

- 重启加载daemon配置


~~~powershell
[root@localhost ~]# systemctl daemon-reload
~~~

- 重启docker


~~~powershell
[root@localhost ~]# systemctl restart docker
~~~

> 如果重启 docker 之后，通过 docker ps 查看进程不是9个
>
> 可以通过  cd /home/harbor 目录，先 docker-compose down 命令关闭再 docker-compose up -d 命令重启



### 3.2 docker tag

- 查看已有容器镜像文件


~~~powershell

[root@localhost ~]# docker images
REPOSITORY                      TAG       IMAGE ID       CREATED        SIZE
centos                          latest    5d0da3dc9764   4 months ago   231MB
~~~

- 为已存在镜像重新添加tag


~~~powershell
[root@localhost ~]# docker tag centos:latest www.kubemsb.com/library/centos:v1
~~~

- 再次查看本地容器镜像


~~~powershell
[root@localhost ~]# docker images
REPOSITORY                       TAG       IMAGE ID       CREATED        SIZE
centos                           latest    5d0da3dc9764   4 months ago   231MB
www.kubemsb.com/library/centos   v1        5d0da3dc9764   4 months ago   231MB
~~~

### 3.3 docker push

~~~powershell
[root@localhost ~]# docker login www.kubemsb.com
Username: admin  用户名 admin
Password:        密码   12345
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/[root@localhost ~]#credentials-store

Login Succeeded 登陆成功
~~~

- 推送本地容器镜像到harbor仓库


~~~powershell
[root@localhost ~]# docker push www.kubemsb.com/library/centos:v1
~~~

![image-20220126002747864](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/22/63f62a79c1b7a.png)



### 3.4 docker pull

> 在其它主机上下载或使用harbor容器镜像仓库中的容器镜像

- 在本地添加域名解析


~~~powershell
[root@localhost ~]# vim /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.10.155 www.kubemsb.com
~~~

- 在本地添加/etc/docker/daemon.json文件，其中为本地主机访问的容器镜像仓库


~~~powershell
在本地添加/etc/docker/daemon.json文件，其中为本地主机访问的容器镜像仓库
[root@localhost ~]# vim /etc/docker/daemon.json
{
        "insecure-registries": ["www.kubemsb.com"]
}
~~~

- 重启配置


~~~powershell
[root@localhost ~]# systemctl daemon-reload
[root@localhost ~]# systemctl restart docker
~~~

- 下载容器镜像


~~~powershell

[root@localhost ~]# docker pull www.kubemsb.com/library/centos:v1
v1: Pulling from library/centos
Digest: sha256:a1801b843b1bfaf77c501e7a6d3f709401a1e0c83863037fa3aab063a7fdb9dc
Status: Downloaded newer image for www.kubemsb.com/library/centos:v1
www.kubemsb.com/library/centos:v1
~~~

- 查看已下载的容器镜像


~~~powershell
[root@localhost ~]# docker images
REPOSITORY                       TAG       IMAGE ID       CREATED        SIZE
www.kubemsb.com/library/centos   v1        5d0da3dc9764   4 months ago   231MB
~~~
