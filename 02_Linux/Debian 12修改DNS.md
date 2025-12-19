# Debian12 修改DNS

先通过**临时修改 DNS 解决拉取问题**，再做**持久化 DNS 配置**（避免重启后失效），步骤简单且针对性解决问题：

 ## 第一步：临时修改 DNS（立即解决拉取镜像问题） 

先临时替换 `/etc/resolv.conf` 为公共 DNS（如 Google/Cloudflare），无需重启服务，立即生效：

```sh
# 备份原 resolv.conf
sudo cp /etc/resolv.conf /etc/resolv.conf.bak

# 替换为公共 DNS（Google + Cloudflare，二选一即可）
sudo echo "nameserver 8.8.8.8" > /etc/resolv.conf
# 可选：添加备用 DNS
sudo echo "nameserver 1.1.1.1" >> /etc/resolv.conf

# 验证 DNS 是否生效（测试解析 docker.1ms.run）
nslookup docker.1ms.run
```

 如果 `nslookup` 输出中能看到 `Address:` 字段（解析出 IP），说明 DNS 已生效。

 ## 第二步：重新拉取 Docker 镜像 

此时再执行拉取命令，即可正常解析域名： ```bash docker pull yycx/oracle11 ``` 

## 第三步：持久化修改 DNS（避免重启后恢复原 DNS） 

临时修改的 `/etc/resolv.conf` 重启后会被覆盖，需按 Debian 12 标准方式做持久化（推荐 `systemd-resolved` 方案）：

```
# 1. 编辑 systemd-resolved 配置文件
sudo nano /etc/systemd/resolved.conf

# 2. 取消注释并修改以下内容（覆盖原 DNS）
[Resolve]
DNS=8.8.8.8 1.1.1.1  # 公共 DNS，空格分隔
FallbackDNS=8.8.4.4 1.0.0.1
Domains=~.  # 所有域名使用此 DNS
# DNSSEC=no  # 可选关闭 DNSSEC，避免解析异常

# 3. 重启 systemd-resolved 服务
sudo systemctl restart systemd-resolved

# 4. 验证持久化配置生效
resolvectl status | grep -A2 "DNS Servers"
```

输出中若看到 `DNS Servers: 8.8.8.8 1.1.1.1`，说明持久化配置成功。

### 补充：若仍拉取失败（镜像仓库本身问题）

如果修改 DNS 后仍报错，可能是 `yycx/oracle11` 镜像仓库（`docker.1ms.run`）本身不可用，可换官方 / 可靠镜像源：

```
# 示例：改用 Oracle 官方镜像（需先登录）
docker pull container-registry.oracle.com/database/express:11.2.0.2-xe

# 或使用国内镜像源（如阿里云）：先配置 Docker 镜像加速
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://阿里云镜像加速地址.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

