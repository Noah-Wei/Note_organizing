# Oracle 中的 `CASE WHEN` 表达式详解

在 Oracle SQL 中，`CASE` 表达式用于基于某些条件返回不同的值，是实现条件判断的常用工具。它广泛应用于 `SELECT`、`UPDATE`、`ORDER BY` 等语句中，使 SQL 语句更具灵活性和可读性。

------

## 一、`CASE` 表达式的基本语法

Oracle 支持两种形式的 `CASE` 表达式：

### 1. 简单 CASE 表达式（Simple CASE）

用于对某个表达式进行等值判断：

```sql
CASE expression
  WHEN value1 THEN result1
  WHEN value2 THEN result2
  ...
  ELSE default_result
END
```

### 2. 搜索型 CASE 表达式（Searched CASE）

用于执行复杂的逻辑判断（更常用）：

```sql
CASE
  WHEN condition1 THEN result1
  WHEN condition2 THEN result2
  ...
  ELSE default_result
END
```

------

## 二、使用示例

### 示例 1：员工性别判断（使用 `MOD` 判断奇偶）

```sql
SELECT empno,
       ename,
       CASE
         WHEN MOD(empno, 2) = 1 THEN '男'
         ELSE '女'
       END AS gender_guess
FROM emp;
```

说明：假设 `empno` 为奇数表示“男”，偶数表示“女”。

------

### 示例 2：工资等级分类

```sql
SELECT ename,
       sal,
       CASE
         WHEN sal >= 5000 THEN '高薪'
         WHEN sal >= 3000 THEN '中薪'
         ELSE '低薪'
       END AS sal_level
FROM emp;
```

------

### 示例 3：判断入职月份属于上半年还是下半年

```sql
SELECT ename,
       hiredate,
       CASE
         WHEN TO_CHAR(hiredate, 'MM') BETWEEN '01' AND '06' THEN '上半年'
         WHEN TO_CHAR(hiredate, 'MM') BETWEEN '07' AND '12' THEN '下半年'
         ELSE '未知'
       END AS 入职时期
FROM emp;
```

------

### 示例 4：`ORDER BY` 中使用 `CASE`

```sql
SELECT ename, job
FROM emp
ORDER BY CASE job
            WHEN 'MANAGER' THEN 1
            WHEN 'ANALYST' THEN 2
            WHEN 'CLERK' THEN 3
            ELSE 4
         END;
```

说明：自定义职位排序规则。

------

## 三、`CASE` 与 `DECODE` 对比

| 特性         | CASE                   | DECODE                 |
| ------------ | ---------------------- | ---------------------- |
| 判断类型     | 支持任意布尔条件       | 只能做等值判断         |
| 表达方式     | 类似 IF...THEN...ELSE  | 更像函数调用           |
| 可读性       | 较好                   | 较差，复杂时不易维护   |
| 推荐使用场景 | 任意条件判断（更通用） | 简单等值替换（更简洁） |

------

## 四、注意事项

1. `CASE` 表达式**不能跨多个列直接判断**，需要显式表达每个条件。
2. `CASE` 表达式最后最好加 `ELSE`，防止无匹配时返回 `NULL`。
3. `CASE` 表达式可以用于几乎所有 SQL 场合（如 `SELECT`、`UPDATE`、`WHERE`、`ORDER BY` 等）。

------

## 五、总结

`CASE WHEN` 表达式是 SQL 中非常实用的条件判断工具，能有效提升 SQL 的逻辑能力与灵活性。对于复杂的业务规则判断、分级、标签分类等场景，`CASE` 是首选方式。

