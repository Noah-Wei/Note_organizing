# 编程题

**根据下面的需求描述，完成简单的用户管理系统，注意封装函数**

1. **后台管理员只有一个用户：admin，密码: admin**
2. **当管理员登陆账号成功后， 可以管理前台会员信息**
3. **会员信息管理包含方法:**
   1. **添加会员信息**
   2. **删除会员信息**
   3. **查看会员信息**
4. **对会员按照年龄降序排序**
5. **退出**

```
思路：
	1.输入用户名和密码跟管理员的账号密码匹配
		不一致的话，登陆失败；
		一致的话，提示登陆成功；
		并列出对应的1、2、3、4、5的操作，输入对应的编号，执行对应的方法

	2.会员信息包含：
		会员编号(mid) ---- 编号是在10000到99999中随机选择一个，不能重复
        会员姓名(name)
        会员性别(sex)
        会员年龄(age)
				
        使用字典保存每个会员信息
            例如{'mid':10000, 'name':'乐乐','sex':'男', 'age':20}
        使用列表保存所有的会员
            例如[{'mid':10000, 'name':'乐乐','sex':'男', 'age':20},{'mid':10001, 'name':'美美','sex':'女', 'age':19}]
```

```python
import random

user_lst = [{'mid': '10000', 'name': '乐乐', 'sex': '男', 'age': 20},
            {'mid': '10001', 'name': '美美', 'sex': '女', 'age': 19}]


# 登录函数
def login():
    for i in range(3):
        admin_name = input("请输入管理员用户名：")
        admin_pwd = input("请输入管理员密 码：")
        if admin_name == "admin" and admin_pwd == "admin":
            print("登录成功")
            return True
        else:
            if i == 2:
                continue
            print(f"账号或密码错误，登录失败！还剩{2 - i}机会")
    else:
        print("连续错误三次，禁止登录")


# 获取会员姓名、年龄、性别函数
def get_mes():
    while True:
        name = input("请输入会员姓名：")
        while True:
            sex = input("请输入会员性别：")
            if sex == "男" or sex == "女":
                break
            else:
                print("性输别入错误，请重新输入")
        while True:
            age = input("请输入会员年龄（18-80）：")
            if age.isdigit() and 18 <= int(age) <= 80:
                age = int(age)
                break
            else:
                print("年龄输入错误，请重新输入")
        break
    return name, sex, age


# 获取id 函数
def get_mid():
    while True:
        mid = str(random.randint(10000, 99999))
        for item in user_lst:
            if mid not in item["mid"]:
                return mid


# 添加会员信息 函数
def add_user(name, sex, age, mid):
    user_dict = dict(zip(("mid", "name", "sex", "age"), (mid, name, sex, age)))
    user_lst.append(user_dict)
    print("会员信息已添加！")


# 查找会员编号mid 函数
def search_mid():
    while True:
        search_mid = str(input("请输入操作的会员编号："))
        for item in user_lst:
            if search_mid in item["mid"]:
                return search_mid
        print(f"输入的编号{search_mid}错误或不存在，请重新输入")


# 删除会员信息 函数
def del_user(mid):
    for item in user_lst:
        if mid in item["mid"]:
            user_lst.remove(item)
    print(f"删除成功，已在数据库中删除编号{mid}的会员信息")


# 查看会员信息 函数
def search_user():
    print("当前会员信息为：")
    for item in user_lst:
        print(f"编号：{item['mid']}\t姓名:{item['name']}\t性别:{item['sex']}\t年龄:{item['age']}\t")


# 按年龄排序 函数
def sort():
    print("按照年龄降序排序：")
    user_lst.sort(key=lambda x: x['age'], reverse=True)
    search_user()


# 主菜单缓存 函数
def wait():
    input("输入任意键返回主菜单：")
    print()


# 主菜单函数
def main():
    print("欢迎使用用户管理系统")
    result = login()
    if result:
        while True:
            print(
                f"{'=' * 10}用户管理后台菜单：{'=' * 10}\n"
                f"1.添加会员信息\n"
                f"2.删除会员信息\n"
                f"3.查看会员信息\n"
                f"4.对会员按照年龄降序排序\n"
                f"5.退出\n"
                f"{'=' * 35}")
            choice = input("选择你要操作的选项（输入数字即可）：")
            if choice == "1":
                name, sex, age = get_mes()
                mid = get_mid()
                add_user(name, sex, age, mid)
                wait()
            elif choice == "2":
                search_user()
                mid = search_mid()
                del_user(mid)
                wait()
            elif choice == "3":
                search_user()
                wait()
            elif choice == "4":
                sort()
                wait()
            elif choice == "5":
                print("系统关闭，欢迎下次使用")
                break
            else:
                print("输入错误，请重新输入选项")
                wait()


main()
```

