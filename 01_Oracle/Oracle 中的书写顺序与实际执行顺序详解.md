# Oracle 中的书写顺序与实际执行顺序详解

在使用 Oracle SQL 进行查询时，我们通常会按照以下格式书写语句：

```sql
SELECT 列名
FROM 表
WHERE 条件
GROUP BY 分组字段
HAVING 分组条件
ORDER BY 排序字段;
```

这就是我们所说的 **SQL 的书写顺序**。然而，**Oracle 在执行 SQL 查询时，并不是按照这个顺序来处理的**。真正的执行顺序与 SQL 的写法有较大差异。

理解书写顺序与执行顺序的区别，是掌握 SQL 查询优化和正确编写语句的基础。本文将从这两个角度出发，进行详细解析。

------

## 一、SQL 的书写顺序（语法结构）

我们在日常开发中，一般按照以下顺序书写 SQL：

1. `SELECT` —— 要显示的字段或表达式
2. `FROM` —— 指定查询的数据表或视图
3. `JOIN` —— 多表连接（可选）
4. `WHERE` —— 行级过滤条件
5. `GROUP BY` —— 分组字段（可选）
6. `HAVING` —— 分组后的条件过滤（可选）
7. `ORDER BY` —— 排序字段（可选）
8. `FETCH/OFFSET` —— 分页（可选）

这是我们**写**的顺序，但并不是 Oracle 实际的执行顺序。

------

## 二、SQL 的实际执行顺序（逻辑处理顺序）

Oracle 在执行 SQL 查询时，采用如下逻辑顺序：

| 执行步骤 | 子句       | 作用说明                               |
| -------- | ---------- | -------------------------------------- |
| ①        | `FROM`     | 指定数据源；处理表连接（JOIN）         |
| ②        | `WHERE`    | 对原始数据进行**行过滤**               |
| ③        | `GROUP BY` | 将过滤后的数据按字段**进行分组**       |
| ④        | `HAVING`   | 对分组后的结果进行**组过滤**           |
| ⑤        | `SELECT`   | 选择要返回的字段；计算表达式和聚合函数 |
| ⑥        | `DISTINCT` | 可选：去重                             |
| ⑦        | `ORDER BY` | 对最终结果排序                         |
| ⑧        | `FETCH`    | 分页、限制返回的结果行数（可选）       |

------

## 三、案例解析：书写顺序 VS 执行顺序

### 示例 SQL：

```sql
SELECT deptno, AVG(sal) AS avg_salary
FROM emp
WHERE job <> 'CLERK'
GROUP BY deptno
HAVING AVG(sal) > 2000
ORDER BY avg_salary DESC;
```

### 实际执行流程解释：

1. **FROM emp**：首先从 `emp` 表中读取数据；
2. **WHERE job <> 'CLERK'**：过滤掉职位是 CLERK 的员工；
3. **GROUP BY deptno**：将剩余员工按部门编号分组；
4. **HAVING AVG(sal) > 2000**：过滤掉平均工资不超过 2000 的分组；
5. **SELECT deptno, AVG(sal)**：提取部门号和平均工资；
6. **ORDER BY avg_salary DESC**：按平均工资降序排序。

------

## 四、常见误区解析

### ❌ 错误写法 1：在 WHERE 中使用聚合函数

```sql
SELECT deptno, AVG(sal)
FROM emp
WHERE AVG(sal) > 2000   -- 错误！AVG 只能在 HAVING 中使用
GROUP BY deptno;
```

**原因**：`WHERE` 是在分组前执行的，不能引用聚合函数。

✅ 正确写法应为：

```sql
SELECT deptno, AVG(sal)
FROM emp
GROUP BY deptno
HAVING AVG(sal) > 2000;
```

------

### ❌ 错误写法 2：在 WHERE 中使用 SELECT 中的别名

```sql
SELECT sal * 12 AS annual_salary
FROM emp
WHERE annual_salary > 20000; -- 错误！annual_salary 是 SELECT 中才定义的
```

**原因**：`WHERE` 在 `SELECT` 之前执行，因此不能使用别名。

✅ 正确写法应改为：

```sql
SELECT sal * 12 AS annual_salary
FROM emp
WHERE sal * 12 > 20000;
```

------

## 五、一句话口诀记忆执行顺序

> **从（FROM）哪里查，挑（WHERE）谁留下；分（GROUP BY）组之后，筛（HAVING）组内行；选（SELECT）出字段，排（ORDER BY）好顺序。**

------

## 六、执行顺序图解

```text
实际执行顺序：
1. FROM         ← 首先读取数据源
2. WHERE        ← 过滤不符合条件的记录
3. GROUP BY     ← 对记录进行分组
4. HAVING       ← 过滤不符合分组条件的组
5. SELECT       ← 提取字段或聚合函数
6. DISTINCT     ← 去重（如有）
7. ORDER BY     ← 排序结果
8. FETCH/OFFSET ← 分页（如有）
```

------

## 七、总结对比

| 步骤 | 书写顺序     | 实际执行顺序 |
| ---- | ------------ | ------------ |
| 1    | SELECT       | FROM         |
| 2    | FROM         | WHERE        |
| 3    | WHERE        | GROUP BY     |
| 4    | GROUP BY     | HAVING       |
| 5    | HAVING       | SELECT       |
| 6    | ORDER BY     | DISTINCT     |
| 7    | FETCH/OFFSET | ORDER BY     |
| 8    | （可选）     | FETCH/OFFSET |

------

## 八、结语

理解 Oracle 中 SQL 的书写顺序与实际执行顺序，是编写高效、正确 SQL 查询的关键一步：

- 能帮你避免初学者常见的错误；
- 提高你对 SQL 查询过程的掌控；
- 为深入优化查询、调试语句、使用窗口函数等高级用法打下基础。