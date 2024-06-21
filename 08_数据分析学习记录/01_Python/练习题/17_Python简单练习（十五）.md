### 面向对象编程题

- **定义Number类，对象方法有加（addition）、减（subtration）、乘（multiplication）、除（division）**
  **创建Number类的对象，调用各个方法，并显示计算结果**

```python
class Number:
    def addition(self, num1, num2):
        return num1 + num2

    def subtration(self, num1, num2):
        return num1 - num2

    def multiplication(self, num1, num2):
        return num1 * num2

    def division(self, num1, num2):
        if num2 == 0:
            return False
        return num1 / num2


result = Number()
print(result.addition(10, 20))
print(result.subtration(10, 20))
print(result.multiplication(10, 20))
print(result.division(10, 20))
print(result.division(10, 0))
```

- **分别定义Circle（圆）类和点（Point）类，计算该圆的周长和面积，并判断某点与该圆的关系**

```python
class Circle:
    PI = 3.14
    __slots__ = ("x_circle", "y_circle", "r")

    def __init__(self, x_circle, y_circle, r):
        self.x_circle = x_circle
        self.y_circle = y_circle
        self.r = r

    def perimeter(self):
        return 2 * self.PI * self.r

    def area(self):
        return self.PI * self.r ** 2

    def relation(self, point):
        a = (point.x_point - self.x_circle) ** 2 + (point.y_point - self.y_circle) ** 2
        b = self.r ** 2
        # 当(x1-a)²+(y1-b)²＞r²时，则点P在圆外。
        if a > b:
            return "在圆外"
        elif a < b:
            return "在圆内"
        return "在圆上"


class Point:
    __slots__ = ("x_point", "y_point")

    def __init__(self, x_point, y_point):
        self.x_point = x_point
        self.y_point = y_point


c = Circle(0, 0, 5)
perimeter = c.perimeter()
area = c.area()
print(f"圆心坐标位：（{c.x_circle}，{c.y_circle}）,半径为：{c.r}")
print(f"周长：{round(perimeter, 2)}；面积：{area}")

p = Point(6, 6)
result = c.relation(p)
print(f"点坐标位：（{p.x_point}，{p.y_point}）")
print(result)
```

- **有一个银行账户类 Account, 包括名字 , 余额等属性，方法有存钱、取钱、查询余额的操作。要求：**	

```
1.在存钱时，注意存款数据的格式
2.取钱时，要判断余额是否充足，余额不够的时候要提示余额不足
3.为了防止余额丢失，余额需要私有化
```

```python
class Account:
    __slots__ = ("name", "__money",)

    def __init__(self, name, __money):
        self.name = name
        self.__money = __money

    # 存钱函数
    def save_money(self, num):
        self.__money += num
        self.inquire()

    # 取钱函数
    def take_money(self, num):
        if num > self.__money:
            print("余额不足")
            self.inquire()
        else:
            self.__money -= num
            print("取出成功")
            self.inquire()

    # 查询函数
    def inquire(self):
        print(f"当前余额为：{self.__money}")
        self.wait()

    # 主菜单缓存 函数
    def wait(self):
        input("输入任意键返回主菜单：")
        print()


# 主菜单函数
def main():
    user = Account("张三", 10000)
    while True:
        print(
            f"{'=' * 10}银行账户操作菜单：{'=' * 10}\n"
            f"1.存钱操作\n"
            f"2.取钱操作\n"
            f"3.查询余额\n"
            f"4.退出系统\n"
            f"{'=' * 35}")
        choice = input("选择你要操作的选项（输入数字即可）：")
        if choice == "1":
            num = int(input("请输入要存的金额:"))
            user.save_money(num)

        elif choice == "2":
            num = int(input("请输入要取的金额:"))
            user.take_money(num)
        elif choice == "3":
            user.inquire()

        elif choice == "4":
            print("系统关闭，欢迎下次使用")
            break
        else:
            print("输入错误，请重新输入选项")
            user.wait()


main()
```

- **按照要求实现下面的功能**


```
学生类Student: 
		属性:学号，姓名，年龄，性别，成绩

班级类 Grade:
 		属性:班级名称，班级中的学生 【使用列表存储学生】 

		方法:
			1.查看该班级中的所有学生的信息 
			2.查看指定学号的学生信息
			3.查看班级中成绩不及格的学生信息 
			4.将班级中的学生按照成绩降序排序
```

```python
class Student:
    __slots__ = ("id", "name", "age", "sex", "score")

    def __init__(self, id, name, age, sex, score):
        self.id = id
        self.name = name
        self.age = age
        self.sex = sex
        self.score = score


class Grade:

    # 初始化
    def __init__(self, gname, slist):
        self.gname = gname
        self.slist = slist

    # 查看所有
    def view_all(self):
        for student in self.slist:
            self.view_id(student.id)

    # 查看指定
    def view_id(self, stu_id):
        for item in self.slist:
            if item.id == stu_id:
                print(f"学号：{stu_id}，姓名：{item.name}，年龄：{item.age}，性别:{item.sex}，成绩:{item.score}")
                return
        print("学号不存在")

    # 查找不及格
    def search(self):
        for student in self.slist:
            if student.score < 60:
                self.view_id(student.id)

    def sorted(self):
        self.slist.sort(key=lambda student: student.score, reverse=True)
        self.view_all()


def main():
    while True:
        s1 = Student(1001, "张三", "18", "男", 10)
        s2 = Student(1002, "李四", "19", "男", 90)
        s3 = Student(1003, "王五", "20", "女", 50)
        s4 = Student(1004, "赵六", "17", "男", 80)
        s5 = Student(1005, "小明", "16", "女", 60)
        s6 = Student(1006, "小白", "18", "男", 20)

        grade = Grade("Python", [s1, s2, s3, s4, s5, s6])

        print(
            f"{'=' * 10}学生信息管理：{'=' * 10}\n"
            f"1.查看该班级中的所有学生的信息\n"
            f"2.查看指定学号的学生信息\n"
            f"3.查看班级中成绩不及格的学生信息\n"
            f"4.将班级中的学生按照成绩降序排序\n"
            f"5.退出\n"
            f"{'=' * 35}")
        choice = input("选择你要操作的选项（输入数字即可）：")
        if choice == "1":
            print(f"{grade.gname}班中的所有学生的信息为：")
            grade.view_all()
        elif choice == "2":
            id = int(input("请输入你要查询的学号:"))
            grade.view_id(id)
        elif choice == "3":
            print("成绩不及格的学生信息为：")
            grade.search()
        elif choice == "4":
            print("按照成绩降序排序为：")
            grade.sorted()
        elif choice == "5":
            print("系统关闭，欢迎下次使用")
            break
        else:
            print("输入错误，请重新输入选项")


if __name__ == '__main__':
    main()
```

