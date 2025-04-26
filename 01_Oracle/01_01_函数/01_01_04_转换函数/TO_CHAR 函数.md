# Oracle 中的 TO_CHAR 函数详解

TO_CHAR 是 Oracle 数据库中最重要的数据类型转换函数之一，主要用于**将日期或数字转换为指定格式的字符串**。

## 基本语法

### 日期转字符串
```sql
TO_CHAR(date, [format_model], [nls_parameters])
```

### 数字转字符串
```sql
TO_CHAR(number, [format_model], [nls_parameters])
```

## 参数说明

| 参数 | 说明 |
|------|------|
| `date/number` | 要转换的日期或数字 |
| `format_model` (可选) | 指定输出格式的格式模型 |
| `nls_parameters` (可选) | NLS 参数，如语言设置 |

## 日期格式化示例

### 常用日期格式元素

| 格式元素 | 说明 | 示例 |
|---------|------|------|
| YYYY | 4位年份 | 2023 |
| YEAR | 年份英文拼写 | TWENTY TWENTY-THREE |
| MM | 月份数字(01-12) | 07 |
| MON | 月份缩写 | JUL |
| MONTH | 月份全名 | JULY |
| DD | 日(01-31) | 25 |
| DY | 星期缩写 | TUE |
| DAY | 星期全名 | TUESDAY |
| D | 星期数字(1-7) | 7 |
| Q | 季度(1-4) | 2 |
| HH24 | 24小时制(00-23) | 14 |
| MI | 分钟(00-59) | 30 |
| SS | 秒(00-59) | 45 |
| AM/PM | 上午/下午 | PM |

### 实际应用示例

```sql
-- 基本日期格式化
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD') FROM dual;  -- 2023-07-25
SELECT TO_CHAR(SYSDATE, 'DD/MM/YYYY HH24:MI:SS') FROM dual; -- 25/07/2023 14:30:45

-- 包含文本的格式化
SELECT TO_CHAR(SYSDATE, '"Today is" DAY, MONTH DD, YYYY') FROM dual;
-- Today is TUESDAY, JULY 25, 2023

-- 季度和周年格式化
SELECT TO_CHAR(SYSDATE, 'Q"季度" YYYY') FROM dual; -- 3季度 2023
SELECT TO_CHAR(SYSDATE, 'WW"周"') FROM dual; -- 30周

-- 中文环境格式化
SELECT TO_CHAR(SYSDATE, 'YYYY"年"MM"月"DD"日" DAY') FROM dual;
-- 2023年07月25日 星期二

-- 特殊fm：去除前导0
SELECT to_char(SYSDATE,'fmyyyy-mm-dd') FROM dual;
-- 2025-4-26  而不是2025-04-26
```

## 数字格式化示例

### 常用数字格式元素

| 格式元素 | 说明 | 示例 |
|---------|------|------|
| 9 | 数字位 | 9999 |
| 0 | 前导零 | 0999 |
| . | 小数点 | 99.99 |
| , | 千位分隔符 | 9,999 |
| $ | 美元符号 | $9999 |
| L | 本地货币符号 | L9999 |
| MI | 负号在右侧 | 9999MI |
| PR | 负数用尖括号 | 9999PR |
| EEEE | 科学计数法 | 9.9EEEE |

### 实际应用示例

> 位数不足会显示#

```sql
-- 基本数字格式化
SELECT TO_CHAR(1234.56, '9999.99') FROM dual;  -- 1234.56
SELECT TO_CHAR(1234.56, '9,999.99') FROM dual; -- 1,234.56

-- 货币格式化
SELECT TO_CHAR(1234.56, 'L999G999D99') FROM dual; -- $1,234.56
SELECT TO_CHAR(1234.56, 'C999G999D99', 'NLS_CURRENCY=¥') FROM dual; -- ¥1,234.56

-- 特殊格式
SELECT TO_CHAR(-1234, '9999PR') FROM dual; -- <1234>
SELECT TO_CHAR(0.45, '99.99%') FROM dual; -- 45.00%

-- 科学计数法
SELECT TO_CHAR(123456789, '9.999EEEE') FROM dual; -- 1.235E+08

-- 位数不足时
SELECT To_char(deptno,'0') FROM emp;
-- ##
```

## 高级用法

### 1. 多语言支持
```sql
-- 更改会话语言设置
ALTER SESSION SET NLS_DATE_LANGUAGE = 'FRENCH';
SELECT TO_CHAR(SYSDATE, 'DAY MONTH DD, YYYY') FROM dual;
-- MARDI JUILLET 25, 2023

ALTER SESSION SET NLS_DATE_LANGUAGE = 'GERMAN';
SELECT TO_CHAR(SYSDATE, 'DAY MONTH DD, YYYY') FROM dual;
-- DIENSTAG JULI 25, 2023
```

### 2. 条件格式化
```sql
SELECT product_name,
       TO_CHAR(price, '999G999D00') || 
       CASE WHEN price > 1000 THEN ' (高价)' ELSE ' (普通)' END AS price_info
FROM products;
```

### 3. 与 DECODE/CASE 结合
```sql
SELECT order_id,
       TO_CHAR(order_date, 'YYYY-MM-DD') AS order_date,
       TO_CHAR(amount, 'L999G999D99', 'NLS_CURRENCY=¥') AS amount,
       DECODE(status, 'C', '已完成', 'P', '处理中', '其他') AS status_desc
FROM orders;
```

## 注意事项

1. **格式匹配**：格式模型必须与输入数据类型匹配（日期/数字）
2. **性能考虑**：大量数据转换时可能影响性能
3. **NLS 依赖**：某些格式元素依赖 NLS 参数设置
4. **NULL 处理**：输入为 NULL 时返回 NULL
5. **溢出处理**：数字格式过小会导致 `###` 显示

T O_CHAR 是 Oracle 数据展示和报表生成中最核心的函数之一，熟练掌握各种格式模型可以极大提高数据展示的灵活性。