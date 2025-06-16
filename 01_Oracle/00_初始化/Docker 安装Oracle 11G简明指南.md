# Docker安装Oracle 11G简明指南

## 一、准备工作

在开始之前，请确保您的系统已安装Docker并可以正常运行。

## 二、拉取Oracle 11G镜像

使用以下命令从Docker Hub拉取Oracle 11G镜像：

```bash
docker pull yycx/oracle11
```

## 三、创建并运行容器

运行以下命令创建并启动Oracle 11G容器：

```bash
docker run -d -p 1521:1521 --name oracle11g yycx/oracle11
```

参数说明：
- `-d`：后台运行容器
- `-p 1521:1521`：将容器端口映射到主机端口
- `--name oracle11g`：为容器指定名称

## 四、访问Oracle数据库

### 1. 进入容器

```bash
docker exec -it oracle11g bash
```

### 2. 切换至oracle用户

```bash
su - oracle
```

### 3. 使用SQL*Plus连接数据库

```bash
sqlplus / as sysdba
```

## 五、数据库基本配置

### 1. 解锁SCOTT用户

```sql
ALTER USER scott ACCOUNT UNLOCK;
COMMIT;
```

首次登录SCOTT用户时需要修改密码（默认密码为tiger）：

```sql
CONN scott/tiger
-- 系统会提示修改密码，可设置为scott或其他安全密码
```

### 2. 修改管理员密码

```sql
ALTER USER sys IDENTIFIED BY 123456;

ALTER USER system IDENTIFIED BY 123456;

ALTER USER scott IDENTIFIED BY 123456;
```

### 3. 密码策略配置

设置密码永不过期：

```sql
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```

取消登录尝试次数限制：

```sql
ALTER PROFILE DEFAULT LIMIT FAILED_LOGIN_ATTEMPTS UNLIMITED;
```

## 六、创建表空间和用户

### 1. 创建表空间

```sql
CREATE SMALLFILE TABLESPACE "ODB" 
DATAFILE '/opt/oracle/app/oradata/orcl/ODB1' 
SIZE 500M 
AUTOEXTEND ON NEXT 500M 
MAXSIZE UNLIMITED 
LOGGING 
EXTENT MANAGEMENT LOCAL 
SEGMENT SPACE MANAGEMENT AUTO;

ALTER TABLESPACE "ODB" 
ADD DATAFILE '/opt/oracle/app/oradata/orcl/ODB2' 
SIZE 500M 
AUTOEXTEND ON NEXT 500M 
MAXSIZE UNLIMITED;
```

### 2. 创建用户

基本创建方式：

```sql
CREATE USER 用户名 IDENTIFIED BY 密码 
DEFAULT TABLESPACE ODB;
```

或带更多选项：

```sql
CREATE USER 用户名 IDENTIFIED BY 密码
DEFAULT TABLESPACE USERS
TEMPORARY TABLESPACE TEMP
QUOTA UNLIMITED ON USERS;
```

### 3. 授予权限

```sql
GRANT CONNECT, RESOURCE, DBA TO 用户名;
```

## 七、连接信息汇总

- **容器信息**：
  - 容器名称：oracle11g
  - 端口：1521
  - SID：orcl

- **系统用户**：
  - root/install
  - oracle/install

- **数据库用户**：
  - SYS/123456（或您修改后的密码）
  - SYSTEM/123456（或您修改后的密码）
  - SCOTT/123456（修改后）

## 八、注意事项

1. 生产环境中请使用更复杂的密码替代示例中的简单密码
2. 根据实际需求调整表空间大小和自动扩展设置
3. 定期备份重要数据
4. 考虑使用数据卷持久化数据库文件
