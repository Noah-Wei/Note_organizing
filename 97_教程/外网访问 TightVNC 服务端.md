# 外网访问 TightVNC 服务端

TightVNC 是一款免费而且开源的远程桌面软件，它允许用户在不同的操作系统之间实现无缝连接，TightVNC支持 Windows、macOS 和 Linux 等多个操作系统，为用户提供高效便捷的远程控制体验。本文将详细的介绍如何在 Windows 系统电脑端安装使用 TightVNC 服务端和客户端，并且结合路由侠内网穿透实现远程桌面连接。

## 一、下载安装 TightVNC 服务端

1. 在本地电脑（也就是被远程的设备）上下载 TightVNC 服务端。
   下载地址：https://www.tightvnc.com/download.php。选择 64 位下载。

   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/46d1799f85e2c75a76825588ecf97217.png)

2. 下载后双击程序开始安装，点击【Next】，进行下一步。
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/f08e05a53319804f2205b074e745b8c3.png)

3. 勾选同意后，点击【Next】，进行下一步。
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/59747e5bfb0246a629b7813b54f402e0.png)

4. 选择第一个轻量级安装。
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/64fae344b1df7754f5dea8df7145570f.png)

5. 点击【Next】，进行下一步。
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/5df89948798e3e68e30dbb6f2175082b.png)

6. 点击【Install】，进行下一步。
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/dbe4850f6bed569ab7136c36e6c4015e.png)

7. 设置密码，点击【Ok】。
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/9a6192e5103156f189e8450305c5fde6.png)

8. 安装完成后，点击屏幕右下角小图标可以看到 VNC 运行服务，打开后可以看到端口是 5900
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/b00a7c0f825e8dd88d88ed1341928079.png)

9. 在 Access Control 选项中把允许 loopback 连接勾选上，点击【Ok】就可以了。下面我们进行远程连接测试
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/352573dcdeb3b29bf1519a12bd9ef05f.png)

## 二、下载安装 vnc 客户端进行远程测试

1. 现在我们在另一台电脑（远程的设备）中安装 vnc 客户端，通过客户端去连接刚才上面我们在被远程设备上安装的 vnc 服务端。
   下载地址：https://www.realvnc.com/en/connect/download/viewer/ 。
   如下图，选择我们自己的系统版本下载，下载后直接安装，安装步骤非常简单，这里就不演示了。
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/acfa3d59c09b8c53f716015261e9b4a5.png)

2. 安装好后，双击打开客户端，如图，在框里输入被远程设备的局域网 IP 地址。按回车键。
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/fae3e39be195daca894f3002c8fd94e8.png)

3. 出现如下图提示，勾选不再提示，点击【Continue】按钮。

   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/5c3c8e3a17a330795c8a78f57b0017b1.png)

4. 输入在 vnc 服务端设置的密码，点击【Ok】。
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/80a101519dd8e0c4b77be1a5b2a126ef.png)

5. 这里就可以看到成功连接上了被远程的电脑。
   ![img](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/lskypro/20250615/92236952fb6a0921069676fddb393d9b.png)

