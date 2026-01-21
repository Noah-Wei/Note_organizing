# Oracle SQL 语言分类详解

在 Oracle 数据库的学习中，理解 SQL（Structured Query Language，结构化查询语言）的分类是构建知识体系的基石。不同的 SQL 指令对应着数据库不同的操作层面，从数据的增删改查到对象的定义与权限管理。

通常，我们将 Oracle SQL 分为五大类：**DQL**、**DML**、**DDL**、**TCL** 和 **DCL**。

---

## 一、DQL (Data Query Language) - 数据查询语言

这是数据库中使用频率最高的部分，用于从数据库中检索数据。

- **核心特点**：只读取数据，**不改变**数据库中的任何数据或结构。
- **主要命令**：
  - `SELECT`：这是 DQL 的核心。

**示例：**

```sql
-- 查询所有员工信息
SELECT * FROM employees;

-- 带条件的查询
SELECT first_name, salary FROM employees WHERE department_id = 10;
```

------

## 二、DML (Data Manipulation Language) - 数据操作语言

DML 用于对表中的**数据（Data）**进行变更（增、删、改）。

- **核心特点**：
  - 操作的是表里的“记录（Rows）”。
  - **不会自动提交（Auto-commit）**。这意味着执行完 DML 后，需要执行 COMMIT 才能永久生效，或者执行 ROLLBACK 撤销操作。
- **主要命令**：
  - `INSERT`：插入新数据。
  - `UPDATE`：更新现有数据。
  - `DELETE`：删除数据。
  - `MERGE`：合并数据（根据条件决定是更新还是插入，Oracle 特有且强大的功能）。

**示例：**

```sql
-- 插入
INSERT INTO departments (id, name) VALUES (100, 'IT Support');

-- 更新
UPDATE employees SET salary = salary * 1.1 WHERE id = 101;

-- 删除（并未释放空间，只是标记删除）
DELETE FROM employees WHERE id = 102;
```

---

## 三、DDL (Data Definition Language) - 数据定义语言

DDL 用于定义或修改数据库的**结构（Structure）\**或\**对象（Schema Objects）**，如表、索引、视图等。

- **核心特点**：
  - 操作的是“对象”而非“数据”。
  - **隐式提交（Implicit Commit）**。这是 DDL 与 DML 最大的区别！一旦执行 DDL 语句，之前所有未提交的 DML 事务也会被强制提交，无法回滚。
- **主要命令**：
  - `CREATE`：创建对象（表、视图、索引、序列等）。
  - `ALTER`：修改对象结构（如添加列、修改列类型）。
  - `DROP`：删除对象。
  - `TRUNCATE`：截断表（**注意：** 虽然它主要表现为删除数据，但它属于 DDL）。
  - `RENAME`：重命名对象。
  - `COMMENT`：添加注释。

**重点辨析：DELETE vs TRUNCATE**

| 特性 | DELETE (DML) | TRUNCATE (DDL) |
| :--- | :--- | :--- |
| **功能** | 删除表中的行 | 删除表中所有行 |
| **条件** | 可以带 WHERE 子句 | 不支持 WHERE，只能全删 |
| **事务** | 需要 Commit，可回滚 | **自动提交，不可回滚** |
| **速度** | 慢（产生大量 Undo/Redo 日志） | 快（直接释放数据块，日志少） |

------

## 四、TCL (Transaction Control Language) - 事务控制语言

TCL 用于维护数据的一致性，主要配合 DML 语句使用，管理 DML 操作组成的事务。

- **主要命令**：
  - `COMMIT`：提交事务，将 DML 的更改永久保存到数据库。
  - `ROLLBACK`：回滚事务，撤销自上次提交以来的所有 DML 更改。
  - `SAVEPOINT`：在事务中设置保存点，允许回滚到事务的特定位置，而不是全部回滚。

**示例场景：**

```sql
INSERT INTO table_a VALUES (1); -- 事务开始
SAVEPOINT sp1;                  -- 设置保存点
DELETE FROM table_a WHERE id=1; -- 删除刚才的数据
ROLLBACK TO sp1;                -- 后悔了，回滚到插入后的状态（数据还在）
COMMIT;                         -- 提交，事务结束
```

------

## 五、 DCL (Data Control Language) - 数据控制语言

DCL 用于定义数据库的访问权限和安全级别。

- **主要命令**：
  - `GRANT`：授予权限（如允许某用户连接数据库、查询某张表）。
  - `REVOKE`：回收权限。


**示例：**

```sql
-- 授权
GRANT CONNECT, RESOURCE TO user_scott;
GRANT SELECT ON employees TO user_scott;

-- 回收
REVOKE SELECT ON employees FROM user_scott;
```

------

## 总结速查表

| 分类    | 全称                         | 常用命令                      | 事务行为       | 作用对象  |
| ------- | ---------------------------- | ----------------------------- | -------------- | --------- |
| **DQL** | Data Query Language          | SELECT                        | 只读           | 数据      |
| **DML** | Data Manipulation Language   | INSERT, UPDATE, DELETE, MERGE | **需手动提交** | 数据      |
| **DDL** | Data Definition Language     | CREATE, ALTER, DROP, TRUNCATE | **自动提交**   | 结构/对象 |
| **TCL** | Transaction Control Language | COMMIT, ROLLBACK, SAVEPOINT   | 控制事务       | 事务      |
| **DCL** | Data Control Language        | GRANT, REVOKE                 | 自动提交       | 权限      |

---

