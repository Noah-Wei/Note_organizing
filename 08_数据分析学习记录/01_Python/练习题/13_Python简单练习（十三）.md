# Python简单练习（十三）

- **封装一个函数，验证一个年是否是闰年**

```
闰年的条件：
	1. 能被4整除但是不能被100整除 
	2. 能被400整除
	3. 条件1和条件2 满足一个即可
```

```python
leap_year = lambda year: True if (year % 4 == 0 and year % 100 != 0) or year % 400 == 0 else False
print(leap_year(200))
```

- **封装一个函数，获取指定月的天数**

```python
注意： 
	1. 闰年和平年下  
    2. 2月份的天数是不一样的
```

```python
def isleapyear(year):
    if (year % 4 == 0 and year % 100 != 0) or year % 400 == 0:
        return 29
    return 28


def get_day(year, month):
    if month in [1, 3, 5, 7, 8, 10, 12]:
        return 31
    elif month in [4, 6, 9, 11]:
        return 30
    else:
        # if (year % 4 == 0 and year % 100 != 0) or year % 400 == 0:
        #     return 29
        # else:
        #     return 28
        isleapyear(year)


day = get_day(2023, 10)
print(day)
```

- **封装一个函数，获取指定月所属的季节**

```
3、4、5——春季
6、7、8——夏季
9、10、11——秋季
12、1、2——冬季
```

```python
def choice_season(month):
    if month in [3, 4, 5]:
        return '春季'
    elif month in [6, 7, 8]:
        return '夏季'
    elif month in [9, 10, 11]:
        return '秋季'
    return '冬季'


season = choice_season(10)
print(f"10月季节是：{season}")
```

- **封装一个函数，验证指定数是否是质数**

```
质数：
	在大于1的自然数中，除了1和它本身以外不再有其他因数的自然数。
```

```python
def judge_quality(x):
    if type(x) == int:
        if x < 2:
            return False
        else:
            for n in range(2, x):
                if x % n == 0:
                    return False
            else:
                return True
    else:
        return False
p =  judge_quality(12.5)
print(p)
```

- **封装一个函数，验证一个数是否是回文**

```
回文： 
	颠倒过来 与 自身数据一样的称为回文
	例如 111  121  1221 12321
```

```python
def ishuiwen(num):
    new_num = int(str(num)[::-1])
    if new_num == num:
        return True
    return False


result = ishuiwen(1121)
print(result)
```

- **封装一个函数，获取多个数中的最大值和平均值**

```python
def get_max_avg(*args):
    max_num = max(args)
    avg_num = sum(args) / len(args)
    return max_num, avg_num


result = get_max_avg(1, 2, 3, 4, 5, 6, 7, 8)
print(result)
```

- **封装一个函数，获取多个数中的平均值并统计其中高于平均数的值个数**

```python
def get_avg_count(*args):
    count = 0
    avg_num = sum(args) / len(args)
    for i in args:
        if i > avg_num:
            count += 1
    return avg_num, count


result = get_avg_count(1, 2, 3, 4, 5, 6, 7, 8)
print(result)
```

- **封装一个函数，获取所有的水仙花数**

```
水仙花数是指一个 3 位数，它的每个位上的数字的 3次幂之和等于它本身（例如：1^3 + 5^3+ 3^3 = 153）
```

```python
def count_num():
    lst = []
    for i in range(100, 1000):
        bai_wei = i // 100
        shi_wei = i % 100 // 10
        ge_wei = i % 10
        if i == bai_wei ** 3 + shi_wei ** 3 + ge_wei ** 3:
            lst.append(i)
    return lst


print(count_num())
```



