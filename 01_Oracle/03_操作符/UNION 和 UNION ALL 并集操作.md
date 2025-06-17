# Oracle 中的并集操作详解

在 Oracle 数据库中，并集操作主要用于**合并多个查询结果集**，去除重复行。Oracle 提供了几种实现并集操作的方法，最常用的是 `UNION` 和 `UNION ALL` 运算符。

## 1. UNION 运算符

### 基本语法
```sql
SELECT column1, column2, ...
FROM table1
UNION
SELECT column1, column2, ...
FROM table2;
```

### 特点
- **去重**：自动去除重复行
- **排序**：结果集默认按第一列升序排序
- **列匹配**：各查询的列数必须相同，对应列的数据类型必须兼容

### 示例
```sql
-- 合并两个部门的员工ID，去除重复
SELECT employee_id FROM employees WHERE department_id = 10
UNION
SELECT employee_id FROM employees WHERE department_id = 20;
```

## 2. UNION ALL 运算符

### 基本语法
```sql
SELECT column1, column2, ...
FROM table1
UNION ALL
SELECT column1, column2, ...
FROM table2;
```

### 特点
- **不去重**：保留所有行，包括重复行
- **不排序**：结果集按查询出现的顺序返回
- **性能更高**：比UNION快，因为不需要去重和排序

### 示例
```sql
-- 合并两个表的所有记录，包括重复
SELECT product_id FROM current_products
UNION ALL
SELECT product_id FROM discontinued_products;
```

## 3. 多表并集操作

可以合并多个查询结果：
```sql
SELECT col1 FROM table1
UNION
SELECT col1 FROM table2
UNION
SELECT col1 FROM table3;
```

## 4. 并集操作的注意事项

### 列名和类型
- 第一个查询的列名作为结果集的列名
- 对应列的数据类型必须兼容（或可隐式转换）

### 排序控制
```sql
-- 对整个结果排序
SELECT col1 FROM table1
UNION
SELECT col1 FROM table2
ORDER BY col1 DESC;

-- 对单个查询排序（需用括号）
(SELECT col1 FROM table1 ORDER BY col1)
UNION ALL
(SELECT col1 FROM table2 ORDER BY col1);
```

### 性能考虑
- UNION 需要排序和去重，开销较大
- 对于大数据集，优先考虑 UNION ALL

## 5. 实际应用场景

### 场景1：合并多表数据
```sql
-- 合并不同年份的销售数据
SELECT product_id, sale_amount FROM sales_2022
UNION ALL
SELECT product_id, sale_amount FROM sales_2023;
```

### 场景2：创建完整列表
```sql
-- 获取所有曾经工作过的部门（包括当前和历史）
SELECT department_id FROM employees
UNION
SELECT department_id FROM job_history;
```

### 场景3：数据汇总
```sql
-- 统计所有产品的销售区域（去重）
SELECT region FROM north_sales
UNION
SELECT region FROM south_sales;
```

## 6. 与其他集合操作比较

| 操作符 | 描述 | 是否去重 | 是否排序 |
|--------|------|---------|---------|
| UNION | 并集 | 是 | 是 |
| UNION ALL | 并集 | 否 | 否 |
| INTERSECT | 交集 | 是 | 是 |
| MINUS | 差集 | 是 | 是 |

## 7. 高级用法

### 使用WITH子句
```sql
WITH combined_data AS (
  SELECT col1, col2 FROM table1
  UNION ALL
  SELECT col1, col2 FROM table2
)
SELECT * FROM combined_data WHERE col1 > 100;
```

### 带聚合函数的并集
```sql
SELECT 'Dept10' AS dept, COUNT(*) FROM employees WHERE department_id = 10
UNION ALL
SELECT 'Dept20' AS dept, COUNT(*) FROM employees WHERE department_id = 20;
```

### 分页并集结果
```sql
SELECT * FROM (
  SELECT col1, col2 FROM table1
  UNION ALL
  SELECT col1, col2 FROM table2
)
WHERE ROWNUM <= 100;
```

## 最佳实践建议

1. 当确定没有重复或不需要去重时，使用 UNION ALL 以获得更好性能
2. 尽量减少参与 UNION 操作的查询返回的列数
3. 对大数据集考虑先过滤再合并
4. 需要排序时，在最后使用一个 ORDER BY 子句

并集操作是 SQL 查询中组合数据的强大工具，合理使用可以简化许多复杂的数据合并需求。