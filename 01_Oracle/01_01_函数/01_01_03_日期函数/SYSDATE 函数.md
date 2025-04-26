# Oracle 中的 SYSDATE 函数详解

SYSDATE 是 Oracle 数据库中最重要的日期函数之一，它返回数据库服务器当前的**系统日期和时间**。

> **日期运算规则**
>
> - 日期 + 数字 = 日期
> - 日期 - 数字 = 日期
> - 日期 - 日期 = 数字（天数）

## 基本语法

```sql
SYSDATE
```

## 函数特点

1. **返回值**：返回当前数据库服务器的日期和时间（DATE 类型）
2. **时区**：基于数据库服务器的操作系统时区
3. **精度**：精确到秒
4. **会话无关**：所有会话获取的 SYSDATE 值相同（与 CURRENT_DATE 不同）

## 基本用法

### 1. 获取当前日期时间

```sql
SELECT SYSDATE FROM dual;
-- 结果示例: 25-JUL-2023 14:30:45
```

### 2. 格式化显示

```sql
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS') FROM dual;
-- 结果示例: 2023-07-25 14:30:45
```

### 3. 在 DML 操作中使用

```sql
INSERT INTO orders (order_id, order_date) 
VALUES (1001, SYSDATE);

UPDATE employees 
SET last_login = SYSDATE
WHERE employee_id = 123;
```

## 日期运算

### 1. 加减天数

```sql
-- 明天此时
SELECT SYSDATE + 1 FROM dual;

-- 昨天此时
SELECT SYSDATE - 1 FROM dual;

-- 1小时前
SELECT SYSDATE - 1/24 FROM dual;

-- 30分钟后
SELECT SYSDATE + 30/1440 FROM dual;
```

### 2. 计算日期差

```sql
-- 计算两个日期之间的天数
SELECT SYSDATE - TO_DATE('2023-01-01', 'YYYY-MM-DD') FROM dual;

-- 计算小时差
SELECT (SYSDATE - start_time) * 24 AS hours_elapsed
FROM processes;
```

## 常用日期截断

### 1. 获取当天开始时间

```sql
SELECT TRUNC(SYSDATE) FROM dual;
-- 结果: 25-JUL-2023 00:00:00
```

### 2. 获取当月第一天

```sql
SELECT TRUNC(SYSDATE, 'MM') FROM dual;
-- 结果: 01-JUL-2023 00:00:00
```

### 3. 获取当年第一天

```sql
SELECT TRUNC(SYSDATE, 'YEAR') FROM dual;
-- 结果: 01-JAN-2023 00:00:00
```

## 与时间相关的系统函数对比

| 函数 | 描述 | 时区依据 | 返回类型 |
|------|------|---------|---------|
| SYSDATE | 数据库服务器时间 | 服务器时区 | DATE |
| CURRENT_DATE | 会话当前日期 | 会话时区 | DATE |
| SYSTIMESTAMP | 数据库服务器时间戳 | 服务器时区 | TIMESTAMP WITH TIME ZONE |
| CURRENT_TIMESTAMP | 会话当前时间戳 | 会话时区 | TIMESTAMP WITH TIME ZONE |
| LOCALTIMESTAMP | 会话当前时间戳 | 会话时区 | TIMESTAMP |

## 实际应用场景

### 1. 记录操作时间

```sql
-- 创建表时自动记录创建时间
CREATE TABLE audit_log (
  log_id      NUMBER,
  action      VARCHAR2(100),
  create_time DATE DEFAULT SYSDATE
);
```

### 2. 时间条件查询

```sql
-- 查询24小时内的订单
SELECT * FROM orders
WHERE order_date >= SYSDATE - 1;

-- 查询当月数据
SELECT * FROM sales
WHERE sale_date BETWEEN TRUNC(SYSDATE, 'MM') 
                    AND LAST_DAY(SYSDATE);
```

### 3. 计算工作日

```sql
-- 计算5个工作日后的日期
SELECT NEXT_DAY(SYSDATE + 5, 'MONDAY') FROM dual;
```

### 4. 生成时间序列

```sql
-- 生成未来7天的日期序列
SELECT TRUNC(SYSDATE) + LEVEL - 1 AS date_series
FROM dual
CONNECT BY LEVEL <= 7;
```

## 注意事项

1. **性能考虑**：在大量数据上使用 SYSDATE 可能影响性能
2. **索引使用**：WHERE 子句中对日期列使用 SYSDATE 可能阻止索引使用
3. **时区问题**：分布式系统中要注意服务器时区差异
4. **与 CURRENT_DATE 区别**：
   ```sql
   ALTER SESSION SET TIME_ZONE = 'America/New_York';
   SELECT SYSDATE, CURRENT_DATE FROM dual;
   -- SYSDATE显示服务器时间，CURRENT_DATE显示会话时区时间
   ```

SYSDATE 是 Oracle 日期处理的核心函数，熟练掌握其用法对于开发时间相关的应用至关重要。