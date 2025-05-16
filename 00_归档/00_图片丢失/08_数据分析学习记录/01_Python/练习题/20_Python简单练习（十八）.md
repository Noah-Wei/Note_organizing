#### 一、已知有文件test.txt里面的内容如下，查找文件中以1000phone开头的语句，并保存到列表中

```
1000phone hello python
mobiletrain 大数据
1000phone java
mobiletrain html5
mobiletrain 云计算
```

```python
import re


# 读取文件
def read_file(path):
    with open(path, "r", encoding="utf-8") as f1:
        r1 = f1.readlines()
        # print(r1)
    return r1


# 查找元素
def findall(readline: list):
    new_list = []
    for item in readline:
        # 去除元素尾的换行符
        item = item.strip("\n")
        # print(item)
        
        r = re.match(r"^1000phone", item)
        # 三目运算符添加元素
        new_list.append(item) if r else None
        
    return new_list


if __name__ == '__main__':
    path = r"data/test.txt"
    all_content = read_file(path)
    result = findall(all_content)
    print(result)
```

#### 二、提取用户输入数据中的数值 (数值包括正负数 还包括整数和小数在内) 并求和 

```
例如:“-3.14good87nice19bye” =====> -3.14 + 87 + 19 = 102.86
```

```python
import re


def get_sum(input_str: str):
    r = re.findall(r"-?\d+(?:\.\d+)?", input_str)
    print(r)
    total = sum(float(number) for number in r)
    return total


if __name__ == '__main__':
    input_str = input("请输入一串字符串：")
    result = get_sum(input_str)
    print(result)
```

