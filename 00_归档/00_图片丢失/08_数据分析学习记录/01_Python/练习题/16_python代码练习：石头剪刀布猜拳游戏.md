# python代码练习：石头剪刀布猜拳游戏

## 题目

使用Python实现人机石头剪刀布猜拳小游戏，并且最后能够统计分数和局数

## 结果展示

![image-20230703164725215](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/07/03/64a28b1f31c34.png)

## 源代码

```python
# -*- coding: utf-8 -*-
# @Course : python 基础
# @Time : 2023/7/2 14:21
# @Author : Eden Wei
# @FileName: 石头剪刀布.py
# @Software: PyCharm 2022.1.3 (Professional Edition)
import random

player_score = 0
computer_score = 0
count = 0
print('''
* * * * * * * 欢迎来到4399游戏平台* * * * * * * 
              石头    剪刀    布               
* * * * * * * * * * * * * * * * * * * * * * * 
''')
player_name = input('请输入玩家姓名：')
print('1.貂蝉     2.曹操    3.诸葛亮')
computer_choice = input('请输入电脑角色：')
if computer_choice == '1':
    computer_choice = '貂蝉'
elif computer_choice == '2':
    computer_choice = '曹操'
elif computer_choice == '3':
    computer_choice = '诸葛亮'
else:
    print('选择错误，隐藏角色！')
    computer_choice = '匿名'
print(player_name, 'VS', computer_choice)
while True:
    count += 1;
    player_fist_choice = eval(input('----------请出拳:  1.石头   2.剪刀   3.布---------'))
    if player_fist_choice == 1:
        player_fist_name = '石头'
    elif player_fist_choice == 2:
        player_fist_name = '剪刀'
    elif player_fist_choice == 3:
        player_fist_name = '步'
    else:  # 用户输入的不是1,2,3，随机选择
        print('输入错误，随机选择')
        player_fist_choice = random.randint(1, 3)
        player_fist_name = ['石头', '剪刀', '布'][player_fist_choice - 1]
    print(player_name, '出拳：', player_fist_name)
    # 电脑出拳
    computer_fist_choice = random.randint(1, 3)
    computer_fist_name = ['石头', '剪刀', '布'][computer_fist_choice - 1]
    print(computer_choice, '出拳', computer_fist_name)

    print('我', player_fist_choice)
    print('他', computer_fist_choice)
    
    # 判断结构，谁赢谁输，有三种结果，平，输，赢
    if computer_fist_choice == player_fist_choice:
        print('平局')
    elif (player_fist_choice == 1 and computer_fist_choice == 2) or \
            (player_fist_choice == 2 and computer_fist_choice == 3) or \
            (player_fist_choice == 3 and computer_fist_choice == 1):  # 玩家赢
        print(player_name, '大获全胜，不服再战啊')
        player_score += 1
    else:
        print(computer_choice, '胜利')
        computer_score += 1
    answer = input('再来一局不？y/n')
    if answer != 'y':
        break

# 判断总比分
# 判断总比分
print('-' * 100)
print(player_name, 'VS', computer_choice)
print(f'一共对战了：{count}局')  # 第一次直播讲了
print(player_name, '得分:', player_score)
print(computer_choice, '得分:', computer_score)
if player_score == computer_score:
    print('不分上下，平分秋色')
elif player_score > computer_score:
    print(player_name, '最终胜利')
else:
    print(f'大下无敌，我是:{computer_choice}')
```

