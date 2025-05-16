- **调用python列表操作中常用函数，实现以下功能：**
  1. **创建一个空列表lst；**
  2)    **在lst列表中依次追加10个数值(78, 93, 66, 83, 100, 95, 77, 93, 85, 98)；**

  3)    **输出lst列表中第7个元素的数值；**

  4)    **输出lst列表中第1~5个元素的数值；**

  5)    **调用insert()函数，在lst列表第7个元素之前添加数值59；**

  6)    **利用变量num保存数值93，调用count()函数，查询num变量值在lst列表中出现的次数；**

  7)    **使用in查询lst列表中是否有num变量值的评分；**

  8)    **调用index()函数，查询lst列表中100的序号；**

  9)    **找出lst列表中数值为59的元素，并加1；**

  10) **调用del()函数删除lst列表中的第1个元素；**

  11) **调用len()函数获得lst列表中元素的个数；**

  12) **调用sort()函数，对列表中所有元素进行排序，输出列表中最高分和最低分；**

  13) **调用reverse()函数，颠倒lst列表中元素的顺序；**

  14) **调用pop()函数删除lst列表中尾部的元素，返回删除的元素；**

  15) **lst列表中用append()函数追加数值83，并输出。调用remove()函数删除lst列表中第一个数值83；**

  16) **创建2个列表lst1和lst2，lst1中包含2个元素值：78，91，lst2中包含3个元素值：84，92，65，合并这两个列表，并输出全部元素；**

  17) **创建lst1列表，其中包含数值2个元素值：78，91，将lst1中元素赋值5遍保存在lst2列表中，输出lst2列表中全部元素;**

  18) **清空lst列表，将lst2列表复制给lst列表，将lst列表中第2个元素变为2，并分别输出lst列表、lst2列表全部元素**

```python

# 1) 创建一个空列表lst
lst = []

# 2) 在lst列表中依次追加10个数值(78, 93, 66, 83, 100, 95, 77, 93, 85, 98)；
lst.append(78)
lst.append(93)
lst.extend((66, 83, 100, 95, 77, 93, 85, 98))
print(lst)
print("-" * 50)

# 3) 输出lst列表中第7个元素的数值；
item = lst[6]
print(f"lst列表中第7个元素的数值为：{item}")
print("-" * 50)

# 4) 输出lst列表中第1~5个元素的数值；
lst4 = []
for item in range(5):
    print(f"列表中第{item + 1}个元素为：{lst[item]}")
    lst4.append(lst[item])
print(f"lst列表中第1~5个元素的数为：{lst4}")
print("-" * 50)

# 5) 调用insert()函数，在lst列表第7个元素之前添加数值59；
lst.insert(6, 59)
print(f"修改后，列表为：{lst}")
print("-" * 50)

# 6) 利用变量num保存数值93，调用count()函数，查询num变量值在lst列表中出现的次数；
num = 93
count6 = lst.count(num)
print(f"num变量值在lst列表中出现的次数为：{count6}")
print("-" * 50)

# 7) 使用in查询lst列表中是否有num变量值的评分；
result7 = num in lst
print(f"lst列表中是否有num变量值的结果为：{result7}")
print("-" * 50)

# 8) 调用index()函数，查询lst列表中100的序号；
num8 = lst.index(100)
print(f"lst列表中100的序号为：{num8}")
print("-" * 50)

# 9) 找出lst列表中数值为59的元素，并加1；
for i in range(len(lst)):
    if lst[i] == 59:
        lst[i] += 1
print(f"修改后，列表为：{lst}")
print("-" * 50)

# 10) 调用del()函数删除lst列表中的第1个元素；
del lst[0]
print(f"修改后，列表为：{lst}")
print("-" * 50)

# 11) 调用len()函数获得lst列表中元素的个数；
length = len(lst)
print(f"lst列表中元素的个数为：{length}")
print("-" * 50)

# 12) 调用sort()函数，对列表中所有元素进行排序，输出列表中最高分和最低分；
lst.sort()
max_num = max(lst)
min_num = min(lst)
print(f"排序后列表为：{lst}")
print(f"最大数为：{max_num}，最小数为：{min_num}")
print("-" * 50)

# 13) 调用reverse()函数，颠倒lst列表中元素的顺序；
lst.reverse()
print(f"逆向排序后列表为：{lst}")
print("-" * 50)

# 14) 调用pop()函数删除lst列表中尾部的元素，返回删除的元素；
pop_num = lst.pop()
print(f"删除的元素为：{pop_num}")
print("-" * 50)

# 15) lst列表中用append()函数追加数值83，并输出。调用remove()函数删除lst列表中第一个数值83；
lst.append(83)
lst.remove(83)
print(f"修改后，列表为：{lst}")
print("-" * 50)

# 16) 创建2个列表lst1和lst2，lst1中包含2个元素值：78，91，lst2中包含3个元素值：84，92，65，合并这两个列表，并输出全部元素；
lst1 = [78, 91]
lst2 = [84, 92, 65]
lst1.extend(lst2)
print(f"合并后列表元素为：{lst1}")
print("-" * 50)

# 17) 创建lst1列表，其中包含数值2个元素值：78，91，将lst1中元素赋值5遍保存在lst2列表中，输出lst2列表中全部元素;
lst1 = [78, 91]
lst2 = lst1 * 5
print(f"lst2中的元素为：{lst2}")
print("-" * 50)

# 18) 清空lst列表，将lst2列表复制给lst列表，将lst列表中第2个元素变为2，并分别输出lst列表、lst2列表全部元素
lst.clear()
lst = lst2.copy()
lst[1] = 2
print(f"lst列表元素为：{lst}")
print(f"lst2列表元素为：{lst2}")
```

- **根据products列表写一个循环，不断询问用户想买什么，用户选择一个商品编号，就把对应的商品添加到购物车里，最终用户输入q退出时，打印购买的商品列表。注意：本题可以自由发挥**

  ```python
  products = [["iphone",6888],["MacPro",14800],["小米6",2499],["Coffee",31],["Book",60],["Nike",699]]
  ```

```python
products = [["iphone", 6888], ["MacPro", 14800], ["小米6", 2499], ["Coffee", 31], ["Book", 60], ["Nike", 699]]
buy_products = []

while True:
    print("当前商品菜单有：")
    for item in range(len(products)):
        print(f"{item + 1}号商品：{products[item][0]}，\t价格{products[item][1]}")
    buy_num = input("请输入您要购买的商品编号，（输入q退出菜单）：")
    if buy_num == "q":
        print("添加商品结束")
        break
    elif buy_num.isdigit():
        buy_num = int(buy_num)
        if 1 <= buy_num <= 6:
            buy_products.append(products[buy_num - 1])
            print(f"您已添加{buy_num}号商品{products[buy_num - 1][0]}一份")
        else:
            print(f"选择错误，超出范围，请重新选择")
    else:
        print("输入错误，请重新输入")

money = 0
total = len(buy_products)
if len(buy_products) < 1:
    print("您未添加任何商品")
else:
    print("您的购物清单为：")
    for item in range(len(buy_products)):
        print(f"第{item + 1}样商品：{buy_products[item][0]}，\t价格{buy_products[item][1]}")
        money += buy_products[item][1]
    print(f"总计{total}样产品，一共需要{money}元")
```

