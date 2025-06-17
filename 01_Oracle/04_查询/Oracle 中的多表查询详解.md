# Oracle 中的多表查询详解

在实际业务系统中，数据往往存储在多个相关的表中，**通过多表查询（Multi-Table Query）才能获得完整的信息**。Oracle 支持多种多表查询方式，理解和掌握这些方法，是数据库开发的必备技能。

------

## 一、什么是多表查询？

多表查询是指在 SQL 中**同时访问两个或两个以上的表**，通过字段之间的逻辑关系，整合并返回符合条件的数据集合。

------

## 二、多表连接分类

Oracle 中多表查询通常采用 **连接（JOIN）** 的方式，可以分为以下几类：

| 类型                 | 关键字       | 说明                                       |
| -------------------- | ------------ | ------------------------------------------ |
| 内连接（Inner Join） | `INNER JOIN` | 只返回两个表中满足条件的匹配记录           |
| 左外连接             | `LEFT JOIN`  | 返回左表所有记录，右表无匹配则为 NULL      |
| 右外连接             | `RIGHT JOIN` | 返回右表所有记录，左表无匹配则为 NULL      |
| 全外连接             | `FULL JOIN`  | 返回两个表所有记录，无匹配则为 NULL        |
| 自连接               | -            | 同一张表内的连接，模拟父子结构、层级结构等 |
| 交叉连接（笛卡尔积） | `CROSS JOIN` | 所有行组合，极少使用                       |

------

## 三、准备示例数据

我们以经典的两个表为例：`emp`（员工表）和 `dept`（部门表）。

### emp 表（员工）：

| empno | ename | job      | sal  | deptno |
| ----- | ----- | -------- | ---- | ------ |
| 7369  | SMITH | CLERK    | 800  | 20     |
| 7499  | ALLEN | SALESMAN | 1600 | 30     |

### dept 表（部门）：

| deptno | dname      | loc      |
| ------ | ---------- | -------- |
| 10     | ACCOUNTING | NEW YORK |
| 20     | RESEARCH   | DALLAS   |
| 30     | SALES      | CHICAGO  |

------

## 四、等值连接（最常用）

```sql
SELECT e.ename, d.dname
FROM emp e
JOIN dept d ON e.deptno = d.deptno;
```

说明：`emp` 表和 `dept` 表通过 `deptno` 字段进行匹配。

------

## 五、Oracle 特有语法：旧式连接写法

在早期 Oracle 中，外连接可以通过 `(+)` 运算符表示。

```sql
-- 左外连接：以 emp 为主表
SELECT e.ename, d.dname
FROM emp e, dept d
WHERE e.deptno = d.deptno(+);
```

等价于：

```sql
SELECT e.ename, d.dname
FROM emp e
LEFT JOIN dept d ON e.deptno = d.deptno;
```

**建议：现代开发中使用 ANSI 标准 JOIN 更清晰、可维护性更好。**

------

## 六、外连接示例

### 1. 左外连接（LEFT JOIN）

```sql
SELECT e.ename, d.dname
FROM emp e
LEFT JOIN dept d ON e.deptno = d.deptno;
```

说明：显示所有员工，即使员工没有对应部门也显示，部门信息为 NULL。

------

### 2. 右外连接（RIGHT JOIN）

```sql
SELECT e.ename, d.dname
FROM emp e
RIGHT JOIN dept d ON e.deptno = d.deptno;
```

说明：显示所有部门，即使没有员工也显示，员工信息为 NULL。

------

### 3. 全外连接（FULL JOIN）

```sql
SELECT e.ename, d.dname
FROM emp e
FULL JOIN dept d ON e.deptno = d.deptno;
```

说明：显示所有员工和所有部门，即使它们没有匹配项。

------

## 七、自连接（Self Join）

用于一个表内的关联查询，如查找员工的上级。

```sql
SELECT e1.ename AS employee, e2.ename AS manager
FROM emp e1
LEFT JOIN emp e2 ON e1.mgr = e2.empno;
```

------

## 八、交叉连接（CROSS JOIN）

会生成表之间所有行的组合（笛卡尔积），用于特定统计分析。

```sql
SELECT e.ename, d.dname
FROM emp e
CROSS JOIN dept d;
```

------

## 九、多表连接查询的组合应用

可以同时连接多个表：

```sql
SELECT e.ename, d.dname, l.city
FROM emp e
JOIN dept d ON e.deptno = d.deptno
JOIN location l ON d.loc = l.loc_code;
```

------

## 十、实战案例

### 案例 1：列出所有员工及其所属部门名和工作地点

```sql
SELECT e.ename, d.dname, d.loc
FROM emp e
JOIN dept d ON e.deptno = d.deptno;
```

### 案例 2：列出没有部门的员工

```sql
SELECT e.ename
FROM emp e
LEFT JOIN dept d ON e.deptno = d.deptno
WHERE d.deptno IS NULL;
```

------

## 十一、多表查询注意事项

| 注意事项                          | 建议做法                            |
| --------------------------------- | ----------------------------------- |
| 明确指定表别名                    | 避免字段冲突，写法简洁              |
| 避免笛卡尔积                      | JOIN 时必须有 ON 条件               |
| 优先使用标准 SQL 语法（JOIN ...） | 替代 `(+)` 外连接写法               |
| WHERE 与 JOIN 的区别需明确        | JOIN 负责表连接，WHERE 控制结果过滤 |
| 多表连接时注意性能和索引使用      | 特别是在大数据量表连接时            |

------

## 十二、总结

| 类型     | 用法                       | 说明                             |
| -------- | -------------------------- | -------------------------------- |
| 内连接   | `INNER JOIN ... ON`        | 最常用，匹配两表中满足条件的记录 |
| 外连接   | `LEFT / RIGHT / FULL JOIN` | 包含无匹配记录（NULL）的行       |
| 自连接   | 同一张表自己连接           | 用于层级或关联结构查询           |
| 笛卡尔积 | `CROSS JOIN`               | 全组合，慎用                     |

------

## 附录：JOIN 类型对比图（简化版）

```
INNER JOIN      → 交集
LEFT JOIN       → 左表所有 + 匹配右表
RIGHT JOIN      → 右表所有 + 匹配左表
FULL JOIN       → 左右表并集
```