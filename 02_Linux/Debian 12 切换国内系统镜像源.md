# Debian 12 切换国内系统镜像源完整指南

在中国使用 Debian 12（Bookworm）时，如果仍然使用官方默认的软件源，常常会遇到以下问题：

- `apt update` 速度极慢
- 连接超时、下载失败
- 安装软件体验较差

因此，**切换到国内镜像源几乎是 Debian 12 的必做操作**。本文将系统性地介绍 Debian 12 如何安全、规范地切换到国内镜像源。

------

## 一、确认 Debian 版本

首先确认当前系统是否为 Debian 12：

```bash
cat /etc/os-release
```

如果看到以下内容：

```text
VERSION_CODENAME=bookworm
```

说明你正在使用 Debian 12，本文内容完全适用。

------

## 二、备份原有软件源（非常重要）

在修改系统软件源之前，务必先进行备份，以便出现问题时可以快速恢复：

```bash
cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

> 即使 `sources.list` 是空文件，也建议进行备份，这是一个良好的运维习惯。

------

## 三、编辑软件源配置文件

在 Debian 12 中，传统的源配置文件仍然是：

```text
/etc/apt/sources.list
```

使用 root 用户直接编辑：

```bash
nano /etc/apt/sources.list
```

> 提示：Debian 12 默认安装后，`sources.list` 可能为空，这是正常现象。

------

## 四、推荐的国内镜像源

下面列出几组 **稳定、常用的国内 Debian 镜像源**，任选其一即可。

### 1️⃣ 清华大学镜像（强烈推荐）

```text
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware
```

特点：

- 更新及时
- 稳定性高
- 使用人数最多

------

### 2️⃣ 阿里云镜像（企业环境友好）

```text
deb https://mirrors.aliyun.com/debian/ bookworm main contrib non-free non-free-firmware
deb https://mirrors.aliyun.com/debian/ bookworm-updates main contrib non-free non-free-firmware
deb https://mirrors.aliyun.com/debian/ bookworm-backports main contrib non-free non-free-firmware

deb https://mirrors.aliyun.com/debian-security bookworm-security main contrib non-free non-free-firmware
```

特点：

- 访问稳定
- 对云服务器支持良好

------

### 3️⃣ 中国科学技术大学（USTC）

```text
deb https://mirrors.ustc.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
deb https://mirrors.ustc.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
deb https://mirrors.ustc.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware

deb https://mirrors.ustc.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware
```

------

## 五、保存并更新软件索引

保存文件后，执行：

```bash
apt update
```

如果看到大量 `Hit:` 或 `Get:` 信息，且下载速度明显提升，说明镜像源已经生效。

------

## 六、升级系统（可选但推荐）

在成功更新软件索引后，可以进行一次系统升级：

```bash
apt upgrade
```

如果是刚安装的新系统，也可以使用：

```bash
apt full-upgrade
```

------

## 七、验证镜像源是否生效

执行以下命令：

```bash
apt-cache policy
```

如果输出中出现类似内容：

```text
500 https://mirrors.tuna.tsinghua.edu.cn/debian bookworm/main
```

说明系统已经成功切换到国内镜像源。

------

