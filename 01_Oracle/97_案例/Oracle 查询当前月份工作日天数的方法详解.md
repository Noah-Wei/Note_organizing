# Oracle 查询当前月份工作日天数的方法详解

在日常企业报表、工资考勤等场景中，我们经常需要统计**某个月有多少个工作日**（不含周六日，甚至不含法定节假日）。本文将介绍如何使用 Oracle SQL 实现这一需求，适用于 Oracle 11g 及以上版本。

------

## 一、基本需求说明

> **目标：** 查询“当前月份”中所有非周末（即周一至周五）的天数，即工作日数量。

------

## 二、核心 SQL 实现（排除周六、周日）

```sql
SELECT COUNT(*) AS 工作日天数
  FROM (
    SELECT TRUNC(SYSDATE, 'MM') + LEVEL - 1 AS dt
      FROM DUAL
    CONNECT BY LEVEL <= TO_CHAR(LAST_DAY(SYSDATE), 'DD')
  )
WHERE TO_CHAR(dt, 'DY', 'NLS_DATE_LANGUAGE=AMERICAN') NOT IN ('SAT', 'SUN');
```

------

### 🔍 SQL 解析：

| 部分代码                                          | 说明                                 |
| ------------------------------------------------- | ------------------------------------ |
| `TRUNC(SYSDATE, 'MM')`                            | 当前月份的第一天                     |
| `LAST_DAY(SYSDATE)`                               | 当前月份的最后一天                   |
| `LEVEL` 与 `CONNECT BY`                           | 用于生成当月每天的日期序列           |
| `TO_CHAR(dt, 'DY', 'NLS_DATE_LANGUAGE=AMERICAN')` | 获取英文缩写的星期几                 |
| `NOT IN ('SAT', 'SUN')`                           | 排除周六（Saturday）和周日（Sunday） |

------

### ✅ 示例结果（以 2025 年 6 月为例）：

```text
工作日天数
----------
        21
```

表示 2025 年 6 月共有 21 个非周末的工作日。

------

## 三、进阶扩展：排除节假日

如果你想要更加精确地计算“实际工作日”，例如**还要排除国家法定节假日**，则需使用一张节假日表。

### 1. 创建节假日表（示例）：

```sql
CREATE TABLE holidays (
  holiday_date DATE
);
```

插入一些节假日（如端午、中秋等）：

```sql
INSERT INTO holidays VALUES (TO_DATE('2025-06-10', 'YYYY-MM-DD')); -- 示例：端午节
```

### 2. 修改 SQL，排除节假日：

```sql
SELECT COUNT(*) AS 实际工作日天数
  FROM (
    SELECT TRUNC(SYSDATE, 'MM') + LEVEL - 1 AS dt
      FROM DUAL
    CONNECT BY LEVEL <= TO_CHAR(LAST_DAY(SYSDATE), 'DD')
  )
WHERE TO_CHAR(dt, 'DY', 'NLS_DATE_LANGUAGE=AMERICAN') NOT IN ('SAT', 'SUN')
  AND dt NOT IN (SELECT holiday_date FROM holidays);
```

> ✅ 这样就实现了排除周末 + 排除法定节假日的真实“工作日统计”。

------

## 四、总结

| 场景                   | 使用方式                                                     |
| ---------------------- | ------------------------------------------------------------ |
| 排除周末               | 使用 `TO_CHAR(..., 'DY') NOT IN ('SAT','SUN')`               |
| 排除节假日             | 创建节假日表 `holidays` 并用 `NOT IN` 排除                   |
| 查询其他月份工作日数量 | 替换 `SYSDATE` 为指定日期，如 `TO_DATE('2025-07-01', 'YYYY-MM-DD')` |

------

## 五、附录：指定月份工作日统计模板

若你想指定某年某月，例如统计 2025 年 7 月的工作日数，可使用如下语句：

```sql
SELECT COUNT(*) AS 工作日天数
  FROM (
    SELECT TO_DATE('2025-07-01', 'YYYY-MM-DD') + LEVEL - 1 AS dt
      FROM DUAL
    CONNECT BY LEVEL <= TO_CHAR(LAST_DAY(TO_DATE('2025-07-01', 'YYYY-MM-DD')), 'DD')
  )
WHERE TO_CHAR(dt, 'DY', 'NLS_DATE_LANGUAGE=AMERICAN') NOT IN ('SAT', 'SUN');
```