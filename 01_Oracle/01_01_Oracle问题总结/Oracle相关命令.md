# Oracle相关命令

- 检查当前数据库时区：

  ```sql
   SELECT DBTIMEZONE FROM dual;
  ```

- 查看服务名（实例/数据库名）

  ```sql
  SELECT value FROM v$parameter WHERE name = 'service_names';
  ```

  