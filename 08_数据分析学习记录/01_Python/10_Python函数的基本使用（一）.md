# Python函数的基本使用（一）

## 一、函数概述

​	在一个完整的项目中，某些功能可能会被反复使用，如果将反复出现的代码封装成函数，以后如果要继续使用该功能则直接使用函数即可，另外，如果要修改需求，只需要修改函数即可

**本质：对某些特殊功能的封装**

**函数的优点：**

- 简化代码结构，提高应用的模块性
- 提高了代码的复用性
- 提高了代码维护性

## 二、函数的定义

### 2.1 函数的语法

```python
def  函数名(变量1，变量2....):
        函数体
        return   返回值
```

### 2.2 语法说明

- **def**是一个关键字，是definition的缩写，专门定义函数
- **函数名**：遵循合法标识符的规则和规范即可，尽量做到见名知意，注意：和变量的定义类似，**全部小写**
- (变量1，变量2....):被称为**形式参数**，是一个参数列表，都只是没有赋值的变量
- **函数体**：封装某些特殊的功能
- **return**是一个关键字，表示返回,注意：只能用在函数中，表示结束函数，可以单独使用，也可以携带数据，当携带数据，则表示该函数的返回
- **返回值**：常量，变量，表达式
- 函数的定义分为两部分：**函数的声明**和**函数的实现**
- 变量1，变量2.... 和 return   返回值 可以根据具体的需求选择性的省略

### 2.3 函数定义的方式

- **无参无返回值**

```python
def fun1():
    print("hello world 11111111")
```

- **有参无返回值**

```python
def fun2(a, b):
    print("hello world 22222222")
```

- **无参有返回值**

```python
def fun3():
    print("hello world 33333333")
    return "abc"
```

- **有参有返回值**

```python
def fun4(x, y, z):
    print("hello world 44444444")
    return 10
```

### 2.4 总结

- **函数名和变量名的本质是一样的，都是标识符**
- **定义函数之后，只是将函数加载到计算机内存中，此时函数体不会执行**
- **一个函数定义完毕之后，只有当函数被调用【使用函数】的时候，函数体才会被执行**

## 三、函数的调用

### 3.1 函数调用语法

```python
# 函数的定义
def  函数名(形参):
	pass

# 函数的调用
函数名(实参)
```

### 3.2 语法说明

- 函数调用的**本质**：就是使用函数的过程，当然，同时需要注意传参的问题
- **传参**：在调用函数的过程中，实参给形参赋值的过程
- **形参**：形式参数，出现在函数的声明部分，实际上是一个变量，等待实参赋值【注意：形参本身可以赋值】
- **实参**：实际参数，出现在函数的调用部分，实际上是一个数据【常量，变量，表达式】，目的是为了给形参赋值

### 3.3 函数调用

- **无参无返回值**

```python
def fun1():
    print("hello world 11111111")
   
# 函数的调用
fun1()
```

- **有参无返回值**

形参个数要和实参个数相等

```python
def fun2(a, b):
    print("hello world 22222222")
    
# fun2()  # TypeError: fun2() missing 2 required positional arguments: 'a' and 'b'
fun2(1, 2)
```

- **无参有返回值**

有返回值，需要用一个变量来接受，此时该变量中存储的是函数的返回值

```python
def fun3():
    print("hello world 33333333")
    return "abc"

r3 = fun3()  # 变量 = 函数() ，此时该变量中存储的是函数的返回值
print(r3)
```

- **有参有返回值**

```python
def fun4(x, y, z):
    print("hello world 44444444")
    return 10

r4 = fun4(45, 3, 6)
print(r4)
```

## 四、函数的参数

### 4.1 参数的分类

- 必需参数
- 默认参数
- 关键字参数
- 不定长参数【可变参数】

### 4.2 必需参数

- **必需参数必需传参，一定要注意实参和形参的数量匹配**
- **如果形参为空，则为空**
- **如果形参不为空，则实参和形参数量相等**
- **代码演示：**

```python
# 参数为空
def fun_a1():
    print("aaaaa____1111")

# 形参为空，实参为空
fun_a1()

# 参数不为空
def fun_a2(n1, n2):
    total = n1 + n2
    print("aaaaa____2222")

# 形参和实参数量相等
fun_a2(10, 20)
```

### 4.3 默认值参数

- **默认值参数：给行参一个默认值**
- **默认参数主要体现在形参上**
- **当形参为多个参数时，可以全部设置成默认值参数**
- **当形参中有默认值参数时，默认值参数必需放在后面，否则将出错**
- **默认参数的好处：给参数一个默认值，可以简化函数的调用**
- 代码演示：

```python
# 注意：默认参数主要体现在形参上，表示形参有默认值
# 默认参数的好处：给参数一个默认值，可以简化函数的调用
# 需求：求一个数和10的和
def b1(a, b=10):
    print(f"{a}+{b}的结果为{a + b}")


b1(45)  # 只需要传入a的值
b1(45, 67)


# 当形参为多个参数时，可以全部设置成默认值参数
def b2(a=20, b=10):
    print(f"{a}+{b}的结果为{a + b}")


b2()
b2(45)  # 按顺序传入参数
b2(45, 67)


# 当形参中有默认值参数时，默认值参数必需放在后面，否则将出错
# def b3(b=10, a):  # SyntaxError: non-default argument follows default argument
#     print(f"{a}+{b}的结果为{a + b}")
```

### 4.4 关键字参数

- **关键字参数主要体现在函数的调用上，实参上**
- **使用关键字的好处，可以不按照形参的顺序传参**
- **关键字参数可以和默认值参数混合使用**
- **使用关键字参数，必须放在后面**
- 代码演示：

```python
# 3.关键字参数
# 注意：关键字参数体现在函数的调用上，使用关键字的好处，可以不按照形参的顺序传参
def c1(name, age):
    print(f"姓名：{name}，年龄：{age}")


c1("张三", 20)  # 默认顺序传参
c1(age=33, name="赵六")  # 不按照形参的顺序传参


def c2(name, age, addr="北京"):	# 关键字参数可以和默认值参数混合使用
    print(f"姓名：{name}，年龄：{age}，地址：{addr}")


c2("ana", 5)
c2(age=5, name="ana", addr="上海")
```

### 4.5 不定长参数（可变参数）

- **实参可以传递若干个数据**
- *：被当做元组处理，一般后面的变量命名使用args
- ** ：被当做字典处理，一般后面的变量命名使用kwargs
- 在同一个函数中，同种符号只能出现一次，但是*和**可以同时出现
- 代码演示：

```python
# 4. 不定长参数【可变参数】
# 注意：实参可以传递若干个数据
# a.*:被当作元组处理
def d1(*args):
    print(args)  # (45,)
    print(type(args))  # <class 'tuple'>


d1()
d1(45)
d1(34, 56, 78, 8, 990, 0, 0, 35, 45, 7)


# b.**：被当作字典处理
# 注意：调用有**x形式参数的函数时，需要按照key=value形式传参
def d2(**kwargs):
    print(kwargs)
    print(type(kwargs))


d2()
d2(x=4, y=2, z=24)  # {'x': 4, 'y': 2, 'z': 24},类似于字典的创建在


# 注意:在同一个函数中，同种符号只能出现一次，但是*和**可以同时出现
def d3(*args, **kwargs):
    print(args, kwargs)


d3(34, 5, 7, 8, 89, 98, 0, 0)
d3(a=10)
d3(34, 5, 7, 8, 89, 98, 0, 0, abc=49
```

## 五、函数的返回值

### 5.1 语法

```python
def xxx(形参):
	函数体【某个特殊的功能】
return 返回值
```

### 5.2 说明

**返回值：表示函数的运算结果，在哪里调用函数，返回值就返回到哪里**

### 5.3 代码演示

#### 5.3.1 返回值的意义

```python
def func2():
    print("start：", "1" * 10)
    # 在函数内定义一个变量（局部变量）
    num1 = 66
    print("end：", "1" * 10, num1)
    return num1


r1 = func2()  # 调用了fun2函数，将func2函数的返回值赋值给r1
print(f"返回值{r1}")

print(func2() + 10)  # 调用了fun2函数，同时将函数的返回值和10进行运算

func2()  # 不需要在函数外面使用返回值，则只需要调用函数即可
```

**注：在函数内部定义的变量，只能在当前函数的内部被访问，这里涉及到变量的作用域**

函数体外使用函数内的变量，将报错： NameError: name 'num1' is not defined

```python
# 定义函数
def func1():
    print("start：", "1" * 10)
    # 在函数内定义一个变量（局部变量）
    num1 = 66
    print("end：", "1" * 10, num1)


# 调用函数
func1()
# func1(num1)  # NameError: name 'num1' is not defined
```

#### 5.3.2 未设置返回值

**注：如果一个函数未通过return设置返回值，默认的返回值为None**

```python
def func3():
    print("2" * 10)


r2 = func3()
print(r2)  # None:空值
```

上诉代码等价于

```python
def func4():
    print("2" * 10)
    return None
```

#### 5.3.3 设置返回值

##### 5.3.3.1 return 单独使用时

- **return只能用在函数中，表示结束函数**

```python
def fun3_1():
    print("start~~~~~")
    return
    # print("end~~~~~")  # 警告，该行代码永远不会执行


fun3_1()
```

- **在函数中，和return平级（即对齐）的情况下，return后面不书写任何语句，否则永远没有执行的机会。**

```python
def fun3_2(n):
    print("start~~~~~")
    if n % 2 == 0:
        return
    print("end~~~~~")  # 有执行的可能性，取决于if判断结果


fun3_2(7)
```

##### 5.3.3.2 return xxx

- **return作用：1、表示结束函数，2、同时将xxx数据返回到调用函数处**

```python
def fun3_3(a, b):
    total = a + b
    return total
    # print("over~~~~~")  # 警告，该行代码永远不会执行


r33 = fun3_3(34, 10)
print(r33)
```

- **同一个函数中，根据条件的区分，可以设置多个 return xxx**

```python
def compare(num1, num2):
    if num1 > num2:
        return 1
    elif num1 < num2:
        return -1
    else:
        return 0


print(compare(29, 18))
print(compare(66, 88))
print(compare(18, 18))
```

- **上述函数代码可以优化，else可以省略**

```python
def compare1(num1, num2):
    if num1 > num2:
        return 1
    elif num1 < num2:
        return -1
    return 0

```

- **return后面还可以接多个数据**

```python
def fun3_4():
    return 1, 234, 45.34, "abc"  # 结果为元组，打包


r34 = fun3_4()
print(r34)
```

#### 5.3.4 break和return 的区别

- **break结束当前循环，break只能使用在循环中，表示结束当前循环**

```python
def fun4_1():
    print("start~~~~~")
    for n in range(3):
        for m in range(5):
            print(n, m)
            if m == 1:
                break  # break结束当前循环，break只能使用在循环中，表示结束当前循环
    print("end~~~~~")


fun4_1()
```

- return只能使用在函数中，表示结束函数，无论return处于多少层循环中

```python
def fun4_2():
    print("start~~~~~")
    for n in range(3):
        for m in range(5):
            print(n, m)
            if m == 1:
                return  # return只能使用在函数中，表示结束函数，无论return处于多少层循环中
    print("end~~~~~")


fun4_2()
```

## 六、函数的封装

### 6.1 注意事项

- 观察需求，是否有未知项参与运算，如果有，则将未知项设置为参数
- 观察需求，函数执行完毕之后，是否有结构，如果有，则将结果设置为返回值

### 6.2 例题演示

#### 6.2.1 打印九九乘法表

```python

def my_print():
    for i in range(1, 10):
        for j in range(1, i + 1):
            print(f"{j} * {i} = {j * i}", end="\t")
        print()


my_print()
```

#### 6.2.2 打印指定行数的九九乘法表

```python
def my_print2(n):
    for i in range(1, n + 1):
        for j in range(1, i + 1):
            print(f"{j} * {i} = {j * i}", end="\t")
        print()


my_print2(3)
my_print2(5)
```

#### 6.2.3 判断一个数是否是偶数

```python
def iseven(num):
    if num % 2 == 0:
        return True
    return False


r1 = iseven(6)
print(r1)
r2 = iseven(7)
print(r2)
```

#### 6.2.4 验证一个年是否是闰年

```python
def isleapyaer(year):
    if (year % 4 == 0 and year % 100 != 0) or year % 400 == 0:
        return True
    return False


y1 = isleapyaer(100)
print(y1)
```

