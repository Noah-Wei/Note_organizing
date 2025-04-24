# Python第二章：Python中变量和数据类型

## 一、二进制与字符编码

- **bit存储内容是0和1；bit是计算机中最小的储存单位**
- **数据：每一个符号（英文、数字或符号等）都会占用1Bytes的记录，每一个中文占2Byte**
- 二进制换算： **1B(Byte)=8b(bit)**；**1KB=1024B**

- [在线ASCII 表]([ASCII 表 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/ascii.html))

- 字符编码发展

![字符编码发展](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/10/62a33f6e94b08.png)

## 二、Python中的标识符与保留字

### 2.1 保留字

- 什么是保留字
  - 有一些单词被Python赋予了特定的含义，这些单词在给任何对象起名字时都**不可以使用**
- 查看保留字

```py
import keyword
print(keyword.kwlist)
```

- 有哪些保留字

```python
['False', 'None', 'True', '__peg_parser__', 'and', 'as', 
 'assert', 'async', 'await', 'break', 'class', 'continue', 
 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 
 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 
 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 
 'try', 'while', 'with', 'yield']
```

### 2.2 标识符

- 什么是标识符
  - 变量、函数、类、模块和其他对象的起的名字就要标识符
- 标识符的命名规则
  - 只包含字母、数字、下划线_
  - 不能以数字开头
  - 不能是保留字
  - 严格区分大小写

## 三、Python中的变量与数据类型

### 3.1 变量

- 什么是变量

  - 变量是内存中一个带有标签的盒子，你把需要的数据放进去

    - ```python
      name = "Python"
      ```

    - 其中：**name**——变量名；**=**——**赋值运算符**；**Python**——值

- 变量的组成部分

  - 标识：标识对象所存储的内存地址，使用内置函数id(obj)来获取
  - 类型：表示的是对象的数据类型，使用内置函数type(obj)来获取
  - 值：表示对象所存储的具体数据，使用函数print(obj)可以将值进行打印输出

- 内存分析图

  ![变量内存分析图](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/10/62a34714e7749.png)

- 代码分析

  - 代码编写

  ```python
  name = '巧克力酸奶'
  print('标识', id(name))
  print('类型', type(name))
  print('值', name)
  ```
  
  - 输出结果
  
  ```python
  标识 2830801861584
  类型 <class 'str'>
  值 巧克力酸奶
  ```
  
### 3.2 变量的多次赋值

- 当多次赋值之后，变量会指向**新的空间**
- 内存示意图

![变量的多次赋值](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/22/62b316e9865e8.png)

- 代码分析

  - 代码编写

  ```python
  name = "玛利亚"
  print(id(name))
  name = "楚留香"
  print(name)
  print(id(name))
  ```

  - 结果分析

  ```python
  2151611071440
  楚留香
  2151611071728
  ```

### 3.3 数据类型

- 常见的数据类型

| 数据类型   | 字符表示 | 举例                   |
| ---------- | -------- | ---------------------- |
| 整数类型   | int      | 98                     |
| 浮点数类型 | float    | 3.1415926              |
| 布尔类型   | bool     | TRUE，False            |
| 字符串类型 | str      | “人生苦短，我用Python” |

-   使用type()函数输出数据类型

#### 3.3.1 整数类型

- 英文为integer，简写**int**，可以表示**正数、负数和零**
- 整数的不同进制表示方式

> window平台，`Win+R`后输入`calc`，可快速调度计算器

| 进制           | 基本数                          | 逢几进一 | 表现形式 |
| -------------- | ------------------------------- | -------- | -------- |
| 十进制（默认） | 0,1,2,3,4,5,6,7,8,9             | 10       | 120      |
| 二进制         | 0,1                             | 2        | 0b101011 |
| 八进制         | 0,1,2,3,4,5,6,7                 | 8        | 0o166    |
| 十六进制       | 0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F | 16       | 0x76     |

- 代码演示

  - 代码编写

  ```python
  # 整数类型，正数，负数，0
  n1 = 90
  n2 = -90
  n3 = 0
  print(n1, type(n1))
  print(n2, type(n2))
  print(n3, type(n3))
  # 正数可以表示为二进制，十进制，八进制，十六进制
  print("十进制", 118)
  print("二进制", 0b1010111)
  print("八进制", 0o176)
  print("十六进制", 0x78A2)
  ```

  - 结果分析

  ```python
  90 <class 'int'>
  -90 <class 'int'>
  0 <class 'int'>
  十进制 118
  二进制 1010111
  二进制 87
  八进制 126
  十六进制 30882
  
  Process finished with exit code 0
  ```

#### 3.3.2 浮点数类型

- 英文为**float**，浮点数由整数部分和**小数部分**组成

- 浮点数存储具有**不确定性**

  - 使用浮点数进行计算时，可能会出现小数点位不确定的情况
  - 解决方案：**导入模块decimal**

- 代码演示

  - 代码编写

  ```python
  a = 3.14159
  print(a, type(a))
  # 小数点位不确定
  n1 = 1.1
  n2 = 2.2
  n3 = 2.1
  print(n1 + n2)
  print(n1 + n3)
  # 解决方案：**导入模块decimal**
  from decimal import Decimal
  print(Decimal("1.1")+Decimal("2.2"))
  ```

  - 结果分析

  ```python
  3.14159 <class 'float'>
  3.3000000000000003
  3.2
  3.3
  
  Process finished with exit code 0
  ```

#### 3.3.3 布尔类型

- 英文名boolean,简写**bool**，用来表示真或假的值

- **True为真**，False为假

- 布尔值可以转化为正数

  - **True——>1**
  - **False——>0**

- 代码演示

  - 代码编写

  ```python
  f1 = True
  f2 = False
  print(f1, type(f1))
  print(f2, type(f2))
  
  # bool型可以转成整型运算
  print(f1 + 1)
  print(f2 + 1)
  ```

  - 结果分析

  ```python
  True <class 'bool'>
  False <class 'bool'>
  2
  1
  
  Process finished with exit code 0
  ```

#### 3.3.4 字符串类型

- 字符串又被称为不可变的字符序列

- 可以使用单引号`' '`，双引号`" "`，三引号`''' ''`'或`"""  """`号来定义

- 单引号和双引号定义的字符串**必须在一行**

- 三引号定义的字符型可以分布在连续的多行

- 代码演示

  - 代码编写

  ```python
  str1 = '人生苦短，我用Python'
  str2 = "人生苦短，我用Python"
  str3 = """人生苦短，
  我用Python"""
  str4 = '''人生苦短，
  我用Python'''
  
  print(str1, type(str1))
  print(str2, type(str2))
  print(str3, type(str3))
  print(str4, type(str4))
  ```

  - 结果分析
  
  ```python
  人生苦短，我用Python <class 'str'>
  人生苦短，我用Python <class 'str'>
  人生苦短，
  我用Python <class 'str'>
  人生苦短，
  我用Python <class 'str'>
  
  Process finished with exit code 0
  ```
### 3.4 数据类型转换


- 为什么需要数据类型转换
  - 将不同数据类型的数据拼接在一起时需要
- 数据类型转换的函数

| 函数名  | 作用                       | 注意事项                                                     | 举例                       |
| ------- | -------------------------- | ------------------------------------------------------------ | -------------------------- |
| str()   | 将其他数据类型转换成字符串 | 也可用引号转换                                               | str(123) <br />123'        |
| int()   | 将其他数据类型转换成整数   | 1.**文字类和小数类字符串，无法转换成整数**<br />2.**浮点数转换成整数：抹零取整** | int('123')<br />           |
| float() | 将其他数据类型转换成浮点数 | 1.**文字类无法转换成整数**<br />2.**整数转成浮点数，尾部为.0** | float('9.9')<br />float(9) |

- 数据转换的图示

![数据类型转换图示](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/26/62b7c937df58d.png)

- 代码演示

  - 代码编写

  ```python
  name = '张三'
  age = 20
  
  print(type(name), type(age))
  # 说明name和age的数据类型不相同
  # print('我叫'+name+'今年'+age+'岁')
  # TypeError: can only concatenate str (not "int") to str
  print('我叫' + name + ',今年' + str(age) + '岁')
  
  print('------------str()：将其他类型转换成str类型--------------')
  a = 10
  b = 198.8
  c = False
  print(type(a), type(b), type(c))
  print(str(a), str(b), str(c), type(str(a)), type(str(b)), type(str(c)))
  
  print('------------int()：将其他类型转换成int类型--------------')
  s1 = '128'
  f1 = 98.1
  s2 = '76.77'
  ff = True
  s3 = 'Hell0'
  print(type(s1), type(f1), type(s2), type(ff), type(s3))
  print(int(s1), type(int(s1)))  # 将str转换成int类型，字符串为 数字串
  print(int(f1), type(int(f1)))  # 将float转换成int类型，截取整数部分，舍掉小数部分
  # print(int(s2), type(int(s2)))  # ValueError: invalid literal for int() with base 10: '76.77'mm，字符串为小数串
  print(int(ff), type(int(ff)))
  # print(int(s3), type(int(s3))) ValueError: invalid literal for int() with base 10: 'Hell0',字符串必须为整数数字串才能转换
  
  print('------------float()：将其他类型转换成float类型--------------')
  ss1 = '128.98'
  ss2 = '76'
  fff = True
  ss3 = 'Hell0'
  i = 98
  print(type(ss1), type(ss2), type(fff), type(ss3), type(i))
  print(float(ss1),type(float(ss1)))
  print(float(ss2),type(float(ss2)))
  print(float(fff),type(float(fff)))
  # print(float(ss3),type(float(ss3)))    # ValueError: could not convert string to float: 'Hell0'
  print(float(i),type(float(i)))
  ```
  
  - 结果分析
  
  ```python
  <class 'str'> <class 'int'>
  我叫张三,今年20岁
  ------------str()：将其他类型转换成str类型--------------
  <class 'int'> <class 'float'> <class 'bool'>
  10 198.8 False <class 'str'> <class 'str'> <class 'str'>
  ------------int()：将其他类型转换成int类型--------------
  <class 'str'> <class 'float'> <class 'str'> <class 'bool'> <class 'str'>
  128 <class 'int'>
  98 <class 'int'>
  1 <class 'int'>
  ------------float()：将其他类型转换成float类型--------------
  <class 'str'> <class 'str'> <class 'bool'> <class 'str'> <class 'int'>
  128.98 <class 'float'>
  76.0 <class 'float'>
  1.0 <class 'float'>
  98.0 <class 'float'>
  
  Process finished with exit code 0
  ```
  

## 四、Python中的注释

- 什么是注释
  - 在代码中对代码的功能进行解释性说明的标注性文字，可以提高代码的可读性
  - 注释的内容会被Python解释器忽略
- 常见的注释类型
  - 单行注释——>以**#**开头，直到换行结束
  - 多行注释——>并没有单独的多行注释标记，将一对三引号之间的代码称为多行注释
  - 中文编码声明注释——>在文件开头加上中文声明注释，用以指定源码文件的编码格式
    - coding:gbk
    - coding:UTF-8
  
- 代码演示

  - 代码编写

  ```python
  # 输出功能(单行注释）
  print('hello')
  '''嘿嘿，
  这里是多行注释
  啦啦啦啦啦'''
  ```

  