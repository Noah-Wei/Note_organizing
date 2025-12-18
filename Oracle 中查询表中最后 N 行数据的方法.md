# Oracle 中查询表中最后 N 行数据的方法

在 Oracle 中，由于没有 `LIMIT` 或 `OFFSET` 子句（如 MySQL），也没有 `TOP` 子句（如 SQL Server），要获取表中最后 N 行数据需要使用一些特殊技巧。以下是几种常见方法：

## 方法1：使用 ROWNUM 和子查询（适用于已知总行数）

```sql
SELECT *
FROM (
    SELECT a.*, ROWNUM rn
    FROM (
        SELECT * FROM your_table ORDER BY rowid DESC  -- 或按某个有序列降序
    ) a
    WHERE ROWNUM <= N  -- N 是你要获取的行数
)
WHERE rn <= N;
```

## 方法2：使用 ROW_NUMBER() 分析函数（Oracle 8i 及以上）

```sql
SELECT *
FROM (
    SELECT t.*, ROW_NUMBER() OVER (ORDER BY rowid DESC) rn
    FROM your_table t
)
WHERE rn <= N;
```

## 方法3：使用 ROWNUM 和 COUNT（适用于小表）

```sql
SELECT *
FROM (
    SELECT a.*, ROWNUM rn
    FROM your_table a
    ORDER BY rowid DESC  -- 或按某个有序列降序
)
WHERE rn <= N;
```

## 方法4：使用 FETCH FIRST（Oracle 12c 及以上）

Oracle 12c 引入了类似其他数据库的语法：

```sql
SELECT *
FROM your_table
ORDER BY rowid DESC  -- 或按某个有序列降序
FETCH FIRST N ROWS ONLY;
```

## 实际示例

假设有一个 EMP 表，要获取最后 5 条员工记录：

```sql
-- 方法1示例
SELECT *
FROM (
    SELECT a.*, ROWNUM rn
    FROM (
        SELECT * FROM EMP ORDER BY EMPNO DESC
    ) a
    WHERE ROWNUM <= 5
)
WHERE rn <= 5;
 
-- 方法2示例（推荐）
SELECT *
FROM (
    SELECT t.*, ROW_NUMBER() OVER (ORDER BY EMPNO DESC) rn
    FROM EMP t
)
WHERE rn <= 5;
 
-- 方法4示例（Oracle 12c+）
SELECT *
FROM EMP
ORDER BY EMPNO DESC
FETCH FIRST 5 ROWS ONLY;
```

## 注意事项

1. Oracle 表在物理上是无序的，所以必须使用 `ORDER BY` 指定排序依据
2. 如果没有合适的排序列（如自增ID或时间戳），可以使用 `ROWID` 或 `ROWNUM`，但这些在数据删除后可能不稳定
3. 对于大表，方法2（ROW_NUMBER()）通常性能最好

最可靠的方法是确保表有一个自增主键或创建时间戳列，然后基于该列排序获取最后N行。