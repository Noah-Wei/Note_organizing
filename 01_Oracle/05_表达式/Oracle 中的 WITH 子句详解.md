# Oracle 中的 `WITH` 子句详解（CTE）

在 Oracle 中，`WITH` 子句用于定义**一次性可复用的临时结果集**，通常被称为 **公用表表达式（CTE）**。它使复杂的 SQL 查询变得更清晰、更模块化，尤其适合处理嵌套查询、递归查询、或多个步骤逻辑的 SQL。

------

## 一、`WITH` 子句的基本语法

```sql
WITH 子查询名 AS (
  SELECT 子查询语句
)
SELECT 查询语句
FROM 子查询名;
```

你可以将 `WITH` 子句理解为临时的命名视图，它只在当前 SQL 中有效。

------

## 二、使用场景

| 使用场景                   | 好处说明                                     |
| -------------------------- | -------------------------------------------- |
| 多次复用的子查询           | 减少重复代码，提高可维护性                   |
| 多层嵌套逻辑拆分           | 将复杂查询逻辑分层，代码结构更清晰           |
| 实现递归查询（如层级结构） | `WITH` + `CONNECT BY` 或递归语法支持层级查询 |
| 临时排序或排名处理         | 配合 `ROWNUM`、`ROW_NUMBER()`、`RANK()` 等   |

------

## 三、基本示例

### 📌 示例：查询每个部门的平均工资，并显示工资高于平均工资的员工

```sql
WITH avg_sal AS (
  SELECT deptno, AVG(sal) AS dept_avg
  FROM emp
  GROUP BY deptno
)
SELECT e.ename, e.sal, e.deptno, a.dept_avg
FROM emp e
JOIN avg_sal a ON e.deptno = a.deptno
WHERE e.sal > a.dept_avg;
```

✅ `avg_sal` 是临时的结果表，在主查询中像普通表一样使用。

------

## 四、多个 CTE 的写法

```sql
WITH t1 AS (
  SELECT * FROM emp WHERE job = 'MANAGER'
),
t2 AS (
  SELECT * FROM emp WHERE sal > 3000
)
SELECT * FROM t1
UNION
SELECT * FROM t2;
```

✅ 多个子查询之间用逗号分隔。

------

## 五、递归查询（Oracle 11g+ 支持）

递归 CTE 用于处理树形结构，如组织结构、菜单层级、部门上下级等。

### 📌 示例：找出员工表中 SCOTT 的所有上级

```sql
WITH emp_hierarchy (empno, ename, mgr, level_no) AS (
  SELECT empno, ename, mgr, 1
  FROM emp
  WHERE ename = 'SCOTT'
  UNION ALL
  SELECT e.empno, e.ename, e.mgr, h.level_no + 1
  FROM emp e
  JOIN emp_hierarchy h ON e.empno = h.mgr
)
SELECT * FROM emp_hierarchy;
```

✅ 表示从 SCOTT 开始，逐层往上查找直属上级，直到最高层。

------

## 六、与子查询、视图的对比

| 比较项     | `WITH` 子句        | 内嵌子查询       | 视图（VIEW）    |
| ---------- | ------------------ | ---------------- | --------------- |
| 定义位置   | 查询内部           | 查询内部         | 数据库对象      |
| 是否可复用 | 本 SQL 内可复用    | 不能复用         | 多个 SQL 可复用 |
| 性能       | 编译器通常优化处理 | 会多次执行       | 可加索引优化    |
| 生命周期   | SQL 执行期间有效   | SQL 执行期间有效 | 永久            |

------

## 七、注意事项

1. `WITH` 子句中的子查询必须先定义再引用；
2. 所有 CTE 必须在主查询开始前定义完；
3. 如果 CTE 中带 `ORDER BY`，必须配合 `ROWNUM` 等子句使用；
4. 递归 CTE 仅支持 Oracle 11g 及以上版本。

------

## 八、面试问答简答模板

> Oracle 中的 `WITH` 子句又叫 CTE（公用表表达式），用于在 SQL 查询中临时定义命名子查询，使查询更清晰、逻辑分层更明确。它在处理复杂嵌套查询、递归层级结构时尤其有用。

------

## 九、总结

| 特性           | 说明                                            |
| -------------- | ----------------------------------------------- |
| 提升可读性     | 将复杂查询拆分成多个命名步骤，便于理解和调试    |
| 多次复用       | 在同一查询中可复用临时表，避免重复逻辑          |
| 支持递归       | 用于层级数据结构的查询，替代 `CONNECT BY`       |
| 优于嵌套子查询 | 有助于性能优化（Oracle 编译器可提前执行和优化） |

