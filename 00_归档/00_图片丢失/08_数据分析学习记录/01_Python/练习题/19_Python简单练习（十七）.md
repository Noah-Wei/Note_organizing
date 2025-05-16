1.邮编查询，读取 youbian.txt 文件中的数据， 完成邮编查询的操作，从控制台输入邮编号，如果有此邮编，请输出对应的城市，否则提示无此邮编

```python
try:
    # 邮编查询
    with open("youbian.txt", "r", encoding="utf-8") as f1:
        you_list = f1.readlines()
        print(you_list)
    youbian = int(input("请输入要查询的邮编："))
except FileNotFoundError as e:
    print("FileNotFoundError：", e)
except ValueError as e:
    print("ValueError：", e)
except LookupError as e:
    print("LookupError：", e)
finally:
    # 判断邮编
    for item in you_list:
        # print(item)
        new_list = eval(item.rstrip(",\n"))
        # print(new_list)
        if youbian in new_list:
            print(f"邮编：{youbian},地区：{new_list[1]}")
            break
    else:
        print("无此邮编")
```

2.将一个英文语句以单词为单位逆序排放到指定的文件中

```
例如：“I am a boy” 逆序排放后 "boy a am I" 将其写入到指定的文件中
```

```python
sentence = "I am a boy"

s_list = sentence.split(" ")[::-1]
sentence_new = " ".join(s_list)
print(sentence_new)
try:
    with open(r"word.txt", "w", encoding="utf-8") as f2:
        f2.write(sentence_new)
except FileNotFoundError as e:
    print("FileNotFoundError：", e)
except ValueError as e:
    print("ValueError：", e)
except LookupError as e:
    print("LookupError：", e)
finally:
    print("操作成功")
```

3.开房查询，从控制台输入名字，查询在kaifanglist.txt文件中的开房记录，如果没有，是一个单纯哥们，如果有的话，将其所有开房信息写入到以这哥们命名的文件中

```python
# 查询所有
def read_all():
    try:
        with open(r"kaifanglist.txt", "r", encoding="utf-8") as f3:
            list_all = f3.readlines()
    except FileNotFoundError as e:
        print("FileNotFoundError：", e)
    except ValueError as e:
        print("ValueError：", e)
    except LookupError as e:
        print("LookupError：", e)
    finally:
        return list_all


# 查询个人
def search_name(name):
    list_all = read_all()
    count = 0  # 统计次数
    for item in list_all:
        list_item = item.strip(",")
        if list_item[0] == name:
            count += 1
            write_name(name, item, count)
    if count == 0:
        return f"{name}是一个单纯哥们"
    return f"{name}开放了，次数：{count}次"


# 追加写入信息
def write_name(name, item, count):
    try:
        with open(rf"{name}.txt", "a", encoding="utf-8") as f3:
            f3.write(item)
    except ValueError as e:
        print("ValueError：", e)
    except LookupError as e:
        print("LookupError：", e)
    finally:
        print(f"查询到第{count}次记录，写入成功")


if __name__ == '__main__':
    name = input("请输入要查询的人姓名：")
    # name = "曹阳"

    result = search_name(name)
    print(result)
```







