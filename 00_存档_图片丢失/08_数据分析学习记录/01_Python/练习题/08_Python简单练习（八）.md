## 编程题（一）

- **已知列表names = ['old_driver', 'rain', 'jack', 'shanshan', 'peiqi', 'black_girl']**
  - **往names列表里black_girl前面插入一个alex**
  - **把shanshan的名字改成中文，姗姗**
  - **往names列表里rain的后面插入一个子列表，[oldboy, oldgirl]**
  - **返回peiqi的索引值**
  - **创建新列表[1,2,3,4,2,5,6,2],合并入names列表**
  - **取出names列表中索引4-7的元素**
  - **取出names列表中索引2-10的元素，步长为2**
  - **取出names列表中最后3个元素**
  - **循环names列表，打印每个元素的索引值，和元素，当索引值 为偶数时，把对应的元素改成-1**

```python
names = ['old_driver', 'rain', 'jack', 'shanshan', 'peiqi', 'black_girl']
# a.往names列表里black_girl前面插入一个alex
index1 = names.index("black_girl")
names.insert(index1, "alex")
print(names)
print("-" * 50)

# b.把shanshan的名字改成中文，姗姗
index2 = names.index("shanshan")
names[index2] = "珊珊"
print(names)
print("-" * 50)

# c.往names列表里rain的后面插入一个子列表，[oldboy, oldgirl]
index3 = names.index("rain")
names.insert(index3 + 1, ["oldboy", "oldgirl"])
print(names)
print("-" * 50)

# d.返回peiqi的索引值
index4 = names.index("peiqi")
print(f"peiqi的索引值为：{index4}")
print("-" * 50)

# e.创建新列表[1,2,3,4,2,5,6,2],合并入names列表
new_list = [1, 2, 3, 4, 2, 5, 6, 2]
names.extend(new_list)
print(names)
print("-" * 50)

# f.取出names列表中索引4-7的元素
for index in range(len(names)):
    if 3 < index < 8:
        print(f"索引为{index}的元素是：{names[index]}")
print("-" * 50)

# g.取出names列表中索引2-10的元素，步长为2
length = len(names)
for index in range(0, length, 2):
    if 1 < index < 11:
        print(f"索引为{index}的元素是：{names[index]}")
print("-" * 50)

# h.取出names列表中最后3个元素
for index in [-1, -2, -3]:
    print(names[index])
print("-" * 50)

# i.循环names列表，打印每个元素的索引值，和元素，当索引值 为偶数时，把对应的元素改成-1
for index in range(len(names)):
    if index % 2 == 0:
        names[index] = -1
    print(f"索引为{index}的元素是：{names[index]}")

```

- **已知一个列表names = ['鲁班七号', '后裔', '狄仁杰', '黄忠', '孙尚香']，编写程序用两种方法获取names中的元素黄忠**

```python
names = ['鲁班七号', '后裔', '狄仁杰', '黄忠', '孙尚香']
# 方式一
index = names.index("黄忠")
print(f"列表中的第{index + 1}个元素为黄忠")

# 方式二
for index in range(len(names)):
    if names[index] == "黄忠":
        print(f"列表中的第{index + 1}个元素为黄忠")
```

- **已知一个数字列表nums = [1, 2, 3,1, 4, 2, 1 ,3, 7, 3, 3]，输出索引为奇数的元素**

```python
nums = [1, 2, 3, 1, 4, 2, 1, 3, 7, 3, 3]
for index in range(len(nums)):
    if index % 2 != 0:
        print(f"索引{index}为奇数，元素为：{nums[index]}")
```

- **已知一个列表 scores = [90, 89, 67, 98, 75, 87, 54, 88]，从控制台输入两个成绩，一个添加到scores的最后，另一个插入到scores的最前面**

```python
scores = [90, 89, 67, 98, 75, 87, 54, 88]
print(f"原列表：{scores}")
num1 = float(input("请输入第一个成绩："))
num2 = float(input("请输入第二个成绩："))
scores.append(num1)
scores.insert(0, num2)
print(f"添加后列表：{scores}")
```

## 编程题（二）

- **已知一个数字列表nums = [1, 2, 5, 9]，根据该列表生成一个新的列表，其中的元素是nums中每个元素的2倍**
  **例如：nums = [1, 2, 5, 9]   ->  nums = [2, 4, 10, 18]**

```python
nums = [1, 2, 5, 9]
new_list = [item * 2 for item in nums]
print(new_list)
```

- **自定义一个数字列表，获取该列表中元素的最小值，注意: 自己实现，不能使用min函数**


```python
num_list = [115, 34, 7865, 123, 3456, 23, 56, 123]
num_list.sort()
print(f"排序后，列表为：{num_list}")

num = num_list[0]
print(f"列表中最小的元素是：{num}")
```

- **已知列表list1 = ['mon','sun','sat','fri','thu','wed'],list2 = ['sun','wed','thu']，将属于list2的元素从list1中删除**


```python
list1 = ['mon', 'sun', 'sat', 'fri', 'thu', 'wed']
list2 = ['sun', 'wed', 'thu']
print(f"原list1列表为：{list1}")

for item in list2:
    if item in list1:
        list1.remove(item)

print(f"修改后list1列表为：{list1}")
```

- **用户输入月份,判断这个月是哪个季节，提示：先用列表定义季节包含的月份，然后再判断**

   ```
   分析：
   3，4，5月----春季  6，7，8----夏季   9，10，11---秋季  12，1，2----冬季 
   ```

```python
season_list = [["春", 3, 4, 5], ["夏", 6, 7, 8], ["秋", 9, 10, 11], ["冬", 12, 1, 2]]
while True:
    num = input("请输入月份，用数字表示：")
    if num.isdigit():
        num = int(num)
        if 1 <= num <= 12:
            for season in season_list:
                for month in season:
                    if month == num:
                        index = season_list.index(season)
                        print(f"{num}月为{season_list[index][0]}季")
            break
        else:
            print("输入错误，请重新输入")
    else:
        print("输入错误，请重新输入")
```

