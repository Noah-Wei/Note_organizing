# Oracle 数据库高效插入百万级数据指南

## 一、准备工作

### 1. 环境检查
- 确保表空间有足够空间（至少预留1.5倍预期数据量）
- 检查UNDO表空间大小（建议至少500MB）
- 确认临时表空间足够处理排序操作

### 2. 性能调优建议
```sql
-- 临时增大SGA/PGA（仅限本次会话）
ALTER SYSTEM SET sort_area_size=200000000 SCOPE=MEMORY;
ALTER SYSTEM SET db_writer_processes=4 SCOPE=MEMORY;
```

## 二、数据插入方案

### 方案1：PL/SQL批量插入（推荐）

```sql
-- 1. 创建优化表结构
CREATE TABLE bulk_employees (
    emp_id NUMBER GENERATED ALWAYS AS IDENTITY,  -- 12c+自增列
    emp_name VARCHAR2(100) NOT NULL,
    hire_date DATE DEFAULT SYSDATE,
    salary NUMBER(10,2),
    dept_id NUMBER,
    CONSTRAINT pk_emp PRIMARY KEY (emp_id)
) NOLOGGING COMPRESS;  -- 禁用日志并启用压缩

-- 2. 高效插入脚本
DECLARE
    TYPE emp_tab IS TABLE OF bulk_employees%ROWTYPE;
    l_data emp_tab := emp_tab();
    l_batch_size NUMBER := 5000;  -- 最佳批次大小需测试确定
    l_start TIMESTAMP;
BEGIN
    l_start := SYSTIMESTAMP;
    
    -- 预分配内存
    l_data.EXTEND(1000000);
    
    -- 使用FORALL批量绑定
    FOR i IN 1..1000000 LOOP
        l_data(i).emp_name := 
            CASE MOD(i,10) 
                WHEN 0 THEN '张' WHEN 1 THEN '李' WHEN 2 THEN '王'
                WHEN 3 THEN '赵' WHEN 4 THEN '刘' WHEN 5 THEN '陈'
                WHEN 6 THEN '杨' WHEN 7 THEN '黄' WHEN 8 THEN '周'
                ELSE '吴' END || '员工'||i;
        l_data(i).hire_date := SYSDATE - MOD(i,3650);
        l_data(i).salary := 3000 + DBMS_RANDOM.VALUE(0, 27000);
        l_data(i).dept_id := MOD(i,20)+1;
        
        IF MOD(i, l_batch_size) = 0 THEN
            FORALL j IN (i-l_batch_size+1)..i
                INSERT /*+ APPEND */ INTO bulk_employees VALUES l_data(j);
            COMMIT;
        END IF;
    END LOOP;
    
    -- 提交剩余记录
    IF l_data.COUNT > 0 THEN
        FORALL j IN 1..l_data.COUNT
            INSERT /*+ APPEND */ INTO bulk_employees VALUES l_data(j);
        COMMIT;
    END IF;
    
    DBMS_OUTPUT.PUT_LINE('插入完成，耗时: ' || 
        EXTRACT(SECOND FROM (SYSTIMESTAMP - l_start)) || '秒');
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('错误: '||SQLERRM);
        ROLLBACK;
END;
/
```

### 方案2：外部表+SQL*Loader（超大数据量）

```sql
-- 1. 创建目录对象
CREATE OR REPLACE DIRECTORY data_dir AS '/path/to/datafile';

-- 2. 创建外部表
CREATE TABLE ext_employees (
    emp_name VARCHAR2(100),
    hire_date DATE,
    salary NUMBER
) ORGANIZATION EXTERNAL (
    TYPE ORACLE_LOADER
    DEFAULT DIRECTORY data_dir
    ACCESS PARAMETERS (
        RECORDS DELIMITED BY NEWLINE
        FIELDS TERMINATED BY ','
        MISSING FIELD VALUES ARE NULL
    )
    LOCATION ('employees.dat')
) REJECT LIMIT UNLIMITED;

-- 3. 使用并行插入
INSERT /*+ APPEND PARALLEL(4) */ INTO bulk_employees
SELECT * FROM ext_employees;
COMMIT;
```

## 三、性能对比测试

| 方法              | 100万条耗时 | 特点               |
| ----------------- | ----------- | ------------------ |
| 单条INSERT        | 25-40分钟   | 简单但极慢         |
| 批量FORALL        | 45-90秒     | 最佳平衡方案       |
| 外部表+SQL*Loader | 30-60秒     | 需要预处理数据文件 |
| 直接路径插入      | 35-75秒     | 需要NOLOGGING权限  |

## 四、高级优化技巧

### 1. 并行处理
```sql
ALTER SESSION FORCE PARALLEL DML PARALLEL 8;
INSERT /*+ PARALLEL(8) */ INTO bulk_employees...;
```

### 2. 分区表策略
```sql
-- 按部门ID范围分区
CREATE TABLE part_employees (...) 
PARTITION BY RANGE(dept_id) (
    PARTITION p1 VALUES LESS THAN (5),
    PARTITION p2 VALUES LESS THAN (10),
    PARTITION p3 VALUES LESS THAN (15),
    PARTITION p4 VALUES LESS THAN (MAXVALUE)
);
```

### 3. 内存优化
```sql
-- 增加排序区大小
ALTER SESSION SET sort_area_size=256000000;
-- 使用PGA聚合
ALTER SESSION SET workarea_size_policy=MANUAL;
```

## 五、后期维护建议

1. **统计信息收集**
```sql
EXEC DBMS_STATS.GATHER_TABLE_STATS(
    ownname => USER, 
    tabname => 'BULK_EMPLOYEES',
    estimate_percent => 100,
    cascade => TRUE
);
```

2. **索引创建策略**
```sql
-- 数据插入完成后创建索引
CREATE INDEX idx_emp_name ON bulk_employees(emp_name) 
NOLOGGING PARALLEL 4;
ALTER INDEX idx_emp_name NOPARALLEL;
```

3. **空间回收**
```sql
ALTER TABLE bulk_employees ENABLE ROW MOVEMENT;
ALTER TABLE bulk_employees SHRINK SPACE COMPACT;
```

## 六、常见问题解决

**问题1**：ORA-01653 表空间不足
```sql
-- 解决方案：
ALTER TABLESPACE users ADD DATAFILE 
'/path/to/newfile.dbf' SIZE 2G AUTOEXTEND ON;
```

**问题2**：UNDO表空间不足
```sql
-- 解决方案：
ALTER SYSTEM SET undo_retention=1800 SCOPE=BOTH;
ALTER TABLESPACE undotbs1 ADD DATAFILE 
'/path/to/undofile.dbf' SIZE 1G;
```

**问题3**：性能突然下降
```sql
-- 检查等待事件
SELECT event, COUNT(*) 
FROM v$session_wait 
WHERE wait_class != 'Idle'
GROUP BY event;
```