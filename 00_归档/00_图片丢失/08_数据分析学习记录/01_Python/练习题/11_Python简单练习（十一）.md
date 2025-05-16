## 编程题（一）

- **输入一个字符串，判断字符串中有多少个字母？多少个数字？多少个其他符号**

  ```python
  例如:'hello, nice to meet you. i am 18. my birthday is 1999-05-23'
  		-- 结果: 字母的个数为33个，数字个数为10个， 其他字符为16个
  ```

```python
str1 = "hello, nice to meet you. i am 18. my birthday is 1999-05-23"
count_str = 0
count_num = 0
count_other = 0
for i in str1:
    if i.isdigit():
        count_num += 1
    elif i in "abcdefghijklmnopqrstuvwxyz":
        count_str += 1
    else:
        count_other += 1
print(f"字母的个数为{count_str}个，数字个数为{count_num}个， 其他字符为{count_other}个")
```

- **以下是一段歌词，请从这段歌词中统计出朋友出现的次数。**

  ```
  “这些年一个人，风也过，雨也走，有过泪，有过错, 还记得坚持甚么，真爱过才会懂，会寂寞会回首，终有梦终有你在心中。朋友一生一起走，那些日子不再有，一句话，一辈子，一生情，一杯酒。朋友不曾孤单过，一声朋友你会懂，还有伤，还有痛，还要走，还有我。”
  ```

```python
str1 = ("这些年一个人，风也过，雨也走，有过泪，有过错, 还记得坚持甚么，真爱过才会懂，会寂寞会回首，终有梦终有你在心中。"
        "朋友一生一起走，那些日子不再有，一句话，一辈子，一生情，一杯酒。"
        "朋友不曾孤单过，一声朋友你会懂，还有伤，还有痛，还要走，还有我。")
count = str1.count("朋友")
print(f"朋友出现的次数为：{count}")
```

- **编写敏感词过滤程序**
  **说明：在网络程序中，如聊天室、聊天软件等，经常需要对一些用户所提交的聊天内容中的敏感性词语进行过滤。如“性”、“色情”、“爆炸”、“恐怖”、“枪”、“军火”等，这些都不可以在网上进行传播，要求输入一段文本，如果包含以上的敏感词汇，需要*替换掉**

  ```
  例如：“军火走私” ---- 结果为 "**走私"
  ```

```python
# str1 = ("说明：在网络程序中，如聊天室、聊天软件等，经常需要对一些用户所提交的聊天内容中的敏感性词语进行过滤。"
#         "如“性”、“色情”、“爆炸”、“恐怖”、“枪”、“军火”等，这些都不可以在网上进行传播，")
word = ["性", "色情", "爆炸", "恐怖", "枪", "军火"]
str1 = input("请输入内容：")
for item in word:
    if item in str1:
        str1 = str1.replace(item, "*" * len(item))
print(f"修改后，内容为：{str1}")
```

- **输入一个用户名，判断用户名是否合法。用户名要求：由英文字母或数字组成，长度是6到12位**

  ```
  例如：‘abcd’ --- 不合法  "123456" -- 合法 ‘ABC23’ -- 不合法   ‘ABC123’ -- 合法
  “abc 124” --- 不合法
  ```

```python
import string

name = input("请输入用户名：")
if 6 <= len(name) <= 12:
    for i in name:
        if i in string.digits or i in string.ascii_letters:
            continue
        else:
            print(f"用户名：{name}，内容不合法")
            break
    else:
        print(f"用户名：{name}，内容合法")
else:
    print(f"用户名：{name}，长度不合法")
```

- **随机生成长度为5的验证码， 验证码的组成是英文字母或者数字**

```python
import random
import string

all_str = string.digits + string.ascii_letters
yzm = ""
for i in range(5):
    str1 = all_str[random.choice(range(len(all_str)))]
    yzm += str1
print(yzm)

```

- **实现将字符串  "1,2,3"   变成列表 ["1","2","3"]**

```python
str1 = "1,2,3"
list1 = str1.split(",")
print(list1)
```

- **判断输入的字符串是否是 .py 结束**

```python
str1 = input("请输入字符串：")
result = str1.endswith(".py")
print(f"符串是否是 .py 结束：{result}")
```

## 编程题（二）

- **输入一个字符串，将字符串中所有的数字符取出来产生一个新的字符串** 

  ```
  例如： 输入: 'abc1shj23kls99+2kkk'   输出： '123992'
  ```

```python
str1 = 'abc1shj23kls99+2kkk'
new_str = ""
for i in str1:
    if i.isdigit():
        new_str += i
print(f"新字符串为：{new_str}")
```

- **输入用户名，判断用户名是否合法，用户名的要求：必须由数字和字母且只能有数字和字母，并且第一个字符是大写字母**

  ```
  例如：  'Abc'  — 不合法    '123'  — 不合法   'abc123'  — 不合法    'Abc123ahs' — 合法
  ```

```python
import string

num_count = 0
str_count = 0
name = input("请输入用户名：")
if name[0] in string.ascii_uppercase:
    for i in name:
        if i in string.digits:
            num_count += 1
            continue
        if i in string.ascii_letters:
            str_count += 1
            continue
        else:
            print(f"用户名：{name}，内容不合法")
            break
    if num_count != 0 and str_count != 0:
        print(f"用户名：{name}，内容合法")
    else:
        print(f"用户名：{name}，内容不合法")
else:
    print(f"用户名：{name}，不合法,第一个字符不是大写字母")
```

- **输入两个字符串，打印两个字符串中公共的字符，如果没有公共字符打印 公共字符不存在**

  ```
  例如： 字符串1为 abc123a , 字符串2为  huak33 , 打印 a3
  ```

```python
str1 = " abc123a "
str2 = "  huak33 "
common_list = []

for i in str2:
    if i in str1:
        common_list.append(i)

common_str = "".join(set(common_list))
print(common_str)
```

- **如下字符串:  "01#张三#60-02#李四#90-03#王五#70", 每一部分表示  学号#姓名#分数，提取学生信息存放于列表中，列表形式如下：**

  ```
  结果显示为:
  [{"学号":'02', '姓名':'李四', '分数':90}, {"学号":'03', '姓名':'王五', '分数':70}, {"学号":'01', '姓名':'张三', '分数':60}]
  ```

```python
stu_str = "01#张三#60-02#李四#90-03#王五#70"
stu_list = []
for item in stu_str.split("-"):
    stu_dict = {
        "学号": item.split("#")[0],
        "姓名": item.split("#")[1],
        "分数": item.split("#")[2],
    }
    stu_list.append(stu_dict)
print(stu_list)
```

