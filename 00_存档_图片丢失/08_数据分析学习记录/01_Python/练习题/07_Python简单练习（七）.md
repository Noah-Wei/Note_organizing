## 编程题

- **要求输入员工的薪资，若薪资小于 0 则重新输入。最后打印出录入员工的数量和薪资明细，以及平均薪资**


```python
wage_list = []
num = 0
while True:
    wage = input(f"请输入{num + 1}号员工的薪资（输入q停止录入）：")
    if wage == "q":
        print("录入结束，停止录入")
        break
    else:
        wage = float(wage)
        if wage < 0:
            print("输入错误，请重新输入")
            continue
        else:
            wage_list.append(wage)
            num += 1
avg = sum(wage_list) / len(wage_list)
print(f"员工数量为：{num}人，薪资总为：{wage_list},平均薪资为：{avg}")
print("薪资明细为：")
for i, j in enumerate(wage_list):
    print(f"{i + 1}号员工，工资为：{j}")
```

- **有一个棋盘，有64个方格，在第一个方格里面放1粒芝麻重量是0.00001kg，第二个里面放2粒，第三个里面放4，求棋盘上放的所有芝麻的重量**


```python
weight_list = []
num = 1
weight = 0
for i in range(64):
    num = 2 ** i
    weight = 0.00001 * num
    weight_list.append(weight)
print(weight_list)
total = sum(weight_list)
print(f"棋盘上放的所有芝麻的重量为：{total}kg")
```

- **假设某人有100,000现金.每经过一次路口需要进行一次交费. 交费规则为当他现金大于50,000时每次需要交5%如果现金小于等于50,000时每次交5,000.请写一程序计算此人可以经过多少次这个路口**


```python
money = 100000
count = 0
while True:
    if money > 50000:
        money = money - money * 0.05
        count += 1
    elif 5000 < money < 50000:
        money -= 5000
        count += 1
    else:
        break
print(f"此人可以经过{count}次这个路口")
```

- **有四个数字，1，2，3，4能组成多少个互不相同且无重复的三位数？各是多少？**

```python
count = 0

for i in (1, 2, 3, 4):
    for j in (1, 2, 3, 4):
        for k in (1, 2, 3, 4):
            # 去重
            if i != j and i != k and j != k:
                print(i, j, k)
                count += 1
print(f"1，2，3，4能组成{count}个互不相同且无重复的三位数")
```



