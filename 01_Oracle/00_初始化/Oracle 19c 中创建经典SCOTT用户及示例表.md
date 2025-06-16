# Oracle 19c 中创建经典SCOTT用户及示例表

## 概述

在Oracle 19c数据库中，默认不再包含经典的SCOTT用户及其示例表。本文详细介绍如何手动创建SCOTT用户，并重建其经典的EMP、DEPT、SALGRADE和BONUS表结构及数据。

## 创建SCOTT用户

### 1. 使用SYSDBA账户连接数据库

```sql
CONNECT / AS SYSDBA;
-- 或
CONNECT sys@orcl AS SYSDBA;
```

### 2. 创建SCOTT用户并授权

```sql
-- 创建用户并设置密码
CREATE USER scott IDENTIFIED BY 123456;

-- 授予基本权限
GRANT CONNECT, RESOURCE TO scott;

-- 授予无限制表空间权限
GRANT UNLIMITED TABLESPACE TO scott;

-- 12c及以上版本需要添加CONTAINER参数
ALTER USER scott DEFAULT TABLESPACE users QUOTA UNLIMITED ON users CONTAINER=ALL;
```

## 创建表结构

### 1. DEPT（部门表）

```sql
-- 创建DEPT表
CREATE TABLE scott.DEPT (
  deptno NUMBER(2)   NOT NULL PRIMARY KEY,
  dname  VARCHAR2(14),
  loc    VARCHAR2(13)
);

-- 插入部门数据
INSERT INTO scott.DEPT VALUES (10, 'ACCOUNTING', 'NEW YORK');
INSERT INTO scott.DEPT VALUES (20, 'RESEARCH', 'DALLAS');
INSERT INTO scott.DEPT VALUES (30, 'SALES', 'CHICAGO');
INSERT INTO scott.DEPT VALUES (40, 'OPERATIONS', 'BOSTON');
COMMIT;
```

### 2. EMP（员工表）

```sql
-- 创建EMP表
CREATE TABLE scott.EMP (
  empno    NUMBER(4)     NOT NULL PRIMARY KEY,
  ename    VARCHAR2(10),
  job      VARCHAR2(9),
  mgr      NUMBER(4),
  hiredate DATE,
  sal      NUMBER(7,2),
  comm     NUMBER(7,2),
  deptno   NUMBER(2)     CONSTRAINT fk_deptno REFERENCES scott.DEPT(deptno)
);

-- 插入员工数据
INSERT INTO scott.EMP VALUES (7369, 'SMITH', 'CLERK', 7902, TO_DATE('17-12-1980', 'DD-MM-YYYY'), 800, NULL, 20);
INSERT INTO scott.EMP VALUES (7499, 'ALLEN', 'SALESMAN', 7698, TO_DATE('20-02-1981', 'DD-MM-YYYY'), 1600, 300, 30);
INSERT INTO scott.EMP VALUES (7521, 'WARD', 'SALESMAN', 7698, TO_DATE('22-02-1981', 'DD-MM-YYYY'), 1250, 500, 30);
INSERT INTO scott.EMP VALUES (7566, 'JONES', 'MANAGER', 7839, TO_DATE('02-04-1981', 'DD-MM-YYYY'), 2975, NULL, 20);
INSERT INTO scott.EMP VALUES (7654, 'MARTIN', 'SALESMAN', 7698, TO_DATE('28-09-1981', 'DD-MM-YYYY'), 1250, 1400, 30);
INSERT INTO scott.EMP VALUES (7698, 'BLAKE', 'MANAGER', 7839, TO_DATE('01-05-1981', 'DD-MM-YYYY'), 2850, NULL, 30);
INSERT INTO scott.EMP VALUES (7782, 'CLARK', 'MANAGER', 7839, TO_DATE('09-06-1981', 'DD-MM-YYYY'), 2450, NULL, 10);
INSERT INTO scott.EMP VALUES (7788, 'SCOTT', 'ANALYST', 7566, TO_DATE('13-06-1987', 'DD-MM-YYYY'), 3000, NULL, 20);
INSERT INTO scott.EMP VALUES (7839, 'KING', 'PRESIDENT', NULL, TO_DATE('17-11-1981', 'DD-MM-YYYY'), 5000, NULL, 10);
INSERT INTO scott.EMP VALUES (7844, 'TURNER', 'SALESMAN', 7698, TO_DATE('08-09-1981', 'DD-MM-YYYY'), 1500, 0, 30);
INSERT INTO scott.EMP VALUES (7876, 'ADAMS', 'CLERK', 7788, TO_DATE('13-06-1987', 'DD-MM-YYYY'), 1100, NULL, 20);
INSERT INTO scott.EMP VALUES (7900, 'JAMES', 'CLERK', 7698, TO_DATE('03-12-1981', 'DD-MM-YYYY'), 950, NULL, 30);
INSERT INTO scott.EMP VALUES (7902, 'FORD', 'ANALYST', 7566, TO_DATE('03-12-1981', 'DD-MM-YYYY'), 3000, NULL, 20);
INSERT INTO scott.EMP VALUES (7934, 'MILLER', 'CLERK', 7782, TO_DATE('23-01-1982', 'DD-MM-YYYY'), 1300, NULL, 10);
COMMIT;
```

### 3. SALGRADE（薪资等级表）

```sql
-- 创建SALGRADE表
CREATE TABLE scott.SALGRADE (
  grade NUMBER,
  losal NUMBER,
  hisal NUMBER
);

-- 插入薪资等级数据
INSERT INTO scott.SALGRADE VALUES (1, 700, 1200);
INSERT INTO scott.SALGRADE VALUES (2, 1201, 1400);
INSERT INTO scott.SALGRADE VALUES (3, 1401, 2000);
INSERT INTO scott.SALGRADE VALUES (4, 2001, 3000);
INSERT INTO scott.SALGRADE VALUES (5, 3001, 9999);
COMMIT;
```

### 4. BONUS（奖金表）

```sql
-- 创建BONUS表（空表，用于演示）
CREATE TABLE scott.BONUS (
  ename VARCHAR2(10),
  job   VARCHAR2(9),
  sal   NUMBER,
  comm  NUMBER
);
```

## 验证安装

```sql
-- 连接到SCOTT用户
CONNECT scott/123456;

-- 查询表数据验证
SELECT * FROM emp;
SELECT * FROM dept;
SELECT * FROM salgrade;
DESC bonus;
```

## 表关系说明

| 表名     | 描述       | 主键   | 外键                  |
| -------- | ---------- | ------ | --------------------- |
| DEPT     | 部门表     | deptno | 无                    |
| EMP      | 员工表     | empno  | deptno → DEPT(deptno) |
| SALGRADE | 薪资等级表 | 无     | 无                    |
| BONUS    | 奖金表     | 无     | 无                    |

## 注意事项

1. **密码安全**：生产环境请使用更复杂的密码
2. **表空间管理**：可根据实际需求调整表空间配置
3. **版本兼容性**：Oracle 12c及以上版本需要`CONTAINER=ALL`参数
4. **数据完整性**：EMP表的deptno列与DEPT表的deptno列建立了外键关系
