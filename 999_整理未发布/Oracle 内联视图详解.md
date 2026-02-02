# Oracle 内联视图（Inline View）学习笔记

## 一、什么是内联视图（Inline View）

**内联视图**，就是：

> **写在 SQL 语句中的子查询，被当作一张“临时表”来使用**

它不像普通视图那样用 `CREATE VIEW` 定义，
而是**只在当前这条 SQL 执行期间存在**。

一句话理解：

> **内联视图 = FROM 子句里的 SELECT**

------

## 二、内联视图的基本写法

### 1️⃣ 最经典结构

```sql
SELECT 列名
FROM (
    SELECT 列名
    FROM 表
    WHERE 条件
) 别名;
```

⚠️ 注意：

- **内联视图必须有别名**（Oracle 强制）

------

### 2️⃣ 基于 EMP 表的简单示例

```sql
SELECT *
FROM (
    SELECT empno, ename, sal
    FROM emp
    WHERE sal > 2000
) t;
```

解释：

- 子查询先执行
- 结果作为一张临时表 `t`

------

## 三、为什么要用内联视图（核心用途）

### 1️⃣ 解决“先算再筛选”的问题

#### ❌ 错误写法（聚合函数不能直接写在 WHERE）

```sql
SELECT empno, ename, sal
FROM emp
WHERE sal > AVG(sal); -- 报错
```

------

#### ✅ 正确写法（内联视图）

```sql
SELECT empno, ename, sal
FROM emp
WHERE sal > (
    SELECT AVG(sal)
    FROM emp
);
```

或者（内联视图形式）：

```sql
SELECT e.empno, e.ename, e.sal
FROM emp e,
     (SELECT AVG(sal) avg_sal FROM emp) a
WHERE e.sal > a.avg_sal;
```

------

### 2️⃣ Top-N 查询（Oracle 11g 必会）

#### 查询工资最高的 3 个员工

```sql
SELECT *
FROM (
    SELECT empno, ename, sal
    FROM emp
    ORDER BY sal DESC
)
WHERE ROWNUM <= 3;
```

📌 重点：

- `ORDER BY` **必须放在内联视图里**

------

## 四、内联视图 + 表连接（非常常用）

### 示例：查各部门平均工资，并关联部门名称

```sql
SELECT d.dname, t.avg_sal
FROM dept d
JOIN (
    SELECT deptno, AVG(sal) avg_sal
    FROM emp
    GROUP BY deptno
) t
ON d.deptno = t.deptno;
```

解释：

- 内联视图先算 **每个部门的平均工资**
- 外层再和 `DEPT` 关联

------

## 五、内联视图 vs 普通视图（对比）

| 对比项          | 内联视图       | 普通视图 |
| --------------- | -------------- | -------- |
| 是否保存        | 否             | 是       |
| 是否需要 CREATE | 否             | 是       |
| 作用范围        | 当前 SQL       | 全局     |
| 可否授权        | 否             | 是       |
| 适合场景        | 一次性复杂查询 | 复用逻辑 |

------

## 六、内联视图和复杂视图的关系

📌 内联视图 **不是** 视图对象的一种类型，
而是一种 **SQL 写法技巧**。

- 内联视图本身：
  - 可以是简单查询
  - 也可以是复杂查询（聚合 / 多表）

> **是否“复杂”，取决于子查询内容，而不是是不是内联视图**

------

## 七、内联视图能不能 UPDATE？

### 结论（直接记）：

> **内联视图本身不能 UPDATE**

但：

- Oracle 会尝试把 DML **“下推”到基表**
- 是否成功，取决于和普通视图一样的规则：
  - 单表
  - key-preserved table

📌 在学习阶段：

> **把内联视图当作“只查询”来用，是最安全的**

------

## 八、常见错误（初学者必踩）

### ❌ 忘记给内联视图起别名

```sql
SELECT *
FROM (
    SELECT * FROM emp
); -- 报错
```

### ✅ 正确写法

```sql
SELECT *
FROM (
    SELECT * FROM emp
) t;
```

------

## 九、学习记忆口诀

> **内联视图 = FROM 里的子查询**
> **先算一层，再在外面查**

