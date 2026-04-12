# Oracle相关命令

### SQL总体类别

- 语言分类

  ```
  1、数据查询语言：DQL：SELECT
  2、数据操作语言：DML：INSERT、UPDATE、DELETE、MERGE
  3、数据定义语言：DDL：CREATE、ALTER、DROP、TRUNCATE、RENAME
  4、数据控制语言：DCL：GRANT、REVOKE、SET ROLE
  5、事务控制语言：TCL：COMMIT、ROLLBACK、SAVEPOINY
  ```

- 检查当前数据库时区：

  ```sql
   SELECT DBTIMEZONE FROM dual;
  ```

- 查看服务名（实例/数据库名）

  ```sql
  SELECT value FROM v$parameter WHERE name = 'service_names';
  ```

### 查询

- 查看当前用户下的所有表

  ```sql
  select * from user_tables;
  ```

- 查询当前用户下的所有视图名

  ```sql
  SELECT VIEW_NAME FROM USER_VIEWS;
  ```

- 查询当前用户下的所有序列

  ```sql
  SELECT SEQUENCE_NAME FROM USER_SEQUENCES;
  ```

  

### 约束

- 查看表的字段约束信息

  ```sql
  SELECT * FROM user_cons_columns WHERE table_name = '表名大写';
  ```

- 添加主键约束（PRIMARY KEY）

  ```sql
  ALTER TABLE 表名 
  ADD CONSTRAINT 约束名 PRIMARY KEY (字段名);
  ```

- 添加唯一约束 (UNIQUE)

  ```sql
  ALTER TABLE 表名 
  ADD CONSTRAINT 约束名 UNIQUE (字段名);
  ```

- 添加非空约束 (NOT NULL)

  ```sql
  ALTER TABLE 表名 
  MODIFY (字段名 NOT NULL);
  ```

- 添加检查约束 (CHECK)

  ```sql
  ALTER TABLE 表名 
  ADD CONSTRAINT 约束名 CHECK (条件表达式);
  ```

- 添加默认值约束 (DEFAULT)

  ```
  ALTER TABLE 表名 
  MODIFY (字段名 DEFAULT 默认值);
  ```

- 添加多字段组合约束

  ```sql
  ALTER TABLE 表名 
  ADD CONSTRAINT 约束名 PRIMARY KEY (字段1, 字段2);
  -- 或
  ALTER TABLE 表名 
  ADD CONSTRAINT 约束名 UNIQUE (字段1, 字段2);
  -- 或
  ALTER TABLE 表名 
  ADD CONSTRAINT 约束名 CHECK (字段1 > 0 AND 字段2 IS NOT NULL);
  ```


### 集合操作

- 并集（UNION）

  ```sql
  -- 并集（UNION），自动去重
  SELECT 列1, 列2 FROM 表1
  UNION
  SELECT 列1, 列2 FROM 表2;
  
  -- 不去重版本：使用 UNION ALL（保留所有重复行，性能更高）：
  SELECT 列1, 列2 FROM 表1
  UNION ALL
  SELECT 列1, 列2 FROM 表2;
  ```

- 交集（INTERSECT）

  ```sql
  -- 返回两个查询结果集的交集部分
  SELECT 列1, 列2 FROM 表1
  INTERSECT
  SELECT 列1, 列2 FROM 表2;
  ```

- 差集（MINUS）

  ```sql
  -- 返回第一个查询结果集减去第二个查询结果集后的剩余行（即存在于第一个结果集但不存在于第二个结果集的行），自动去重
  SELECT 列1, 列2 FROM 表1
  MINUS
  SELECT 列1, 列2 FROM 表2;
  ```


### 视图

- 删除视图

  ```sql
  DROP VIEW [schema.]view_name [CASCADE CONSTRAINTS];
  -- schema：视图所属的用户（模式），如果省略则默认为当前用户。
  -- view_name：要删除的视图名称。
  -- CASCADE CONSTRAINTS（可选）：如果视图被其他对象（如其他视图或约束）引用，强制删除并级联清除相关依赖。
  ```


### MERGE INTO

- MERGE INTO

  ```sql
  MERGE INTO 目标表
  USING 原表
  ON (条件 t.key_column = s.key_column)
  WHEN MATCHED THEN
      UPDATE SET 
          t.column1 = s.column1,
          t.column2 = s.column2,
          ...
  WHEN NOT MATCHED THEN
      INSERT (column1, column2, ...)
      VALUES (s.column1, s.column2, ...);
  ```

### 序列

- 创建序列

  ```sql
  CREATE SEQUENCE sequence_name
    [START WITH start_number]
    [INCREMENT BY increment_value]
    [MAXVALUE max_value | NOMAXVALUE]
    [MINVALUE min_value | NOMINVALUE]
    [CYCLE | NOCYCLE]
    [CACHE cache_size | NOCACHE]
    [ORDER | NOORDER];
    
  /*
  START WITH：序列起始值（默认1）
  INCREMENT BY：步长，可为负数实现递减序列
  MAXVALUE/MINVALUE：序列最大/最小值限制
  CYCLE：达到极值后是否循环（需谨慎使用）
  CACHE：预生成数值缓存到内存（提升性能，但断电或回滚可能导致数值间隙）
  ORDER：保证并发场景下按请求顺序生成（影响性能，默认NOORDER）
  */
  ```

- 查询当前用户下的所有序列

  ```sql
  SELECT SEQUENCE_NAME FROM USER_SEQUENCES;
  ```

- 修改序列

  ```sql
  -- 修改步长
  ALTER SEQUENCE seq_order_id INCREMENT BY 5;  
  ```

- 删除

  ```sql
  -- 删除序列
  DROP SEQUENCE MY_SEQ;
  ```

### 其他

求最后N行的数据

```sql
借助ROWNUM
先查出所有的数据
然后差集上，总行-前N行的数据
SELECT ROWNUM,表.* FROM 表
MINUS
SELECT * FROM 表 WHERE ROWNUM <= (SELECT COUNT(1)-N FROM 表);
```

解锁数据库的练习账户

```
alter user scott account unlock	
```

修改密码

```
alter user 用户名 identified by 密码
```

