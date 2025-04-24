# Python包和模块

## 一、包

- 概念：包【package】是一种管理 Python 模块命名空间的形式，采用"点模块名称"，即`.模块`

使用模块的时候，你不用担心不同模块之间的全局变量相互影响一样，采用点模块名称这种形式也不用担心不同库之间的模块重名的情况

- 说明：

  - 为了区分或者归纳不同功能的文件，可以通过文件夹或包的方式进行区分

  - 不管是文件夹还是包，使用方式是完全相同的

  - 当导入模块的时候，涉及到路径问题

  - 一般当一个文件夹中含有一个`__init__.py`时，我们叫它为包。

    - 例：图中`aaa`和`bbb`为包，但`ccc`为文件夹

    ![image-20231019151214392](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/19/6530d6d51fbd3.png)

## 二、模块

### 2.1 模块的概念

​	当代码比较少，写在一个文件中还体现不出什么缺点，但是随着代码量越来越多，代码就越来越难以维护。

​	为了解决难以维护的问题，我们把很多相似功能的函数进行分组，分别放到不同的文件中。这样每个文件所包含的内容相对较少，而且对于每一个文件的大致功能可用文件名来体现。很多编程语言都是这么来组织代码结构。

**注意：其实一个.py文件就是一个模块**

- **模块的优点：**
  - 提高代码的可维护性
  - 提高了代码的复用度，当一个模块书写完毕，可以被多个地方引用
  - 引用其他的模块
  - 避免函数名和变量名的命名冲突

### 2.2 系统模块

Python自带的模块，可以直接导入使用

- 系统常用的模块

```python
import random  # 产生随机数
import math  # 进行数学运算
import string  # 获取字符串
import functools  # 关于函数运算
```

- 数据分析三剑客

```python
import numpy
import pandas
import matplotlib.pyplot
```

### 2.3 自定义模块

**自定义模块：自己封装一个模块，该模块中实现某些特定的功能**

- **注意事项**
  - 实际上，一个`.py`文件就是一个模块，py文件的文件名相当于模块名，所以一个合法的模块必须要遵循标识符的规则和规范
  - 在导入自定义模块时，需要注意模块的路径问题，需要将模块所在的包或文件夹声明，所以需要使用相对路径表示
  - 书写自定义模块，格式：xxx.xxx.xx.....,不管是包还是文件夹，用法完全相同

#### 2.3.1 使用一：import xxx

语法：**模块名.函数/变量**

- 导入系统模块：

可以分开写，也可以连在一起写

```python
import random  # 产生随机数
import math  # 进行数学运算
import string  # 获取字符串
import functools  # 关于函数运算


r1 = random.choice('3435445')
r2 = random.sample('343554',2)
```

连在一起写

```python
import random, math, string
```

- 导入自定义模块

导入自定义模块的时候，需要注意路径问题，采用相对路径的方式

例：导入`a1.py`和`c1.py`

![image-20231019160544185](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/19/6530e357e58fa.png)

```python
import aaa.a1
import ccc.cc.c1
```

- 简化写法，使用as，来起别名

```python
import random as r
import aaa.a1 as a
import ccc.cc.c1 as c

r1 = r.choice('3435445')
r2 = r.sample('343554', 2)
```

#### 2.3.2 使用二：from xxx import xxx

**from xxx import xxx**

相比较于第一种方式，这种方式是导入模块中的具体内容（函数、变量、类等）

- from 模块名 import 函数/变量/类

```python
# from 模块名  import  函数/变量/类

# 语法：函数/变量
from random import  choice,randint,sample
from aaa.a1 import name1,func1
from ccc.cc.c1 import name2,func2

r1 = choice('twetaw')

print(name1)
func1()

print(name2)
func2()
```

- from 模块名  import  *

其中`*`表示所有

```python
# b.from 模块名  import  *
from bbb.b1 import *
print(num1)
print(num2)
f1()
f2()
```

### 2.4 `__name__`的使用

`__name__` == `'__main__'`可以用于判断正在运行的是否是当前模块文件

如果运行的是当前文件，则结果为`__main__`,如果当前文件被导入到其他文件中，运行的是其他文件，则结果是模块名