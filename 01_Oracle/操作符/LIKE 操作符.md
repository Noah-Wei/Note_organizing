# Oracle 中的 LIKE 操作符详解

LIKE 是 Oracle SQL 中用于**字符串模式匹配**的操作符，它允许使用通配符进行灵活的字符串查询。

## 基本语法

```sql
WHERE column_name LIKE 'pattern'
WHERE column_name NOT LIKE 'pattern'
```

## 通配符说明

| 通配符 | 描述 | 示例 | 匹配示例 |
|--------|------|------|---------|
| `%` | 匹配任意数量字符（包括0个） | `'S%'` | 'Smith', 'S', 'Sales' |
| `_` | 匹配单个字符 | `'_A%'` | 'BAKER', 'Carter' |
| ESCAPE | 转义特殊字符 | `'50\%' ESCAPE '\'` | '50%' |

## 使用示例

### 1. 基本匹配

```sql
-- 查找以'S'开头的姓氏
SELECT * FROM employees 
WHERE last_name LIKE 'S%';

-- 查找包含'ing'的名字
SELECT * FROM employees
WHERE first_name LIKE '%ing%';
```

### 2. 单字符匹配

```sql
-- 查找第二个字母是'a'的姓氏
SELECT * FROM employees
WHERE last_name LIKE '_a%';

-- 查找正好5个字符的姓氏
SELECT * FROM employees
WHERE last_name LIKE '_____';
```

### 3. 混合使用通配符

```sql
-- 查找以'S'开头且至少3个字符的姓氏
SELECT * FROM employees
WHERE last_name LIKE 'S__%';

-- 查找第三个字符是'm'的姓氏
SELECT * FROM employees
WHERE last_name LIKE '__m%';
```

### 4. 转义特殊字符

```sql
-- 查找包含'_'的备注
SELECT * FROM product_descriptions
WHERE notes LIKE '%\_%' ESCAPE '\';

-- 查找以'50%'开头的折扣码
SELECT * FROM promotions
WHERE discount_code LIKE '50\%%' ESCAPE '\';
```

### 5. NOT LIKE 用法

```sql
-- 查找不以'Test'开头的数据
SELECT * FROM system_logs
WHERE message NOT LIKE 'Test%';
```

## 性能优化建议

1. **避免前导通配符**：`LIKE '%abc'` 无法使用索引
2. **函数索引**：对频繁使用 LIKE 的列创建函数索引
   ```sql
   CREATE INDEX emp_last_name_idx ON employees(UPPER(last_name));
   -- 查询时
   WHERE UPPER(last_name) LIKE 'SM%'
   ```
3. **使用绑定变量**：减少硬解析
   ```sql
   WHERE last_name LIKE :pattern || '%'
   ```

## 与正则表达式比较

对于复杂模式，可考虑使用 REGEXP_LIKE：

```sql
-- 使用LIKE
WHERE phone LIKE '02%' OR phone LIKE '03%'

-- 使用REGEXP_LIKE
WHERE REGEXP_LIKE(phone, '^0[23]')
```

## 实际应用场景

### 1. 数据搜索

```sql
-- 产品名称模糊搜索
SELECT * FROM products
WHERE product_name LIKE '%' || :user_input || '%';
```

### 2. 数据清洗

```sql
-- 查找不符合格式的数据
SELECT * FROM customers
WHERE phone NOT LIKE '05_________';
```

### 3. 日志分析

```sql
-- 查找错误日志
SELECT * FROM system_logs
WHERE log_message LIKE '%Error%' 
   OR log_message LIKE '%Exception%';
```

### 4. 模式验证

```sql
-- 验证邮箱格式
SELECT * FROM users
WHERE email LIKE '%@%.%'
  AND email NOT LIKE '% %'
  AND email NOT LIKE '@%'
  AND email NOT LIKE '%@';
```

## 注意事项

1. **大小写敏感**：LIKE 默认大小写敏感，取决于 NLS 参数
   ```sql
   -- 不区分大小写的查询
   WHERE UPPER(column_name) LIKE UPPER('%abc%')
   ```
2. **NULL 值**：`column_name LIKE pattern` 不会匹配 NULL 值
3. **性能影响**：大量使用 LIKE 可能影响查询性能

LIKE 操作符是 Oracle SQL 中处理字符串匹配的基础工具，合理使用可以满足大多数模糊查询需求。