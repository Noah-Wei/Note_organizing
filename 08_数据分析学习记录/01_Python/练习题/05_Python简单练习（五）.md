## 编程题（一）

- **计算从1到100以内所有奇数的和并输出**

```python
total = 0
for i in range(1, 101, 2):
    total += i
print(f"1到100以内所有奇数的和为：{total}")
```

- **统计1到100之间可以被7整除的数的个数**

```python
count = 0
for i in range(1, 101):
    if i % 7 == 0:
        count += 1
print(f"1到100之间可以被7整除的数的个数为：{count}")
```

- **计算从1到100以内所有能被3或者17整除的数的和并输出**

```python
total = 0
n = 1
while n < 101:
    if n % 3 == 0 or n % 17 == 0:
        total += n
    n += 1
print(f"从1到100以内所有能被3或者17整除的数的和为：{total}")
```

- **计算1到100以内能被7或者3整除但不能同时被这两者整除的数的个数**


```python
count = 0
n = 1
while n < 101:
    if n % 3 == 0 or n % 7 == 0:
        count += 1
        if n % 3 == 0 and n % 7 == 0:
            count -= 1
    n += 1
print(f"从1到100以内所有能被3或者7整除的数的个数为：{count}")
```

- **计算1到500以内能被7整除但不是偶数的数的个数**


```python
count = 0
n = 1
while n <= 500:
    if n % 7 == 0 and n % 2 != 0:
        count += 1
    n += 1
print(f"从1到100以内所有能被3或者7整除的数的个数为：{count}")
```

- **计算从1到1000以内所有能同时被3，5和7整除的数的和并输出**


```python
total = 0
for i in range(1001):
    if i % 5 == 0 and i % 3 == 0 and i % 7 == 0:
        total += i
print(f"从1到1000以内所有能同时被3，5和7整除的数的和为：{total}")
```

- **统计100以内个位数是2并且能够被3整除的数的个数**

```python
count = 0
for i in range(100):
    if i % 10 == 2 and i % 3 == 0:
        count += 1
print(f"100以内个位数是2并且能够被3整除的数的个数为：{count}")
```

## 编程题（二）

- **输入任意一个正整数，求他是几位数？注意: 不能使用字符串，只能用循环**


```python
num = input("请输入一个正整数：")
length = 1
if num.isdigit():
    new_num = int(num)
    while new_num // 10 > 0:
        length += 1
        new_num //= 10
    print(f"输入的数为：{num},为{length}位数")
else:
    print("输入错误")
```

- **3000米长的绳子，每天减一半。问多少天这个绳子会小于5米？不考虑小数**


```python
length = 3000
time = 0
while length > 5:
    length /= 2
    time += 1
print(f"第{time}天这个绳子会小于5米")
```

- **打印出所有的水仙花数,所谓水仙花数是指一个三位数，其各位数字⽴方和等于该数本身。例如:153是 ⼀个⽔仙花数,因为  `1³ + 5³ + 3³ ` 等于 153**

```python
for i in range(100, 1000):
    bai = i // 100
    shi = (i % 100) // 10
    ge = i % 10
    if i == bai ** 3 + shi ** 3 + ge ** 3:
        print(i)
```

