# Oracle 视图（View）详解

## 一、什么是视图（View）

**视图（View）** 是一种 **虚拟表**，本身不存储数据，而是保存了一条 `SELECT` 查询语句。

- 表：真正存储数据
- 视图：从表（或其他视图）中“看到”的结果

可以理解为：

> 视图 = 给复杂 SQL 起的一个名字

------

## 二、为什么要使用视图

### 1️⃣ 简化复杂查询

```sql
SELECT E.EMPNO, E.ENAME, E.SAL, D.DNAME
FROM EMP E
JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

如果经常使用，可以封装成视图。

### 2️⃣ 提高安全性（隐藏字段）

- 不让普通用户看到 `SAL`、`COMM`
- 只暴露必要列

### 3️⃣ 统一业务逻辑

- 例如：公司规定“在职员工”定义
- 用视图统一规则，避免各处 SQL 不一致

------

## 三、创建视图（CREATE VIEW）

### 1️⃣ 最基本的视图

```sql
CREATE VIEW v_emp AS
SELECT empno, ename, job, deptno
FROM emp;
```

查询视图：

```sql
SELECT * FROM v_emp;
```

⚠️ 注意：

- 创建视图需要 `CREATE VIEW` 权限

  ```sql
  GRANT CREATE VIEW TO 用户;
  ```

------

### 2️⃣ 基于多表的视图（常用）

```sql
CREATE VIEW v_emp_dept AS
SELECT e.empno, e.ename, e.job, e.sal, d.dname
FROM emp e
JOIN dept d ON e.deptno = d.deptno;
```

用途：

- 员工 + 部门信息一次查出

------

### 3️⃣ 带条件的视图

```sql
CREATE VIEW v_emp_10 AS
SELECT empno, ename, sal
FROM emp
WHERE deptno = 10;
```

该视图只显示 **10号部门员工**。

------

## 四、视图中的 DML 操作

### 1️⃣ 可以更新的简单视图

满足以下条件时，视图 **可 DML（增删改）**：

- 单表
- 不包含：
  - 聚合函数（SUM、AVG…）
  - DISTINCT
  - GROUP BY
  - 子查询

```sql
UPDATE v_emp
SET job = 'CLERK'
WHERE empno = 7369;
```

➡️ 实际更新的是 `EMP` 表。

------

### 2️⃣ 不可更新的视图（常见）

```sql
CREATE VIEW v_dept_avg_sal AS
SELECT deptno, AVG(sal) avg_sal
FROM emp
GROUP BY deptno;
```

❌ 不能执行 INSERT / UPDATE / DELETE。

------

## 五、WITH CHECK OPTION（检查约束）

### 问题场景

```sql
CREATE VIEW v_emp_20 AS
SELECT empno, ename, deptno
FROM emp
WHERE deptno = 20;
-- 更新表
UPDATE v_emp_20
SET deptno = 30
WHERE empno = 7566;
```

👉 结果：

- 更新成功
- 但该员工 **从视图中消失**

------

### 使用 WITH CHECK OPTION

```sql
CREATE VIEW v_emp_20 AS
SELECT empno, ename, deptno
FROM emp
WHERE deptno = 20
WITH CHECK OPTION;
```

效果：

- ❌ 不允许修改后不符合视图条件

------

## 六、只读视图（READ ONLY）

```sql
CREATE VIEW v_emp_readonly AS
SELECT empno, ename, sal
FROM emp
WITH READ ONLY;
```

特点：

- 只能查询
- 所有 DML 都会报错

------

## 七、删除视图（DROP VIEW）

```sql
DROP VIEW v_emp;
```

⚠️ 注意：

- 删除视图 **不会影响原表数据**

------

## 八、视图 vs 表

| 对比项       | 表   | 视图     |
| ------------ | ---- | -------- |
| 是否存储数据 | 是   | 否       |
| 占用空间     | 大   | 几乎不占 |
| 可否简化 SQL | ❌    | ✅        |
| 安全性       | 低   | 高       |

------

