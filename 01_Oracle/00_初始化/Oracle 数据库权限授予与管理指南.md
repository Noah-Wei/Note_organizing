# Oracle 数据库权限授予与管理指南

## 一、权限授予基础语法

```sql
GRANT 权限或角色 [, 权限或角色...]
TO 用户名或角色 [, 用户名或角色...]
[WITH ADMIN OPTION];
```

## 二、系统预定义角色详解

### 1. CONNECT 角色

**核心功能**：基础数据库连接权限

**包含权限**：
- `CREATE SESSION` - 允许连接到数据库
- `ALTER SESSION` - 修改会话参数
- `CREATE TABLE` - 在用户模式下创建表
- `CREATE VIEW` - 创建视图
- `CREATE SEQUENCE` - 创建序列
- `CREATE SYNONYM` - 创建同义词

**适用场景**：
- 只需要查询权限的报表用户
- 前端应用只读账户

### 2. RESOURCE 角色

**核心功能**：对象创建与管理权限

**包含权限**：
- `CREATE TABLE`/`VIEW`/`SEQUENCE`等所有对象创建权限
- `CREATE PROCEDURE`/`FUNCTION`/`PACKAGE` - 创建存储程序
- `CREATE TRIGGER` - 创建触发器
- `CREATE TYPE` - 创建对象类型

**典型应用**：
- 应用开发人员账户
- 需要创建数据库对象的中级用户

### 3. DBA 角色

**核心功能**：数据库完全控制权限

**关键权限**：
- 所有系统权限（`SELECT ANY TABLE`, `CREATE ANY TABLE`等）
- 用户管理权限（`CREATE USER`, `ALTER USER`等）
- 空间管理权限（`UNLIMITED TABLESPACE`）
- 数据库维护权限（`ALTER DATABASE`等）

**安全警告**：
⚠️ 此角色等同于超级用户权限
⚠️ 生产环境应严格控制授予范围

## 三、权限授予最佳实践

### 1. 标准权限组合

```sql
-- 开发环境基本权限
GRANT connect, resource TO dev_user;

-- 只读用户权限
GRANT connect TO report_user;
GRANT SELECT ON schema.tables TO report_user;

-- 管理员权限（谨慎使用）
GRANT connect, resource, dba TO dba_user;
```

### 2. 精细化权限控制

```sql
-- 精确授予对象权限
GRANT SELECT, INSERT, UPDATE ON hr.employees TO app_user;
GRANT EXECUTE ON pkg_utilities TO app_user;

-- 列级权限控制
GRANT SELECT(emp_id, emp_name) ON employees TO auditor;
```

### 3. 权限管理增强功能

```sql
-- 允许被授权者继续授权（WITH ADMIN OPTION）
GRANT create session TO user1 WITH ADMIN OPTION;

-- 角色授权
CREATE ROLE app_developer;
GRANT create table, create view TO app_developer;
GRANT app_developer TO dev_user;
```

## 四、权限回收与审计

### 1. 权限回收语法

```sql
REVOKE 权限或角色 FROM 用户名;
```

### 2. 权限审计方法

```sql
-- 查看用户系统权限
SELECT * FROM dba_sys_privs WHERE grantee = 'USERNAME';

-- 查看用户角色权限
SELECT * FROM dba_role_privs WHERE grantee = 'USERNAME';

-- 查看用户对象权限
SELECT * FROM dba_tab_privs WHERE grantee = 'USERNAME';
```

## 五、安全建议与注意事项

1. **权限分配原则**
   - 遵循最小权限原则
   - 生产环境避免直接使用DBA角色
   - 为不同应用创建专用角色

2. **权限管理策略**
   - 定期审查权限分配
   - 使用角色简化权限管理
   - 重要操作实施权限分离

3. **特殊场景处理**
   - 多租户环境注意CONTAINER参数
   - 12c+版本注意公用用户与本地用户区别
   - 敏感数据考虑使用VPD(虚拟私有数据库)

4. **审计与监控**
   ```sql
   -- 启用权限变更审计
   AUDIT grant, revoke BY ACCESS;
   
   -- 监控权限使用情况
   SELECT * FROM dba_audit_trail 
   WHERE action_name IN ('GRANT','REVOKE');
   ```

## 六、常见问题解决方案

**问题1**：ORA-01031: 权限不足

```sql
-- 解决方案：检查并补充所需权限
SELECT * FROM session_privs;  -- 查看当前权限
```

**问题2**：权限传递问题

```sql
-- 通过角色授予的权限在存储过程中不可用
-- 解决方案：直接授予权限或使用AUTHID CURRENT_USER
```

**问题3**：权限回收级联影响

```sql
-- 使用ADMIN OPTION授予的权限回收时不会级联
-- 需要单独回收每个用户的权限
```
