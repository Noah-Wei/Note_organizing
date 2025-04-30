# Oracle数据库插入1百万条数据

## 完整SQL脚本

```sql
-- 1. 创建测试表
-- 使用CTAS(Create Table As Select)方式快速创建表结构
-- 这里创建一个简单的员工表，包含ID、姓名、入职日期和薪资字段
CREATE TABLE test_employees (
    emp_id NUMBER PRIMARY KEY,          -- 员工ID，主键
    emp_name VARCHAR2(100),            -- 员工姓名
    hire_date DATE,                    -- 入职日期
    salary NUMBER(10,2)                -- 薪资，保留两位小数
);
 
-- 2. 创建序列用于生成自增ID
CREATE SEQUENCE emp_seq
    START WITH 1
    INCREMENT BY 1
    NOCACHE
    NOCYCLE;
 
-- 3. 批量插入100万条数据
-- 使用PL/SQL匿名块进行批量插入，比单条INSERT效率高很多
DECLARE
    -- 定义变量
    v_start_time NUMBER;  -- 用于记录开始时间(计算执行时间)
    v_end_time NUMBER;    -- 用于记录结束时间
    v_count NUMBER := 0;  -- 计数器
    
    -- 定义批量提交的批次大小
    v_batch_size NUMBER := 10000;  -- 每10000条提交一次
    
    -- 定义姓名前缀数组(用于生成不同的姓名)
    TYPE name_array IS TABLE OF VARCHAR2(10) INDEX BY BINARY_INTEGER;
    v_names name_array;
BEGIN
    -- 记录开始时间(使用DBMS_UTILITY.GET_TIME获取百分之一秒精度的时间)
    v_start_time := DBMS_UTILITY.GET_TIME;
    DBMS_OUTPUT.PUT_LINE('开始插入数据...');
    
    -- 初始化姓名数组
    v_names(1) := '张';
    v_names(2) := '李';
    v_names(3) := '王';
    v_names(4) := '赵';
    v_names(5) := '刘';
    v_names(6) := '陈';
    v_names(7) := '杨';
    v_names(8) := '黄';
    v_names(9) := '周';
    v_names(10) := '吴';
    
    -- 使用FOR循环插入100万条数据
    FOR i IN 1..1000000 LOOP
        -- 使用序列获取ID
        -- 使用SYSDATE - MOD(i,365) 生成不同日期(过去1000年内的随机日期)
        -- 使用DBMS_RANDOM.VALUE生成随机薪资(3000-30000之间)
        INSERT INTO test_employees VALUES (
            emp_seq.NEXTVAL,  -- 自增ID
            v_names(MOD(i,10)+1) || '员工' || TO_CHAR(i),  -- 姓名格式: 张员工1, 李员工2,...
            SYSDATE - MOD(i,365),  -- 入职日期
            3000 + DBMS_RANDOM.VALUE(0, 27000)  -- 随机薪资
        );
        
        -- 每插入v_batch_size条记录就提交一次
        v_count := v_count + 1;
        IF MOD(v_count, v_batch_size) = 0 THEN
            COMMIT;  -- 提交事务
            DBMS_OUTPUT.PUT_LINE('已插入 ' || v_count || ' 条记录');
        END IF;
    END LOOP;
    
    -- 提交剩余未提交的记录
    COMMIT;
    
    -- 记录结束时间并计算总耗时
    v_end_time := DBMS_UTILITY.GET_TIME;
    DBMS_OUTPUT.PUT_LINE('插入完成，共插入100万条记录');
    DBMS_OUTPUT.PUT_LINE('总耗时: ' || (v_end_time - v_start_time)/100 || ' 秒');
    
    -- 可选: 创建索引(如果之前没有创建)
    -- 通常建议在数据插入后再创建索引，因为先创建索引会降低插入速度
    -- EXECUTE IMMEDIATE 'CREATE INDEX idx_emp_name ON test_employees(emp_name)';
    -- EXECUTE IMMEDIATE 'CREATE INDEX idx_hire_date ON test_employees(hire_date)';
EXCEPTION
    WHEN OTHERS THEN
        -- 发生错误时回滚并输出错误信息
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('发生错误: ' || SQLERRM);
        RAISE;
END;
/
```

## 在PL/SQL Developer 14中的操作步骤

1. **打开PL/SQL Developer 14**，连接到你的Oracle 11g数据库

2. **新建SQL窗口**：

   - 点击工具栏上的"新建"按钮
   - 选择"SQL窗口"或按快捷键Ctrl+N

3. **粘贴上述SQL脚本**：

   - 将上面的完整SQL脚本复制到SQL窗口中

4. **执行脚本**：

   - 按F8键或点击工具栏上的"执行"按钮(绿色三角形图标)
   - 注意：如果表已存在，需要先删除(DROP TABLE test_employees CASCADE CONSTRAINTS;)

5. **查看执行结果**：

   - 在"DBMS输出"窗口中查看输出信息
   - 如果没有看到DBMS输出窗口，可以通过菜单"视图"->"DBMS输出"打开

6. **验证数据**：

   - 执行查询验证数据是否插入成功：

     ```sql
     SELECT COUNT(*) FROM test_employees;
     SELECT * FROM test_employees WHERE ROWNUM <= 10;
     ```

## 性能优化建议

1. **批量提交**：脚本中已经使用了每10000条提交一次的批量提交方式，这比单条提交效率高很多。

2. **禁用日志**：如果需要更高的插入速度，可以考虑：

   ```sql
   ALTER TABLE test_employees NOLOGGING;
   -- 插入完成后恢复
   ALTER TABLE test_employees LOGGING;
   ```

3. **并行插入**：对于更大的数据量，可以考虑使用并行插入：

   ```sql
   ALTER SESSION ENABLE PARALLEL DML;
   -- 然后在INSERT语句中添加/*+ APPEND PARALLEL(4) */提示
   ```

4. **使用外部表**：对于超大数据量，可以考虑使用外部表方式加载。