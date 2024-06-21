# MySQL问题：ERROR 1819 (HY000): Your password does not satisfy the current policy requirements

## 1、问题描述

当修改mysql密码时，如果密码设置的太简单的话，会提示报错：

```mysql
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
```

![image-20230225134329765](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9a0143f70f.png)

## 2、出现原因

- **mysql安装了validate_password密码校验插件，导致要修改的密码不符合密码策略的要求。**

### 2.1 查看当前的密码策略

![image-20230225134832883](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9a130b92ed.png)

- 策略说明

```mysql
mysql> SHOW VARIABLES LIKE 'validate_password%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password_check_user_name    | OFF    |	决定是否使用该插件(及强制/永久强制使用):ON/OFF/FORCE/FORCE_PLUS_PERMANENT
| validate_password_dictionary_file    |        |	插件用于验证密码强度的字典文件路径
| validate_password_length             | 8      |	密码最小长度
| validate_password_mixed_case_count   | 1      |	密码至少要包含的小写字母个数和大写字母个数
| validate_password_number_count       | 1      |	密码至少要包含的数字个数
| validate_password_policy             | MEDIUM |	密码强度检查等级，0/LOW、1/MEDIUM、2/STRONG
| validate_password_special_char_count | 1      |	密码至少要包含的特殊字符数
+--------------------------------------+--------+
7 rows in set (0.02 sec)
													0/LOW：只检查长度。
                                                    1/MEDIUM：检查长度、数字、大小写、特殊字符。
                                                    2/STRONG：检查长度、数字、大小写、特殊字符字典文件。
```

## 3、可用的解决方案

> 思路：
>
> 1、遵从策略
>
> 2、修改策略
>
> 3、策略失效

### 3.1 按照要求输入上述要求的密码

如输入的密码为：`Wxq3012@`

```mysql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Wxq3012@';
Query OK, 0 rows affected (0.00 sec)
```

![image-20230225135101086](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9a1c4e82bb.png)

### 3.2 更改策略：修改全局变量（临时性）

- 修改全局变量，但**重启mysql**后会失效

```mysql
mysql> set global validate_password_policy=0;
Query OK, 0 rows affected (0.00 sec)
```

- 在设置密码为 12345678，成功

```mysql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '12345678';
Query OK, 0 rows affected (0.00 sec)
```

- 查看密码策略

```mysql
mysql> SHOW VARIABLES LIKE 'validate_password%';
```

![image-20230225140956808](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9a634b156c.png)

#### 3.2.1 重启mysql后失效

- 重启mysql

```powershell
[root@localhost ~]# systemctl restart mysqld
```

- 查看密码策略，可见策略已恢复

```mysql
mysql> SHOW VARIABLES LIKE 'validate_password%';
```

![image-20230225141404988](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9a72cd566c.png)

### 3.3 更改策略：在my.cnf文件添加参数

> my.cnf文件在所在目录：/etc/my.cnf

- 在my.cnf文件添加参数

```mysql
[root@localhost ~]# vim /etc/my.cnf

...
...
validate_password_policy=0
```

![image-20230225142205144](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9a90d0ac8b.png)

- 重启mysql使参数生效

```powershell
[root@localhost ~]# systemctl restart mysqld
```

- 查看密码策略，可见策略已更改

```mysql
mysql> SHOW VARIABLES LIKE 'validate_password%';
```

![image-20230225142415507](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9a98f638d9.png)

### 3.4 禁用插件

- 在my.cnf文件添加参数，禁用validate_password密码校验插件

```powershell
[root@localhost ~]# vim /etc/my.cnf

...
...
validate-password=OFF
```

![image-20230225142700552](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9aa346586a.png)

- 重启mysql使参数生效

```powershell
[root@localhost ~]# systemctl restart mysqld
```

- 查看密码策略，可见策略已更改

```mysql
mysql> SHOW VARIABLES LIKE 'validate_password%';
```

![image-20230225142415507](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9a98f638d9.png)

### 3.5 删除插件

- 删除插件

```mysql
mysql> uninstall plugin validate_password;
```

![image-20230225143224812](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/02/25/63f9ab78ae762.png)