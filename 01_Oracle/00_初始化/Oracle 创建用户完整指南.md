# Oracle 数据库用户管理完整指南

## 一、用户创建基础语法

Oracle 创建用户的核心语法结构如下：

```sql
CREATE USER 用户名 
IDENTIFIED BY 密码
[DEFAULT TABLESPACE 默认表空间]
[TEMPORARY TABLESPACE 临时表空间]
[QUOTA 空间配额 ON 表空间]
[PROFILE 配置文件]
[PASSWORD EXPIRE]
[ACCOUNT {LOCK | UNLOCK}];
```

## 二、用户创建实战示例

### 1. 基础创建示例

```sql
-- 最简形式（仅用户名密码）
CREATE USER basic_user IDENTIFIED BY "Temp123!";

-- 推荐生产环境使用
CREATE USER app_user 
IDENTIFIED BY "P@ssw0rd_2023"
DEFAULT TABLESPACE user_data
TEMPORARY TABLESPACE temp
QUOTA 1G ON user_data
QUOTA 200M ON indexes
PROFILE app_profile
PASSWORD EXPIRE
ACCOUNT UNLOCK;
```

### 2. 多表空间配额分配

```sql
CREATE USER data_user 
IDENTIFIED BY "Data_secure1"
DEFAULT TABLESPACE user_data
TEMPORARY TABLESPACE temp
QUOTA 2G ON user_data
QUOTA 500M ON report_data
QUOTA 300M ON indexes;
```

## 三、参数详解

| 参数                   | 说明               | 最佳实践                                       |
| ---------------------- | ------------------ | ---------------------------------------------- |
| `IDENTIFIED BY`        | 用户密码           | Oracle 12c+ 建议使用双引号包裹密码，区分大小写 |
| `DEFAULT TABLESPACE`   | 默认存储表空间     | 必须指定，避免使用SYSTEM表空间                 |
| `TEMPORARY TABLESPACE` | 临时工作区         | 通常指定为TEMP表空间                           |
| `QUOTA`                | 空间使用限额       | 按需分配，避免UNLIMITED                        |
| `PROFILE`              | 密码和资源限制配置 | 建议创建专用profile                            |
| `PASSWORD EXPIRE`      | 强制修改密码       | 新用户建议启用                                 |
| `ACCOUNT LOCK/UNLOCK`  | 账户状态           | 可先锁定后按需解锁                             |

## 四、权限管理

创建用户后必须授予适当权限：

```sql
-- 基础连接权限
GRANT CREATE SESSION TO app_user;

-- 开发常用权限包
GRANT CONNECT, RESOURCE TO dev_user;

-- 精细化权限控制
GRANT SELECT, INSERT, UPDATE ON hr.employees TO report_user;
GRANT EXECUTE ON pkg_utilities TO app_user;

-- 角色授权
GRANT read_only_role TO analyst_user;
```

## 五、多租户环境注意事项

12c及以上版本需注意：

```sql
-- 切换到目标PDB
ALTER SESSION SET CONTAINER = orclpdb;

-- 在PDB中创建用户
CREATE USER pdb_user IDENTIFIED BY "Pdb123#"
DEFAULT TABLESPACE users
TEMPORARY TABLESPACE temp;

-- 授予PDB权限
GRANT CREATE SESSION, RESOURCE TO pdb_user CONTAINER = CURRENT;
```

## 六、用户管理维护

### 1. 查询用户信息

```sql
SELECT username, account_status, created, 
       default_tablespace, temporary_tablespace
FROM dba_users
WHERE username = 'APP_USER';

-- 查看配额使用情况
SELECT tablespace_name, bytes/1024/1024 MB_used, max_bytes/1024/1024 MB_max
FROM dba_ts_quotas
WHERE username = 'APP_USER';
```

### 2. 修改用户属性

```sql
-- 修改密码
ALTER USER app_user IDENTIFIED BY "NewPass123!";

-- 修改表空间配额
ALTER USER app_user QUOTA 2G ON user_data;

-- 锁定/解锁账户
ALTER USER app_user ACCOUNT LOCK;
ALTER USER app_user ACCOUNT UNLOCK;
```

### 3. 安全删除用户

```sql
-- 检查用户对象
SELECT object_type, COUNT(*) 
FROM dba_objects 
WHERE owner = 'APP_USER'
GROUP BY object_type;

-- 级联删除用户及对象
DROP USER app_user CASCADE;
```

## 七、最佳实践建议

1. **密码策略**：
   - 使用复杂密码（大小写字母+数字+特殊字符）
   - 定期修改密码（通过PROFILE配置）
   - 避免使用默认密码

2. **权限管理**：
   - 遵循最小权限原则
   - 使用角色管理权限组
   - 定期审计权限分配

3. **资源控制**：
   - 合理设置表空间配额
   - 为不同应用创建专用用户
   - 监控资源使用情况

4. **多租户环境**：
   - 明确区分CDB和PDB用户
   - 使用CONTAINER参数控制授权范围
   - 为每个PDB维护单独的用户体系

## 八、常见问题解决方案

**问题1**：ORA-65096: 公用用户名或角色名无效

```sql
-- 原因：12c+版本在CDB$ROOT创建了不符合规定的用户名
-- 解决方案：
ALTER SESSION SET "_ORACLE_SCRIPT"=true;
CREATE USER legacy_user IDENTIFIED BY password;
ALTER SESSION SET "_ORACLE_SCRIPT"=false;
```

**问题2**：ORA-01950: 表空间'USERS'无权限

```sql
-- 原因：用户缺少表空间配额
-- 解决方案：
ALTER USER app_user QUOTA 100M ON users;
```

**问题3**：ORA-28000: 账户被锁定

```sql
-- 解决方案：
ALTER USER locked_user ACCOUNT UNLOCK;
```
