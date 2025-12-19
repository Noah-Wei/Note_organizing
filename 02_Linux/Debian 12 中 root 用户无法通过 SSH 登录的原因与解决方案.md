# Debian 12 中 root 用户无法通过 SSH 登录的原因与解决方案

> [!CAUTION]
>
> 本文适用于 Debian 12 /Debian 13，其他发行版可能略有不同。

## 一、问题背景

在安装 **Debian 12** 系统后，很多用户会遇到这样一个现象：

- 普通用户可以正常通过 SSH 登录服务器
- 使用 `root` 用户通过 SSH 登录却始终失败
- 即使确认 root 密码正确，仍然提示 `Permission denied`

这往往会让人误以为是系统安装错误或 SSH 服务配置异常。

------

## 二、问题根本原因

### 1. Debian 12 的默认安全策略

在 **Debian 11 / Debian 12** 中，OpenSSH 服务默认 **禁止 root 用户使用密码方式进行 SSH 登录**。

默认配置效果等同于：

```conf
PermitRootLogin prohibit-password
```

这意味着：

- ❌ root 用户 **不能使用密码** 进行 SSH 登录
- ✅ 普通用户可以通过 SSH 登录
- ✅ root 用户只能在本地登录，或通过 `sudo` 提权

这是 Debian 官方的安全设计，而不是系统故障。

------

### 2. 为什么要禁止 root 的 SSH 登录？

主要原因包括：

- root 是系统中权限最高的用户，是暴力破解的首要目标
- 直接暴露 root SSH 登录接口会显著增加被入侵风险
- 使用普通用户 + sudo 的方式更安全、更可控

因此，Debian 官方推荐的运维模式是：

```text
普通用户 SSH 登录 → 使用 sudo 执行管理操作
```

------

## 三、如何确认是否是该原因？

使用 **普通用户登录服务器** 后，执行以下命令：

```bash
sudo sshd -T | grep permitrootlogin
```

常见输出结果：

```text
permitrootlogin prohibit-password
```

或：

```text
permitrootlogin no
```

如果是以上结果，即可确认 root 的 SSH 登录被系统策略禁止。

------

## 四、Debian 官方推荐的正确使用方式（推荐）

```bash
ssh user@server_ip
sudo -i
```

这种方式的优点：

- 不暴露 root 账户
- 操作更安全、可审计
- 符合生产环境最佳实践

------

## 五、如果一定要开启 root SSH 登录（不推荐）

> ⚠️ **不建议在公网服务器或生产环境中使用**
> ⚠️ 仅适用于内网、学习或测试环境

### 1️⃣ 修改 SSH 配置文件

```bash
sudo nano /etc/ssh/sshd_config
```

找到或添加以下配置：

```conf
PermitRootLogin yes
```

如果原本存在：

```conf
PermitRootLogin prohibit-password
```

请将其修改为：

```conf
PermitRootLogin yes
```

------

### 2️⃣ 确保 root 已设置密码

```bash
sudo passwd root
```

------

### 3️⃣ 重启 SSH 服务

```bash
sudo systemctl restart ssh
```

------

### 4️⃣ 测试 root SSH 登录

```bash
ssh root@服务器IP
```

------

## 六、更安全的替代方案（强烈推荐）

### 方案一：root 仅允许密钥登录

```conf
PermitRootLogin prohibit-password
```

并将 root 的公钥写入：

```bash
/root/.ssh/authorized_keys
```

**优点：**

- 禁用密码
- 有效防止暴力破解
- 安全性显著提升

------

### 方案二（最推荐）：完全禁用 root SSH

```conf
PermitRootLogin no
```

使用方式：

```bash
ssh user@server
sudo -i
```

这是 **Debian / Ubuntu 官方最推荐的运维模式**。

------

## 七、其他可能导致 root SSH 登录失败的原因

如果你已经修改了配置，但 root 仍无法登录，可能还存在以下情况：

- `/etc/ssh/sshd_config.d/*.conf` 中的配置覆盖了主配置（Debian 12 常见）
- 使用了 `AllowUsers` / `DenyUsers` 限制
- Fail2Ban 或防火墙规则封禁
- 云厂商镜像（如 AWS、阿里云）强制禁用 root 登录

