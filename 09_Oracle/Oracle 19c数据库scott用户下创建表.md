# Oracle 19c数据库scott用户下创建表

## 前言

从 Oracle Database 12c 开始，Oracle 不再默认安装 scott 用户。在 Oracle Database 12c 和更新的版本中，Oracle 建议用户创建自定义的测试用户或者使用其他的示例数据库和工具来进行测试和演示。所以**Oracle 19c数据库默认没有scott用户及数据表**

但是以前版本中scott用户下的几张表，给我们用来学习十分方便，为此记录一下创建表及添加数据的代码

## 创建 SCOTT 用户

- 创建用户

  > 123456为用户的密码

  ```sql
  CREATE USER scott IDENTIFIED BY 123456 ;
  ```

- 赋予权限

  ```
  GRANT CONNECT,RESOURCE,UNLIMITED TABLESPACE TO scott CONTAINER=ALL ;
  ```

## 创建表

### DEPT 表

- 创建表

  ```sql
  -- Create table
  create table DEPT
  (
    deptno NUMBER(2) not null,
    dname  VARCHAR2(14),
    loc    VARCHAR2(13)
  );
  -- Create/Recreate primary, unique and foreign key constraints
  alter table DEPT
    add constraint PK_DEPT primary key (DEPTNO)
  ;
  ```

- 插入数据

  ```sql
  insert into dept (DEPTNO, DNAME, LOC)
  values ('10', 'ACCOUNTING', 'NEW YORK');
  
  insert into dept (DEPTNO, DNAME, LOC)
  values ('20', 'RESEARCH', 'DALLAS');
  
  insert into dept (DEPTNO, DNAME, LOC)
  values ('30', 'SALES', 'CHICAGO');
  
  insert into dept (DEPTNO, DNAME, LOC)
  values ('40', 'OPERATIONS', 'BOSTON');
  
  COMMIT;
  ```

### EMP 表

- 创建表

  ```sql
  -- Create table
  create table EMP
  (
    empno    NUMBER(4) not null,
    ename    VARCHAR2(10),
    job      VARCHAR2(9),
    mgr      NUMBER(4),
    hiredate DATE,
    sal      NUMBER(7,2),
    comm     NUMBER(7,2),
    deptno   NUMBER(2)
  );
  -- Create/Recreate primary, unique and foreign key constraints
  alter table EMP
    add constraint PK_EMP primary key (EMPNO);
  alter table EMP
    add constraint FK_DEPTNO foreign key (DEPTNO)
    references DEPT (DEPTNO);
  ```

- 插入数据

  ```sql
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7369', 'SMITH', 'CLERK', '7902', to_date('17-12-1980', 'dd-mm-yyyy'), '800', null, '20');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7499', 'ALLEN', 'SALESMAN', '7698', to_date('20-02-1981', 'dd-mm-yyyy'), '1600', '300', '30');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7521', 'WARD', 'SALESMAN', '7698', to_date('22-02-1981', 'dd-mm-yyyy'), '1250', '500', '30');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7566', 'JONES', 'MANAGER', '7839', to_date('02-04-1981', 'dd-mm-yyyy'), '2975', null, '20');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7654', 'MARTIN', 'SALESMAN', '7698', to_date('28-09-1981', 'dd-mm-yyyy'), '1250', '1400', '30');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7698', 'BLAKE', 'MANAGER', '7839', to_date('01-05-1981', 'dd-mm-yyyy'), '2850', null, '30');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7782', 'CLARK', 'MANAGER', '7839', to_date('09-06-1981', 'dd-mm-yyyy'), '2450', null, '10');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7788', 'SCOTT', 'ANALYST', '7566', to_date('13-06-0187', 'dd-mm-yyyy'), '3000', null, '20');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7839', 'KING', 'PRESIDENT', null, to_date('17-11-1981', 'dd-mm-yyyy'), '5000', null, '10');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7844', 'TURNER', 'SALESMAN', '7698', to_date('08-09-1981', 'dd-mm-yyyy'), '1500', '0', '30');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7876', 'ADAMS', 'CLERK', '7788', to_date('13-06-0187', 'dd-mm-yyyy'), '1100', null, '20');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7900', 'JAMES', 'CLERK', '7698', to_date('03-12-1981', 'dd-mm-yyyy'), '950', null, '30');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7902', 'FORD', 'ANALYST', '7566', to_date('03-12-1981', 'dd-mm-yyyy'), '3000', null, '20');
  
  insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  values ('7934', 'MILLER', 'CLERK', '7782', to_date('23-01-1982', 'dd-mm-yyyy'), '1300', null, '10');
  
  COMMIT;
  ```

### SALGRADE 表

- 创建表

  ```sql
  create table SALGRADE
  (
    grade NUMBER,
    losal NUMBER,
    hisal NUMBER
  );
  ```

- 插入数据

  ```sql
  insert into salgrade (GRADE, LOSAL, HISAL)
  values ('1', '700', '1200');
  
  insert into salgrade (GRADE, LOSAL, HISAL)
  values ('2', '1201', '1400');
  
  insert into salgrade (GRADE, LOSAL, HISAL)
  values ('3', '1401', '2000');
  
  insert into salgrade (GRADE, LOSAL, HISAL)
  values ('4', '2001', '3000');
  
  insert into salgrade (GRADE, LOSAL, HISAL)
  values ('5', '3001', '9999');
  
  COMMIT;
  ```

### BONUS 表

- 创建表

  ```sql
  create table BONUS
  (
    ename VARCHAR2(10),
    job   VARCHAR2(9),
    sal   NUMBER,
    comm  NUMBER
  );
  ```

