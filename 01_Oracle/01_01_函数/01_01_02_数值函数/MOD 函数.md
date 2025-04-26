# Oracle 中的 MOD 函数详解

MOD 函数是 Oracle 数据库中用于**计算两数相除的余数**（模运算）的函数，它在数学计算、数据分组和周期处理等场景中非常有用。

## 基本语法

```sql
MOD(dividend, divisor)
```

## 参数说明

| 参数       | 说明           |
| ---------- | -------------- |
| `dividend` | 被除数（分子） |
| `divisor`  | 除数（分母）   |

## 函数特点

1. **返回值**：返回 dividend 除以 divisor 的余数
2. **符号规则**：结果的符号与被除数(dividend)相同
3. **零处理**：如果除数为0，Oracle 返回被除数
4. **NULL 处理**：任一参数为NULL则返回NULL
5. **数据类型**：支持所有数值类型(NUMBER, BINARY_FLOAT, BINARY_DOUBLE等)

## 使用示例

### 1. 基本取模运算

```sql
SELECT MOD(10, 3) FROM dual;    -- 结果: 1 (10÷3余1)
SELECT MOD(-10, 3) FROM dual;   -- 结果: -1 (符号与被除数一致)
SELECT MOD(10, -3) FROM dual;   -- 结果: 1 
```

### 2. 奇偶判断

```sql
SELECT employee_id, 
       CASE MOD(employee_id, 2) 
         WHEN 0 THEN '偶数' 
         ELSE '奇数' 
       END AS 奇偶性
FROM employees;
```

### 3. 周期分组

```sql
-- 将数据分成5个一组
SELECT product_id,
       MOD(product_id, 5) AS group_id
FROM products;
```

### 4. 时间周期计算

```sql
-- 计算今天是周几(0=周日,1=周一,...,6=周六)
SELECT MOD(TO_CHAR(SYSDATE, 'D')-1, 7) AS weekday_num FROM dual;
```

### 5. 与表列结合使用

```sql
SELECT order_id, 
       order_amount,
       MOD(order_amount, 100) AS remainder
FROM orders;
```

## 特殊注意事项

1. **除数为0**：
   ```sql
   SELECT MOD(10, 0) FROM dual;  -- 结果: 10 (Oracle特有处理)
   ```

2. **小数运算**：
   ```sql
   SELECT MOD(10.5, 3) FROM dual;  -- 结果: 1.5
   ```

3. **与 REMAINDER 函数区别**：
   ```sql
   SELECT MOD(10, 3), REMAINDER(10, 3) FROM dual;  -- 结果: 1 | 1
   SELECT MOD(-10, 3), REMAINDER(-10, 3) FROM dual; -- 结果: -1 | -1
   SELECT MOD(10, -3), REMAINDER(10, -3) FROM dual; -- 结果: 1 | -2
   ```

## 实际应用场景

1. **数据分片**：
   ```sql
   -- 将数据均匀分布到4个分区
   SELECT *
   FROM large_table
   WHERE MOD(object_id, 4) = :partition_num;
   ```

2. **轮询分配**：
   ```sql
   UPDATE tasks
   SET assigned_to = MOD(task_id, :team_size) + 1;
   ```

3. **循环缓冲区**：
   ```sql
   INSERT INTO circular_buffer
   VALUES (MOD(:current_index + 1, :buffer_size), :data);
   ```

4. **校验位计算**：
   ```sql
   SELECT id, 
          MOD(id, 10) AS check_digit
   FROM transaction_log;
   ```

5. **时间窗口计算**：
   ```sql
   SELECT event_time,
          MOD(EXTRACT(HOUR FROM event_time), 4) AS time_window
   FROM system_events;
   ```

## 性能提示

1. 对于大数运算，MOD 比手动计算(dividend - divisor*FLOOR(dividend/divisor))更高效
2. 在WHERE子句中使用MOD可能影响索引使用
3. 对于固定除数的模运算，Oracle会优化计算

MOD函数是处理周期性、分组性和循环性数据的强大工具，在开发中合理使用可以简化许多复杂逻辑。