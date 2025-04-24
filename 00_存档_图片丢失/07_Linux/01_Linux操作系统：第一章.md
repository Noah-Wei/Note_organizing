# 第一章：Linux概述与安装

## 1.1 Linux概述

### 1.1.1 操作习题（OS）

- **什么是操作系统？**
  - 管理计算机硬件与软件资源的计算机程序
- **操作系统化的作用**
  - 是把计算机系统中对硬件设备的操作封装起来，供应用软件调用，也是提供一个让用户与系统交互的操作界面。、
- **常见的操作系统**
  - PC端：Windows、MacOS、Linux
  - 移动端：Android、Ios

### 1.1.2操作系统发展史

- **unix**
  
  - **1970年，美国贝尔实验室的 Ken Thompson，以 BCPL语言 为基础，设计出很简单且很接近硬件的 B语言（取BCPL的首字母），并且他用B语言写了第一个UNIX操作系统。**
  - 因为B语言的跨平台性较差，为了能够在其他的电脑上也能够运行这个非常棒的Unix操作系统，Dennis Ritchie和Ken Thompson 从B语言的基础上准备研究一个更好的语言。1972年，美国贝尔实验室的 Dennis Ritchie在B语言的基础上最终设计出了一种新的语言，他取了BCPL的第二个字母作为这种语言的名字，这就是**C语言**
  - 1973年初，C语言的主体完成。Thompson和Ritchie迫不及待地开始用它完全重写了现在大名鼎鼎的Unix操作系统
  
- **Minix**

  - 因为AT&T(通用电气)的政策改变，在Version 7 Unix推出之后，发布新的使用条款，将UNIX源代码私有化，在大学中不再能使用UNIX源代码。Andrew S. Tanenbaum(塔能鲍姆)教授为了能在课堂上教授学生操作系统运作的实务细节，决定在不使用任何AT&T的源代码前提下，自行开发与UNIX兼容的操作系统，以避免版权上的争议。他以小型UNIX（mini-UNIX）之意，将它称为MINIX。

- **Linux**

  - Minix最有名的学生用户是Linus Torvalds，他在芬兰的赫尔辛基大学用Minix操作平台建立了一个新的操作系统的内核，他把它叫做**Linux**。
  - Linux是一套免费使用和自由传播的类Unix操作系统，是一个基于POSIX和UNIX的多用户、多任务、支持多线程和多CPU的操作系统
  - Linux能运行主要的UNIX工具软件、应用程序和网络协议。它支持32位和64位硬件。Linux继承了Unix以网络为核心的设计思想，是一个性能稳定的多用户网络操作系统。
  - **目前市面上较知名的发行版有：Ubuntu（PC端用多）、RedHat和CentOS（服务器端用多）、Debain、Fedora、SuSE、OpenSUSE。**
  

![image-20221120162359969](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379e42902675.png)

#### 1.1.2.1 Linux和Unix区别

- **最大区别**
  - **Linux**开放源代码的自由软件。开发是处在一个**完全开放**的环境之中
  - **Unix**是对源代码实行知识产权保护的传统商业软件。开发完全是处在一个黑箱之中,只有**相关的开发人员**才能够接触的产品的原型
- **具体区别**
  - UNIX系统大多是与硬件配套的，而Linux则可运行在多种硬件平台
  - UNIX是商业软件，收费，而Linux是自由软件，免费、公开源代码的
  - Linux商业化的有RedHat Linux 、SuSe Linux、slakeware Linux、国内的红旗等，还有Turbo Linux
  - Unix主要有Sun 的Solaris、IBM的AIX,　HP的HP-UX，以及x86平台的的SCO Unix/Unixware

### 1.1.3 Linux发行版本

| 排行 | 版本              | 描述                                                   |
| ---- | ----------------- | ------------------------------------------------------ |
| 1    | **MX Linux**      | **基于Debian和antiX**                                  |
| 2    | **Linux Mint**    | **基于Ubuntu和Debian**                                 |
| 3    | **Ubuntu**        | **顶级Linux发行版之一，企业桌面端应用多一点**          |
| 4    | **Elementary OS** | **基于Debian**                                         |
| 5    | **Manjaro Linux** | **基于Arch Linux**                                     |
| 6    | **Zorin OS**      | **基于Debian**                                         |
| 7    | **Fedora**        | **软件技术方面处于领先地位**                           |
| 8    | **Debian**        | **设计得非常稳定**                                     |
| 9    | **CentOS**        | **基于Fedora和Red Hat的企业最佳Linux，服务器端应用多** |
| 10   | **Kali Linux**    | **基于Debian**                                         |

- **Linux能做什么**
  - Linux可作为企业级服务器，或嵌入式开发平台也包含个人桌面系统。包含虚拟化、数据库服务器、Web服务器、开发平台等等

- **哪些人要学习Linux**
  - Linux管理员，oracle管理员，网络工程师，程序开发者等等。Linux系统涉及方面非常广泛，生态也越来越强大，非常适合大家学习

- **应用领域**
  - 个人桌面领域
    - 此领域是传统Linux应用最薄弱的环节，传统Linux由于界面简单、操作复杂、应用软件少的缺点，一直被windows所压制，但近些年来随着ubuntu、fedora等优秀桌面环境的兴起，同时各大硬件厂商对其支持的加大，Linux在个人桌面领域的占有率在逐渐的提高
    - 典型代表：ubuntu、fedora、suse linux
  - 服务器领域
    - linux免费、稳定、高效等特点在这里得到了很好的体现，但早期因为维护、运行等原因同样受到了很大的限制，但近些年来linux服务器市场得到了飞速的提升，尤其在一些高端领域尤为广泛
    - 典型代表： Red Hat公司的AS系列、完全开源的debian系列、suse EnterPrise 11系列等
  - 嵌入式领域
    - linux运行稳定、对网络的良好支持性、低成本，且可以根据需要进行软件裁剪，内核最小可以达到几百KB等特点，使其近些年来在嵌入式领域的应用得到非常大的提高

## 1.2 安装VMware软件

### 1.2.1 安装VMware

> 基于Windows系统

傻瓜式安装，官网下载，依次往下装。

在线教程：[VMware超详细安装完整教程_咩了个咩咩的博客-CSDN博客_vmware安装](https://blog.csdn.net/weixin_56306210/article/details/125910847)

### 1.2.2 安装Linux系统

>  这里Linux使用发行版的Centos7.6版本

    	CentOS：是一个基于Red Hat Linux 提供的可自由使用源代码的企业级Linux发行版本。每个版本的 CentOS都会获得十年的支持（通过安全更新方式）。新版本的 CentOS 大约每两年发行一次，而每个版本的 CentOS 会定期（大概每六个月）更新一次，以便支持新的硬件。这样，建立一个安全、低维护、稳定、高预测性、高重复性的 Linux 环境。CentOS是Community Enterprise Operating System的缩写。
    	CentOS 是RHEL(Red Hat Enterprise Linux)源代码再编译的产物，而且在RHEL的基础上修正了不少已知的 Bug ，相对于其他 Linux 发行版，其稳定性值得信赖

#### 1.2.2.1 获取CentOS镜像文件

- 进入官网直接下载：[Download - CentOS Wiki](https://wiki.centos.org/Download)

- 使用阿里云镜像战下载：[阿里巴巴开源镜像站-OPSX镜像站-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/mirror/)

  - 进入网站，选择`OS镜像`，然后选择需要的版本，并下载
  - 选择`minimal`这一选项系统，最简系统，没有可视化桌面，模拟企业环境

  ![image-20221120170909926](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379eeb6b68e1.png)

#### 1.2.2.2 安装虚拟机

- 打开`Vmware`软件，点击首页的`创建新的虚拟机`，然后选择`自定义`，然后`下一步`

![image-20221120173021750](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f3ae6359c.png)

- 默认不变，点击`下一步`

![image-20221120173127309](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f3efe202d.png)

- 选择第三个选项：`稍后安装操作系统`，然后点击`下一步` 

![image-20221120173241359](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f439ebfc9.png)

- 选择`Linux`选项，并且选择对应的版本，然后点击`下一步` 

![image-20221120173335239](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f46fd1d6b.png)

- 根据自己的需求，修改名称和位置，然后点击`下一步` 

![image-20221120173612654](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f50d40a0e.png)

- 根据自己电脑的性能决定是否，增大数字，也可默认，然后点击`下一步` 

![image-20221120173757538](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f57621d0a.png)

- 按需分配内存，调大则是虚拟机运行速度加快一点，也可默认，然后点击`下一步` 

![image-20221120173854459](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f5af233a8.png)

- 网络类型选择第二个选项，然后点击`下一步` 

![image-20221120173948301](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f5e4df56f.png)

- 保持默认，然后点击`下一步` 

![image-20221120174112492](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f6391f75e.png)

- 保持默认，然后点击`下一步` 

![image-20221120174131719](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f64c5038b.png)

- 保持默认，然后点击`下一步` 

![image-20221120174145550](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f65a26a86.png)

- 保持默认，然后点击`下一步` 。磁盘大小按需分配，由于前面磁盘类型选择的是`SCSI`，所以是用多少占用多少

![image-20221120174235467](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f68c1c98e.png)

- 保持默认，然后点击`下一步` 

![image-20221120174400262](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379f6e0d6c09.png)

- 然后默认，点击完成。至此，**虚拟机创作完成，但未安装系统**

#### 1.2.2.3 安装CentOS系统

> 注意事项：
>
> 安装的时候虚拟机与宿主机**键盘操作切换**
>
> 当在虚拟机系统内输入内容时，需要退出来，可以使用同时按住`Ctrl+Alt`，将鼠标退出虚拟机
>
> 可以使用鼠标单击进入虚拟机

- 点击`编辑虚拟机设置`，选择`CD/DVD`，使用`ISO映像文件`，这里的映像文件就是我们前面下载下来的ISO文件，然后确定

![image-20221120175926799](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379fa7f78869.png)

- 点击开启此虚拟机，进入虚拟机，可以选择按 ENTEN 或者不操作也可以，会自动安装

![image-20221120180152156](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379fb10bf8f4.png)

- 这里建议选择English，点击Continue

![image-20221120180358865](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379fb8f81059.png)

- 设置一下安装位置

![image-20221120180636201](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/6379fc2cdc0c9.png)

- 配置root超级管理员账号的密码，和 创建用户账号

![image-20221120191645208](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/637a0c9e26efd.png)

- 至此安装系统结束

#### 1.2.2.4 基本配置与SSH连接

> 安装完VM与系统后，需要进行**网络配置**。第一个目标为可以进行SSH连接，可以从本机到VM进行文件传送
>
> 需要远程登录的软件：XShell和XFtp搭配 或者 FinalShell
>
> 虚拟机网络是属于在主机的**子网**中

- **XShell和XFtp**
  - 官方地址：[XSHELL - NetSarang Website](https://www.xshell.com/zh/xshell/)
  - XShell和XFtp个人用户可以免费试用
  - 下载完成后，出来安装位置根据个人需要需改，其余默认到底，直至安装结束

- **FinalShell软件安装**

  - 官网地址：[SSH工具 SSH客户端 (hostbuf.com)](http://www.hostbuf.com/)
  - 下载地址：http://www.hostbuf.com/downloads/finalshell_install.exe
  - 下载完成后，出来安装位置根据个人需要需改，其余默认到底，直至安装结束

- **网络设置配置**

  - 网络模型图

  ![image-20221120201238117](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/637a19b6d9284.png)

  - 打开本机的`控制面板`，然后点击`网络和共享中心`，点击`更改适配器设置`，查看VMnet是否开启（有两个）

  ![image-20221120195319055](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/637a15300696a.png)

  - 打开`Vmware`，点击`编辑`，然后点击`虚拟网络编辑器`，点击右下角的`更改设置`，获取权限

  ![image-20221120201501648](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/637a1a465a81f.png)

  - 点击`VMnet8`，选择下方的`NAT设置`，记住`子网IP`、`子网掩码`以及`网关IP`

    - 三种模式介绍
      - **桥接模式**：VMware 虚拟出来的操作系统就像是**局域网中的一台独立的主机**，它可以访问网内任何一台机器，需要手工为虚拟系统，配置 IP 地址、子网掩码，而且还要和宿主机器处于同一网段，这样虚拟系统才能和宿主机器进行通信。同时，由于这个虚拟系统是局域网中的一个独立的主机系统，那么就可以手工配置它的 TCP/IP 配置信息，以实现通过局域网的网关或路由器访问互联网
      - **NAT模式**：使用 NAT 模式，就是让虚拟系统借助 NAT（网络地址转换）功能，**通过宿主机器所在的网络来访问公网**。也就是说，使用 NAT 模式可以实现在虚拟系统里访问互联网，但前提是主机可以访问互联网。NAT 模式下的虚拟系统的 TCP/IP，配置信息是由 VMnet8（NAT）虚拟网络的 DHCP 服务器提供的，无法进行手工修改，因此虚拟系统也就无法和本局域，网中的其他真实主机进行通讯
      - **主机模式**：虚拟网络是一个**全封闭**的网络，它唯一能够访问的就是主机，当然**多个虚拟机之间也可以互相访问**。其实 Host-only网络和 NAT 网络很相似，不同的地方就是 Host-only 网络没有 NAT 服务，所以虚拟网络不能连接到Internet。**主机和虚拟机之间的通信**是通过 VMware Network Adepter **VMnet1** 虚拟网卡来实现的。此时如果想要虚拟机上外 则需要主机联网并且网络共享

    - 三个VMnet介绍
      - **VMnet0**：用于虚拟桥接网络下的虚拟交换机
      - **VMnet1**：用于虚拟Host-Only网络下的虚拟交换机
      - **VMnet8**：用于虚拟NAT网络下的虚拟交换机

  ![image-20221120201654751](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/637a1ab776125.png)

  - 设置好VM虚拟机，现在开始进入系统设置

    - 我们在系统内使用**命令**（ip addr）查看我们网卡及部分网络信息，在虚拟机内我们使用的是**ens33**，我们看到现在还是没有网络，更没有ip

    ![image-20221120202404466](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/637a1c65325d5.png)

    - 这里我们先cd到网络配置文件路径下

    ```shell
    [root@localhost ~]# cd /etc/sysconfig/network-scripts/
    ```

    - 在查看信息文件

    ```shell
    [root@localhost ~]# ls
    ```

    - 看到ifcfg-en33  文件编辑

    ```shell
    [root@localhost ~]# vi ifcfg-en33
    ```

    ![image-20221120202619435](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/637a1cec2b766.png)

    - 进入编辑界面后，设置一些参数

    ```shell
    BOOTPROYTO=DHCP     修改为    BOOTPROYTO=static
    
    ONBOOT=no           修改为    ONBOOT=yes
    
    增加：
    # 增加IP，这里的ip需要时在同一个网段下即可
    IPADDR=192.168.22.3
    
    # 子网掩码与主机一直即可
    NETMASK=255.255.255.0
    
    # 配置网关与主机一直即可
    GATEWAY=192.168.0.1
    ```

    - 我们保存好后，重启一下网络服务即可

    ```shell
    [root@localhost network-scripts]# service network restart
    Restarting network (via systemctl):                        [  OK  ]
    ```

    ![image-20221120202816109](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/11/20/637a1d60c2730.png)

  - XShell7远程登录

    - 按需输入ip地址，用户名和密码即可

