# Oracle 简单性能测试脚本

```sql
-- =========================================
-- Oracle 11g Performance Test Script
-- =========================================
-- 建议开启服务器输出
SET SERVEROUTPUT ON
SET TIMING ON
SET FEEDBACK ON
SET PAGESIZE 1000
SET LINESIZE 200

PROMPT ====== Step 1: 清理环境 ======

BEGIN
    EXECUTE IMMEDIATE 'DROP TABLE perf_test PURGE';
EXCEPTION
    WHEN OTHERS THEN
        IF SQLCODE != -942 THEN
            RAISE;
        END IF;
END;
/

PROMPT ====== Step 2: 创建测试表 ======

CREATE TABLE perf_test (
    id            NUMBER PRIMARY KEY,
    name          VARCHAR2(50),
    created_at    DATE,
    amount        NUMBER(10,2),
    remark        VARCHAR2(200)
);

PROMPT ====== Step 3: 批量插入数据（100万行） ======

DECLARE
    v_start NUMBER := DBMS_UTILITY.get_time;
BEGIN
    FOR i IN 1..1000000 LOOP
        INSERT INTO perf_test VALUES (
            i,
            'NAME_' || i,
            SYSDATE - MOD(i, 365),
            DBMS_RANDOM.VALUE(1, 10000),
            RPAD('TEST', 100, 'X')
        );

        IF MOD(i, 10000) = 0 THEN
            COMMIT;
        END IF;
    END LOOP;
    COMMIT;

    DBMS_OUTPUT.put_line(
        'Insert Time (seconds): ' ||
        (DBMS_UTILITY.get_time - v_start) / 100
    );
END;
/

PROMPT ====== Step 4: 创建索引 ======

CREATE INDEX idx_perf_test_created ON perf_test(created_at);
CREATE INDEX idx_perf_test_amount ON perf_test(amount);

PROMPT ====== Step 5: 全表扫描测试 ======

DECLARE
    v_cnt NUMBER;
    v_start NUMBER := DBMS_UTILITY.get_time;
BEGIN
    SELECT COUNT(*) INTO v_cnt FROM perf_test;
    DBMS_OUTPUT.put_line('Total Rows: ' || v_cnt);
    DBMS_OUTPUT.put_line(
        'Full Scan Time (seconds): ' ||
        (DBMS_UTILITY.get_time - v_start) / 100
    );
END;
/

PROMPT ====== Step 6: 索引查询测试 ======

DECLARE
    v_cnt NUMBER;
    v_start NUMBER := DBMS_UTILITY.get_time;
BEGIN
    SELECT COUNT(*)
    INTO v_cnt
    FROM perf_test
    WHERE created_at > SYSDATE - 30;

    DBMS_OUTPUT.put_line('Recent Rows: ' || v_cnt);
    DBMS_OUTPUT.put_line(
        'Index Query Time (seconds): ' ||
        (DBMS_UTILITY.get_time - v_start) / 100
    );
END;
/

PROMPT ====== Step 7: 聚合查询测试 ======

DECLARE
    v_avg NUMBER;
    v_start NUMBER := DBMS_UTILITY.get_time;
BEGIN
    SELECT AVG(amount)
    INTO v_avg
    FROM perf_test
    WHERE amount > 5000;

    DBMS_OUTPUT.put_line('AVG Amount: ' || v_avg);
    DBMS_OUTPUT.put_line(
        'Aggregation Time (seconds): ' ||
        (DBMS_UTILITY.get_time - v_start) / 100
    );
END;
/

PROMPT ====== Step 8: 更新性能测试 ======

DECLARE
    v_start NUMBER := DBMS_UTILITY.get_time;
BEGIN
    UPDATE perf_test
    SET amount = amount * 1.05
    WHERE id <= 500000;
    COMMIT;

    DBMS_OUTPUT.put_line(
        'Update Time (seconds): ' ||
        (DBMS_UTILITY.get_time - v_start) / 100
    );
END;
/

PROMPT ====== Step 9: 删除性能测试 ======

DECLARE
    v_start NUMBER := DBMS_UTILITY.get_time;
BEGIN
    DELETE FROM perf_test WHERE id > 900000;
    COMMIT;

    DBMS_OUTPUT.put_line(
        'Delete Time (seconds): ' ||
        (DBMS_UTILITY.get_time - v_start) / 100
    );
END;
/

PROMPT ====== Performance Test Completed ======

```

