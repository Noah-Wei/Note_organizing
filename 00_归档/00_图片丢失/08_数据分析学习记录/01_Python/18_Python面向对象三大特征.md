# Python面向对象三大特征

面向对象的三大特征：**封装**，**继承**和**多态**

## 一、封装

- **广义的封装**：函数的定义和类的定义
- **狭义的封装**：一个类中的某些属性，如果不希望被外界直接访问，则可以将该属性私有化，该属性只能在当前类中被直接访问；如果在类的外面需要访问（即获取或修改），则可以通过暴露给外界的函数间接访问
- **封装的本质**：将类中的属性进行私有化

### 1.1 私有属性

> 以实例属性为例，类属性一样，私有化只要在类属性前面添加两个下划线`_`，

- 公有属性

  正常使用`self.变量 = 值`定义，可以在类的外面直接用对象进行访问到

```python
# 定义类
class Person1():
    def __init__(self, name, age):
        # 公开属性
        self.name = name
        self.age = age


# 创建对象
p1 = Person1("张三", 10)
# 通过对象可以直接访问
print(p1.name, p1.age)
```

- 私有属性

  在定义属性的时候，在前面添加两个下划线`_`，如上诉的`name`修改为`__name`

  - **注意**
    - 此时变量名为`__name`，而不是原来的`name`
    - 进行限制属性动态绑定的时候，也要书写`__xxx`形式
    - 在**当前类中**的函数中，可以通过 `self.__xxxx`访问
    - 类的外面直接用对象**无法访问**到

```python
# b.私有属性/属性的私有化
class Person2():
    # 注意事项：哪怕是定义了私有属性，进行了限制属性动态绑定的时候，也要书写__xxx形式
    __slots__ = ("__name", "__age")

    def __init__(self, name, age):
        # 私有属性，只需要在属性名的前面添加两个下划线__
        self.__name = name
        self.__age = age

    # 注意3：在当前类中的函数中，可以self.__xxx访问的
    def show(self):
        print(f"姓名：{self.__name}，年龄：{self.__age}")


# 创建对象，通过对象可以直接访问
p2 = Person2("张三", 10)
# 通过对象无法直接访问
# print(p2.__name, p2.__age)  # 报错：AttributeError: 'Person2' object has no attribute 'name'
p2.show()
```

### 1.2 私有函数

既然属性可以私有，那么函数也可以私有

函数私有化方式和属性私有化类似，只要在函数名前面添加两个下划线`_`

```python
class Person3:
    # 创建公有函数
    def func1(self):
        print("func1函数~~~")

    # 创建私有函数
    def __func2(self):
        print("__func2私有函数~~~")


# 创建对象
p3 = Person3()
# 通过对象调用函数
p3.func1()
# p3.__func2()  # AttributeError: 'Person3' object has no attribute '__func2'
```

此时，私有函数无法再类外通过对象进行调用，如果访问，则会报错

如果要访问私有函数，可以在**共有函数中调用私有函数**

```python
class Person3:
    # 创建公有函数
    def func1(self):
        print("func1函数~~~")

        # 调用私有函数
        self.__func2()

    # 创建私有函数
    def __func2(self):
        print("__func2私有函数~~~")


# 创建对象
p3 = Person3()
# 通过对象调用函数
p3.func1()
```

输出结果

```
func1函数~~~
__func2私有函数~~~
```

### 1.3 类比总计

**解释下面不同形式的变量出现在类中的意义**

`a`：普通属性，也被称为公开属性，在类的外面可以直接访问

`_a`：在类的外面可以直接访问,但是**不建议使用**，容易和私有属性混淆

`__a`:私有属性，只能在类的内部被直接访问。类的外面可以通过暴露给外界的函数访问

`__a__`:在类的外面可以直接访问,但是**不建议使用**，因为系统属性和魔术方法都是这种形式的命名

​	如：`__slots__` ，`__init__` ， `__new__` ， `__del__`，`__name__`，`__add__`，`__sub__`，`__mul__`等

综上所述：公有属性使用`a`，私有属性使用`__a`，系统属性使用`__a__`

## 二、继承

- 如果两个或者两个以上的类具有相同的属性和方法，我们可以抽取一个类出来，在抽取出来的类中声明各个类公共的部分。其中：

​	被抽取出来的类——**父类**（father class）或着**超类**（super class）或者基类（base class）

​	两个或两个以上的类——**子类**或者**派生类**

​	他们之间的关系——子类**继承**自父类；或者父类**派生**了子类

- 简单来说

​	一个子类**只有一个**父类，被称为**单继承**

​	一个子类**有多个**父类，被称为**多继承**

- **语法：**

```python
# 单继承
class 子类类名(父类类名):
	类体
# 多继承
class 子类类名(父类类名1,父类类名2.......):
	类体
```

**注意：object是Python中所有类的根类**

- **继承的好处**：

​	简化代码

​	复用性

​	可读性

### 2.1 单继承

一个子类**只有一个**父类，被称为**单继承**

**父类**

```python
class Person():
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def eat(self):
        print("eating~~~~~~")
```

**子类1**：子类中没有定义`__init__`，创建子类对象，自动调用父类中的`__init__`

```python
class Doctor(Person):
    pass


doc = Doctor("王大夫", 45)
print(doc.name, doc.age)
doc.eat()
```

**子类2**：如果子类中定义了`__init__`，且定义了特有的属性，则在子类的`__init__`中调用父类的`__init__`

有三种方式可以调用

- 方式一

```python
super(当前类，self).__init__(参数列表）
```

- 方式二

```python
super().__init__(参数列表)
```

- 方式三

```python
父类.__init__(self,参数列表)
```

```python
# b.如果子类中定义了__init__，且定义了特有的属性，则在子类的__init__中调用父类的__init__
class Student(Person):
    def __init__(self, name, age, score):
        # 方式一：super(当前类，self).__init__(参数列表）
        # super(Student, self).__init__(name, age)

        # 方式二：super().__init__(参数列表)
        # super().__init__(name, age)
        # 方式三：父类.__init__(self,参数列表)
        Person.__init__(self, name, age)
        self.score = score

    def study(self):
        print("studying~~~~~~~")


stu = Student("小明", 10, 88)
print(stu.score)
print(stu.name, stu.age)
stu.eat()
stu.study()
```

### 2.2 多继承

一个子类**有多个**父类，被称为**多继承**

如果在子类的`__init__`中调用父类的`__init__`，方式如上，建议使用方式三，目标明确

```python
# 多继承，一个子类有多个父类
# 父类1
class Flyable():
    def fly(self):
        print("飞行")


# 父类2
class Runable():
    def run(self):
        print("行走")


# 子类
class Bird(Flyable, Runable):
    pass


# 创建子类对象
bird = Bird()
# 调用父类1的方法
bird.fly()
# 调用父类2的方法
bird.run()
```

## 三、多态

**继承是多态的前提**

- 体现形式：

  - 一种事物的多种体现形式，举例：动物有很多种

  ```python
  class Animal:
      pass
  
  
  class Cat(Animal):
      pass
  
  
  class SmallCat(Cat):
      pass
  
  
  # isinstance(对象，类型) :判断对象是否是制定的类型
  sc = SmallCat()	# sc 是多态的体现形式
  print(isinstance(sc, SmallCat))  # True
  print(isinstance(sc, Cat))  # True
  print(isinstance(sc, Animal))  # True
  print(isinstance(sc, object))  # True
  ```

  - 定义时并不确定是什么类型，要调用的是哪个方法，只有运行的时候才能确定调用的是哪个

  ```python
  class Animal:
      def style(self):
          print("walking~~~~~")
  
  
  class Cat(Animal):
      pass
  
  
  class Dog(Animal):
      pass
  
  
  class Fish(Animal):
      def style(self):
          print("swimming~~~~~")
  
  
  class Bird(Animal):
      def style(self):
          print("flying~~~~~")
  
  
  def show(ani):  # ani 就是多态的体现形式
      ani.style()
  
  
  d = Dog()
  c = Cat()
  f = Fish()
  b = Bird()
  
  show(d)
  show(c)
  show(f)
  show(b)
  ```

  输出结果

  ```python
  walking~~~~~
  walking~~~~~
  swimming~~~~~
  flying~~~~~
  ```