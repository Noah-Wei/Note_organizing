# Oracle DBMS_RANDOM 包详解

DBMS_RANDOM 是 Oracle 提供的一个内置包，用于生成**随机数**和**随机字符串**，在数据生成、抽样测试和加密等场景中非常有用。

## 主要函数概览

| 函数/过程 | 描述 |
|-----------|------|
| `VALUE` | 生成随机数 |
| `NORMAL` | 生成正态分布随机数 |
| `STRING` | 生成随机字符串 |
| `SEED` | 重置随机数种子 |
| `RANDOM` | 生成整数随机数（已过时） |
| `TERMINATE` | 关闭随机数生成器 |

## 详细用法

### 1. VALUE 函数 - 生成随机数

**基本语法**：
```sql
DBMS_RANDOM.VALUE 
  RETURN NUMBER;  -- 返回0~1之间的随机数

DBMS_RANDOM.VALUE(
  low  IN NUMBER,
  high IN NUMBER) 
  RETURN NUMBER;  -- 返回指定范围的随机数，不包含high
```

**示例**：
```sql
-- 生成0~1之间的随机小数
SELECT DBMS_RANDOM.VALUE FROM dual;

-- 生成10~20之间的随机数，不包含20
SELECT DBMS_RANDOM.VALUE(10, 20) FROM dual;

-- 需求：随机产生5个整数，范围在10~20之间
SELECT  TRUNC(dbms_random.value(10,21)) FROM dual  CONNECT BY LEVEL < 6;

-- 生成表格数据时使用
INSERT INTO test_data 
VALUES (DBMS_RANDOM.VALUE(1,100), 
        DBMS_RANDOM.VALUE(1000,2000));
```

### 2. NORMAL 函数 - 正态分布随机数

**语法**：
```sql
DBMS_RANDOM.NORMAL 
  RETURN NUMBER;  -- 均值为0，标准差为1
```

**示例**：
```sql
-- 生成正态分布随机数
SELECT DBMS_RANDOM.NORMAL FROM dual;

-- 生成特定均值和标准差的正态分布
SELECT 50 + DBMS_RANDOM.NORMAL * 10 FROM dual;  -- 均值50，标准差10
```

### 3. STRING 函数 - 随机字符串

**语法**：
```sql
DBMS_RANDOM.STRING(
  opt  IN CHAR,
  len  IN NUMBER)
  RETURN VARCHAR2;
```

**参数选项(opt)**：
- 'u' 或 'U'：大写字母
- 'l' 或 'L'：小写字母
- 'a' 或 'A'：混合大小写字母
- 'x' 或 'X'：大写字母和数字
- 'p' 或 'P'：任意可打印字符

**示例**：
```sql
-- 生成8位大写随机字符串
SELECT DBMS_RANDOM.STRING('U', 8) FROM dual;

-- 生成10位包含数字的随机字符串
SELECT DBMS_RANDOM.STRING('X', 10) FROM dual;

-- 创建随机密码
UPDATE users 
SET temp_password = DBMS_RANDOM.STRING('A', 12);
```

### 4. SEED 过程 - 设置随机种子

**语法**：
```sql
DBMS_RANDOM.SEED(
  val  IN NUMBER);  -- 数字种子

DBMS_RANDOM.SEED(
  val  IN VARCHAR2); -- 字符串种子
```

**示例**：
```sql
-- 设置数字种子（可重现随机序列）
BEGIN
  DBMS_RANDOM.SEED(123);
  FOR i IN 1..5 LOOP
    DBMS_OUTPUT.PUT_LINE(DBMS_RANDOM.VALUE);
  END LOOP;
END;
/

-- 设置字符串种子
BEGIN
  DBMS_RANDOM.SEED('Oracle');
END;
/
```

### 5. RANDOM 函数（已过时）

**注意**：Oracle 建议使用 VALUE 函数替代

**示例**：
```sql
SELECT DBMS_RANDOM.RANDOM FROM dual;  -- 返回[-2^31, 2^31)的整数
```

## 实际应用场景

### 1. 测试数据生成
```sql
-- 生成1000条随机测试数据
BEGIN
  FOR i IN 1..1000 LOOP
    INSERT INTO test_table VALUES (
      DBMS_RANDOM.VALUE(1,100),
      DBMS_RANDOM.STRING('A', 20),
      SYSDATE - DBMS_RANDOM.VALUE(1,365)
    );
  END LOOP;
  COMMIT;
END;
/
```

### 2. 随机抽样
```sql
-- 从表中随机选择10%的记录
SELECT * FROM large_table
WHERE DBMS_RANDOM.VALUE <= 0.1;
```

### 3. 随机排序
```sql
-- 随机排序查询结果
SELECT * FROM products
ORDER BY DBMS_RANDOM.VALUE;
```

### 4. 密码重置
```sql
-- 批量生成临时密码
UPDATE users 
SET temp_password = DBMS_RANDOM.STRING('X', 10),
    pwd_reset_date = SYSDATE
WHERE status = 'ACTIVE';
```

## 注意事项

1. **性能影响**：大量调用 DBMS_RANDOM 可能影响SQL性能
2. **安全性**：不适用于加密用途，如需加密安全随机数应使用 `DBMS_CRYPTO.RANDOMBYTES`
3. **版本差异**：
   - Oracle 10g 之前需要先初始化：`DBMS_RANDOM.INITIALIZE`
   - Oracle 10g 开始自动初始化
4. **种子设置**：设置相同种子可重现随机序列，适用于测试

## 高级用法示例

### 生成随机日期
```sql
SELECT TO_DATE('2000-01-01', 'YYYY-MM-DD') + 
       DBMS_RANDOM.VALUE(0, 365*20) AS random_date
FROM dual;
```

### 生成随机布尔值
```sql
SELECT CASE WHEN DBMS_RANDOM.VALUE > 0.5 THEN 'Y' ELSE 'N' END AS random_flag
FROM dual;
```

### 加权随机选择
```sql
SELECT product_name 
FROM (
  SELECT product_name, 
         DBMS_RANDOM.VALUE * weight AS random_val
  FROM products
)
ORDER BY random_val DESC
FETCH FIRST 1 ROW ONLY;
```

DBMS_RANDOM 是 Oracle 数据库中进行随机操作的强大工具，合理使用可以大大简化开发和测试工作。