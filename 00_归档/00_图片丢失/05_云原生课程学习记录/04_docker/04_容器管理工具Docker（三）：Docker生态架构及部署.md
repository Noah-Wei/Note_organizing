# 容器管理工具Docker（三）：Docker生态架构及部署

## 一、Docker生态架构

### 1.1 Docker 容器无处不在

![image-20220118165726624](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee38ec60fc0.png)

### 1.2 生态架构

![image-20220118170228476](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee46d2ac1a6.png)

#### 1.2.1 Docker Host

用于安装Docker daemon的主机，即为Docker Host，并且该主机中可基于容器镜像运行容器。

#### 1.2.2 Docker daemon

用于管理Docker Host中运行的容器、容器镜像、容器网络等，管理由Containerd.io提供的容器。

#### 1.2.3 Registry

容器镜像仓库，用于存储已生成容器运行模板的仓库，用户使用时，可直接从容器镜像仓库中下载容器镜像，即容器运行模板，就可以运行容器镜像中包含的应用了。例如：Docker Hub,也可以使用Harbor实现企业私有的容器镜像仓库。

#### 1.2.4 Docker client

Docker Daemon客户端工具，用于同Docker Daemon进行通信，执行用户指令，可部署在Docker Host上，也可以部署在其它主机，能够连接到Docker Daemon即可操作。

#### 1.2.5 Image

把应用运行环境及计算资源打包方式生成可再用于启动容器的不可变的基础设施的模板文件，主要用于基于其启动一个容器。

#### 1.2.6 Container

由容器镜像生成，用于应用程序运行的环境，包含容器镜像中所有文件及用户后添加的文件，属于基于容器镜像生成的可读写层，这也是应用程序活跃的空间。

#### 1.2.7 Docker Dashboard

> 仅限于MAC与Windows操作系统上安装使用。

Docker Dashboard 提供了一个简单的界面，使您能够直接从您的机器管理您的容器、应用程序和映像，而无需使用 CLI 来执行核心操作。

### 1.3 Docker版本

- Docker-ce Docker社区版，主要用于个人开发者测试使用，免费版本
- Docker-ee Docker企业版，主要用于为企业开发及应用部署使用，收费版本，免费试用一个月

## 二、Docker部署

> 安装Docker-ce版本。

### 2.1 使用YUM源部署

> YUM源可以使用官方YUM源、清华大学开源镜像站配置YUM源，也可以使用阿里云开源镜像站提供的YUM源，建议选择使用阿里云开源镜像站提供的YUM源，原因速度快。

#### 2.1.1 获取阿里云开源镜像站YUM源文件

- 获取阿里云开源镜像站YUM源文件：[docker-ce镜像_docker-ce下载地址_docker-ce安装教程-阿里巴巴开源镜像站 (aliyun.com)](https://developer.aliyun.com/mirror/docker-ce)

![image-20230216222401802](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee3c808ce81.png)



![image-20230216222450714](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee3cb162ecb.png)

![image-20220118182432607](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee46d999401.png)

- 在docker host上使用 wget下载到/etc/yum.repos.d目录中即可。


~~~powershell
[root@localhost ~]# wget -O /etc/yum.repos.d/docker-ce.repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
~~~

![image-20220118182749037](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee46e1d3edd.png)

#### 2.1.2 安装Docker-ce

> 在docker host上安装即可，本次使用YUM源中稳定版本，由于版本在不断更新，不同的时间安装版本也不相同，使用方法基本一致。

- 安装docker-ce，此为docker daemon，所有依赖将被yum自动安装，含docker client等。


~~~powershell
[root@localhost ~]# yum -y install docker-ce
~~~

![image-20220118183627705](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee46e7a0a4e.png)

#### 2.1.3 配置Docker Daemon启动文件

> 由于Docker使用过程中会对Centos操作系统中的Iptables防火墙中的FORWARD链默认规划产生影响及需要让Docker Daemon接受用户自定义的daemon.json文件，需要要按使用者要求的方式修改。

~~~powershell
[root@localhost ~]# vim /usr/lib/systemd/system/docker.service


ExecStartPost=/sbin/iptables -P FORWARD ACCEPT
~~~

![image-20220118184308554](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee46ebe40bb.png)



![image-20220118184420795](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee46efa4295.png)

#### 2.1.4 启动Docker服务并查看已安装版本

- 重启加载daemon文件

```powershell
[root@localhost ~]# systemctl daemon-reload
```

- 启动docker daemon

```powershell
[root@localhost ~]# systemctl start docker
```

- 设置开机自启动

```powershell
[root@localhost ~]# systemctl enable docker
```

- 使用docker version客户端命令查看已安装docker软件版本

~~~powershell
[root@localhost ~]# docker version
Client: Docker Engine - Community		客户端
 Version:           23.0.1
 API version:       1.42
 Go version:        go1.19.5
 Git commit:        a5ee5b1
 Built:             Thu Feb  9 19:51:00 2023
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community		服务端-管理引擎
 Engine:
  Version:          23.0.1
  API version:      1.42 (minimum version 1.12)
  Go version:       go1.19.5
  Git commit:       bc3805a
  Built:            Thu Feb  9 19:48:42 2023
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.16
  GitCommit:        31aa4358a36870b21a992d3ad2bef29e1d693bec
 runc:
  Version:          1.1.4
  GitCommit:        v1.1.4-0-g5fd4c4d
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
~~~

### 2.2 使用二进制文件部署

> 官方不建议此种部署方式，主因为不能自动更新，在条件有限制的情况下使用。

二进制安装参考网址：https://docs.docker.com/engine/install/binaries/

![image-20220118185625614](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee46f54fbea.png)



![image-20220118185711358](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee46f9108f5.png)



![image-20220118185814368](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee4701d7aaf.png)



![image-20220118185911155](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee470566356.png)



![image-20220118190235182](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee470b892e1.png)



![image-20220118190327846](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee470ea12b1.png)



![image-20220118190507974](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee47124fee7.png)



![image-20220118190622614](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee47153e463.png)



![image-20220118190654730](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/16/63ee4718b9d9e.png)



~~~powershell
获取二进制文件，此文件中包含dockerd与docker 2个文件。
# wget https://download.docker.com/linux/static/stable/x86_64/docker-20.10.9.tgz
~~~



~~~powershell
解压下载的文件
# tar xf docker-20.10.9.tgz
查看解压出的目录
# ls docker
containerd       containerd-shim-runc-v2  docker   docker-init   runc
containerd-shim  ctr                      dockerd  docker-proxy
~~~





~~~powershell
安装解压后的所有二进制文件
# cp docker/* /usr/bin/
~~~



~~~powershell
运行Daemon
# dockerd &

会有大量的信息输出，停止后，直接回车即可使用。
~~~



>如果您需要使用其他选项启动守护程序，请相应地修改上述命令或创建并编辑文件`/etc/docker/daemon.json` 以添加自定义配置选项。



~~~powershell
确认是否可以使用docker客户端命令
# which docker
/usr/bin/docker

使用二进制安装的docker客户端
# docker version
Client:
 Version:           20.10.9
 API version:       1.41
 Go version:        go1.16.8
 Git commit:        c2ea9bc
 Built:             Mon Oct  4 16:03:22 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.9
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.8
  Git commit:       79ea9d3
  Built:            Mon Oct  4 16:07:30 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.4.11
  GitCommit:        5b46e404f6b9f661a205e28d59c982d3634148f8
 runc:
  Version:          1.0.2
  GitCommit:        v1.0.2-0-g52b36a2d
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

~~~
