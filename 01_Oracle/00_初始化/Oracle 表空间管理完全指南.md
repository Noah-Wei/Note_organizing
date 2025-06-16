# Oracle 表空间管理完全指南

## 一、表空间基础概念

表空间是Oracle数据库的逻辑存储单元，由一个或多个数据文件组成。合理规划表空间对数据库性能和维护至关重要。

### 主要表空间类型
- **永久表空间**：存储用户数据（表、索引等）
- **临时表空间**：存储排序等临时操作数据
- **UNDO表空间**：存储事务回滚信息
- **系统表空间**：存储数据字典等系统对象

## 二、创建表空间完整语法

```sql
CREATE [TEMPORARY|UNDO] TABLESPACE tablespace_name
DATAFILE|TEMPFILE 'file_path' SIZE size_spec [K|M|G]
[AUTOEXTEND [ON|OFF] NEXT next_size [K|M|G] 
 MAXSIZE [max_size [K|M|G]|UNLIMITED]]
[EXTENT MANAGEMENT [LOCAL|DICTIONARY]
 [AUTOALLOCATE|UNIFORM [SIZE size_spec [K|M|G]]]
[SEGMENT SPACE MANAGEMENT [AUTO|MANUAL]]
[BLOCKSIZE size_in_kb]
[LOGGING|NOLOGGING]
[ONLINE|OFFLINE]
[DEFAULT STORAGE (storage_clause)]
[FLASHBACK [ON|OFF]]
[COMPRESS [FOR OLTP|BASIC]]
[ENCRYPTION [USING 'encrypt_algorithm']]
[ENCRYPT];
```

## 三、表空间创建实战示例

### 1. 标准数据表空间

```sql
CREATE TABLESPACE app_data
DATAFILE '/oracle/oradata/DB01/app_data01.dbf' SIZE 1G
AUTOEXTEND ON NEXT 100M MAXSIZE 5G
EXTENT MANAGEMENT LOCAL AUTOALLOCATE
SEGMENT SPACE MANAGEMENT AUTO
LOGGING
ONLINE;
```

### 2. 大文件表空间（Oracle 10g+）

```sql
CREATE BIGFILE TABLESPACE big_data
DATAFILE '/oracle/oradata/DB01/big_data01.dbf' SIZE 10G
AUTOEXTEND ON NEXT 1G MAXSIZE 32G
EXTENT MANAGEMENT LOCAL UNIFORM SIZE 10M
SEGMENT SPACE MANAGEMENT AUTO;
```

### 3. 加密表空间（Oracle 11g+）

```sql
CREATE TABLESPACE secure_data
DATAFILE '/oracle/oradata/DB01/secure_data01.dbf' SIZE 500M
ENCRYPTION USING 'AES256'
DEFAULT STORAGE(ENCRYPT);
```

## 四、表空间管理操作

### 1. 修改表空间

```sql
-- 添加数据文件
ALTER TABLESPACE app_data
ADD DATAFILE '/oracle/oradata/DB01/app_data02.dbf' SIZE 2G;

-- 调整自动扩展
ALTER DATABASE DATAFILE 
'/oracle/oradata/DB01/app_data01.dbf'
AUTOEXTEND ON NEXT 200M MAXSIZE 10G;

-- 重命名表空间
ALTER TABLESPACE old_name RENAME TO new_name;
```

### 2. 表空间状态管理

```sql
-- 只读/读写切换
ALTER TABLESPACE app_data READ ONLY;
ALTER TABLESPACE app_data READ WRITE;

-- 联机/脱机
ALTER TABLESPACE app_data OFFLINE;
ALTER TABLESPACE app_data ONLINE;
```

### 3. 删除表空间

```sql
-- 安全删除（保留数据文件）
DROP TABLESPACE app_data INCLUDING CONTENTS;

-- 完全删除（包括数据文件）
DROP TABLESPACE app_data INCLUDING CONTENTS AND DATAFILES;

-- 强制删除（有外键约束时）
DROP TABLESPACE app_data INCLUDING CONTENTS AND DATAFILES CASCADE CONSTRAINTS;
```

## 五、表空间监控与维护

### 1. 关键数据字典视图

```sql
-- 表空间基本信息
SELECT * FROM dba_tablespaces;

-- 数据文件信息
SELECT * FROM dba_data_files;

-- 空间使用情况
SELECT * FROM dba_free_space;

-- 临时表空间信息
SELECT * FROM dba_temp_files;
```

### 2. 常用监控查询

```sql
-- 表空间使用率
SELECT 
    t.tablespace_name,
    ROUND(SUM(d.bytes)/1024/1024,2) "总大小(MB)",
    ROUND(SUM(d.bytes)/1024/1024 - SUM(NVL(f.bytes,0))/1024/1024,2) "已使用(MB)",
    ROUND(SUM(NVL(f.bytes,0))/1024/1024,2) "剩余空间(MB)",
    ROUND((SUM(d.bytes) - SUM(NVL(f.bytes,0))) * 100 / SUM(d.bytes),2) "使用率(%)"
FROM 
    dba_data_files d,
    dba_free_space f,
    dba_tablespaces t
WHERE 
    d.tablespace_name = f.tablespace_name(+)
    AND d.tablespace_name = t.tablespace_name
GROUP BY 
    t.tablespace_name;
```

## 六、最佳实践建议

1. **规划原则**
   - 分离系统数据与用户数据
   - 为不同应用创建独立表空间
   - 数据与索引分开存储
   - 大对象(LOB)使用专用表空间

2. **性能优化**
   - 将频繁访问的表空间分散到不同物理磁盘
   - 为排序操作配置足够大的临时表空间
   - 考虑使用本地管理的表空间（现代Oracle默认）

3. **安全建议**
   - 对敏感数据使用加密表空间
   - 定期备份表空间元数据
   - 限制SYSTEM表空间的使用

4. **维护策略**
   - 设置合理的自动扩展参数
   - 定期监控空间使用情况
   - 考虑使用OMF(Oracle Managed Files)简化管理

## 七、常见问题解决方案

**问题1**：ORA-01144: 文件大小超出最大值

```sql
-- 解决方案：使用大文件表空间或增加DB_FILES参数
ALTER SYSTEM SET DB_FILES=1000 SCOPE=SPFILE;
```

**问题2**：ORA-01653: 表空间不足

```sql
-- 解决方案：扩展表空间
ALTER TABLESPACE app_data 
ADD DATAFILE '/new_path/app_data03.dbf' SIZE 2G;
```

**问题3**：ORA-03206: 自动扩展表空间达到MAXSIZE限制

```sql
-- 解决方案：调整MAXSIZE或添加数据文件
ALTER DATABASE DATAFILE 
'/path/to/datafile.dbf'
RESIZE 10G;
```