## 编程题

- **从控制台输入一个数，判断一个数是否能被3整除**

```python
num = input("请输入一个数：")
if num.isdigit():
    num = int(num)
    if num % 3 == 0:
        print(f"该数{num}可以被3整除")
    else:
        print(f"该数{num}不可以被3整除")
else:
    print("输出有误")

```

- **从控制台输入一个数，判断一个数是否能同时被5和8整除**

```python
num = input("请输入一个数：")
if num.isdigit():
    num = int(num)
    result = True if num % 5 == 0 and num % 8 == 0 else False
    print(result)
else:
    print("输出有误")
```

- **模拟玩骰子游戏，根据骰子点数决定什么惩罚【例如：1.跳舞，2.唱歌....】**

```python
import random

num = random.randint(1, 8)
if num == 1:
    print(f"点数为{num},惩罚为——跳舞")
elif num == 2:
    print(f"点数为{num},惩罚为——唱歌")
elif num == 3:
    print(f"点数为{num},惩罚为——绘画")
elif num == 4:
    print(f"点数为{num},惩罚为——写作")
elif num == 5:
    print(f"点数为{num},惩罚为——表演")
else:
    print(f"点数为{num},惩罚为——速写")
```

- **从控制台输入一个年份，判断该年是否是闰年，是闰年的条件: 能被4整除但是不能被100整除或者能够被400整除的年**

```python
num = input("请输入一个年份：")
if num.isdigit() and len(num) > 3:
    num = int(num)
    result = True if (num % 4 == 0 and num % 100 != 0) or (num % 400 == 0) else False
    print(result)
else:
    print("输出有误")
```

- **定义两个变量保存一个人的身高和体重，编程实现判断这个人的身材是否正常!**

   **公式: `体重(kg)/身高(m)的平方值` 在18.5 ~ 24.9之间属于正常。**

```python
height, weight = eval(input("请输入您的身高(kg)和体重(m),并用英文逗号隔开:"))
print(f"您的身高是：{height}m，体重是{weight}kg")
result = weight / height ** 2
if result < 18.5:
    print("偏瘦")
elif result > 24.9:
    print("偏胖")
else:
    print("正常")

```

- **x 为 0-99 取一个数,y 为 0-199 取一个数,如果 x>y 则输出 x， 如果 x 等于 y 则输出 x+y，否则输出y**

```python
import random

x = random.randint(0, 101)
y = random.choice(range(200))
print(f"产生的x为：{x}，y为：{y}")
if x > y:
    print(f"x > y，结果为x:{x}")
elif x == y:
    print(f"x == y，结果为x+y：{x + y}")
else:
    print(f"都不满足，结果为y:{y}")
```

- **从控制台输入三个数，输出较大的值**

```python
num1, num2, num3 = eval(input("请输入三个数，并用英文逗号隔开："))
print(f"num1:{num1},num2:{num2},num3:{num3}")
if num1 > num2:
    if num1 > num3:
        print(f"最大的数是：{num1}")
    else:
        print(f"最大的数是：{num3}")
elif num2 > num3:
    print(f"最大的数是：{num2}")
else:
    print(f"最大的数是：{num3}")
```

- **从控制台输入一个三位数，如果是水仙花数就打印“是水仙花数”，否则打印“不是水仙花数”，例如：153 = 1的三次方 + 5的三次方 + 3的三次方，则153是水仙花数**

```python
num = input("请输入三位数：")
if num.isdigit() and len(num) == 3:
    num = int(num)
    bai = num // 100
    shi = (num % 100) // 10
    ge = num % 10
    if num == bai ** 3 + shi ** 3 + ge ** 3:
        print(f"该数{num}是水仙花数")
    else:
        print(f"该数{num}不是水仙花数")
else:
    print("输出有误")
```

- **从控制台输入一个五位数，如果是回文数就打印“是回文数”，否则打印“不是回文数”，例如：11111   12321   12221**

```python
num = input("请输入五位数：")
if num.isdigit() and len(num) == 5:
    num = int(num)
    wan = num // 10000
    qian = (num % 10000) // 1000
    bai = (num // 100) % 10
    shi = (num % 100) // 10
    ge = num % 10
    if wan == ge and qian == shi:
        print(f"该数{num}是回文数")
    else:
        print(f"该数{num}不是回文数")
else:
    print("输出有误")
```
