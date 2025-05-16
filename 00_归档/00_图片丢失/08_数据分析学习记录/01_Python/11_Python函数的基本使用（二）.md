# Python函数的基本使用（二）

## 一、匿名函数

### 1.1 概念

不再使用`def`这种标准形式定义函数，而是使用`lambda表达式`来创建函数，该函数没有函数名，被称为匿名函数

### 1.2 语法

```python
lambda 形参:返回值
```

### 1.3 说明

- lambda只是一个表达式，用一行代码实现一个简单的逻辑，可以达到对函数的简化【优点】
- lambda主体是一个表达式，而不是一个代码块，只能封装有限的逻辑【缺点】
- lambda拥有自己的命名空间，不能访问自有列表之外或者全局命名空间里的参数

### 1.4 实例演示

- 常规函数写法

```python
# 定义函数
def add(n):
    return n + 10


print(add)  # <function add at 0x00000220A254F040>
# 调用函数并获取返回值
r1 = add(6)
print(r1)
```

- 匿名函数写法

```python
# 定义函数/创建函数
f1 = lambda n: n + 10
print(f1)  # <function <lambda> at 0x00000220A291C3A0>
# 调用函数并获取返回值
r1 = f1(34)
print(r1)
```

### 1.5 习题：求两个数的平方和

- 常规函数写法

```python
def check1(a, b):
    return a ** 2 + b ** 2


r1 = check1(4, 5)
print(r1)
```

- 匿名函数写法

```python
check2 = lambda a, b: a ** 2 + b ** 2
r2 = check2(4, 5)
print(r2)
```

### 1.6 匿名函数的应用

**使用场景：常常将匿名函数作为另一个函数的参数使用**

例：

- 需求：将下面的列表根据学生的成绩降序排序

```python
students = [
    {'name': '小花', 'age': 19, 'score': 90, 'gender': '女', 'tel': '15300022839'},
    {'name': '明明', 'age': 20, 'score': 40, 'gender': '男', 'tel': '15300022838'},
    {'name': '华仔', 'age': 18, 'score': 90, 'gender': '女', 'tel': '15300022839'},
    {'name': '静静', 'age': 16, 'score': 90, 'gender': '不明', 'tel': '15300022428'},
    {'name': 'Tom', 'age': 17, 'score': 59, 'gender': '不明', 'tel': '15300022839'},
    {'name': 'Bob', 'age': 18, 'score': 90, 'gender': '男', 'tel': '15300022839'}]
```

- 错误写法

  - 原因：列表中的元素是字典，字典和字典之间无法比较大小

  ```python
  # students.sort(reverse=True)  # TypeError: '<' not supported between instances of 'dict' and 'dict'
  # print(students)
  ```

- 正确写法

  ```python
  列表.sort(key=func,reverse)
  ```

  - 说明：
    - key：当列表中的元素无法比较大小，或者 需要自定义排序规则时，则给key赋值一个函数即可
  - 工作原理
    - 将列表中的元素依次传参给func函数，该函数的返回值就是指定的排序的规则
    - 一定要注意，该返回值必须能比较大小

- 常规函数写法（不推荐）

```python
def rule(x):
    return x['score']


students.sort(key=rule, reverse=True)
print(students)
```

- lambda函数写法（推荐）

```python
students.sort(key=lambda stu_dict: stu_dict['score'], reverse=True)
print(students)
```

## 二、闭包

### 2.1 函数的本质

- 函数本质是一个变量，函数名其实就是一个变量名

  - 调用type()函数，打印结果相似

  ```python
  name = "zhangsan"  # 变量
  
  def func1():  # 函数
      print("11111")
  
  print(type(name))  # <class 'str'>
  print(type(func1))  # <class 'function'>
  func1()
  ```

  - 变量都可以重新赋值

  ```python
  name = [23, 34, 546]
  func1 = 10
  print(type(name))  # <class 'list'>
  print(type(func1))  # <class 'int'>
  ```

  - **注意：自定义标识符的时候，不要使用系统的函数名，否则会导致系统的函数失效**

  ```python
  print(sum)  # <built-in function sum>，正常输出
  # sum = 349
  # print(sum(1, 2, 3, 4, 5))  # TypeError: 'int' object is not callable，报错
  ```

- A函数可以作为B函数的参数或返回值使用，只需要传递或返回函数名即可

  - 使用场景：闭包，高阶函数，装饰器等

### 2.2 函数的嵌套定义

**需求：在func2中访问func1中的变量num1，求num1与num2的和**

- 方式一：设置返回值

```python
def func1():
    num1 = 78
    return num1


def func2():
    num2 = 10
    total = func1() + num2
    print(f"和为：{total}")


func2()
```

- **方式二：采用函数的嵌套：直接在func1中调用func2函数**

```python
def func1():
    num1 = 78

    def func2():
        num2 = 10
        total = num1 + num2
        print(f"和为：{total}")

    # 调用内部函数 func2()
    func2()


# 调用外部函数func1()
func1()
```

- **方式三：采用函数的嵌套：将func2设置为func1的返回值，这种方式以后比较常用，主要应用在闭包和装饰器中**
  - 注意：A函数作为B函数的返回值使用，只需要使用函数名即可

```python
def func1():
    num1 = 78

    def func2():
        num2 = 10
        total = num1 + num2
        print(f"和为：{total}")

    # 注意：A函数作为B函数的返回值使用，只需要使用函数名即可
    return func2


# 调用外部函数func1()
f = func1()  # f中存储的时func1函数的返回值
print(f)  # f中存储的是func2函数，<function func1.<locals>.func2 at 0x000001C729C09800>
f()  # 相当于调用func2
```

### 2.3 闭包

> 闭包就是建立在函数嵌套的基础上（上面的方式三）

​	函数只是一段可执行代码，编译后就“固化”了，每个函数在内存中只有一份实例，得到函数的入口点便可以执行函数了。函数还可以嵌套定义，即在一个函数内部可以定义另一个函数，有了嵌套函数这种结构，便会产生闭包问题。

闭包：**如果两个函数嵌套定义，如果在内部函数中访问了外部函数中的变量，则构成一个闭包**

例：上文中的方式三：在内部函数（func2）中访问了外面函数（func1）中的变量（num1）

#### 2.3.1 闭包的使用方式

- **方式一：内外函数都没有参数**
  - 例：上文中的方式三：在内部函数（func2）中访问了外面函数（func1）中的变量（num1）

```python
def func1():
    num1 = 78

    def func2():
        num2 = 10
        total = num1 + num2
        print('和:', total)

    return func2


f1 = func1()
f1()
```

- **方式二：外部函数有参数**
  - 调用外部函数的时候正常传参

```python
def func1_2(a, b, c):
    num1 = 78

    def func2_2():
        num2 = 10
        total = num1 + num2
        print('和:', total, a, b, c)

    return func2_2


f2 = func1_2(11, 33, 44)
f2()
```

- **方式三：外部函数和内部函数都有参数**
  - 调用内外部函数的时候都需要正常传参

```python
def func1_3(a, b, c):
    num1 = 78

    def func2_3(x, y):
        num2 = 10
        total = num1 + num2
        print('和:', total, a, b, c)

    return func2_3


f3 = func1_3(11, 33, 44)
f3(11, 22)
```

- **方式四：外部函数和内部函数都有参数,并且内部函数也有返回值**
  - 调用内外部函数的时候都需要正常传参，在调用内部函数的时候，还需要用一个变量来接收返回值

```python
def func1_4(a, b, c):
    num1 = 78

    def func2_4(x, y):
        num2 = 10
        total = num1 + num2
        print('和:', total, a, b, c)
        return "hello"

    return func2_4


f4 = func1_4(11, 33, 44)
f44 = f4(11, 22)
print(f44)
```

#### 2.3.2 总结

- 无论是函数的嵌套定义还是闭包，本质还是一个函数，所以默认参数，关键字参数和不定长参数都可以使用
- 返回值也可以正常使用
- 所以在调用内外函数的时候，一定要注意参数的传参问题

## 三、 变量的作用域

- 作用域：**指变量可以使用的范围**
- 程序的变量并不是在任意位置都可以访问，**访问权限取决于这个变量是在哪里赋值的**
- 变量的作用域决定了在哪一部分程序可以访问哪个特定的变量名称。

### 3.1 作用域的分类

> Python中的作用域一共有4种，主要用前三种

- L：Local，局部作用域，【特指内部函数】
  - 特点：只能在内部函数中被访问
- E：Enclosing，函数作用域，【特指嵌套定义的函数中，定义在外部函数中的变量】
  - 特点：只能在外部函数中被访问
- G：Global，全局作用域
  - 特点：在当前py文件的任意位置都可以被访问
- B：Built-in，内建作用域【内置作用域】

#### 3.1.1 作用域的分类

变量`num1`是全局作用域，变量`num2`是函数作用域，变量`num3`是局部作用域，

```python
num1 = 10  # G：Global，全局作用域


def outter1():
    num2 = 20  # E：Enclosing，函数作用域，外部函数中【特指嵌套定义的函数中，定义在外部函数中的变量】

    def inner1():
        num3 = 30  # L：Local，局部作用域，特指内部函数
```

#### 3.1.2 不同作用域的访问权限

变量`num1`是全局作用域，所以在当前py文件的任意位置都可以被访问

变量`num2`是函数作用域，所以只能在外部函数中被访问

变量`num3`是局部作用域，所以只能在内部函数中被访问

```python
num1 = 10  # G:Global,全局作用域,特点：在当前py文件的任意位置都可以被访问


def outter1():
    num2 = 20  # E:Enclosing,函数作用域【特指嵌套定义的函数中，定义在外部函数中的变量】，特点：只能在外部函数中被访问
    
    def inner1():
        num3 = 30  # L:Local,局部作用域，特指内部函数，特点：只能在内部函数中被访问
        print('inner:', num1, num2, num3)

    inner1()
    print('outter:', num1, num2)


outter1()
print('global:', num1)
```

输出结果

```
inner: 10 20 30
outter: 10 20
global: 10
```

#### 3.1.3 不同作用域中的变量重名

- 注意事项
  1. 如果不同作用域中的变量重名，变量被访问的原则：**就近原则**
  2. 虽然不同作用域中的变量重名，但是**相互之间没有任何关系**，表示**不同的变量**
- 代码

```python
num = 10


def outter1():
    num = 20

    def inner1():
        num = 30
        print('inner:', num)  # 30

    inner1()
    print('outter:', num)  # 20


outter1()
print('global:', num)  # 10
```

- 结果

```python
inner: 30
outter: 20
global: 10
```

#### 3.1.4 总结对比

- 会引入新的作用域：
  - 函数  、类 、模块
- 不会引入新的作用域
  - if 、for 、while 、try-except等

### 3.2 关键字global

**在变量重名的前提下**
global:全局的，在局部变量中【单层函数】 或者  函数作用域中【嵌套函数】声明一个变量来自于全局变量，对全局变量做出指定的操作

- 分析以下代码（一）

```python
n1 = 10


def func1():
    n1 = 20


func1()
print(n1)  # 10
```

输出结果：n1 = 10，原因：变量的作用域

- 分析一下代码（二）

  - 代码

  ```python
  n1 = 10
  def func1():
      
      n1 += 20
  func1()
  print(n1)
  ```

  - 结果

    - 报错：

      ```python
      UnboundLocalError: local variable 'n1' referenced引用 before assignment赋值
      ```

  - 分析原因

    - `n1 += 20` 等价于`n1 = n1 + 20`
    - 赋值运算中，永远都是先计算=的右边，将右边计算的结果给=左边的变量赋值
    - 不同作用域内的变量重名，访问的原则是就近原则

  - 解决方案一 ：表示不同的变量

  ```python
  n1 = 10
  def func1():
      n1 = 10
      n1 += 20
      print('内部：', n1)  # 30
  
  
  func1()
  print('外部：', n1)  # 10
  ```

  - 解决方案二：表示相同的变量，在函数使用global对变量进行声明

  ```python
  n1 = 10
  
  def func1():
      # global x:声明x变量来自于全局变量
      global n1
      n1 += 20
      print('内部：', n1)  # 30
  
  
  func1()
  print('外部：', n1)  # 30
  
  n1 = 10
  ```

### 3.3 关键字nonlocal

**在变量重名的前提下**

**使用场景：nonlocal只能使用在嵌套定义的函数中，发生在局部作用域和函数作用域之间**

```python
def outter1():
 name = 'abc'
 def inner1():
     # nonlocal x:声明x变量来自于函数作用域
     # UnboundLocalError: local variable 'name' referenced before assignment
     nonlocal name
     name += '123'
     print('局部:',name)  # abc123
 inner1()
 print("函数：",name)    # abc123
outter1()
```

## 四、生成器

### 4.1 生成器

#### 4.1.1 定义生成器的方式

> 1、将列表推导式中的[]改为()
> 2、函数结合yield，定义函数生成器

- 方式一：将列表推导式中的[]改为()

```python
lst1 = [i * 2 for i in range(10)]
# 打印列表，可以查看里面的元素
print(lst1)  # [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
print(type(lst1))  # <class 'list'>

# 生成器
ge1 = (i * 2 for i in range(10))
# 打印生成器，看不到里面的元素，不会占用内存空间，只有在使用里面的元素的时候，才会被调用
print(ge1)  # <generator object <genexpr> at 0x000001F818EAF9F0>
print(type(ge1))  # <class 'generator'>
```

- 方式二：函数结合yield，定义函数生成器
  - yield后面跟的数据是指你在生成器中放的数据的个数
  - 函数中可以写多个yield 表达式

```python
def func1():
    yield 10


r1 = func1()
print(r1)  # <generator object func1 at 0x000001D374FBDB10>


# yield后面跟的数据是指你在生成器中放的数据的个数
def func2():
    yield 10
    yield 20
    yield 30


r2 = func2()
print(r2)
```

#### 4.1.2 访问生成器中的元素

> 生成器中的元素只能被访问一次

```python
def func3():
    for i in range(10):
        yield i ** 2


r3 = func3()
print(r3)  # <generator object func3 at 0x0000016AEC25FB90>
```

- 方式一：转成列表

```python
print(list(r3))  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

- 方式二：遍历

```python
for item in r3:
    print(item)
```

- 方式三：使用next()方法，一次取出一个元素

```python
print(next(r3))
print(next(r3))
```

**注意事项：生成器中的元素只能被访问一次**

```python
for item in r3:
    print(item)
# 此时，生成器r3中的所有元素已经被访问
print(next(r3)) # StopIteration，报错
```

### 4.2 可迭代对象和迭代器

**可迭代对象和迭代器之间的区别和联系**：

- **区别**

  - **可迭代对象**：Iterable，可以直接作用于for循环的对象【可以使用for循环遍历其中元素的对象】
    - 如：list，tuple，dict，set，str，range()，生成器等
  - **迭代器**：Iterator，可以直接作用于for循环，或者可以通过next()获取下一个元素的对象
    - 如：生成器

- **联系：**

  - 迭代器一定是可迭代对象，但是可迭代对象不一定是迭代器
  - 但是，可以通过系统功能iter()将不是迭代器的可迭代对象转换为迭代器

  ```python
  lst1 = [i * 2 for i in range(10)]
  # 打印列表，可以查看里面的元素
  print(lst1)  # [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
  print(type(lst1))  # <class 'list'>
  
  lst2 = iter(lst1) # iter()将列表转换成迭代器
  print(lst2)  # <list_iterator object at 0x000001A25CF83FD0>
  print(type(lst2))  # <class 'list_iterator'>
  ```

  



