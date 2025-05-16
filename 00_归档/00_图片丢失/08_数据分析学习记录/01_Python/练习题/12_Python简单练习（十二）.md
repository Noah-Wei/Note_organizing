- **简述Python中列表，元组，字典以及集合各自的特点**

| 数据类型  | 空容器符号 | 是否可变 | 是否有序 |      是否存储重复元素      |        可以存储的数据类型        |
| :-------: | ---------- | :------: | :------: | :------------------------: | :------------------------------: |
| 列表list  | []         |   可变   |   有序   |          可以重复          |             任意类型             |
| 元组tuple | ()         |  不可变  |   有序   |          可以重复          |             任意类型             |
| 字典dict  | {}         |   可变   |   无序   | key:唯一的，value:可以重复 | key:不可变的数据，value:任意类型 |
|  集合set  | 无         |   可变   |   无序   |         不可以重复         |           不可变的数据           |
| 字符串str | ""         |  不可变  |   有序   |          可以重复          |               ----               |

- **简述Python中深拷贝和浅拷贝的区别并举例说明**

> 但凡是可变数据类型（列表，字典，集合），都有拷贝的功能，
>
> 以列表lst1为例
>
> - **浅拷贝：**
>   - 操作方法：**列表.copy()/ 导入copy 包，copy.copy() /切片**
>   - **拷贝列表的时候，只会拷贝列表的最外层，所以推荐一维列表使用**
>
> - **深拷贝：**
>   - 操作方法：**导入copy包，copy.deepcopy()**
>   - **无论一个列表如何修改，另一个列表都不受影响**
>   - **所以一般推荐多维列表使用**

- **写出下面代码的输出结果并说明原因**

```python
list1 = ['a', 'b', 'c', 'd', 'e']
print(list1[10:])   
```

> 输出结果没空列表：[]
>
> 输出内容为对列表的切片，但是符合切片的语法，虽然索引超出的原列表的范围，但是不会报错，返回一个空值

- **写出下面代码的输出结果并说明原因**

```python
str1 = 'hello python'
str1.title()
print(str1)       
```

> 输出结果不变：hello python
>
> 字符串为不可变序列，虽然对其调用了title()方法，但是打印的还是其本身，所以不变

**写出下面代码执行的结果并说明原因**

```Python
list1=[5,3,1,9,12] 
r = (x for x in list1 if x%3==0) 
print(type(r))  
```

> 输出结果：<class 'generator'>
>
> <class 'generator'> ,这是一个生成器的语法，得到的结果 r 是一个生成器

- **在控制台中重复录入在西游记中你喜欢的人物。输入空字符串，打印所有人物。**

```python
lst1 = []
while True:
    character = input("请输入你喜欢的人物：")
    if character == "":
        print("输入结束")
        break
    else:
        if character in lst1:
            print(f"该人物在你喜欢的列表中，请重新输入")
            continue
        else:
            lst1.append(character)
print(f"喜欢的人物列表为：{lst1}")
```

- **在控制台中录入，所有学生名字，如果姓名重复，则提示"姓名已经存在"，不添加到列表中#，如果录入空字符串，则倒序打印所有学生**

```python
name_lst = []
while True:
    name = input("请输入学生姓名：")
    if name == "":
        print("输入结束")
        break
    else:
        if name in name_lst:
            print(f"姓名已经存在")
            continue
        else:
            name_lst.append(name)
print(f"学生列表为【正序】：{name_lst}")
print(f"学生列表为【逆序】：{name_lst[::-1]}")
```

- **输入一个数字，转换成中文数字。比如：1 -----> 壹，5 -----> 伍**

```python
num_dict = {'1': '壹', '2': '贰', '3': '叁', '4': '肆', '5': '伍', '6': '陆', '7': '柒', '8': '捌', '9': '玖',
            '10': '拾'}
while True:
    num = input("请输入一个数字:")
    if num.isdigit():
        for i in num:
            print(num_dict[i], end="")
        print()
    else:
        print("输入错误，请重新输入")
```

- **有如下商品价格：568，239，368，425，121，219，834，1263，26，请输入随意一个价格区间进行商品的筛选，并能够对筛选出的商品进行从大到小和从小到大进行排序，并求出这个区间的商品的平均价格**

```python
price_list = [568, 239, 368, 425, 121, 219, 834, 1263, 2]
choice_list = []
while True:
    num1 = input("请输入一个价格，作为价格区间的一个端点：")
    num2 = input("请输入一个价格，作为价格区间另一个端点：")
    if num1.isdigit() and num2.isdigit():
        min_price = min(int(num1), int(num2))
        max_price = max(int(num1), int(num2))
        
        for i in price_list:
            if min_price <= i <= max_price:
                choice_list.append(i)
                
        avg_price = sum(choice_list) / len(choice_list)
        
        print(f"在区间{min_price}——{max_price}区间的商品价格有：")
        
        choice_list.sort(reverse=True)
        print(f"从大到小排序：{choice_list}")
        
        choice_list.sort()
        print(f"从小到大排序：{choice_list}")
        
        print(f"平均价格是：{avg_price}")
        break
    else:
        print(f"价格区间输入错误，请重新输入")
```

- **编写程序，使用列表生成表达式生成一个包含50个随机整数的列表，然后删除其中所有奇数**

```python
import random

all_list = [random.randint(0, 1000) for i in range(50)]
print(all_list)
final_list = [i for i in all_list if i % 2 == 0]
print(final_list)
```

