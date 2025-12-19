# 在 **Debian 12** 上配置 **Docker 镜像加速**

> 整体思路：
>
> ​	给 Docker 配置 `registry-mirrors`，让它从国内/可达性更好的镜像源拉取镜像。

------

## 一、确认 Docker 已安装并正常运行

```bash
docker -v
systemctl status docker
```

------

## 二、创建 / 修改 Docker 配置文件

Docker 的配置文件路径是：

```bash
/etc/docker/daemon.json
```

### 1️⃣ 创建或编辑文件

```bash
sudo mkdir -p /etc/docker
sudo nano /etc/docker/daemon.json
```

### 2️⃣ 写入镜像加速配置（推荐）

#### ✅ 通用稳定版（建议先用）

```json
{
  "registry-mirrors": [
    "https://docker.1ms.run",
    "https://docker.m.daocloud.io",
    "https://mirror.ccs.tencentyun.com",
    "https://hub-mirror.c.163.com"
  ]
}
```

> 说明：

- **DaoCloud**：稳定、速度快
- **腾讯云 / 网易**：作为备用
- Docker 会按顺序自动选择可用的

⚠️ **注意 JSON 格式，不能有多余逗号**

------

## 三、重启 Docker 服务

```bash
sudo systemctl daemon-reexec
sudo systemctl restart docker
```

确认 Docker 正常启动：

```bash
systemctl status docker
```

------

## 四、验证镜像加速是否生效

```bash
docker info
```

在输出中找到：

```text
Registry Mirrors:
 https://docker.m.daocloud.io/
 https://mirror.ccs.tencentyun.com/
 https://hub-mirror.c.163.com/
```

看到这些就说明 **配置成功了** ✅

------

## 五、测试拉取镜像

```bash
docker pull hello-world
```

如果速度明显变快、且不再 timeout / EOF，说明加速器在工作。

------

## 六、⚠️ 如果你是 NAT 云服务器（重点）

### 如果你遇到：

- `TLS handshake timeout`
- `connection reset by peer`
- `i/o timeout`

那说明：

- **不是 Docker 配置错**
- 而是 **NAT 出口 / 国际网络受限**

### 这时的解决方案优先级：

1️⃣ Docker 镜像加速（你现在在做的）
2️⃣ 从另一台能正常拉镜像的服务器 **导出镜像再传过来**
3️⃣ 自建私有 Registry（内网/同地域）

------

## 七、可选：开启日志 & 并发优化（进阶）

如果你后面要拉很多大镜像（如 Oracle / Elasticsearch）：

```json
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io"
  ],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m",
    "max-file": "3"
  }
}
```

