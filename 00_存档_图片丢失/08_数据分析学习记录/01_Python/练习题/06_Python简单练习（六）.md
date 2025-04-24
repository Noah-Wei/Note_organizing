## 编程题（一）

- **求1-2+3-4………+97-98+99-100的结果**


```python
sum_ji = 0
sum_ou = 0
for i in range(1, 101):
    if i % 2 == 0:
        sum_ou += i
    else:
        sum_ji += i
total = sum_ji - sum_ou
print(f"1-2+3-4………+97-98+99-100的结果为：{total}")
```

- **求15的阶乘**


```python
total = 1
for i in range(1, 16):
    total *= i
print(f"15的阶乘为：{total}")
```

- **一张纸的厚度大约是0.08mm，对折多少次之后能达到珠穆朗玛峰的高度（8848.13⽶米）**


```python
paper_length = 0.08
mount_length = 8848.13 * 1000
count = 0
while True:
    paper_length *= 2
    count += 1
    if paper_length >= mount_length:
        break
    
print(f"对折{count}次之后能达到珠穆朗玛峰的高度")
```

- **统计101~200中质数的个数，并且输出所有的质数**


```python
count = 0
for num in range(101, 201):
    for i in range(2, num):
        if num % i == 0:
            break
    else:
        count += 1
        print(f"{num}是质数")
print(f"统计101~200中质数的个数为：{count}")
```

- **输出9行内容:第1行输出1，第2行输出12，第3行输出123，以此类推，第9行输出123456789**


```python
message = ""
for i in range(1, 10):
    i = str(i)
    message += i
    print(message)
```

- **编写一个程序：可以不断的输⼊数字，直到输入的数字是0时打印 over 后结束程序**

```python
while True:
    num = input("请输入一个数字：")
    if num == "0":
        print(f"over")
        break
```

## 编程题（二）

- **求1/1! + 1/2! + 1/3! + ..... 1/20!的结果**


```python
total = 0
num_down = 1
for i in range(1, 21):
    num_down *= i
    total += 1 / num_down
print(total)
```

- **模拟用户的登录过程，让用户输入自己的用户名和密码，如果用户名为admin，密码为abc123,则表示登录成功，允许错误三次，如果三次输入错误，则禁止登录**

```python
for i in range(3):
    name = input("请输入用户名：")
    pwd = input("请输入密码：")
    if name == "admin" and pwd == "abc123":
        print("登录成功")
        break
    else:
        if i == 2:
            continue
        print("账号或密码错误，请重新输入")
else:
    print("已输入错误三次，禁止登录")
```



