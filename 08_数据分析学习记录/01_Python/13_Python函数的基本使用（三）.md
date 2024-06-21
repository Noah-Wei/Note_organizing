# Python函数的基本使用（三）

## 一、高阶函数

函数的本质：函数是一个变量，函数名是一个变量名，一个函数可以作为另一个函数的参数或返回值使用

如果A函数作为B函数的参数，B函数调用完成之后，会得到一个结果，则B函数被称为高阶函数

常用的高阶函数：`map()`，`reduce()`，`filter()`，`sorted()`

一般高阶函数的使用配合lambda匿名函数使用

### 1.1 高阶函数之map

#### 1.1.1 map说明

- **语法**

```python
map(func,iterable.....)
```

- **说明**
  - 返回值是一个iterator【容器，迭代器】
  - func：函数
  - iterable：可迭代对象【容器】，可以是多个，常用列表
- **功能**
  - 将iterable容器中的每一个元素传递给func,func返回一个结果，结果会成为iterator中的元素
- **流程**
  - 原容器————>func————>新的容器

#### 1.1.2 代码演示

需求生成一个容器，其中的元素是1,4,9,16,25

- **方式一：使用列表生成式**

```python
list1 = [n ** 2 for n in range(1, 6)]
print(list1)
```

但是，这种方式，会在内存中分配地址，如果不使用，会造成资源的浪费

- **（优化）方式二：使用生成器表达式**

```python
r1 = (n ** 2 for n in range(1, 6))
print(r1)  # <generator object <genexpr> at 0x000001C2AD1EF1D0>
print(list(r1))
```

不会全部将数据一次加载到内存中

- **（优化）方式三：使用map函数**

```python
def func1(x):
    return x ** 2


r11 = map(func1, range(1, 6))
print(r11)
print(list(r11))
```

对于这种函数中只有一行表达式的，还可以使用匿名函数

- **（优化）方式三：使用map函数和lambda匿名函数**

```python
r12 = map(lambda x: x ** 2, range(1, 6))
print(list(r12))
```

**注**：**map中的func函数需要设置几个参数，取决于有几个iterable参与运算**

​	**工作原理：将多个iterable相同位置的元素同时传参给func**

```python
def func2(a, b):
    return a + b


r21 = map(func2, [1, 5, 2], [3, 1, 8])
print(list(r21))

r22 = map(lambda a, b: a + b, [1, 5, 2], [3, 1, 8])
print(list(r22))
```

**如果多个iterable中的元素不相等，按最小数量的iterable数量算**

```python
r4 = map(lambda x, y: x + y, range(1, 9), (1, 2, 3, 4, 5, 6, 7))
print(r4)
print(list(r4))  # [2, 4, 6, 8, 10, 12, 14]
```

#### 1.1.3 总结

- map语法：

  ```python
  map(func,iterable.....)
  ```

  - 其中，iterable部分可以是多个容器，当iterable部分为多个容器时，func的形参数应和iterable数量相等
  - 返回值是一个iterator【容器，迭代器】

### 1.2 高阶函数之reduce

#### 1.2.1 reduce说明

- **语法**

```python
import functools
functools.reduce(func, seq)
```

- **说明**
  - func：函数
  - seq：有序序列【容器】
  - 结果是一个值
- **功能**
  - reduce：减少
  - 首先将seq中的第0个元素和第1个元素传递给func,进行运算，返回结果1
    接着，将 结果1 和第2个元素传递给func,进行运算，返回结果2
    接着，将 结果2 和第3个元素传递给func,进行运算，返回结果3
    ....
    直到所有的元素全部参与运算，表示运算结束

#### 1.2.2 代码演示

**需求：求1~100之间所有整数的和**

- **方式一：使用for/while循环**
- **方式二：使用sum()**
- **(高级)方式三：使用reduce()**

```python
def func1(a, b):
    return a + b


s3 = functools.reduce(func1, range(1, 101))
print(s3)
```

- **使用匿名函数优化**

```python
s4 = functools.reduce(lambda x, y: x + y, range(101))
print(s4)
```

**需求：求15的阶乘**

```python
s5 = functools.reduce(lambda x, y: x * y, range(1, 16))
print(s5)
```

### 1.3 高阶函数之filter

#### 1.3.1 filter说明

- **语法**

```python
filter(func,iterable)
```

- **说明**
  - func：函数
  - iterable：可迭代对象【容器】
  - 返回值是一个iterator【容器，迭代器】
- **功能**
  - filter：过滤
  - 将iterable中的元素依次传递给func，根据func的返回值决定是否保留该元素，
    如果func的返回值为True，则表示当前元素需要保留，如果为False，则表示过滤

#### 1.3.2 代码演示

**需求：已知一个列表, 挑出其中的偶数**

```python
def func1(x):
    if x % 2 == 0:
        return True


list1 = [34, 56, 23, 34, 7, 8, 19, 45, 7, 9, 10, 46, 7997]
r2 = filter(func1, list1)
print(list(r2))
```

- **使用匿名函数优化**

```python
r3 = filter(lambda x: True if x % 2 == 0 else False, list1)
print(list(r3))
```

- **对其中的三目运算符再优化**

```python
r4 = filter(lambda x: x % 2 == 0, list1)
print(list(r4))
```

### 1.4 高阶函数之sorted

#### 1.4.1 列表中的sort函数和高阶函数sorted的区别和联系

- 调用语法

  - 列表：

  ```python
  列表.sort(reverse,key=func)
  ```

  - sorted（）

  ```python
  sorted(iterable,reverse,key=func)
  ```

- 输出结果

  - sort是在原列表内部排序的
  - sorted是生成了一个新的列表

- 排列顺序

  - 二者默认情况下都是升序排序,如果要降序，则都是设置reverse=True

- 自定义排序规则，都是设置key=func

#### 1.4.2 代码演示

**需求1：已知一个列表，对列表进行排序**

- sort（）方法，原列表中的数据已发生更改

```python
list1 = [34, 67, 324, 34, 3, 6, 89]

print(list1)  # [34, 67, 324, 34, 3, 6, 89]

list1.sort()  # 升序排列
print(list1)  # [3, 6, 34, 34, 67, 89, 324]

list1.sort(reverse=True)  # 降序排列
print(list1)
```

- sorted函数，产生一个新的列表，原列表元素不变

```python
list1 = [34, 67, 324, 34, 3, 6, 89]
print(list1)  # [34, 67, 324, 34, 3, 6, 89]

list2 = sorted(list1)
print(list1)  # [34, 67, 324, 34, 3, 6, 89]
print(list2)  # [3, 6, 34, 34, 67, 89, 324]

list3 = sorted(list1, reverse=True)  # 降序排列
print(list1)  # [34, 67, 324, 34, 3, 6, 89]
print(list3)
```

**需求2：对字典中的元素，按照成绩来排序**

```python
students = [
    {'name': '小花', 'age': 19, 'score': 90, 'gender': '女', 'tel':
        '15300022839'},
    {'name': '明明', 'age': 20, 'score': 40, 'gender': '男', 'tel':
        '15300022838'},
    {'name': '华仔', 'age': 18, 'score': 90, 'gender': '女', 'tel':
        '15300022839'},
    {'name': '静静', 'age': 16, 'score': 100, 'gender': '不明', 'tel':
        '15300022428'},
    {'name': 'Tom', 'age': 17, 'score': 59, 'gender': '不明', 'tel':
        '15300022839'},
    {'name': 'Bob', 'age': 18, 'score': 98, 'gender': '男', 'tel':
        '15300022839'}
]
```

- sort()方法

```python
students.sort(key=lambda x: x["score"], reverse=True)
print(students)
```

- sorted()方法

```python
students_2 = sorted(students, key=lambda x: x["score"], reverse=True)
print(students_2)
```

## 二、函数递归

- 函数递归：函数调用自身

- 处理递归的关键：
  - 需要找到一个临界值【让程序停止下来的条件】
  - 函数相邻两次调用之间的关系
- 递归的优缺点
  - 优点：占用内存多，效率低下
  - 缺点：思路和代码简单
- 斐波那契数列

```python
1   1   2   3   5   8  13   21  34  55......
```

**案例1：斐波那契数列，求第七位上的数字**

```python
def fun(n):
    if n == 1 or n == 2:
        return 1
    else:
        return fun(n - 1) + fun(n - 2)


a = fun(7)
print(a)
```

**案例2：求1~n之间所有整数的和**

```python
def total(n):
    if n == 1:
        return 1
    else:
        return total(n - 1) + n


print(total(100))
```

