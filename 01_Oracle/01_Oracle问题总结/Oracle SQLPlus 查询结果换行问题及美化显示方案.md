# Oracle SQL*Plus 查询结果换行问题及美化显示方案

在使用 Oracle 数据库的命令行工具 SQL*Plus 进行查询时，常会遇到如下问题：

```sql
SQL> SELECT * FROM emp;
```

输出结果如下：

```
EMPNO ENAME      JOB         MGR  HIREDATE
SAL   COMM       DEPTNO
----- ---------- ----------- ---- ----------
1600  300        30

7369  SMITH      CLERK       7902 17-DEC-80
800             20
...
```

我们可以看到：字段显示被**拆成了两行**，阅读体验极差。出现这种情况的原因，是 SQL*Plus 默认的行宽（LINESIZE）和字段宽度（COLUMN FORMAT）设置不够，导致字段内容被强制换行。

本文将介绍如何通过简单设置，让 SQL*Plus 查询结果**在一行内完整展示，排版清晰美观**。

------

## 一、问题原因分析

SQL*Plus 是 Oracle 提供的命令行查询工具，默认使用以下参数：

| 参数       | 默认值 | 说明                     |
| ---------- | ------ | ------------------------ |
| `LINESIZE` | 80     | 每行最多显示的字符数     |
| `PAGESIZE` | 14     | 每页显示的行数           |
| `COLUMN`   | 自动   | 某些字段宽度根据内容决定 |

当查询结果中字段较多或内容较长时，如果超出 `LINESIZE` 限制，就会被**强制换行**，造成字段错位或分列显示不完整。

------

## 二、解决方案：美化显示设置

进入 SQL*Plus 后，输入以下设置命令即可：

```sql
-- 设置每行最多显示 200 个字符（根据你屏幕可适当调整）
SET LINESIZE 200

-- 设置每页显示 1000 行，防止分页暂停
SET PAGESIZE 1000

-- 设置各列显示宽度（可根据需要调整）
COLUMN ENAME FORMAT A10
COLUMN JOB FORMAT A12
COLUMN HIREDATE FORMAT A12
```

### 示例设置说明：

- `SET LINESIZE 200`：避免字段因一行太短而被换行；
- `SET PAGESIZE 1000`：防止分页带来的干扰，适合一次性显示全部结果；
- `COLUMN ENAME FORMAT A10`：设置 ENAME 列宽为 10 个字符；
- `COLUMN JOB FORMAT A12`：设置 JOB 列宽为 12 个字符；
- `COLUMN HIREDATE FORMAT A12`：设置 HIREDATE 日期列宽为 12 字符。

------

## 三、执行效果展示

设置完成后，再次执行查询语句：

```sql
SELECT * FROM emp;
```

输出结果类似：

```
EMPNO ENAME      JOB         MGR  HIREDATE    SAL    COMM   DEPTNO
----- ---------- ----------- ---- ----------- ------ ------ -------
7369  SMITH      CLERK       7902 17-DEC-80    800            20
7499  ALLEN      SALESMAN    7698 20-FEB-81   1600    300     30
...
```

所有字段都在**同一行完整显示**，且排版整齐，极大提升了可读性。

------

## 四、进阶：自动加载配置

每次进入 SQL*Plus 都要手动设置比较繁琐，可以将以上设置写入一个自动加载脚本：

### 步骤：

1. 在某个路径（如 Oracle 安装目录或 SQL 文件目录）创建文件：`login.sql`
2. 文件内容写入以下设置：

```sql
SET LINESIZE 200
SET PAGESIZE 1000
COLUMN ENAME FORMAT A10
COLUMN JOB FORMAT A12
COLUMN HIREDATE FORMAT A12
```

1. 启动 SQL*Plus 后，使用如下方式执行：

```cmd
SQL> @login.sql
```

你也可以将该文件路径设置到环境变量或 SQL*Plus 启动配置中，实现**自动加载默认设置**（可选进阶操作）。

------

## 五、总结

| 操作               | 建议设置                |
| ------------------ | ----------------------- |
| 行宽（LINESIZE）   | `SET LINESIZE 200`      |
| 页大小（PAGESIZE） | `SET PAGESIZE 1000`     |
| 字段列宽（COLUMN） | `COLUMN 列名 FORMAT Ax` |

通过以上设置，即可解决 SQL*Plus 查询结果换行错位的问题，使输出更整洁、更易阅读