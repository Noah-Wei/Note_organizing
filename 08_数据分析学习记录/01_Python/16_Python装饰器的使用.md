# Python装饰器的使用

## 一、概念

> 装饰器是Python中独有的语法

- 装饰器的概念：已知一个函数，如果需要给该函数增加新的功能，但是不希望修改原函数，在Python中，这种在代码运行期间动态执行的机制被称为装饰器【Decorator】

- 装饰器的作用：为已经存在的函数或者类添加额外的功能

- 装饰器的本质：实际上就是一个闭包

  - 闭包概念：内部函数访问外部函数中的变量【函数】
  - 闭包函数

  ```python
  def outter(n):
   def inner():
       print(n)
   return inner
  f = outter(66)   # 调用outter函数， f ------>inner
  print(f)  # <function outter.<locals>.inner at 0x000001D6C0A6C3A0>
  f()  # 相当于调用inner函数
  ```

## 二、三种使用形式

### 2.1 使用一：直接使用

> 直接使用，较繁琐

- 装饰器的使用
  - 1、定义闭包，给外部函数设置参数，参数表示需要被装饰的函数，命名：f/fun/func
  - 2、在内部函数中调用被装饰的函数，同时增加新的功能

**需求：已有一个函数my_print，现在需要给他新增一个功能，多打印一条信息，但是不改变原函数**

**分析**：

- 定义一个闭包函数

```python
def outter(n):
 def inner():
     print(n)
 return inner
f = outter(66)   # 调用outter函数， f ------>inner
print(f)  # <function outter.<locals>.inner at 0x000001D6C0A6C3A0>
f()  # 相当于调用inner函数
```

- 在闭包函数的内部函数中调用被装饰的函数，同时增加新的功能

```python
# 已知函数
def my_print():
 print("拼搏到无能为力~~~~")

# 装饰器
# 1.定义闭包，给外部函数设置参数，参数表示需要被装饰的函数，命名：f/fun/func
def outter(func):
 def inner():
     # 2.在内部函数中调用被装饰的函数，同时增加新的功能
     print('inner:',func)  # inner: <function my_print at 0x00000147E9DEC430>
     func()   # 调用原函数my_print
     print('new~~~~~~~')  # 新增的功能
 return inner

f = outter(my_print)   # 需要装饰my_print，所以将my_print传参给func
f()        # 调用inner函数
```

- 总结
  - outter是装饰器的名称【外部函数的函数名】
  - inner是装饰器的核心部分【调用原函数，新增新的功能】
  - 在inner中，新增功能还是调用原函数，理论上没有先后顺序，一般需要根据具体的需求决定

### 2.2 使用二：使用@xxx

**使用说明：**

- 使用  @xxx  可以将一个装饰器作用于一个函数上，只需要将@xxx书写在一个函数的前面，则表示xxx装饰器装饰指定的函数
- @xxx xxx表示装饰器的名称【外部函数的函数名】
- 如果使用@xxx加载装饰器，则必须装饰器先存在，然后才能使用

**代码演示**

```python
# 装饰器
def outter(func):
    def inner():
        print("inner:", func)
        func()
        print("new++++++")

    return inner


# 已知函数
@outter  # 等价于 f = outter(my_print)，调用了outter()函数，同时将my_print传参给func
def my_print():
    print("拼搏到无能为力")


print(my_print)  # 此时不在指向原函数，<function outter.<locals>.inner at 0x0000019EE45AE480>指向了内部函数inner()
my_print()  # 等价于f(),实际调用了inner()函数
```

- 装饰器函数写在原函数的前面
- 在原函数的前面使用`@outter`，表示装饰器`outter()`，装饰了原函数 `my_print()`
  - 并且该代码等价于：第一种写法中的 `f = outter(my_print)` ，表示调用了 `outter()` 函数，同时将 `my_print` 传参给`func`
- 此时打印`my_print`，可以发现，结果为`<function outter.<locals>.inner at 0x0000019EE45AE480>`，
  - 不在指向了原函数，而是指向了内部函数`inner()`
- 此时在调用my_print()
  - 该代码等价于：第一种写法中的`f()` ，表示调用了`inner()`函数，

### 2.3 使用三：一个装饰器装饰多个函数

**需求：书写一个装饰器，同时装饰多个函数，给多个函数同时增加一个新的功能**

- 注意事项
  - 如果同一个装饰器装饰多个不同的函数，为了适配所有的函数，则给装饰器的内部函数**设置不定长参数**

```python
# 装饰器
def outter(func):
    def inner(*args, **kwargs):
        func(*args, **kwargs)  # 调用原函数,注意传参问题
        print("new++++++")

    return inner

# 函数1
@outter
def a():
    print("aaaaaaa")

# 函数2
@outter
def b(num1, num2):
    print("bbbbbbb", num1, num2)

# 函数3
@outter
def c(x, y, z, num):
    print("ccccccc", x, y, z, num)


# 调用函数
a()
b(1, 2)
c(1, 2, 3, 4)
```

- 函数1，2，3的参数数目都不相同，但是都需要用同一个装饰器，所以为了适配，给装饰器的内部函数设置不定长参数`*args, **kwargs`，将原函数的参数进行打包，在内部函数中通过`func`调用原函数，在传入参数`*args, **kwargs`进行解包

### 2.4实例演示

**需求：书写一个装饰器，可以统计任意一个函数的执行时间**

```python
import time


def wrapper(func):
    def get_time(*args, **kwargs):
        start_time = time.time()  # 开始的时间戳
        func(*args, **kwargs)
        end_time = time.time()  # 结束的时间戳
        return end_time - start_time

    return get_time


@wrapper
def check():
    for i in range(1000000):
        pass


r = check()
print(f"花费的时间为：{r}")
```

