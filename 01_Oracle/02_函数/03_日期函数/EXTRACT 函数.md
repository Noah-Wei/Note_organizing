# Oracle 中的 EXTRACT 函数详解

在 Oracle 数据库中，`EXTRACT` 函数是一种用于**从日期或时间间隔中提取特定部分（如年、月、日等）**的工具。它广泛应用于时间数据的分析与拆分，如按年份汇总、筛选某月数据等，是时间维度处理不可或缺的一部分。

------

## 一、函数语法

```sql
EXTRACT(field FROM {date_value | interval_value})
```

### 参数说明：

| 参数             | 说明                                          |
| ---------------- | --------------------------------------------- |
| `field`          | 要提取的时间字段，如 YEAR、MONTH、DAY 等      |
| `date_value`     | 日期或时间戳类型的值（如 DATE、TIMESTAMP）    |
| `interval_value` | 时间间隔类型的值（如 INTERVAL YEAR TO MONTH） |

------

## 二、功能说明

- `EXTRACT` 可用于提取时间字段中的年、月、日、小时、分钟、秒等信息。
- 对于 `INTERVAL` 类型，可提取其内部的具体时间段（如年数或月数）。
- 与 `TO_CHAR(date, 'YYYY')` 相比，`EXTRACT` 返回的是 `NUMBER` 类型，便于数值运算与比较。
- 该函数也适用于 `TIMESTAMP WITH TIME ZONE` 类型，可提取时区小时与分钟。

------

## 三、实用示例

### 示例 1：提取当前年份

```sql
SELECT EXTRACT(YEAR FROM SYSDATE) AS 当前年份 FROM dual;
```

**输出结果：**

```
当前年份
----------
      2025
```

------

### 示例 2：提取指定日期的月份

```sql
SELECT EXTRACT(MONTH FROM DATE '2025-06-16') AS 月份 FROM dual;
```

**结果说明：**

- 提取的是 `06`（6 月）

------

### 示例 3：提取时间戳中的秒数

```sql
SELECT EXTRACT(SECOND FROM SYSTIMESTAMP) AS 当前秒 FROM dual;
```

**返回结果：** 秒数可以带小数点，例如 `42.123456`

------

### 示例 4：从 INTERVAL 中提取年与月

```sql
SELECT 
  EXTRACT(YEAR FROM INTERVAL '3-7' YEAR TO MONTH) AS 年份,
  EXTRACT(MONTH FROM INTERVAL '3-7' YEAR TO MONTH) AS 月份
FROM dual;
```

**返回结果：**

```
年份  月份
----  ---
   3     7
```

------

### 示例 5：配合分组统计使用

```sql
SELECT 
  EXTRACT(YEAR FROM hire_date) AS 入职年份,
  COUNT(*) AS 员工人数
FROM employees
GROUP BY EXTRACT(YEAR FROM hire_date);
```

该写法用于按入职年份统计员工数量，常用于人事分析报表。

------

## 四、常见字段值列表

| 字段名          | 说明             |
| --------------- | ---------------- |
| YEAR            | 年               |
| MONTH           | 月               |
| DAY             | 日               |
| HOUR            | 小时             |
| MINUTE          | 分钟             |
| SECOND          | 秒（支持小数）   |
| TIMEZONE_HOUR   | 时区中的小时部分 |
| TIMEZONE_MINUTE | 时区中的分钟部分 |

------

## 五、注意事项

- `EXTRACT` 返回的是 `NUMBER` 类型，适合做数值运算、排序、分组等操作。
- 不支持直接提取星期几，如需提取可使用 `TO_CHAR(date, 'D')`。
- 如果对 NULL 日期调用 `EXTRACT`，返回值也为 NULL。
- 提取无效字段（如对 `DATE` 使用 `TIMEZONE_HOUR`）会导致错误。

------

## 六、结语

`EXTRACT` 是 Oracle 中处理时间字段的重要函数，它结构清晰、语义明确，特别适合在时间维度分析、报表开发中使用。相比于 `TO_CHAR` 函数的字符处理方式，`EXTRACT` 更适合做数值运算与逻辑判断，是编写高质量 SQL 的常用利器之一。