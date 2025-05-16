# 云服务器 SSH 连接保持长时间不断开

要让 **云服务器 SSH 连接保持长时间不断开**，可以调整 SSH 服务端和客户端的配置，防止因超时断开。以下是具体方法：  

## 方法 1：修改服务端 SSH 配置（`sshd_config`）
1. **登录云服务器**，编辑 SSH 配置文件：  
   
   ```bash
   sudo vim /etc/ssh/sshd_config
   ```
   
2. **修改或添加以下参数**（确保去掉注释 `#`）：  
   ```ini
   ClientAliveInterval 60      # 每 60 秒向客户端发送一次保活信号
   ClientAliveCountMax 10      # 如果 10 次（10×60秒≈10分钟）无响应才断开
   TCPKeepAlive yes            # 启用 TCP 保活机制
   ```

3. **重启 SSH 服务** 使配置生效：  
   ```bash
   sudo systemctl restart sshd   # Ubuntu/Debian
   sudo systemctl restart ssh    # CentOS/RHEL
   ```

## 方法 2：修改 SSH 客户端配置

这里以 **WindTerm** 为例，**修改 WindTerm 会话设置**

### 步骤：

1. **打开 WindTerm**，进入 **会话**，选择 **首选项**，然后选择次级目录中的 **默认首选项**。

   ![image-20250516170420855](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com//Typora/image-20250516170420855.png)

2. 在 **SSH** 选项卡下，找到 **SSH 连接** 设置

   ![image-20250516170746726](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com//Typora/image-20250516170746726.png)


## 方法 3：使用 `tmux` 或 `screen` 保持会话

即使 SSH 断开，`tmux` 或 `screen` 也能保持会话运行：
```bash
# 安装 tmux（如未安装）
sudo apt install tmux    # Debian/Ubuntu
sudo yum install tmux    # CentOS/RHEL

# 启动 tmux 会话
tmux new -s mysession

# 断开后重新连接
tmux attach -t mysession
```

---

## 方法 4：调整云服务器安全组/防火墙
某些云厂商（如阿里云、腾讯云）可能会强制断开长时间空闲的 SSH 连接，需检查：
1. **安全组规则** 是否有限制  
2. **云厂商的会话超时策略**（如阿里云的 ECS 默认 10 分钟无操作会断开）  

## 总结
| 方法                     | 适用场景                 | 备注                         |
| ------------------------ | ------------------------ | ---------------------------- |
| **修改 `sshd_config`**   | 长期有效，适用于所有用户 | 需服务器权限                 |
| **修改 SSH 客户端配置**  | 个人使用，不影响服务器   | 适合本地调整                 |
| **使用 `tmux`/`screen`** | 防止意外断连             | 适合长时间任务               |
| **检查云厂商策略**       | 排除厂商强制断连         | 如阿里云、AWS 可能有默认限制 |
