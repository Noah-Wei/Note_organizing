## 编程题（一）

- **已知字典 dic = {"k1": "v1", "k2": "v2", "k3": "v3"}，实现以下功能**
  1. **遍历字典 dic 中所有的key**
  2. **遍历字典 dic 中所有的value**
  3. **循环遍历字典 dic 中所有的key和value**
  4. **添加一个键值对"k4","v4",输出添加后的字典 dic**
  5. **删除字典 dic 中的键值对"k1","v1",并输出删除后的字典 dic**
  6. **删除字典 dic 中 'k5' 对应的值，若不存在，使其不报错，并返回None**
  7. **获取字典 dic 中“k2”对应的值**
  8. **已知字典dic2 = {'k1':"v111",'a':"b"} 编写程序，使得dic2 = {'k1':"v111",'k2':"v2",'k3':"v3",a':"b"}**

```python
dic = {"k1": "v1", "k2": "v2", "k3": "v3"}
# a.遍历字典 dic 中所有的key
for key in dic.keys():
    print(key)
print("-" * 50)

# b.遍历字典 dic 中所有的value
for value in dic.values():
    print(value)
print("-" * 50)

# c.循环遍历字典 dic 中所有的key和value
for key, value in dic.items():
    print(key, value)
print("-" * 50)

# d.添加一个键值对"k4","v4",输出添加后的字典 dic
dic["k4"] = "v4"
print(f"修改后，字典为：{dic}")
print("-" * 50)

# e.删除字典 dic 中的键值对"k1","v1",并输出删除后的字典 dic
dic.pop("k1")
print(f"修改后，字典为：{dic}")
print("-" * 50)

# f.删除字典 dic 中 'k5' 对应的值，若不存在，使其不报错，并返回None
for key in dic.keys():
    if key == "k5":
        dic.pop("k5")
        break
else:
    print(None)
print("-" * 50)

# g.获取字典 dic 中“k2”对应的值
value = dic["k2"]
print(f"字典 dic 中“k2”对应的值为{value}")
print("-" * 50)

# h.已知字典dic2 = {'k1':"v111",'a':"b"} 编写程序，使得dic2 = {'k1':"v111",'k2':"v2",'k3':"v3",a':"b"}
dic2 = {'k1': "v111", 'a': "b"}
dic2.update(dic)
dic2.pop("k4")
print(f"修改后，dic2为：{dic2}")
print("-" * 50)
```

- **已知列表list1 = [11, 22, 11, 33, 44, 55, 66, 55, 66]，统计列表中每个元素出现的次数，生成一个字典，结果为{11:2,22:1.....}**

```python
list1 = [11, 22, 11, 33, 44, 55, 66, 55, 66]
dict1 = {}

for item in list1:
    if item not in dict1.keys():
        dict1[item] = list1.count(item)
print(dict1)
```

- **已知如下列表students，在列表中保存了6个学生的信息，根据要求完成下面的题目**：

  1. **统计不及格学生的个数**
  2. **打印不及格学生的名字和对应的成绩**
  3. **统计未成年学生的个数**
  4. **打印手机尾号是8的学生的名字**
  5. **打印最高分和对应的学生的名字**
  6. **删除性别不明的所有学生**

  ```python
  students = [
      {'name': '小花', 'age': 19, 'score': 90, 'gender': '女', 'tel':
          '15300022839'},
      {'name': '明明', 'age': 20, 'score': 40, 'gender': '男', 'tel':
          '15300022838'},
      {'name': '华仔', 'age': 18, 'score': 90, 'gender': '女', 'tel':
          '15300022839'},
      {'name': '静静', 'age': 16, 'score': 90, 'gender': '不明', 'tel':
          '15300022428'},
      {'name': 'Tom', 'age': 17, 'score': 59, 'gender': '不明', 'tel':
          '15300022839'},
      {'name': 'Bob', 'age': 18, 'score': 90, 'gender': '男', 'tel':
          '15300022839'}
  ]
  ```

```python
# a.统计不及格学生的个数
# b.打印不及格学生的名字和对应的成绩
num1 = 0
for item in students:
    for key, value in item.items():
        if key == "score" and value <= 60:
            print(f"学生姓名为：{item['name']}，成绩为{value}")
            num1 += 1
print(f"不及格学生的个数为：{num1}人")
print("-" * 50)

# c.统计未成年学生的个数
num2 = 0
for item in students:
    for key, value in item.items():
        if key == "age" and value <= 18:
            num2 += 1
print(f"未成年学生的个数为：{num2}人")
print("-" * 50)

# d.打印手机尾号是8的学生的名字
print("手机尾号是8的学生有:")
for item in students:
    if item["tel"][-1] == "8":
        print(f"学生姓名：{item['name']}，手机号码：{item['tel']}")
print("-" * 50)

# e.打印最高分和对应的学生的名字
stu_score = []
for item in students:
    stu_score.append(item["score"])
max_score = max(stu_score)
for item in students:
    if item["score"] == max_score:
        print(f"最高分:{max_score},学生为：{item['name']}")
print("-" * 50)

# f.删除性别不明的所有学生
for item in students[:]:
    if item["gender"] == "不明":
        students.remove(item)
print(f"修改后，列表为：{students}")
```

- **根据列表推导式完成下面的题目：**

  - **生成一个存放1-100之间个位数为3的数据列表**

    ```python
    结果为 [3, 13, 23, 33, 43, 53, 63, 73, 83, 93]
    ```

  - **利用列表推导式将已知列表中的整数提取出来**

    ```python
    例如：[True, 17, "hello", "bye", 98, 34, 21] --- [17, 98, 34, 21]
    ```

  - **利用列表推导式存放指定列表中字符串的长度**

    ```python
    例如 ["good", "nice", "see you", "bye"] --- [4, 4, 7, 3]
    ```

  - **生成一个列表，其中的元素为'0-1'，'1-2'，'2-3'，'3-4'，'4-5'**

```python

# a. 生成一个存放1-100之间个位数为3的数据列表
lst1 = []
for i in range(1, 101):
    if i % 10 == 3:
        lst1.append(i)
print(f"生成的列表为：{lst1}")
print("-" * 50)

# b. 利用列表推导式将已知列表中的整数提取出来
lst2 = [True, 17, "hello", "bye", 98, 34, 21]
new_lst2 = [i for i in lst2 if type(i) == int]
print(f"生成新列表为：{new_lst2}")
print("-" * 50)

# c.利用列表推导式存放指定列表中字符串的长度
lst3 = ["good", "nice", "see you", "bye"]
new_lst3 = [len(i) for i in lst3]
print(f"生成新列表为：{new_lst3}")
print("-" * 50)

# d.生成一个列表，其中的元素为'0-1'，'1-2'，'2-3'，'3-4'，'4-5'
lst4 = [str(i) + "-" + str(i + 1) for i in range(5)]
print(f"生成新列表为：{lst4}")
print("-" * 50)
```

## 编程题（二）

- **写一个学生作业情况录入并查询的小程序**
  1. **录入学生作业情况：字典添加**
  2. **查看学生作业情况：字典查询**
  3. **录入时允许输入3次，3次输入不正确提示失败次数过多，禁止继续录入**

```python
# stu_dict = {
#     "张三": {"Java": "提交", "Python": "提交", "Go": "提交"},
#     "王五": {"Java": "提交", "Python": "提交", "Go": "提交"},
#     "赵六": {"Java": "提交", "Python": "提交", "Go": "提交"}
# }

stu_dict = {}  # 主字典
homework = {}  # 二级字典
print(stu_dict)
while True:
    choice = input("请输入你的选择：按1录入学生作业情况，按2查看学生作业情况，按q退出：")
    if choice == "q":
        print("查看结束，欢迎使用。")
        break
    elif choice == "1":
        name = input("请输入学生姓名：")

        # 录入Java状态
        for i in range(3):
            j_choice = input("请输入java作业的提交状态：1为提交，2为未提交：")
            if j_choice == "1" or j_choice == "2":
                homework["Java"] = "提交"
                if j_choice == "2":
                    homework["Java"] = "未提交"
                stu_dict[name] = homework
                print("录入完成")
                break
            else:
                if i == 2:
                    continue
                print("输入错误，请重新输入")
        else:
            print("3次输入不正确提示失败次数过多，禁止继续录入")
            break

        # 录入Python状态，重复操作
        for i in range(3):
            p_choice = input("请输入python作业的提交状态：1为提交，2为未提交：")
            if p_choice == "1" or p_choice == "2":
                homework["Python"] = "提交"
                if p_choice == "2":
                    homework["Python"] = "未提交"
                print("录入完成")
                break
            else:
                if i == 2:
                    continue
                print("输入错误，请重新输入")
        else:
            print("3次输入不正确提示失败次数过多，禁止继续录入")
            break

        # 录入Go状态，重复操作
        for i in range(3):
            g_choice = input("请输入go作业的提交状态：1为提交，2为未提交：")
            if g_choice == "1" or g_choice == "2":
                homework["go"] = "提交"
                if g_choice == "2":
                    homework["go"] = "未提交"
                print("录入完成")
                break
            else:
                if i == 2:
                    continue
                print("输入错误，请重新输入")
        else:
            print("3次输入不正确提示失败次数过多，禁止继续录入")
            break

    elif choice == "2":  # 查询操作
        if len(stu_dict) == 0:
            print("当前未录入学生信息，无法查看")
        else:
            for key1, value1 in stu_dict.items():
                print(f"学生：{key1}，作业信息如下：")
                for kay2, value2 in value1.items():
                    print(f"课程：{kay2}，作业状态：{value2}", end="\t")
                print()
    else:
        print("输入错误，请重新输入")

```

- **dict_list = [{“科目”:“政治”, “成绩”:98}, {“科目”:“语文”, “成绩”:77}, {“科目”:“数学”, “成绩”:99}, {“科目”:“历史”, “成绩”:65}]，去除列表中成绩小于70的字典**
  **【列表推导式完成】**

  ```python
  结果为： [{“科目”:“政治”, “成绩”:98}, {“科目”:“语文”, “成绩”:77}, {“科目”:“数学”, “成绩”:99}]
  ```

```python
dict_list = [{"科目": "政治", "成绩": 98},
             {"科目": "语文", "成绩": 77},
             {"科目": "数学", "成绩": 99},
             {"科目": "历史", "成绩": 65}]
# 去除列表中成绩小于70的字典 【列表推导式完成】
new_list = [item for item in dict_list if item["成绩"] > 70]
print(f"修改后，列表为：{new_list}")
```
