# 本地连接远程Oracle

## 1、下载Oracle Instant Client

- 官网
  - [Oracle Instant Client Downloads](https://www.oracle.com/database/technologies/instant-client/downloads.html)

## 2、配置环境变量

根目录添加到系统变量中的 path

## 3、登录

```sql
-- 正常登录
sqlplus 用户名/密码@//主机名:端口/服务名

sqlplus scott/123456@122.51.163.205:1521/orcl

-- 高权限登录
sqlplus sys/123456@122.51.163.205:1521/orcl as sysdba
```

