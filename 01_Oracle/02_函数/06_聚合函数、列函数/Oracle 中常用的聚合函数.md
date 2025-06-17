# Oracle 中常用的聚合函数一览

在 Oracle 中，**聚合函数（Aggregate Functions）**是对一组行进行计算并返回单个结果值的函数，常用于 `GROUP BY` 查询或统计分析。

| 函数名     | 作用说明                             | 示例                                                |
| ---------- | ------------------------------------ | --------------------------------------------------- |
| `COUNT`    | 统计行数（可以统计全部或非空值）     | `COUNT(*)` / `COUNT(sal)`                           |
| `SUM`      | 求和（通常用于数值字段）             | `SUM(sal)`                                          |
| `AVG`      | 计算平均值                           | `AVG(sal)`                                          |
| `MAX`      | 取最大值                             | `MAX(sal)`                                          |
| `MIN`      | 取最小值                             | `MIN(sal)`                                          |
| `STDDEV`   | 计算标准差                           | `STDDEV(sal)`                                       |
| `VARIANCE` | 计算方差                             | `VARIANCE(sal)`                                     |
| `GROUPING` | 判断某列是否为 `GROUP BY` 中的字段   | `GROUPING(deptno)`                                  |
| `LISTAGG`  | 将多行字符串聚合为一行（11g 及以上） | `LISTAGG(ename, ',') WITHIN GROUP (ORDER BY ename)` |
| `MEDIAN`   | 计算中位数（11g 及以上）             | `MEDIAN(sal)`                                       |
| `CORR`     | 计算相关系数（统计分析函数）         | `CORR(sal, comm)`                                   |

