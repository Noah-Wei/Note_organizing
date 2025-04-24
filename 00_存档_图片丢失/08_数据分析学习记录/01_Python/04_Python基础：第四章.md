# Python第四章：Python中程序的组织结构

## 一、程序的组织结构

> 1996年，计算机科学家证明了这样的事实，任何简单或复杂的算法都可以有**顺序结构**、**选择结构**和**循环结构**，这三种基本结构组合而成

![1-4-1 程序的组织结构](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/27/62b9a1291dcc0.png)

## 二、顺序结构

- **什么是顺序结构**
  - 程序从上到下顺序地执行代码，中间没有任何的判断和跳转，直到程序的结束
- **顺序结构流程图**

![1-4-2 顺序结构](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/27/62b9a20898d7f.png)

- **代码演示**

  - 代码编写

  ```python
  """把大象当冰箱一共需要几步"""
  print('---------程序开始------------')
  print('1.把冰箱打开')
  print('2.把大象放进去')
  print('3.关闭冰箱门')
  print('---------程序结束------------')
  ```

  - 结果分析

  ```python
  ---------程序开始------------
  1.把冰箱打开
  2.把大象放进去
  3.关闭冰箱门
  ---------程序结束------------
  ```

## 三、对象的布尔值

> **Python中一切皆对象**，所有对象都有一个布尔值

- **获取对象的布尔值**
  
  - 使用内置函数**bool()**
- 以下对象的布尔值为**False**,其他对象的布尔值为True
  - **False**
  - **数值0**
  - **None**
  - **空字符串**
  - **空列表**
  - **空元组**
  - **空字典**
  - **空集合**

- 代码演示

  - 代码编写

  ```python
  # 测试对象的bool
  print("以下对象的布尔值为False")
  print(bool(False))  # False
  print(bool(0))  # False
  print(bool(0.0))  # False
  print(bool(None))  # False
  print(bool(""))  # False   空字符串
  print(bool([]))  # False   空列表
  print(bool(list()))  # False   空列表
  print(bool(()))  # False   空元组
  print(bool(tuple()))  # False  空元组
  print(bool({}))  # False   空字典
  print(bool(dict()))  # False   空字典
  print(bool(set()))  # False 空集合
  print("其他对象的布尔值为True")
  ```

## 四、分支结构（选择结构）

- 什么是选择结构
  - 程序根据判断条件选择性的执行部分代码，**明确的让计算机知道在什么条件下，该去做什么**

### 4.1 单分支if结构

- 中文含义
  - 如果......就......
    - 如果下雨，就带伞
- 语法结构

```python
if 条件表达式:
	条件执行体
```

- 流程图

![1-4-3 单分支结构流程图](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/27/62b9a865732a3.png)

- 代码演示：取款操作

  - 代码编写

  ```python
  """取款操作"""
  money = 1000  # 余额
  s = int(input("请输入取款金额:"))  # 取款金额
  # 进行判断，余额是否充足
  if money >= s:
      money = money - s
      print("取款成功，余额为：", money)
  ```

### 4.2 双分支if...else结构

- 中文含义
  - 如果......不满足......就......
    - 如果下雨，就带伞，没下雨，就不带伞
- 语法结构

```python
if 条件表达式:
	条件执行体1
else
	条件执行体2
```

- 流程图

![1-4-4 双分支结构流程图](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/27/62b9ab3b7cf73.png)

- 代码演示：取款操作
  - 代码编写
  
  ```python
  "从键盘录入一个数，编写程序需让计算机判断是奇数还是偶数"
  num = int(input("请输入一个整数："))
  
  # 条件判断
  if num % 2 == 0:
      print(num, "为偶数")
  else:
      print(num, "为奇数")
  ```

### 4.3 多分支if...elif...else结构

- 语法结构

```
if 条件表达式1：
	条件执行体1
elif 条件表达式2：
	条件执行体2
	....
elif 条件表示式N:
	条件执行体N
[else:]
	条件执行体N+1
```

- 流程图

![1-4-5 多分支结构流程图](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/07/03/62c1051f5b81d.png)

- 代码演示

```python
"""从键盘录入一个整数升级，并判断
90-100  A
80-89   B
70-79   C
60-69   D
0-59    E
小于0或者大于100的数位非法数据（不是成绩的有效范围）"""
score = int(input("请输入一个成绩:"))
if 90 <= score <= 100:
    print("A")
elif 80 <= score <= 89:
    print("B")
elif 70 <= score <= 79:
    print("C")
elif 60 <= score <= 69:
    print("D")
elif 0 <= score <= 59:
    print("E")
else:
    print("非法数据")
```

### 4.4 if语句的嵌套 

- 语法结构

```
if 条件表达式1:
	if 内层条件表达式:
		内层条件执行体1
	else 内层条件执行体2：
else:
	条件执行体
```

- 流程图

![1-4-6 嵌套if构流程图](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/07/03/62c109471d4c7.png)

- 代码演示

```python
answer = input("您是会员吗？Y或N")
money = float(input("请输入你的购物金额："))
if answer == "Y":
    print("会员")
    if money >= 200:
        print("付款金额为：",money*0.8)
    elif money>=100:
        print("付款金额为：", money * 0.9)
    else:
        print("不打折，付款余额为：、")
else:
    print("非会员")
```

### 4.5 条件表达式

- 本质：if...else...的简写
- 语法结构

```python
x	if	判断条件	else	y	
```

- 运算规则
  - 如果判断条件的布尔值为**True**，条件表达式的返回值为**x**，**否则**条件表达式的返回值为**False**
- 代码演示

```python
"""从键盘录入两个整数，并判断大小"""
num_a = int(input("请输入第一个整数："))
num_b = int(input("请输入第二个整数："))
# 比较大小
"""if num_a >= num_b:
    print(num_a, "大于等于", num_b)
else:
    print(num_a, "小于", num_b)"""

print("使用条件表达式进入比较")
print(str(num_a) + "大于等于" + str(num_b) if num_a >= num_b else str(num_a) + "小于" + str(num_b))
```

### 4.6 pass语句

- 什么是pass语句
  - 语句什么都不做，**只是一个占位符**，用在语法上需要语句的地方
- 什么时候用
  - 先搭建语法结构，还没想好代码怎么写的时候
- 可以和哪些语句一起使用
  - if语句的条件执行体
  - for-in语句的循环体
  - 定义函数时的函数体

- 代码演示

```python
answer = input("您是会员吗？：y/n")
if answer == "y":
    pass
else:
    pass
# 代码不会报错
```

## 五、循环结构

### 5.1 range()函数的使用

- 用于生成一个**整数**序列

- 创建range()对象的**三种方式**
  
  参数为<font color="red">**左闭右开**</font>
  
  - **range(stop)**：创建一个[0,stop]之间的整数序列，步长为1
  - **range(start,stop)**：创建一个[start,stop]之间的整数序列，步长为1
  - **range(start,stop,step)**：创建一个[start,stop,stop]之间的整数序列，步长为step
  
- range函数的返回值是一个**迭代器对象**

- range类型的优点：

  - 不管range对象表示的整数序列有多长，所有range对象占有的内存空间都是**相同的**
    因为**仅仅需要存储start,stop和step**，**只有当用到range对象**时，**才会去计算序列中的相关元素**

- **in**与**not in** 判断整数序列中是否**存在（不存在）**指定的整数

- 代码演示

```python
# range创建的三种方式
"""第一种创建方式，只有一个参数"""
r = range(10)  # 默认从零开始，步长默认为1
print(r)
print(list(r))
"""第二种方式，给了两个参数"""
r2 = range(1, 10)
print(list(r2))

"""第三种方式，给了三个参数"""
r3 = range(1, 10, 2)
print(list(r3))

"""in 与 not in
判断指定的整数在不在序列中"""
print(10 in r3)
print(10 not in r3)
```

### 5.2  while循环

- 语法结构

```python
while	条件表达式:
	条件执行体（循环体）
```

- 流程图

![1-4-7 while循环流程图](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/07/11/62cc13687db9d.png)

- 选择结构的if和循环结构的while的**区别**
  - **if是判断一次**，条件为True执行一次
  - **while是判断N+1次**，条件为True执行N次
- while循环的执行流程（四步循环法）
  **初始化的变量**与**条件判断的变量**与**改变的变量**为同一个
  - **初始化变量**
  - **条件判断**
  - **条件执行体（循环体）**
  - **改变变量**
- 代码演示

```python
"""计算0到4的累加和"""
# 初始化变量
a = 0
sum = 0
# 条件判断
while a <= 4:
    # 条件执行体
    sum += a
    # 改变变量
    a += 1
print("和为：", sum)
```

- 练习题：计算1到100之间的偶数和

```python
"""代码1"""
# 初始化变量
a = 1
sum = 0
# 条件判断
while a <= 100:
    # 条件执行体
    if not bool(a % 2) :
        sum += a
    # 改变变量
    a += 1
print("1到100之间的偶数和为：", sum)

"""代码2"""
# 初始化变量
a = 1
sum = 0
# 条件判断
while a <= 100:
    # 条件执行体
    if a % 2 == 0:
        sum += a
    # 改变变量
    a += 1
print("1到100之间的偶数和为：", sum)
```

### 5.3 for-in循环

- for-on循环概念
  - in 表达从（字符串、序列）中依次取值，又称为遍历
  - for-in遍历的对象必须是可迭代对象
- 语法结构

```python
for 自定义的变量 in 可迭代对象:
	循环体
```

- 流程图

![1-4-8 for-in循环流程图](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/07/11/62cc1b4e63a10.png)

- **循环体内不需要访问自定义变量，可以将自定义变量替代为下划线**
- 代码演示

```python
for item in "Python":
    print(item)

for i in range(0, 10):
    print(i)

# 如果在循环体中不需要使用到自定义变量，可将自定义变量写为_
for _ in range(5):
    print("人生苦短，我用Python")  # Python循环5次

"""使用for in 计算1到100之间的偶数和"""
sum = 0
for item in range(1, 101):
    if item % 2 == 0:
        sum += item
print("1到100的偶数和为：", sum)
```

- 练习：输出1到999之间的水仙花数

```python
"""输出100到999之间的水仙花数
水仙花数，举例
153=3*3*3+5*5*5+1*1*1"""
for item in range(100, 1000):
    ge = item % 10
    shi = item // 10 % 10
    bai = item // 100
    if item == ge ** 3 + bai ** 3 + shi ** 3:
        print(item)
```

### 5.4 break、continue、与else语句

#### 5.4.1 break语句

- **用于结束循环结构**，通常与分支结构if一起使用、
- 代码演示

```python
"""从键盘录入密码，最多录入三次，如果正确就结束循环"""
for item in range(1, 4):
    pwd = input("请输入密码：")
    if pwd == "8888":
        break
    else:
        print("密码不正确")
```

#### 5.4.2 continue语句

- **用于结束当前循环**，进入下一次循环，通常与分支结构中的if一起使用
- 代码演示

```python
"""输出1到50之前所有的倍数"""
for item in range(1, 51):
    if item % 5 != 0:
        continue
    print(item)
```

#### 5.4.3 else语句

```python
for item in range(3):
    pwd = input("请输入密码：")
    if pwd == "9999":
        print("密码正确")
    else:
        print("密码不正确")
else:
    print("对不起，三次机会已经用完")
```

### 5.5 嵌套循环 

- 嵌套循环
  - 循环结构中又嵌套了另外的完整的循环结构，其中内层循环作为外层循环的循环体执行
- 代码演示

```python
"""输出一个三行四列的矩形"""
for i in range(1,4):
    for j in range(1,5):
        print("*",end="\t")
    print()
    
"""输出一个9行的直角三角形"""
for i in range(1,10):
    for j in range(1,i):
        print("*",end="\t")
    print()
```

- 二重循环中的break和continue

```python
for i in range(1, 5):
    for j in range(1, 11):
        if j % 2 == 0:
            continue
        print(j,end="\t")
    print()
```

