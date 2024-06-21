# Python中random模块常用函数介绍

## 一、random模块简介

​	Python标准库中的random函数可以生成随机浮点数、整数、字符串，甚至帮助你随机选择列表序列中的一个元素，打乱一组数据等。

## 二、常用函数

- **random.random()**
  - 用于随机生成一个`0`到`1`的随机**符点数**: `0 <= n < 1.0`，左闭右闭
- **random.uniform()**
  - 函数原型：`random.uniform(a, b)`
  - 随机生成指定区间内的**浮点数**，左闭右闭
  - 如果`a < b`，范围：`a <= n <= b`
  - 如果`a > b`，范围：`b <= n <= a`
  - 如果`a = b`，范围：`b = n = a`，`n`是浮点数
- **random.randint()**
  - 函数原型为：`random.randint(a, b)`
  - 随机生成指定区间内的一个**整数**，左闭右闭
  - `a、b`取值范围必需是：`a <=  b`，结果范围：`a <= n <= b`
- **random.choice()**
  - 函数原型为：`random.choice(sequence)`
  - 从序列中获取一个随机元素
  - `sequence`：有序序列（容器），如`list`，`str`，`tulpe`，`range()`等
- **random.choices()**
  - 函数原型为：`random.choices(population, weights=None, *, cum_weights=None, k=1)`
  - 从序列中获取指定数量随机元素

  - `population` 是必填参数，必须是一个序列，可以是列表，元组，字符串等等。表示要从中选取元素的序列。
  - `weights` 是可选参数，必须是一个数字序列，长度必须和`population`相同。表示每个元素被选中的概率，可以是小数，但必须大于等于0。不填跟 `random.choice()` 没区别。
  - `cum_weights` 是可选参数，必须是一个数字序列，长度必须和`population相同`。表示累计权重，即前n个元素被选中的概率，如果指定了`cum_weights`，那么不能同时指定`weights`。
  - `k` 是可选参数，用于定义返回列表的长度，默认 `1` 个。
- **random.randrange()**
  - 函数原型为：`random.randrange( start, stop=None, step=_ONE)`
  - 返回指定范围内的一个整数。
  - `start` ：可选参数， 一个整数，指定开始值，默认值为 0。
  - `stop`： 必填参数， 一个整数，指定结束值。
  - `step`：可选参数， 一个整数，指定步长，默认值为 1。
- **random.shuffle()**
  - 函数原型为：`random.shuffle(x[, random])`，
  - 将一个列表中的元素打乱，不会生成新的对象
- **random.sample()**
  - 函数原型为：`random.sample(sequence, k)`
  - 从指定序列中随机获取指定长度的片断，不会修改原有序列。

## 三、代码演示

### 3.1 random.random()

```python
import random
#用于随机生成一个 0 到 1 的随机符点数: 0 <= n < 1.0，左闭右闭
a = random.random()
print(a)  # 0.4715778796909328
```

### 3.2 random.uniform()

```python
import random

# 随机生成指定区间内的浮点数，左闭右闭
b = random.uniform(3, 6.3)
print(b)  # 3.8102949650595095

c1 = random.uniform(3, 3)
print(c1)  # 3.0

c2 = random.uniform(3, 4.5)
print(c2)  # 4.195489977282959

c3 = random.uniform(7.4, 3)
print(c3)  # 3.850107114192627
```

###  3.3 random.randint()

```python
import random

result = random.randint(1,10)      #返回 [1, 10] 之间的任意整数
print(result)
```

### 3.4 random.choice()

```python
import random

# 从序列中获取一个随机元素
d1 = random.choice("hello Python")
print(d1)  # e

d2 = random.choice([1, 2, 5, 6, "hello", "Python"])
print(d2)  # Python

d3 = random.choice((2, 435.3, "hello", "go"))
print(d3)  # go

d4 = random.choice(range(10))
print(d4)  # 1
```

### 3.5 random.choices()

```python
import random

# 从序列中获取指定数量随机元素
list1 = [1, 2, 5, 6, "hello", "Python", "go", 1.2]
e1 = random.choices(list1)
print(e1)  # ['hello']

e2 = random.choices(list1, k=2)
print(e2)  # [1.2, 6]

e3 = random.choices(list1, weights=[10, 1, 1,  1, 10, 1, 1], k=3)
print(e3)  # [1, 1, 'Python']
```

### 3.6 random.randrange()

```python
import random

# 返回指定范围内的一个整数。
f1 = random.randrange(1, 9)
print(f1)
```

### 3.7 random.shuffle()

```python
import random

# 将一个列表中的元素打乱
list1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
random.shuffle(list1)
print(list1) # [3, 9, 5, 6, 7, 4, 8, 2, 1, 10]
```

### 3.8 random.sample()

```python
import random

list1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
new_list = random.sample(list1, 5)  # 从 list 中随机获取 5 个元素，作为一个片断返回
print(new_list)  # [10, 8, 4, 7, 3]
print(list1)  # 原有序列不会改变,[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

