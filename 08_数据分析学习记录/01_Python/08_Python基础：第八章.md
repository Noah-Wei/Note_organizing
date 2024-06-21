# Python第八章：Python中的字符串

## 一、字符串的驻留机制

### 1.1 字符串

字符串是Python中的基本数据类型，是一个**不可变**的字符序列

### 1.2 什么叫字符串驻留机制

- 仅保存一份相同且不可变字符串的方法，不同的值被存放在字符串的驻留池中，Python的驻留机制对相同的字符串只保留一份拷贝，后续创建相同的字符串时，不会开辟新的空间，而是把该字符串的地址赋给新创建的变量

**图示**
![image-20230505174515883](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/05/05/6454d06ac4dd4.png)

**代码演示**

```python
"""
    字符串的驻留机制
"""
a = 'Python'
b = "Python"
c = '''Python'''
print(a, id(a))  # 内存地址相同
print(b, id(b))
print(c, id(c))
```

### 1.3 驻留机制的几种情况（交互模式）

> sys中的intern方法强制2个字符串指向同一对象
>
> PyCharm对字符串进行了优化处理

- 字符串的长度为 0 或 1 时
- 符合标识符的字符串
- 字符串只在编译时进行驻留，而非运行时
- [-5,,256]之间的整数数字

## 二、字符串的常用操作

### 2.1 字符串的查询操作

**常用方法**

| 方法名称 | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| index()  | 查找子串第一次出现的位置，如果查找的子串不存在时，则抛出异常ValueError |
| rindex() | 查找子串最后一次出现的位置，如果查找的子串不存在时，则抛出异常ValueError |
| find()   | 查找子串第一次出现的位置，如果查找的子串不存在时，则返回-1   |
| rfind()  | 查找子串最后一次出现的位置，如果查找的子串不存在时，则返回-1 |

**代码演示**

```python
s = 'hello,hello'

print('1.', s.index('lo'))
print('2.', s.find('lo'))
print('3.', s.rindex('lo'))
print('4.', s.rfind('lo'))

# print('5.', s.index('lo0')) # 抛出异常
print('6.', s.find('lo0'))
print('7.', s.rfind('lo0'))
```

### 2.2 字符串的大小写转换

> 会产生新的字符串对象
>
> 即使转换之后字符串和之前的内容相同，id还是不同

| 方法名称     | 作用                                                         |
| ------------ | ------------------------------------------------------------ |
| upper()      | 把字符串中所有字符都转成大写字母                             |
| lower()      | 把字符串中所有字符都转成小写字母                             |
| swapcase()   | 把字符串中所有大写字母转成小写字母，所有小写字母转换成大写字母 |
| capitalize() | 把第一个字符转成大写，把其余字符转换成小写                   |
| title()      | 把每个单词的第一个字符转换成大写，把单个剩下的字母转换成小写 |

**代码演示**

```python
s = 'hello,python'
print('0.', s, id(s))

a = s.upper()
print('1.', a, id(a))

b = s.lower()
print('2.', b, id(b))

s2 = 'hello,Python'
c = s2.swapcase()
print('3.', c)

d = s2.title()
print('4.', d)
```

### 2.3 字符串的内容对其操作

| 方法名称 | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| center() | 居中对其<br />第一个参数指定宽度，第二个参数指定填充符（可选项，默认为空格）<br />如果设置的宽度小于实际宽度，则返回原字符串 |
| ljust()  | 左对齐<br />第一个参数指定宽度，第二个参数指定填充符（可选项，默认为空格）<br />如果设置的宽度小于实际宽度，则返回原字符串 |
| rjust()  | 右对齐<br />第一个参数指定宽度，第二个参数指定填充符（可选项，默认为空格）<br />如果设置的宽度小于实际宽度，则返回原字符串 |
| zfill()  | 左对齐<br />右边用 0 填充，该方法只接受一个参数，用于指定字符串的宽度<br />如果设置的宽度小于等于字符串的长度，则返回原字符串 |

**代码演示**

```python
s = 'hello,Python'
print('原字符：', s)
print('中对齐：', s.center(20, '*'))
print('中对齐：', s.center(10, '*'))
print('左对齐：', s.ljust(20, '*'))
print('右对齐：', s.rjust(20, '*'))
print('右对齐：', s.zfill(20))
```

### 2.4 字符串内容劈分操作

| 方法名称 | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| split()  | 从字符串的左边开始劈分，默认的劈分符是空格字符串，返回的值都是一个列表<br />可以通过参数`sep`指定劈分符<br />通过参数`maxslpit`指定最大劈分次数，在经过最大次劈分之后，剩余的子串会单独作为一部分 |
| rsplit() | 从字符串的右边开始劈分，默认的劈分符是空格字符串，返回的值都是一个列表<br />可以通过参数`sep`指定劈分符<br />通过参数`maxslpit`指定最大劈分次数，在经过最大次劈分之后，剩余的子串会单独作为一部分 |

**代码演示**

```python
s = 'hello world Python'
lst = s.split()
print(lst)
s1 = 'hello|world|Python'
print(s1.split(sep='|'))
print(s1.split(sep='|', maxsplit=1))
print('-------------------------------')
'''rsplit()从右侧开始劈分'''
print(s.rsplit())
print(s1.rsplit('|'))
print(s1.rsplit(sep='|', maxsplit=1))
```

### 2.5 字符串的判断操作

| 方法名称       | 作用                                                         |
| -------------- | ------------------------------------------------------------ |
| isidentifier() | 判断指定的字符串是不是合法的标识符                           |
| isspace()      | 判断指定的字符串是否全部由空白字符组成（回车、换行、水平制表符） |
| isslpha()      | 判断指定的字符串是否全部由字母组成                           |
| isdecimal()    | 判断指定字符串是否全部由十进制的数字组成                     |
| isnumeric()    | 判断指定的字符串是否全部由数字组成                           |
| isalnum()      | 判断指定字符穿是否全部由数字组成                             |

**代码演示**

```python
s = 'abc%'
s1 = 'hellopython'
print(s.isidentifier())  # False
print(s1.isidentifier())  # True
print('\t'.isspace())  # True
print('abc'.isalpha())  # True
print('abc1'.isalpha())  # False
print('张三'.isalpha())  # True

print('123'.isdecimal())  # True
print('123四'.isdecimal())  # False

print('123'.isnumeric())  # True
print('123四'.isnumeric())  # True
print('IIIIIIIV'.isnumeric())  # False

print('abc123'.isalnum())  # True
print('123张'.isalnum())  # True
print('123!'.isalnum())  # False
```

### 2.6 字符串的其他常用操作

| 功能         | 方法名称  | 作用                                                         |
| ------------ | --------- | ------------------------------------------------------------ |
| 字符串替换   | replace() | 第一个参数指定被替换的子串，第二个参数指定替换子串的字符串<br />该方法返回替换后得到的字符串，替换前的字符串不发生改变<br />调用该方法时可以通过第三个参数指定最大替换次数 |
| 字符串的合并 | join()    | 将列表或元祖中的字符串合并成一个字符串                       |

**代码演示**

```python
s = 'hello,Python'
print(s.replace('Python', 'Java'))
s1 = 'hello,Python,Python,Python'
print(s1.replace('Python', 'Java', 2))

lst = ['hello', 'java', 'Python']
print('|'.join(lst))
print(''.join(lst))

t = ('hello', 'Java', 'Python')
print(''.join(t))

print('*'.join('Python'))
```

## 三、字符串的比较

- **运算符**

```
>	>=	<	<=	==	!=
```

- **比较规则**
  - 首先比较两个字符串中的第一个字符，如果相等则继续比较下一个字符，依次比较下去，直到两个字符串中的字符不相等时，其比较结果就是两个字符串的比较结果，两个字符串中的所有后续字符将不再被比较

- **比较原理**
  - 两上字符进行比较时，比较的是其ordinal value(原始值),调用内置函数`ord`可以得到指定字符的ordinal value。
  - 与内置函数ord对应的是内置函数`chr`，调用内置函数chr时指定ordinal value可以得到其对应的字符
- **代码演示**

```python
rint('apple' > 'app')  # True
print('apple' > 'banana')  # False   ，相当于97>98 >False
print(ord('a'), ord('b'))
print(ord('魏'))

print(chr(97), chr(98))
print(chr(39759))

'''
    ==  与is的区别
    ==  比较的是    value   是否相等
    is  比较的是    id  是否相等
'''
a = b = 'Python'
c = 'Python'
print(a == b)  # True
print(b == c)  # True

print(a is b)  # True
print(a is c)  # True
print(id(a))  # 2204259933168
print(id(b))  # 2204259933168
print(id(c))  # 2204259933168
```

## 四、字符串的切片操作

> 字符串是不可变的类型：
>
> ​	不具备增删改的操作
>
> ​	切片操作将产生新的对象

**代码演示**

```python
s = 'hello,Python'
s1 = s[:5]  # 由于没有指定起始位置，所以从0开始切
s2 = s[6:]  # 由于没有指定结束位置，所以切到字符串的最后一个元素
s3 = '!'
newstr = s1 + s3 + s2

print(s1)
print(s2)
print(newstr)
print('--------------------')
print(id(s))
print(id(s1))
print(id(s2))
print(id(s3))
print(id(newstr))

print('------------------切片[start:end:step]-------------------------')
print(s[1:5:1])  # 从1开始截到5（不包含5），步长为1
print(s[::2])  # 默认从0 开始，没有写结束，默认到字符串的最后一个元素 ,步长为2  ，两个元素之间的索引间隔为2
print(s[::-1])  # 默认从字符串的最后一个元素开始，到字符串的第一个元素结束，因为步长为负数
print(s[-6::1])  # 从索引为-6开始，到字符串的最后一个元素结束，步长为1
```

## 五、格式化字符串

**格式化字符串的两种方式**

![image-20230506152710676](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/05/06/64560155b063f.png)

**代码演示**

```python
"""第一种：% 占位符"""
name = '小米'
age = 20
print('我叫%s,今年%d岁' % (name, age))

"""第二种方法 {} 占位符 """
print('我叫{0}，今年{1}岁'.format(name, age))

"""第三种方法 f-string方法"""
print(f'我叫{name},今年{age}岁')
```

**对精度的表示**

```python
print('%10d' % 99)  # 10表示宽度
print('%f' % 3.1415926)
# 保留三位小数
print('%.3f' % 3.1415926)
# 同时设置宽度和精度：总宽度为10，小数点为3位
print('%10.3f' % 3.1415926)

print('{0}'.format(3.1415936))
print('{0:.3}'.format(3.1415936))  # .3表示一共是三位
print('{0:.3f}'.format(3.1415936))  # .3f表示是三位小数
# 同时设置宽度和精度：总宽度为10，小数点为3位
print('{0:10.3f}'.format(3.1415926))
```

## 六、字符串的编码转换

- 为什么需要字符串的编码转换

![image-20230506162649866](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/05/06/64560f49dce73.png)

- 编码与解码的方式
  - 编码：将字符串转换成二进制数据（bytes）
  - 解码：将bytes类型的数据转换成字符串类型

- 代码演示

> 爬虫部分会应用

```python
s = '天涯共此时'
# 编码
print(s.encode(encoding='GBK'))  # 在GBK格式中 一个中文占2个字节
print(s.encode(encoding='UTF-8'))  # UTF-8格式中，一个中文占3个字节

# 解码(解码格式 要和 编码格式 相同)
# byte代表一个二进制数据（字节类型数据）
byte = s.encode(encoding='GBK')
print(byte.decode(encoding='GBK'))
# print(byte.decode(encoding='UTF-8'))# 报错
```

