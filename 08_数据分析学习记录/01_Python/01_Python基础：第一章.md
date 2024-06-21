# Python第一章：Python起步

## 一、Python简介

- 百度百科：
  - Python由荷兰数学和计算机科学研究学会的吉多·范罗苏姆 于1990 年代初设计，作为一门叫做ABC语言的替代品。 Python提供了高效的高级数据结构，还能简单有效地面向对象编程。Python语法和动态类型，以及解释型语言的本质，使它成为多数平台上写脚本和快速开发应用的编程语言，随着版本的不断更新和语言新功能的添加，逐渐被用于独立的、大型项目的开发。
  -  Python解释器易于扩展，可以使用C语言或C++（或者其他可以通过C调用的语言）扩展新的功能和数据类型。 Python 也可用于可定制化软件中的扩展程序语言。Python丰富的标准库，提供了适用于各个主要系统平台的源码或机器码。
  - 2021年10月，语言流行指数的编译器Tiobe将Python加冕为最受欢迎的编程语言，20年来首次将其置于Java、C和JavaScript之上。
- 简单理解：
  - 是一种跨平台的计算机程序设计语言——可以在Window，Linux，MacOS中运行
  - 是一种解释型语言——在开发过程中是没有编译这个环节的，这一点和Java不一样
  - 是一种交互式语言——可以在提示符>>>后直接执行代码
  - 是一种面向对象的语言——一切皆对象
  - 对初学者非常友好的语言——语法简单，写起来没那么复杂，支持广泛的应用程序的开发

## 二、Python开发环境的安装

> 根据自己的电脑选择合适的版本
>
> 因使用的电脑为Window 11 64 位操作系统, 基于 x64 的处理器，所以一切操作基于此版本

### 2.1 选择Python解释器

- 官网：https://www.python.org


  - 其他版本下载地址：https://www.python.org/downloads/release/python-399/

![image-20220603062613061](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/6299390df1343.png)

### 2.2 安装Python解释器

- 进入官网，下载合适的Python解释器

![image-20220603064818614](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/62993e3417a0c.png)

- 下载后，双击软件`python-3.9.9-amd64`直接安装

  选择第二项 `Customize installation`：自定安装（可选择安装路径和组件）

  并且勾选`Add Python to PATH`（添加Python解释器的安装路径到系统变量，目的：为了操作系统更快的找到Python解释器）

![image-20220603065449830](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/62993fbb55d9a.png)

- 然后，全部默认勾选，点击`next`

![image-20220603072316566](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/6299466630414.png)

- 前五项勾选，下方可选择软件安装路径M

![image-20220603072632755](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/6299472a4562a.png)

- 等待结束，关闭窗口即可

![image-20220603073102862](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/629948386072e.png)

- 验证是否成功，按win+R，输入`cmd` ，输入`Python`回车

![image-20220603073152972](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/6299486a71808.png)

- 按win,就可以在目录找到安装的Python
  -  IDLE：Python编辑器
  - Python：交互式命令行

![image-20220603092000630](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/629961c1e9e21.png)

## 三、集成开发环境PyCharm安装教程

### 3.1 PyCharm介绍

​		PyCharm是由jetbrains开发的python IDE（集成开发环境）,其具有智能代码编辑器，能理解 Python 的特性并提供卓越的生产力推进工具：自动代码格式化、代码完成、重构、自动导入和一键代码导航等。

​		PyCharm共发布两个版本：Professional Edition和Free Community Edition。

​		Professional Edition是专业版（付费），提供更加高级的扩展功能，如：Web开发、Python Web 框架、远程开发、Python 分析器、支持数据库等功能；Free Community Edition是免费社区版，基本可以胜任大部分工作。

### 3.2 安装PyCharm 专业版

> 因使用的电脑为Window 11 64 位操作系统, 基于 x64 的处理器，所以一切操作基于此版本
>
> 选择的版本是2021.3.2

- 进入官网：https://www.jetbrains.com/pycharm/

​		选择合适的版本进行下载

- 下载完成后，双击进行安装

![image-20220603093627102](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/6299659c60630.png)

- 点击`next`进入下一步，点击`Browse`选择合适的安装路径，然后再次点击`next`到下一步

![image-20220603093913767](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/62996642c118b.png)

- 在此界面中，左1为创建桌面快捷方式，右1 为添加环境变量（若不勾选，需手动添加环境变量，较麻烦），建议四个全部勾选。

![image-20220603094129970](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/629966caf11f2.png)

- 点击`next`，选择以后建立工程的地方，默认就行。在点击`install`进行安装

![image-20220603094323233](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/6299673c443c0.png)

- 耐心等待，安装完成后点击选择重新启动，或者稍后重启。注意：**这会重启你的电脑的**，所以谨慎选择

![image-20220603094702990](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/629968180d0dc.png)

- 点击`finish`即完成安装，如果你选择了重启，就会重启电脑，重启后桌面，有`PyCharm`的快捷方式，说明安装成功，打开就可以使用了。

- 到此安装结束

### 3.3 激活PyCharm

> 声明：**本教程 补丁、激活码均收集于网络，请勿商用，仅供个人学习使用，如有侵权，请联系作者删除。**

- 前言
  - 先清空以前使用的PyCharm激活方式（重中之重）
    这一步不能略过，比如：<font color=Red>修改过的hosts要还原、引用过的补丁要移除等</font>，避免很多不必要的问题
- 下载激活补丁
  - 下载地址：
    - 百度网盘：[点击下载](https://pan.baidu.com/s/1H5sKRP0Y2Id1PNI-6Dzc3Q?pwd=6iti )	提取码：6iti
- 下载之后进行解压，会看到如下的目录
  - 注意：
    - **<font color=Red>解压之后的目录文件不要移动，选择一个合适的位置存放，并且之后不要删除文件</font>**
    - scripts是放脚本文件的
    - vmoptions是放配置文件的（直接会加载这个文件夹下的vmoptions，不用大家手动配置）

![image-20220603100120745](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/62996b71c09d2.png)

- 执行脚本

  - 打开scripts 文件，可以看到有以下几个脚本
    - **<font color=Red>vbs结尾的文件都是windows系统下使用的脚本</font>**
    - <font color=Red>**sh结尾的文件是在linux和mac系统下使用的脚本**</font>
    - 从命名上来看，有install，也有对应的uninstall，非常的贴心。

  > windows系统区分多用户，所以作者也是提供了针对不同用户范围的vbs文件，
  >
  > 如果你只想要当前登录windows电脑的这个用户使用，那你就运行install-current-user.vbs脚本
  >
  > 如果你想要所有登录windows电脑的这个用户都能使用，那你就运行install-all-user.vbs脚本。

  - 以我当前Windows为例，双击执行` install-all-users.vbs` **，**点击确定

  ![image-20220603100550309](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/62996c7f4ddf0.png)

  - 这个脚本会自动配置vmoptions文件，所以耐心等待几秒钟，会弹出一个 Done的提示，这个时候就代表执行成功了。

![image-20220603100606406](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/62996c8f64ffd.png)

- 激活（两种方式）

  - 第一种：使用server

    - 执行上面的脚本之后，选择 `License server`，输入 

      - ```xml
        https://jetbra.in
        ```

    ![image-20220603101926477](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/62996faf8a670.png)

    - 然后点击`Activate`，等待数秒即可

    ![image-20220603102021916](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/62996fe6ebaf9.png)

    - 如上图所示就成功激活了，只不过LicenseName信息使用的是这台电脑上当前登录的用户名。

  - 第二种方式：使用激活码

    - 也可以选择通过激活码的方式激活，这个方法可以自定义LicenseName 以及激活时间

      **前提还是要先执行上面的脚本！**

      ```
      BA8C4XBS50-eyJsaWNlbnNlSWQiOiJCQThDNFhCUzUwIiwibGljZW5zZWVOYW1lIjoi5rC45LmF5r+A5rS7IHd3d8K3YWppaHVvwrdjb20iLCJhc3NpZ25lZU5hbWUiOiIiLCJhc3NpZ25lZUVtYWlsIjoiIiwibGljZW5zZVJlc3RyaWN0aW9uIjoiIiwiY2hlY2tDb25jdXJyZW50VXNlIjpmYWxzZSwicHJvZHVjdHMiOlt7ImNvZGUiOiJEUE4iLCJwYWlkVXBUbyI6IjIwMjItMDYtMjQiLCJleHRlbmRlZCI6ZmFsc2V9LHsiY29kZSI6IkRCIiwicGFpZFVwVG8iOiIyMDIyLTA2LTI0IiwiZXh0ZW5kZWQiOmZhbHNlfSx7ImNvZGUiOiJQUyIsInBhaWRVcFRvIjoiMjAyMi0wNi0yNCIsImV4dGVuZGVkIjpmYWxzZX0seyJjb2RlIjoiSUkiLCJwYWlkVXBUbyI6IjIwMjItMDYtMjQiLCJleHRlbmRlZCI6ZmFsc2V9LHsiY29kZSI6IlJTQyIsInBhaWRVcFRvIjoiMjAyMi0wNi0yNCIsImV4dGVuZGVkIjp0cnVlfSx7ImNvZGUiOiJHTyIsInBhaWRVcFRvIjoiMjAyMi0wNi0yNCIsImV4dGVuZGVkIjpmYWxzZX0seyJjb2RlIjoiRE0iLCJwYWlkVXBUbyI6IjIwMjItMDYtMjQiLCJleHRlbmRlZCI6ZmFsc2V9LHsiY29kZSI6IlJTRiIsInBhaWRVcFRvIjoiMjAyMi0wNi0yNCIsImV4dGVuZGVkIjp0cnVlfSx7ImNvZGUiOiJEUyIsInBhaWRVcFRvIjoiMjAyMi0wNi0yNCIsImV4dGVuZGVkIjpmYWxzZX0seyJjb2RlIjoiUEMiLCJwYWlkVXBUbyI6IjIwMjItMDYtMjQiLCJleHRlbmRlZCI6ZmFsc2V9LHsiY29kZSI6IlJDIiwicGFpZFVwVG8iOiIyMDIyLTA2LTI0IiwiZXh0ZW5kZWQiOmZhbHNlfSx7ImNvZGUiOiJDTCIsInBhaWRVcFRvIjoiMjAyMi0wNi0yNCIsImV4dGVuZGVkIjpmYWxzZX0seyJjb2RlIjoiV1MiLCJwYWlkVXBUbyI6IjIwMjItMDYtMjQiLCJleHRlbmRlZCI6ZmFsc2V9LHsiY29kZSI6IlJEIiwicGFpZFVwVG8iOiIyMDIyLTA2LTI0IiwiZXh0ZW5kZWQiOmZhbHNlfSx7ImNvZGUiOiJSUzAiLCJwYWlkVXBUbyI6IjIwMjItMDYtMjQiLCJleHRlbmRlZCI6ZmFsc2V9LHsiY29kZSI6IlJNIiwicGFpZFVwVG8iOiIyMDIyLTA2LTI0IiwiZXh0ZW5kZWQiOmZhbHNlfSx7ImNvZGUiOiJBQyIsInBhaWRVcFRvIjoiMjAyMi0wNi0yNCIsImV4dGVuZGVkIjpmYWxzZX0seyJjb2RlIjoiUlNWIiwicGFpZFVwVG8iOiIyMDIyLTA2LTI0IiwiZXh0ZW5kZWQiOnRydWV9LHsiY29kZSI6IkRDIiwicGFpZFVwVG8iOiIyMDIyLTA2LTI0IiwiZXh0ZW5kZWQiOmZhbHNlfSx7ImNvZGUiOiJSU1UiLCJwYWlkVXBUbyI6IjIwMjItMDYtMjQiLCJleHRlbmRlZCI6ZmFsc2V9LHsiY29kZSI6IkRQIiwicGFpZFVwVG8iOiIyMDIyLTA2LTI0IiwiZXh0ZW5kZWQiOnRydWV9LHsiY29kZSI6IlBEQiIsInBhaWRVcFRvIjoiMjAyMi0wNi0yNCIsImV4dGVuZGVkIjp0cnVlfSx7ImNvZGUiOiJQV1MiLCJwYWlkVXBUbyI6IjIwMjItMDYtMjQiLCJleHRlbmRlZCI6dHJ1ZX0seyJjb2RlIjoiUFNJIiwicGFpZFVwVG8iOiIyMDIyLTA2LTI0IiwiZXh0ZW5kZWQiOnRydWV9LHsiY29kZSI6IlBQUyIsInBhaWRVcFRvIjoiMjAyMi0wNi0yNCIsImV4dGVuZGVkIjp0cnVlfSx7ImNvZGUiOiJQQ1dNUCIsInBhaWRVcFRvIjoiMjAyMi0wNi0yNCIsImV4dGVuZGVkIjp0cnVlfSx7ImNvZGUiOiJQR08iLCJwYWlkVXBUbyI6IjIwMjItMDYtMjQiLCJleHRlbmRlZCI6dHJ1ZX0seyJjb2RlIjoiUFBDIiwicGFpZFVwVG8iOiIyMDIyLTA2LTI0IiwiZXh0ZW5kZWQiOnRydWV9LHsiY29kZSI6IlBSQiIsInBhaWRVcFRvIjoiMjAyMi0wNi0yNCIsImV4dGVuZGVkIjp0cnVlfSx7ImNvZGUiOiJQU1ciLCJwYWlkVXBUbyI6IjIw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
      ```

    - 也可以通过 http://idea.hicxy.com/  这个网站重新获取

      将激活码复制粘贴到如下位置，显示绿色，就证明激活码有效

      ![image-20220603102906890](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/629971f3ede96.png)

    - 然后点击 `Activate`，之后出现如下界面

    ![image-20220603102924684](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/62997205b9ba3.png)

    - 永久激活、2088年等信息可以通过修改 `config-jetbrains`文件下`mymap.conf` 自定义

    ![image-20220603103006666](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/62997230b4c1f.png)

- 至此，Pycharm激活结束，可以安全食用。

## 四、PyCharm模板注释使用

### 4.1 效果显示

在创建文件后，在文件中默认生成一串代码，声明信息

![image-20220603103904813](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/62997449d4f2c.png)

### 4.2 设置流程

​	点击`File`——`settings`——`Editor`，展开后点击`File and Code Templates`，找到`Python Script`，编写以下代码

```python
##!/usr/bin/python3
# -*- coding: utf-8 -*-
# @Course : python 基础
# @Time : ${DATE} ${TIME}
# @Author : Saudade
# @FileName: ${NAME}.py
# @Software: ${PRODUCT_NAME} 2021.3.2 (Professional Edition)
```

### 4.3 Python常用快捷键

![Pycharm-快捷键](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/6299be4a252ba.png)

## 五、Python中的输出函数

### 5.1 print()函数介绍

- python中的一个可以直接使用的函数，可以将你想展示的东西在IDLE或者标准的控制台上显示

![image-20220603104934367](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2022/06/03/629976bf76804.png)

### 5.2 print()函数的使用

- print()可以输出那些内容
  - print()函数输出的内容可以是数字
  
  ```py
  print(520)  # 520
  print(12.34)  # 12.34
  ```
  
  - print()函数输出的内容可以是字符串
  
  ```py
  print("hello World")
  print('hello world')
  ```
  
  - print()函数输出的内容可以是含有运算符的表达式
  
  ```py
  print(3 + 1)  # 3和1是操作数，+为运算符
  # 输出结果为：表达式的结果为：4
  ```
- print()函数可以将内容输出的目的地
  - 控制台显示器
  - 文件
    - 注意点
      1. 所指定的盘符需要存在
      2. 使用file=fp
  
  ```py
  fp = open("D:/text.txt", 'a+')  # 如果文件不存在，则创建；存在则就在文件内容的后面继续追加
  print("hello world", file=fp)
  fp.close()
  ```
- print()函数的输出形式
  - 换行：默认皆为换行输出
  - 不换行
  
  ```py
  print('hello', 'world', 'Python')
  # 输出结果 hello world Python
  ```

## 六、转义字符

### 6.1 什么是转义字符

- 就是**反斜杠+**想要实现的转移功能**字母**

### 6.2 为什么需要用转义字符

- 当字符串中包含**换行**、**回车**、**水平制表符**或**退格**等**无法直接表示的字符**时，也可以使用转义字符。

| 要表示的字符 | 转义字符 | 输出显示 |
| ------------ | -------- | -------- |
| 反斜杠       | \\\      | \        |
| 单引号       | \\'      | '        |
| 双引号       | \\"      | "        |

```python
print('http:\\\\www.baidu.com')
print('老师说:\'大家好\'')
print('老师说：\"老师说\"')
```

- 当字符串中包含**反斜杠**、**单引号**、和**双引号**等有**特殊用途的字符**时，必须使用反斜杠对这些字符进行转义（转换一个含义）

| 要表示的字符 | 转义字符 | 备注                                         |
| ------------ | -------- | -------------------------------------------- |
| 换行         | \n       | newline光标移动到下一行的开头                |
| 回车         | \r       | return光标移动到本行的开头                   |
| 水平制表符   | \t       | tab光标移动到一下制表位（占4个空格）的开始处 |
| 退格         | \b       | backspace光标回退一个字符                    |

```python
print('hello\nworld')  			
print('hello\tworld')  
print('hello0000\tworld')  
print('hello\rworld')  
print('hello\bworld')  			# \b光标回退一个字符
```

### 6.3 原字符

- 不希望字符串中的转义字符起作用，就是用原字符，就是在字符串之前加上r，或者R
- **注意事项**：**最后一个字符不能**是反斜杠，**最后两个字符可以**是反斜杠

```python
print(r'hello\nworld')
# 错误 print(r'hello\nworld\')
print(r'hello\nworld\\')
```
