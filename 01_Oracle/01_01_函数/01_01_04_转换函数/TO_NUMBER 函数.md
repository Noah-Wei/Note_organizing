# Oracle 中的 TO_NUMBER 函数详解

TO_NUMBER 是 Oracle 数据库中用于**将字符串转换为数字**的函数，它是数据处理和类型转换中非常重要的工具。

## 基本语法

```sql
TO_NUMBER(string, [format_model], [nls_parameters])
```

## 参数说明

| 参数 | 说明 |
|------|------|
| `string` | 要转换的字符串表达式 |
| `format_model` (可选) | 指定字符串格式的模型 |
| `nls_parameters` (可选) | NLS 参数，如数字格式设置 |

## 函数特点

1. **严格转换**：字符串必须符合数字格式要求
2. **格式控制**：可指定格式模型处理特殊格式的数字字符串
3. **NLS 支持**：能适应不同语言环境的数字格式
4. **错误处理**：格式不匹配会引发 ORA-01722 错误

## 使用示例

### 1. 基本转换

```sql
-- 简单字符串转数字
SELECT TO_NUMBER('1234.56') FROM dual;  -- 结果: 1234.56

-- 带格式的字符串转数字
SELECT TO_NUMBER('$1,234.56', '$9,999.99') FROM dual;  -- 结果: 1234.56
```

### 2. 处理不同数字格式

```sql
-- 处理科学计数法
SELECT TO_NUMBER('1.234E+03', '9.999EEEE') FROM dual;  -- 结果: 1234

-- 处理百分数
SELECT TO_NUMBER('45.67%', '99.99%')/100 FROM dual;  -- 结果: 0.4567

-- 处理带千位分隔符的数字
SELECT TO_NUMBER('1,234,567.89', '9,999,999.99') FROM dual;
```

### 3. 与 NLS 参数结合

```sql
-- 欧洲格式数字转换（逗号作为小数点）
SELECT TO_NUMBER('1.234,56', '9G999D99', 'NLS_NUMERIC_CHARACTERS='',.''') 
FROM dual;  -- 结果: 1234.56

-- 不同货币符号处理
SELECT TO_NUMBER('¥1,234.56', 'L9,999.99', 'NLS_CURRENCY=''¥''') 
FROM dual;
```

### 4. 错误处理示例

```sql
-- 格式不匹配会报错
SELECT TO_NUMBER('$1,234.56', '9999.99') FROM dual;
-- 报错: ORA-01722: 无效数字

-- 使用DEFAULT ON CONVERSION ERROR（12c及以上版本）
SELECT TO_NUMBER('ABC' DEFAULT 0 ON CONVERSION ERROR) FROM dual;
-- 结果: 0
```

## 实际应用场景

### 1. 数据清洗

```sql
-- 清理包含货币符号的数据
UPDATE financial_data
SET amount = TO_NUMBER(
  REGEXP_REPLACE(amount_str, '[^0-9.]', ''), 
  '999999999.99')
WHERE amount_str IS NOT NULL;
```

### 2. 动态SQL处理

```sql
-- 安全地将字符串参数转换为数字
CREATE PROCEDURE process_order(p_order_id VARCHAR2) AS
  v_num_id NUMBER;
BEGIN
  v_num_id := TO_NUMBER(p_order_id);
  -- 后续处理...
EXCEPTION
  WHEN VALUE_ERROR THEN
    DBMS_OUTPUT.PUT_LINE('无效的订单ID格式');
END;
```

### 3. 报表数据转换

```sql
-- 将格式化字符串转为数字计算
SELECT report_month,
       TO_NUMBER(REPLACE(revenue_str, ',', ''), '9999999.99') AS revenue
FROM monthly_reports;
```

### 4. 接口数据处理

```sql
-- 处理外部系统导入的数值数据
INSERT INTO transaction_log
SELECT TO_NUMBER(REGEXP_SUBSTR(log_entry, '\d+\.\d{2}')) AS amount
FROM external_data;
```

## 特殊注意事项

1. **隐式转换**：Oracle 在某些情况下会自动进行字符串到数字的转换，但显式使用 TO_NUMBER 更安全可靠
2. **性能考虑**：大量数据转换时，TO_NUMBER 可能成为性能瓶颈
3. **NULL 处理**：输入为 NULL 时返回 NULL
4. **格式匹配**：格式模型必须与输入字符串完全匹配
5. **替代方案**：对于简单的整数转换，CAST 函数也是一种选择
   ```sql
   SELECT CAST('1234' AS NUMBER) FROM dual;
   ```

## 与相关函数对比

| 函数 | 用途 | 示例 |
|------|------|------|
| TO_NUMBER | 字符串转数字 | `TO_NUMBER('123.45')` |
| TO_CHAR | 数字转字符串 | `TO_CHAR(123.45, '999.99')` |
| CAST | 类型转换 | `CAST('123.45' AS NUMBER)` |
| EXTRACT | 从日期提取数字 | `EXTRACT(YEAR FROM SYSDATE)` |

TO_NUMBER 是 Oracle 数据转换中的基础函数，合理使用可以确保数据类型的正确性和计算的准确性，特别是在处理外部数据源或用户输入时尤为重要。