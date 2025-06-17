# Oracle 中的 `CASE WHEN` 与 `DECODE` 函数详解与对比

在 Oracle 数据库中，`CASE WHEN` 和 `DECODE` 都是用来实现条件判断和数据转换的重要工具。虽然它们可以实现类似的功能，但使用方式和适用场景略有不同。

本文将系统讲解这两个函数的语法、使用场景、各自优势，并通过示例进行对比分析。

------

## 一、`CASE WHEN` 表达式

### 1. 基本语法

#### 简单形式：

```sql
CASE expression
  WHEN value1 THEN result1
  WHEN value2 THEN result2
  ...
  ELSE default_result
END
```

#### 搜索形式（更灵活）：

```sql
CASE
  WHEN condition1 THEN result1
  WHEN condition2 THEN result2
  ...
  ELSE default_result
END
```

### 2. 示例

```sql
SELECT ename,
       sal,
       CASE
         WHEN sal >= 5000 THEN '高薪'
         WHEN sal >= 3000 THEN '中薪'
         ELSE '低薪'
       END AS 薪资等级
FROM emp;
```

------

## 二、`DECODE` 函数

### 1. 基本语法

```sql
DECODE(expression, search1, result1, search2, result2, ..., default)
```

等价于：

```sql
CASE expression
  WHEN search1 THEN result1
  WHEN search2 THEN result2
  ...
  ELSE default
END
```

### 2. 示例

```sql
SELECT ename,
       DECODE(deptno,
              10, '会计部',
              20, '研发部',
              30, '销售部',
              '其他') AS 部门名称
FROM emp;
```

------

## 三、功能对比

| 比较项         | `CASE WHEN`                                   | `DECODE`                         |
| -------------- | --------------------------------------------- | -------------------------------- |
| 支持的判断方式 | 任意布尔表达式（如 `=`, `>`, `<`, `LIKE` 等） | 仅支持等值判断                   |
| 可读性         | 高，结构清晰                                  | 中，结构紧凑但嵌套复杂时不易阅读 |
| 是否支持嵌套   | 支持                                          | 支持，但语法更复杂               |
| 空值处理       | 更直观，NULL 可用 `IS NULL` 等方式判断        | 遇到 NULL 时可能出现匹配失败     |
| 数据类型支持   | 支持更多数据类型                              | 对某些类型兼容较差               |
| 使用场景       | 条件复杂时首选                                | 简单替换、报表标签等场景较方便   |

------

## 四、同场景下的对比示例

### 1. 根据部门编号显示部门名称

#### 使用 `CASE`

```sql
SELECT ename,
       CASE deptno
         WHEN 10 THEN '会计部'
         WHEN 20 THEN '研发部'
         WHEN 30 THEN '销售部'
         ELSE '其他'
       END AS 部门名称
FROM emp;
```

#### 使用 `DECODE`

```sql
SELECT ename,
       DECODE(deptno,
              10, '会计部',
              20, '研发部',
              30, '销售部',
              '其他') AS 部门名称
FROM emp;
```

两种写法结果相同，但 `DECODE` 更简洁，`CASE` 更直观。

------

### 2. 判断工资等级（区间判断）

#### 使用 `CASE`（推荐）

```sql
SELECT ename,
       sal,
       CASE
         WHEN sal >= 5000 THEN '高薪'
         WHEN sal >= 3000 THEN '中薪'
         ELSE '低薪'
       END AS 薪资等级
FROM emp;
```

#### 使用 `DECODE`（不适用）

```sql
-- DECODE 只能做等值判断，无法处理区间条件
```

结论：需要使用不等号或范围判断时，必须用 `CASE`。

------

## 五、嵌套使用建议

可以将 `DECODE` 嵌套使用，但建议复杂逻辑还是使用 `CASE`，如下：

```sql
-- 嵌套 DECODE
SELECT DECODE(job,
              'MANAGER', DECODE(deptno, 10, '高级管理', '管理'),
              'CLERK', '文员',
              '其他') AS 职位描述
FROM emp;
```

对比：

```sql
-- 使用 CASE 更清晰
SELECT CASE
         WHEN job = 'MANAGER' AND deptno = 10 THEN '高级管理'
         WHEN job = 'MANAGER' THEN '管理'
         WHEN job = 'CLERK' THEN '文员'
         ELSE '其他'
       END AS 职位描述
FROM emp;
```

------

## 六、总结与最佳实践

| 使用场景                       | 推荐方式                 |
| ------------------------------ | ------------------------ |
| 简单等值替换                   | `DECODE`                 |
| 多条件/范围判断/逻辑组合       | `CASE`                   |
| 可读性和可维护性优先的脚本开发 | `CASE`                   |
| 高性能、极简查询语句           | `DECODE`（在简单场景下） |

------

## 七、结语

无论是 `CASE WHEN` 还是 `DECODE`，都是 Oracle SQL 中强大的数据转换与条件判断工具。掌握它们的使用技巧，可以极大提升 SQL 脚本的灵活性与表达能力。实际开发中，建议根据判断逻辑的复杂程度和代码可维护性选择合适的方式。

