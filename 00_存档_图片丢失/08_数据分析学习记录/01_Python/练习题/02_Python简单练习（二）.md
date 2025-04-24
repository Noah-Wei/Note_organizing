## 一、选择题

- 下面关于input的用法结果正确的是（ C ）

  ```python
  a = input("请输入一个数字：")
  print(type(a))
  ```

  -  A.报错                
  - B.<class  'int'>
  - C.<class 'str'>
  - D.<class 'list'>


- Python不支持的数据类型有（ C ）
  - A.int      
  - B.str     
  - C.char     
  - D.list

## 二、填空题

- 语句a, b=10,20执⾏后，a的值是(  10  )；语句a, a = 10, 20 执⾏后，a的值是(  20  )
- 布尔类型中的True和False分别表示数字(   1   )和（   0  ）
- 如果想要查看⼀个数据或者变量的数据类型，可以用（  type()  ）功能
- 如果要查看一个数据或变量在内存中的地址，可以用（  id() ）功能

## 三、编程题

**提示：Python中的加减乘除分别表示为  +     -      *    /**

- 提示⽤户输入⽤户名和密码，将数据以指定格式打印出来，要求分别用占位符和f""实现，

   格式：用户名：xxx,密码：xxx

   ```python
   name = input("请输入用户名：")
   pwd = input("请输入密码：")
   print("用户名：%s,密码：%s" % (name, pwd))
   print(f"用户名：{name},密码：{pwd}")
   ```

- 已知数据’aaa‘,'bbb','ccc',用一个print输出，最终结果为aaa=bbb=ccc

   ```python
   s1 = "aaa"
   s2 = "bbb"
   s3 = "ccc"
   print(f"{s1}={s2}={s3}")
   ```

- 从控制台输入两个数据，分别定义给两个变量，然后实现变量值的交换

   ```python
   num1 = 10
   num2 = 20
   num1, num2 = num2, num1
   print(num1, num2)
   ```

- 从控制台输入圆的半径，计算该圆的周长和面积，圆周率可以定义为3.14

   ```python
   r = float(input("请输入圆的半价："))
   PI = 3.14
   print(f"该圆的周长为：{2 * PI * r:.2f}，面积为{PI * r ** 2}")
   ```

- 一辆汽车以40km/h的速度行驶,行驶了45678.9km,求所用的时间

   ```python
   speed = 40
   distance = 45678.9
   t = distance / speed
   print(f"行驶时间为：{t}h")
   ```

   
