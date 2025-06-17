# Oracle 中的 `SUM()` 与 `COUNT()` 函数详解

在 Oracle SQL 中，`SUM()` 和 `COUNT()` 是最常见的聚合函数（Aggregate Functions）之一，广泛应用于数据汇总、报表统计和业务分析等场景。虽然这两个函数经常一起使用，但它们的作用、用法和注意事项却存在明显差异。

本文将系统介绍 `SUM()` 与 `COUNT()` 的语法、功能差别、使用场景以及典型案例，帮助你在实际开发中灵活运用它们。

------

## 一、`SUM()` 函数 —— 数值求和

### 1. 基本语法

```sql
SUM(numeric_expression)
```

`SUM()` 用于对数值列中的值进行求和操作，忽略其中的 `NULL` 值。

### 2. 示例：计算部门总工资

```sql
SELECT deptno, SUM(sal) AS total_salary
FROM emp
GROUP BY deptno;
```

**说明**：统计每个部门的工资总额，结果中不会包含工资为 `NULL` 的行。

------

## 二、`COUNT()` 函数 —— 计数统计

### 1. 基本语法

```sql
-- 统计所有行（包括 NULL）
COUNT(*)

-- 统计某列非 NULL 的行数
COUNT(column_name)

-- 统计某列中唯一的非 NULL 值数量
COUNT(DISTINCT column_name)
```

### 2. 示例：统计各部门员工人数

```sql
SELECT deptno, COUNT(*) AS emp_count
FROM emp
GROUP BY deptno;
```

**说明**：`COUNT(*)` 统计所有行，不管某些列是否为 `NULL`。

------

## 三、NULL 值处理的不同

### 示例数据：

| EMPNO | ENAME | SAL  | COMM |
| ----- | ----- | ---- | ---- |
| 7369  | SMITH | 800  | NULL |
| 7499  | ALLEN | 1600 | 300  |
| 7521  | WARD  | 1250 | 500  |
| 7566  | JONES | 2975 | NULL |

### 比较：

```sql
SELECT SUM(comm), COUNT(comm), COUNT(*) FROM emp;
```

| SUM(COMM) | COUNT(COMM) | COUNT(*) |
| --------- | ----------- | -------- |
| 800       | 2           | 4        |

- `SUM(comm)` 忽略 NULL，仅加总 300 + 500；
- `COUNT(comm)` 忽略 NULL，仅统计两条非空记录；
- `COUNT(*)` 统计全部行（包括 `comm` 为 NULL 的行）。

------

## 四、联合使用的典型示例

```sql
SELECT deptno,
       COUNT(*) AS emp_count,
       SUM(sal) AS total_salary,
       ROUND(SUM(sal)/COUNT(*), 2) AS avg_salary
FROM emp
GROUP BY deptno;
```

**说明**：该语句统计每个部门的员工人数、工资总额和平均工资，常用于生成部门报表。

------

## 五、`SUM()` 与 `COUNT()` 对比总结

| 项目           | `SUM()`                | `COUNT()`                          |
| -------------- | ---------------------- | ---------------------------------- |
| 功能           | 对数值列求总和         | 对行或列进行计数                   |
| 忽略 NULL 吗？ | 是                     | `COUNT(*)` 否，`COUNT(column)` 是  |
| 应用于数据类型 | 只能用于数值列         | 可用于任意列                       |
| 是否可分组使用 | 是                     | 是                                 |
| 常见用途       | 销售额、工资等数值统计 | 行数统计、非空值统计、唯一值统计等 |

------

## 六、实战推荐使用方式

| 场景                       | 推荐函数              |
| -------------------------- | --------------------- |
| 统计所有员工数量           | `COUNT(*)`            |
| 统计工资不为 NULL 的员工数 | `COUNT(sal)`          |
| 统计所有工资总额           | `SUM(sal)`            |
| 统计发放了奖金的员工数     | `COUNT(comm)`         |
| 统计奖金总额               | `SUM(comm)`           |
| 统计不同职位的个数         | `COUNT(DISTINCT job)` |

------

## 七、常见误区提醒

- `SUM(NULL)` 的结果是 `NULL`，而不是 0，因此要避免直接对全为 NULL 的列求和；
- `COUNT(*)` 包含所有记录，不管列是否为 NULL；
- 求平均数时使用 `AVG()` 更直接，但 `SUM() / COUNT()` 可以提供更细粒度的控制（例如仅对部分列进行分母统计）；

------

## 八、结语

`SUM()` 与 `COUNT()` 是 Oracle SQL 中不可或缺的聚合工具。它们在数据统计与业务分析中扮演着核心角色。掌握它们之间的区别和使用技巧，不仅能写出高效的 SQL 查询，还能在面对复杂数据处理需求时游刃有余。