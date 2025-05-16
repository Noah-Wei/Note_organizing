# Python面向对象：重写、单例设计、异常

## 一、函数重写

### 1.1 概念和注意事项

**重写的概念**：override，在继承的前提下，如果在子类中重新实现了父类的函数

**重写时的注意事项：**

1. 什么时候需要重写函数？
   - 如果一个类有很多子类，大多数子类可以直接使用父类中实现的功能；
   - 但是，如果父类中实现的需求满足不了部分子类的使用，则需要在子类中重写函数
2. 重写需要注意的事项
   - 保留函数的声明部分：`def  xxx(self,形参列表)`
   - 重新实现函数的实现部分（函数体）

### 1.2 自定义函数的重写

- 如果子类未重写父类中的函数，则子类可以继承父类中的函数

- 如果子类中重写了父类中的函数，子类对象将默认调用子类中重写之后的函数

- 子类重写函数的时候，如果仍然需要使用父类中函数的功能，则可以在子类函数中调用父类函数

  - 在子类函数中调用父类函数的三种方式

    - 方式一：

      ```python
      super(当前类，self).__init__(参数列表）
      ```

    - 方式二：

      ```python
      super().__init__(参数列表)
      ```

    - 方式三：

      ```python
      父类.__init__(self,参数列表)
      ```

```python
# 父类
class Animal():
    def style(self):
        print("walking")


# 子类1：如果子类未重写父类中的函数，则子类可以继承父类中的函数
class Cat(Animal):
    pass


# 子类2：如果子类中重写了父类中的函数，子类对象将默认调用子类中重写之后的函数
class Fish(Animal):
    def style(self):
        print("swimming")


# 子类3：子类重写函数的时候，如果仍然需要使用父类中函数的功能，则可以在子类函数中调用父类函数
class Bird(Animal):
    def style(self):
        super(Bird, self).style()
        # super().style()
        # Animal.style(self)
        print("flying")


c = Cat()
c.style()

f = Fish()
f.style()

b = Bird()
b.style()
```

### 1.3 系统函数的重写

实际应用可能更多

> 以重写系统的魔术方法`__str__`为例

魔术方法`__str__`：当打印一个对象时，默认会调用系统的魔术方法`__str__`，该函数的返回值默认就是当前对象在内存中的地址

```python
class Person1(object):
 def __init__(self,name,age):
     self.name = name
     self.age = age

p1 = Person1('张三',18)
# 注意2：当打印一个对象时，默认会调用系统的魔术方法__str__,该函数的返回值默认就是当前对象在内存中的地址
print(p1)  # <__main__.Person1 object at 0x0000012CBD588E20>
print(p1.__str__())  # <__main__.Person1 object at 0x0000012CBD588E20>
```

重写`__str__`：改为返回当前对象的字符串描述信息

**必须返回一个字符串**，一般情况下，返回和当前对象相关的属性信息

```python
class Person2(object):
 def __init__(self,name,age):
     self.name = name
     self.age = age
 # 重写__str__
 def __str__(self):
     # 注意：必须返回一个字符串，一般情况下，返回和当前对象相关的属性信息
     return f"姓名：{self.name},年龄：{self.age}"

p2 = Person2('张三',18)
print(p2)  # 姓名：张三,年龄：18
print(p2.__str__()) # 姓名：张三,年龄：18
```

但是将对象添加到容器中，直接打印容器，默认仍然显示地址，所以此时需要重写`__repr__`

重写前：

```python
class Person2(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    # 重写__str__
    def __str__(self):
        return f"姓名：{self.name},年龄：{self.age}"


p2 = Person2('张三', 18)
print(p2)  # 姓名：张三,年龄：18

lst = [p2]
print(lst)  # [<__main__.Person2 object at 0x0000015E2D9A1C90>]
```

重写后：

```python
class Person2(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    # 重写__str__
    def __str__(self):
        return f"姓名：{self.name},年龄：{self.age}"

    # 重写__repr__
    def __repr__(self):
        return f"姓名：{self.name},年龄：{self.age}"


p2 = Person2('张三', 18)
print(p2)  # 姓名：张三,年龄：18

lst = [p2]
print(lst)  # [姓名：张三,年龄：18]
```

可以发现，重写的函数`__str__`和`__repr__`函数体相同，根据函数的本质（变量），可以将函数进行重新赋值，将`__str__`赋值给`__repr__`

即，将函数体

```python
    # 重写__str__
    def __str__(self):
        return f"姓名：{self.name},年龄：{self.age}"

    # 重写__repr__
    def __repr__(self):
        return f"姓名：{self.name},年龄：{self.age}"
```

等价于

```python
__repr__ = __str__
```

完整版

```python
class Person2(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    # 重写__str__
    def __str__(self):
        return f"姓名：{self.name},年龄：{self.age}"
    
    __repr__ = __str__ 
```

## 二、单例设计模式

### 2.1 装饰器

装饰器可以装饰函数，也可以装饰类，并且语法一致

装饰器必需在函数或者类前面

- 装饰器可以装饰函数

```python
def wrapper(func):  # 参数命名：f/func/func,表示需要被装饰的函数
    def inner():
        func()  # 调用原函数
        print("函数~~~~~new~~~~")

    return inner


@wrapper  # 调用外部函数wrapper
def check():
    print('函数~~~~~~check')


check()  # 调用内部函数inner
```

- 装饰器装饰类

其中装饰器外部函数的参数推荐使用`cls`

```python
def wrapper(cls):  # 参数命名：cls，表示需要被装饰的类
    def inner():
        print("类~~~~~new~~~~")
        return cls()  # 创建对象

    return inner


@wrapper  # 调用外部函数wrapper
class Check():
    def __init__(self):
        print('init~~~~~类~~~~~Check')


c = Check()  # 调用内部函数inner
print(c)
```

### 2.2 概念

- 什么是设计模式？
  - 设计模式是经过总结、优化的，对我们经常会碰到的一些编程问题的可重用解决方案
  - 设计模式更为高级，它是一种必须在特定情形下实现的一种方法模板。设计模式不会绑定具体的编程语言
  - 23种设计模式，其中比较常用的是单例设计模式，工厂设计模式，代理模式，装饰者模式等等
- 什么是单例设计模式？
  - 对象：又被称为实例
  - 单例：单个实例/单个对象，一个类只能创建一个对象，只能创建出一个对象的类被称为单例类
  - 程序运行过程中，确保某一个类只有一个实例（对象），不管在哪个模块获取这个类的对象，获取到的都是同一个对象。例如：一个国家只有一个主席，不管他在哪

- 单例设计模式的核心
  - 一个类有且仅有一个实例，并且这个实例需要应用于整个程序中，该类被称为单例类

- 验证两个变量中是否存储的是同一个对象，该如何验证？
  - 方式一：`p1  is  p2` 
  - 方式二：`id(p1) == id(p2)`

### 2.3 应用场景

应用程序中描述当前使用用户对应的类 ———> 当前用户对于该应用程序的操作而言是唯一的——> 所以一般将该对象设计为单例

**实际应用**：

**数据库连接池操作**:应用程序中多处地方连接到数据库 ———> 连接数据库时的连接池只需一个就行，没有必要在每个地方都创建一个新的连接池，这种也是浪费资源 ————> 解决方案也是单例

### 2.3 实现

#### 2.3.1 实现单例类方式一

- 普通类

如果两个变量中存储的数据的地址相同，则说明这两个变量中存储的是同一个对象

下面创建的为两个对象

```python
class Book():
 def __init__(self,name,date):
     self.name = name
     self.date = date
b1 = Book('坏蛋时怎样练成的',2001)
b2 = Book('Python从入门到精通',2015)

# 注意：如果两个变量中存储的数据的地址相同，则说明这两个变量中存储的是同一个对象
# 比较两个变量中是否存储的时同一个对象
print(b1 is b2)          # False
print(id(b1) == id(b2))  # False
```

- 单例类

需求：书写一个装饰器，可以使得任意一个类都成为单例类

```python
# 定义一个装饰器
def wrapper(cls):
    # 储存被装饰的类可以创建的唯一的对象
    instance = None

    def inner(*args, **kwargs):
        nonlocal instance
        if not instance:
            # cls(*args,**kwargs):每执行一次，就会创建一个新的对象，要实现单例类，则只要控制该句代码只执行一次
            instance = cls(*args, **kwargs)
        return instance

    return inner


# 被装饰的类
@wrapper
class Person():
    def __init__(self, name, age):
        self.name = name
        self.age = age


p1 = Person("张三", 12)
p2 = Person("李四", 23)
print(p1 is p2)  # True
print(id(p1) == id(p2))  # True

print(p1.name, p2.name)  # 张三 张三

p1.name = 'Jack'
print(p1.name, p2.name)  # Jack Jack
```

#### 2.3.2 实现单例类方式二

```python
def wrapper(cls):
    # key:被装饰的类cls   value:类可以创建的唯一的对象
    instance_dict = {}

    def inner(*args, **kwargs):
        if not instance_dict:
            instance_dict[cls] = cls(*args, **kwargs)
        return instance_dict[cls]

    return inner


@wrapper
class Person():
    def __init__(self, name, age):
        self.name = name
        self.age = age


p1 = Person("张三", 12)
p2 = Person("李四", 23)
print(p1 is p2)  # True
print(id(p1) == id(p2))  # True

print(p1.name, p2.name)  # 张三 张三

p1.name = 'Jack'
print(p1.name, p2.name)  # Jack Jack
```

## 三、异常和错误

### 3.1 概念

- Python有两种错误很容易辨认：**语法错误**和**异常**
  - 语法错误：也可以称为解析错误，是初学者经常碰到的，比如缺少冒号等
  - 异常：在程序运行过程中，总会遇到各种各样的错误，有的错误是程序编写有问题造成的，这种错误我们通常称之为bug，bug是必须修复的；有的错误是用户输入造成的，这种错误可以通过检查用户输入来做相应的处理；还有一类错误是完全无法在程序运行过程中预测的，比如写入文件的时候，磁盘满了，写不进去了，或者从网络抓取数据，网络突然断掉了，这类错误被称为异常，在程序中通常是必须处理的，否则，程序会因为各种问题终止并退出

- 异常的特点
  - 当程序执行的过程中，遇到了异常，而且异常未被处理，则程序会终止异常处
- 处理异常的思想
  - 将可能存在异常的代码监测起来，如果代码遇到异常，则跳过异常，继续执行后面的代码

```python
print('start~~~~')

num = int(input("请输入一个数字："))  # ValueError: invalid literal for int() with base 10: 'fhajg'
list1 = [34,45,2,43,6,89]
print(f"获取到的元素为：{list1[num]}")  # IndexError: list index out of range

print('end~~~~~~')
```

### 3.2 常见异常

| 异常                | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| NameError           | 使用一个还未被赋值的变量                                     |
| ValueError          | 传入一个调用者不期望的值，即使值的类型是正确的               |
| TypeError           | 传入对象类型与要求的不符合                                   |
| IndexError          | 下标索引超出序列边界，比如当x只有三个元素，却试图访问x[5]    |
| AttributeError      | 试图访问一个对象没有的属性，比如foo.x，但是foo没有属性x      |
| ModuleNotFoundError | 模块导入错误，一般是模块的路径或模块名书写错误               |
| FileNotFoundError   | 文件路径错误                                                 |
| UnboundLocalError   | 试图访问一个还未被设置的局部变量，基本上是由于另有一个同名的全局变量，导致你以为正在访问它 |
| IOError             | 输入/输出异常；基本上是无法打开文件                          |
| ImportError         | 无法引入模块或包；基本上是路径问题或名称错误                 |
| IndentationError    | 语法错误（的子类） ；代码没有正确对齐                        |
| KeyError            | 试图访问字典里不存在的键                                     |
| KeyboardInterrupt   | Ctrl+C被按下                                                 |

```python
# 1.NameError 使用一个还未被赋值的变量
# print(num)   # NameError: name 'num' is not defined

# 2.ValueError 传入一个调用者不期望的值，即使值的类型是正确的
# num = int(input("请输入一个数字："))  # ValueError: invalid literal for int() with base 10: 'fagah'

# 3.TypeError 传入对象类型与要求的不符合
# print(10 + 'fagjha') # TypeError: unsupported operand type(s) for +: 'int' and 'str

# 4.IndexError 下标索引超出序列边界，比如当x只有三个元素，却试图访问x[5]
# numlist = [234,5,6]
# print(numlist[100])   # IndexError: list index out of range

# 5.AttributeError 试图访问一个对象没有的属性，比如foo.x，但是foo没有属性x
# 'abc'.reverse()  # AttributeError: 'str' object has no attribute 'reverse'

# 6.ModuleNotFoundError:模块导入错误，一般是模块的路径或模块名书写错误
# import rand  # ModuleNotFoundError: No module named 'rand'

# 7.FileNotFoundError:文件路径错误

# 8.UnboundLocalError 试图访问一个还未被设置的局部变量，基本上是由于另有一个同名的全局变量，导致你以为正在访问它
# a = 34
# def func():
#     a += 1   # UnboundLocalError: local variable 'a' referenced before assignment
# func()
```

### 3.3 异常处理方式

- 处理异常的本质
  - 没有从根本上解决问题（修改代码），只是将异常忽略，可以让后面的代码继续执行
- 异常处理语法

```python
try:
 可能存在异常的代码
except 错误码 as 变量:
 出现异常异常之后的提示
finally:
	pass
```

- 语法说明
  - 将可能存在异常的代码书写到`try`代码块中
  - 如果`try`中的代码存在异常，`try`中的代码会执行到异常处，然后直接执行`except`分支
  - 如果`try`中的代码不存在异常，`try`中的代码会执行完毕，然后直接执行`try-except`后面的语句
  - 常用的形式
    - `try-except`
    - `try-finally`
    - `try-except-finally`

```python
print('start~~~~')

# 出现错误:抛出异常
# 处理错误：捕获异常

try:
    # 可能存在异常的代码
    num = int(input("请输入一个数字："))
    list1 = [34, 45, 2, 43, 6, 89]
    print(f"获取到的元素为：{list1[num]}")
except ValueError as e:
    # e:异常的描述信息
    print('ValueError', e)
except IndexError as e:
    print('IndexError', e)
finally:
    # 任何情况下都会执行，一般 用来进行文件的关闭或者数据库的关闭
    print('finally~~~')

print('end~~~~~~')
```