# 本地连接虚拟机Oracle数据库配置指南

## 环境准备

- **本地主机**：Windows 11
- **虚拟化软件**：VMware Workstation 17
- **虚拟机系统**：Windows 11
- **数据库版本**：Oracle 19c
- **网络配置**：NAT模式
- **虚拟机IP地址**：192.168.9.128

## 连接方式选择

1. **SQL*Plus命令行连接**
2. **PL/SQL Developer图形化工具连接**

## 详细配置步骤

### 一、本地主机配置

#### 1.1 安装Oracle Instant Client

1. 访问[Oracle官网下载页面](https://www.oracle.com/database/technologies/instant-client/downloads.html)
2. 下载适用于Windows的Instant Client基础包和SQL*Plus附加包
3. 解压到本地目录（如`C:\oracle\instantclient_19_10`）

#### 1.2 配置系统环境变量

1. 添加新的系统变量：
   - 变量名：`ORACLE_HOME`
   - 变量值：`C:\oracle\instantclient_19_10`

2. 修改Path变量，添加：
   ```
   %ORACLE_HOME%
   ```

3. 创建`tnsnames.ora`文件（位于`%ORACLE_HOME%\network\admin`）：
   ```ora
   ORCL_VM =
     (DESCRIPTION =
       (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.9.128)(PORT = 1521))
       (CONNECT_DATA =
         (SERVER = DEDICATED)
         (SERVICE_NAME = orcl)
       )
     )
   ```

### 二、虚拟机配置

#### 2.1 检查监听器状态

```cmd
lsnrctl status
```

若监听器未运行，执行：
```cmd
lsnrctl start
```

#### 2.2 配置监听器文件

编辑`listener.ora`（通常位于`C:\app\<用户名>\product\19.0.0\dbhome_1\network\admin`）：

```ora
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.9.128)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )
```

重启监听器：
```cmd
lsnrctl stop
lsnrctl start
```

#### 2.3 配置TNS文件

编辑`tnsnames.ora`（同目录）：

```ora
ORCL =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.9.128)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )
```

#### 2.4 防火墙配置

开放1521端口：
```cmd
netsh advfirewall firewall add rule name="Oracle1521" dir=in action=allow protocol=TCP localport=1521
```

或临时关闭防火墙（测试用）：
```cmd
netsh advfirewall set allprofiles state off
```

#### 2.5 数据库服务注册

```sql
sqlplus / as sysdba
ALTER SYSTEM SET LOCAL_LISTENER='(ADDRESS=(PROTOCOL=TCP)(HOST=192.168.9.128)(PORT=1521))' SCOPE=BOTH;
ALTER SYSTEM REGISTER;
exit
```

验证注册状态：
```cmd
lsnrctl status
```

### 三、连接测试

#### 3.1 使用SQL*Plus连接

```cmd
sqlplus 用户名/密码@192.168.9.128:1521/orcl
```

或使用TNS别名：
```cmd
sqlplus 用户名/密码@ORCL_VM
```

#### 3.2 使用PL/SQL Developer连接

1. 打开PL/SQL Developer
2. 新建连接：
   - 用户名：您的数据库用户名
   - 密码：对应的密码
   - 数据库：`192.168.9.128:1521/orcl`或`ORCL_VM`
   - 连接为：Normal

## 常见问题排查

### 错误：ORA-12541: TNS:no listener

**解决方案**：
1. 确认虚拟机监听器已启动
2. 检查`listener.ora`中的HOST是否为虚拟机IP
3. 验证防火墙设置

### 错误：ORA-12170: TNS:Connect timeout occurred

**解决方案**：
1. 检查网络连通性（ping 192.168.9.128）
2. 确认NAT网络配置正确
3. 验证端口是否真正开放（使用telnet测试）

### 错误：ORA-01017: invalid username/password

**解决方案**：
1. 确认用户名/密码正确
2. 检查用户是否被锁定
3. 尝试用sysdba账户登录后重置密码

## 附录：重要文件位置参考

| 文件         | 默认路径                                              |
| ------------ | ----------------------------------------------------- |
| listener.ora | C:\app\<用户名>\product\19.0.0\dbhome_1\network\admin |
| tnsnames.ora | 同上                                                  |
| sqlnet.ora   | 同上                                                  |
