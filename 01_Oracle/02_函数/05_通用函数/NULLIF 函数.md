# Oracle 中的 NULLIF 函数详解

在 Oracle 数据库中，`NULLIF` 是一个非常实用的函数，用于比较两个表达式是否相等。如果相等，则返回 `NULL`；如果不相等，则返回第一个表达式的值。它常被用于避免除零错误、简化 `CASE` 表达式等场景。

------

## 一、函数语法

```sql
NULLIF(expr1, expr2)
```

- **expr1**：第一个表达式。
- **expr2**：第二个表达式。

**返回值：**

- 如果 `expr1 = expr2`，则返回 `NULL`；
- 否则返回 `expr1` 的值。

------

## 二、基本用法示例

### 示例 1：两个值相等时返回 NULL

```sql
SELECT NULLIF(100, 100) AS result FROM dual;
```

**输出：**

```
RESULT
------
(NULL)
```

说明：因为两个值相等，结果为 `NULL`。

------

### 示例 2：两个值不相等时返回第一个值

```sql
SELECT NULLIF(100, 50) AS result FROM dual;
```

**输出：**

```
RESULT
------
100
```

说明：因为两个值不相等，返回第一个值 `100`。

------

## 三、实用场景示例

### 场景 1：避免除以零错误

使用 `NULLIF` 可以优雅地避免除以零的问题。

```sql
SELECT salary / NULLIF(bonus, 0) AS ratio
FROM employees;
```

说明：

- 如果 `bonus = 0`，则 `NULLIF(bonus, 0)` 返回 `NULL`，从而整个除法表达式返回 `NULL`，不会报错。
- 如果 `bonus ≠ 0`，则正常计算除法。

------

### 场景 2：简化 CASE 表达式

很多时候，我们使用 `CASE WHEN` 比较两个值是否相等，其实可以用 `NULLIF` 简化逻辑。

**传统写法：**

```sql
SELECT CASE WHEN a = b THEN NULL ELSE a END AS result FROM your_table;
```

**使用 NULLIF 简化：**

```sql
SELECT NULLIF(a, b) AS result FROM your_table;
```

结果是等价的，`NULLIF` 更简洁、可读性更高。

------

## 四、注意事项

1. **数据类型要兼容**：两个参数的数据类型应当一致或可隐式转换。
2. **NULL 与 NULL 的比较**：`NULLIF(NULL, NULL)` 的结果是 `NULL`，因为 Oracle 中 `NULL = NULL` 是未知，但 `NULLIF` 会将两个 `NULL` 视为相等。
3. **与 DECODE、CASE 区别**：`NULLIF` 语法更短，但逻辑只能处理简单的相等比较。

------

## 五、总结

| 特性     | 说明                                                |
| -------- | --------------------------------------------------- |
| 功能     | 比较两个值是否相等，相等返回 NULL，不等返回第一个值 |
| 常见用途 | 避免除以零、简化条件判断                            |
| 等价表达 | `CASE WHEN expr1 = expr2 THEN NULL ELSE expr1 END`  |
| 返回类型 | 与 `expr1` 的数据类型相同                           |

`NULLIF` 虽然是一个简单的函数，但在写出简洁、高质量 SQL 查询时非常有价值，特别是在处理数据清洗、报表展示或防止运行时错误的场景中。

