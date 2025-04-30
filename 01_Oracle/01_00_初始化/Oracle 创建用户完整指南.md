# Oracle 创建用户完整指南

在 Oracle 数据库中创建用户的基本语法如下，我将详细介绍各种选项和实际示例：

## 基本创建用户语法

```sql
CREATE USER 用户名 
IDENTIFIED BY 密码
[DEFAULT TABLESPACE 表空间名]
[TEMPORARY TABLESPACE 临时表空间名]
[QUOTA 大小 ON 表空间名]
[PROFILE 配置文件]
[PASSWORD EXPIRE]
[ACCOUNT {LOCK | UNLOCK}];
```

## 常用创建用户示例

### 1. 最简单形式（仅用户名和密码）
```sql
CREATE USER new_user IDENTIFIED BY "password123";
```

### 2. 完整形式（推荐生产环境使用）
```sql
CREATE USER app_user 
IDENTIFIED BY "Str0ngP@ssw0rd"
DEFAULT TABLESPACE users
TEMPORARY TABLESPACE temp
QUOTA 500M ON users
QUOTA 100M ON index_ts
PROFILE default
PASSWORD EXPIRE
ACCOUNT UNLOCK;
```

## 关键参数说明

1. **IDENTIFIED BY** - 设置用户密码
   - Oracle 12c 之后建议使用引号包裹密码（区分大小写）
   
2. **DEFAULT TABLESPACE** - 指定默认表空间
   - 如果不指定，默认为 SYSTEM 表空间（不推荐）

3. **TEMPORARY TABLESPACE** - 临时表空间
   - 用于排序操作等临时工作区

4. **QUOTA** - 表空间配额
   - 限制用户可使用空间大小（如 `QUOTA 500M ON users`）
   - `UNLIMITED` 表示无限制（慎用）

5. **PROFILE** - 用户配置文件
   - 控制密码策略、资源限制等

6. **PASSWORD EXPIRE** - 强制首次登录修改密码
   - 安全最佳实践

7. **ACCOUNT LOCK/UNLOCK** - 账户锁定状态
   - 可先创建锁定账户，稍后解锁

## 创建后必要授权

创建用户后通常需要授予权限：

```sql
-- 基本连接权限
GRANT CREATE SESSION TO new_user;

-- 开发常用权限组合
GRANT CONNECT, RESOURCE TO new_user;

-- 特定对象权限
GRANT SELECT, INSERT ON schema.table TO new_user;
```

## 注意事项

1. 需要具有 CREATE USER 系统权限（通常为 DBA 角色）
2. 密码复杂度要求取决于 Oracle 版本和配置
3. 生产环境应避免使用简单密码
4. 12c 及以上版本的多租户环境中，需要在正确的 PDB 中创建用户
   ```sql
   ALTER SESSION SET CONTAINER = orclpdb;
   CREATE USER pdb_user IDENTIFIED BY password;
   ```

## 验证用户创建

```sql
SELECT username, account_status, created 
FROM dba_users 
WHERE username = 'NEW_USER';
```

## 删除用户

```sql
DROP USER new_user CASCADE;  -- CASCADE 会删除用户所有对象
```
