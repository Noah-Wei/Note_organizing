# V2ray搭建教程（简易版）

## 1. 视频教程

V2ray搭建视频教程：▶ https://youtu.be/VSKhge6dKm8

## 2. 操作步骤

1.  **选择服务器**

2.  **打开 SSH 工具，连接上云服务器**

   - FinalShell：[点击下载>>](https://kjfx.lanzoui.com/iqm6Uosbzha)
   - 备用下载（含MAC版）：<a href="http://www.hostbuf.com/t/988.html" target="_blank">点击下载>></a>

3.  **V2Ray一键安装代码**

   ```bash
   bash <(curl -s -L https://git.io/v2ray-setup.sh)
   ```

4.  **放行端口**

   ```bash
   #放行端口
   iptables -I INPUT -p tcp --dport 80 -j ACCEPT
   iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
   ```

5.  **BBR加速（可选）**

   ```bash
   echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
   echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
   sysctl -p
   ```

6.  **科学上网软件下载**

   - 软件下载：https://github.com/Kejifaxian/welcome
