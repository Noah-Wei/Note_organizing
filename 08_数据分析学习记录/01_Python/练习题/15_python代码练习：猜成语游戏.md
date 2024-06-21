# python代码练习：猜成语游戏

## 题目

成语填填乐，随机输出一条包含一个空格的成语，填写答案并判断是否正确，正确加2分，输出“正确，你真棒”，错误减2分，输出“错了”，显示正确答案，什么也不填，则表式 忽略本成语，输出“过”。本游戏一共8关，游戏完成输出成绩，选手初始分数为20分。

随机可以使用内置模块random来实现

## 结果展示

![image-20230705140937829](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/07/05/64a50920ddfaa.png)

## 源代码

```python
# -*- coding: utf-8 -*-
# @Course : python 基础
# @Time : 2023/7/5 13:57
# @Author : Eden Wei
# @FileName: 作业2.py
# @Software: PyCharm 2022.1.3 (Professional Edition)

import random

t = ('春䁔花开', '十字路口', '千军万马', '白手起家', '张灯结彩', '风和日丽', '万里长城', '人来人往',
     '自由自在', '瓜田李下', '助人为乐', '红男绿女', '春风化雨', '马到成功', '拔苗助长', '安居乐业',
     '走马观花', '念念不忘', '落花流水', '一往无前', '落地生根', '天罗地网', '东山再起', '一事无成',
     '山清水秀', '别有洞天', '语重心长', '水深火热', '鸟语花香', '自以为是')
i = 1  # 初始化答题的次数
count = 20  # 初始化积分
print('直接填写答案，回车进入下一关，什么也不甜忽略本成语')
while True:
    word = random.choice(t)  # 随机选择一个
    bank = random.randint(0, 3)
    new = word[:bank] + '__' + word[bank + 1:]
    print(new)
    num = input('请输入缺失的字：\t')
    if num == '':
        print('过')
        continue
    elif num == word[bank]:
        count += 2
        print('正确，你真棒')
    else:
        count -= 2
        print('错误，正确答案：', word[bank])
    i += 1
    if i > 8:
        break
print('游戏结束')
print('选手最后得分', count)

```

