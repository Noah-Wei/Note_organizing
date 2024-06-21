# Python第六章：Python基本数据结构——字典

## 前言

> 字典是 Python 中最基本的数据结构。
>
> 字典是另一种可变容器模型，且可存储任意类型对象。
>
> 字典的每个键值 **key=>value** 对用冒号 **:** 分割，每个对之间用逗号(**,**)分割，整个字典包括在花括号 **{}** 中 。
>
> 字典作为 Python 的关键字和内置函数，变量名不建议命名为 **dict**。
>
> 键必须是唯一的，但值则不必。
>
> 值可以取任何数据类型，但键必须是不可变的，如字符串，数字。

## 一、字典的概述

- **什么是字典**
  - 字典是 Python 中最基本的**数据结构**之一，与列表一样是**可变**序列
  - 以**键值对**的方式存储数据，字典是一个**无序**的序列

- **字典的示意图**

![image-20220819185856654](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/08/19/62ff6cf92f875.png)

- **字典的实现原理**
  - 字典的实现原理与查字典类似，查字典是先根据部首或者拼音查找对应的页码，Python中的字典根据**Key**查找**Value**所在的位置


## 二、字典的创建

- 最常用的方式：使用花括号

  - 代码演示

  ```python
  """1、使用{}创建字典"""
  scores = {"张三": 100, "李四": 98, "王五": 88}
  print(scores)
  print(type(scores))
  ```

- 使用内置函数dict()

  - 代码演示

  ```python
  """2、使用内置函数dict()创建"""
  student = dict(name="tom", age=19)
  print(student)
  ```

- 创建一个空字典

  - 代码演示

  ```python
  """创建一个空字典"""
  dict3 = {}
  print(dict3)
  ```

## 三、字典的常用操作

### 3.1 字典中元素的获取

- 获取元素的方法

  | 获取字典元素的方法 | 举例              | 区别                                                         |
  | ------------------ | ----------------- | ------------------------------------------------------------ |
  | **[]**             | score["张三"]     | **[]**如果字典中**不存在指定的key**，**抛出KeyError异常**    |
  | **get()方法**      | score.get("张三") | **get()方法**取值，如果字典中**不存在指定的key**，并不会抛出KeyError而是**返回None**<br />可以通过**参数设置默认的Value**，以便指定的**key不存在时返回** |

  - 代码演示

  ```python
  """获取字典中的元素"""
  scores = {"张三": 100, "李四": 98, "王五": 88}
  """第一种方式，使用[]"""
  print(scores["张三"])
  
  """第二种方式，使用get()方法"""
  print(scores.get("张三"))
  
  """差异性"""
  # print(scores["阿萨德"])
  print(scores.get("阿萨德"))
  print(scores.get("阿达", 99))
  ```

### 3.2 字典中元素的判断

- 字典中key的判断

  | 方法/语句  | 说明                                |
  | ---------- | ----------------------------------- |
  | **in**     | 指定的Key在字典中**存在**返回True   |
  | **not in** | 指定的Key在字典中**不存在**返回True |

  - 代码演示

  ```python
  """key的判断"""
  scores = {"张三": 100, "李四": 98, "王五": 88}
  print("张三" in scores)
  print("张三" not in scores)
  
  print("阿达" in scores)
  print("阿达" not in scores)
  ```

### 3.3 字典中元素的删除、添加、修改

- 代码演示

```python
"""删除指定的key——value"""
del scores["张三"]
print(scores)
# 清空字典所有元素
scores.clear()
print(scores)

"""字典元素的新增操作"""
scores["柯蓝"] = 100
print(scores)

"""字典元素的修改操作"""
scores["柯蓝"] = 10
print(scores)
```

### 3.4 获取字典的视图

- 获取字典视图的三个方法

| 方法     | 说明                                |
| -------- | ----------------------------------- |
| key()    | 获取字典中所有的key                 |
| values() | 获取字典中所有的value               |
| items()  | 获取字典中所有的 key : vaule 键值对 |

- 代码演示

```python
scores = {"张三": 100, "李四": 98, "王五": 88}
"""获取所有的key"""
keys = scores.keys()
print(keys)
print(type(keys))
# 将所有的key组成的视图转换成列表
print(list(keys))

print("\n")
"""获取所有的value"""
values = scores.values()
print(values)
print(type(values))
print(list(values))

print("\n")
"""获取所有的key-value"""
items = scores.items()
print(items)
print(type(items))
print(list(items))  # 转换之后的列表元素有元组组成
```

### 3.5 字典元素的遍历

- 代码演示

```
scores = {"张三": 100, "李四": 98, "王五": 88}
"""字典元素的遍历"""
for item in scores:
    print(item)
    print(item, scores[item], scores.get(item))
```

## 四、字典的特点

- **字典中的所有元素都是key——value键值对，key不允许重复，但是value允许重复**
- **字典中的元素时无序的**
- **字典中的key必须是不可变对象**
- **字典也可以根据需要动态的伸缩**
- **字典会浪费较大的内存，是一种使用空间换时间的数据结构**

## 五、字典的生成式

字典生成式，简称“生成字典的公式“

- 调用内置函数——**zip()**
  - 用于将可迭代对象作为参数，将对象中的元素打包成一个元组，然后返回这些元组组成的列表

- 代码演示

```python
"""已知两个列表，生成一个字典
items = ['Fruits', 'Books', 'Others']
prices = [96, 78, 85,100,200]
"""
items = ['Fruits', 'Books', 'Others']
prices = [96, 78, 85,100,45566]

d = {item.upper(): price for item, price in zip(items, prices)}
print(d)
# 如果用于生成的两个列表中的参数个数不相等，取较小的一个
```

