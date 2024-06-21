# Spider练习（二）：使用BS4抓取豆瓣电影T250

## 一、需求

使用BS4提取豆瓣电影Top250，1-10页的电影数据，包括'电影名称', '详情页地址', '上映年份','国家','类型', '评分','评论人数'，并将结果保存到Excel文件中

网址：https://movie.douban.com/top250

## 二、代码

**说明：**

- 获取多页数据，即翻页，一般情况下，需要借助循环，找到url的规律进行拼接

- 注意事项2：避免请求太快，导致服务器将程序识别为攻击网站的行为
  - 可以设置每运行一次，让程序休眠，但是休眠时间不要固定，需要调用模块time
- 数据持久化
  - 方式一：使用csv模块写入csv
  - 方式二：使用openpyxl模块写入excel
- 获取到的数据，一个类型对应一个列表
  - 可以借助map函数：`list(map(lambda 元素1,元素2,元素3:[元素1,元素2,元素3],列表1,列表2,列表3))`，然后借助for循环，写入数据
  - 或者使用zip()函数

### 2.1 select()方式

```python
# -*- coding: utf-8 -*-
# @Time : 2023/12/2 21:45
# @Author : Eden Wei
# @FileName: demo1_bs4解析豆瓣电影Top250一.py
# @Software: PyCharm 2023.2.4 (Professional Edition)
# 导入包
import random

import requests, openpyxl, time
from bs4 import BeautifulSoup


# 1.获取数据
def get_data():
    # 注意事项1：要实现翻页，一般情况下，需要借助循环，找到url的规律进行拼接
    for page in range(10):
        url = f"https://movie.douban.com/top250?start={page * 25}&filter="
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                          'Chrome/119.0.0.0 Safari/537.36'
        }

        # 注意事项2：避免请求太快，导致服务器将程序识别为攻击网站的行为
        # 可以设置每运行一次，让程序休眠，但是休眠时间不要固定，需要调用模块time
        time.sleep(random.randint(3, 6))

        resp = requests.get(url, headers=headers)
        if resp.status_code == 200:
            print("获取成功")
            resp.encoding = "utf-8"
            # print(resp.text)
            parser_data(resp.text)
        else:
            print(f"抓取失败，状态码：{resp.status_code}")


# 2.解析数据
# '电影名称', '详情页地址', '上映年份','国家','类型', '评分'，'评论人数'
def parser_data(html):
    soup = BeautifulSoup(html, 'html.parser')
    # a.获取电影的名称
    """
    xx:first-child  表示匹配符合条件的第一个标签
    xx:last-child  表示匹配符合条件的最后个标签
    xx:nth-child(n)  表示匹配符合条件的第N个标签
    """
    names_tag = soup.select("div.hd > a > span:first-child")
    names = [tag.text for tag in names_tag]
    print(names)

    # b.详情页的地址
    details_tag = soup.select("div.hd > a")
    details = [tag.attrs["href"] for tag in details_tag]
    print(details)

    # c.电影上映年份，国家，类型
    infos_tag = soup.select("div.bd > p:first-child")
    # print(infos_tag)
    """
    <p class="">
                            导演: 陈凯歌 Kaige Chen   主演: 张国荣 Leslie Cheung / 张丰毅 Fengyi Zha...<br/>
                            1993 / 中国大陆 中国香港 / 剧情 爱情 同性
                        </p>,
    """
    # 注意：tag.text的本质是一个字符串，所以在设计到数据处理时，都是对字符串进行操作
    infos = [tag.text.split("\n")[-2].strip() for tag in infos_tag]
    year_country_type = [info.split("\xa0/\xa0") for info in infos]
    # print(year_country_type)
    years = [ele[0] for ele in year_country_type]
    print(years)
    countrys = [ele[1] for ele in year_country_type]
    print(countrys)
    types = [ele[2] for ele in year_country_type]
    print(types)

    # d.评分
    scores_tag = soup.select(".rating_num")
    scores = [tag.text for tag in scores_tag]
    print(scores)

    # e.评论人数
    comments_tag = soup.select(".star > span:last-child")
    comments = [tag.text for tag in comments_tag]
    print(comments)

    # f.评价
    say_tag = soup.select("p.quote > span")
    says = [tag.text for tag in say_tag]
    print(says)

    # # 数据整合
    # data_list = list(
    #     map(lambda ele1, ele2, ele3, ele4, ele5, ele6, ele7, ele8: [ele1, ele2, ele3, ele4, ele5, ele6, ele7, ele8],
    #         names, details,
    #         years, countrys, types, scores, comments, says))
    # print(data_list)

    save_data(names, details, years, countrys, types, scores, comments, says)


# 3.保存数据
def save_data(names, details, years, countrys, types, scores, comments, says):
    # zip()函数：将传递序列中的数据进行重组
    zip_data = list(zip(names, details, years, countrys, types, scores, comments, says))
    for row_data in zip_data:
        sheet.append(row_data)


if __name__ == '__main__':
    # 数据持久化
    # 方式一：使用csv模块写入csv
    # 方式二：使用openpyxl模块写入excel

    # 创建工作簿，创建工作簿的同时会创建一个工作表
    wb = openpyxl.Workbook()
    # 确定活动的工作簿，默认创建的工作表名位“Sheet”
    sheet = wb.active
    sheet.append(["电影名称", "详情页地址", "年份", "国家", "类型", "评分", "评论人数", "评语"])
    get_data()
    wb.save("豆瓣电影Top250.xlsx")
```

### 2.2 select_one()方式

```python
"""
需求：使用bs4提取豆瓣电影Top250 1-10页
【https://movie.douban.com/top250】的电影数据
包括'电影名称', '详情页地址', '上映年份','国家','类型', '评分'，'评论人数'，
并将结果保存到Excel文件中
"""
import os.path

# 导入包
import requests, openpyxl
from bs4 import BeautifulSoup


# 1.获取数据
def get_data(num):
    url = "https://movie.douban.com/top250?start=" + str(num) + "&filter="
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36'
    }
    resp = requests.get(url, headers=headers)
    if resp.status_code == 200:
        print("获取成功")
        resp.encoding = "utf-8"
        # print(resp.text)
        parser_data(resp.text)
    else:
        print(f"抓取失败，状态码：{resp.status_code}")


# 2.解析数据
# '电影名称', '详情页地址', '上映年份','国家','类型', '评分'，'评论人数'
def parser_data(html):
    # 创建bs4对象
    soup = BeautifulSoup(html, 'html.parser')

    # 获取表示所有电影的li标签
    li_list = soup.select('ol.grid_view > li')

    data_list = []

    for li in li_list:
        # 电影名称，后代选择器
        movies_name = li.select_one("div.hd  span.title").text
        # print(movies_name)

        # 详情页地址
        movies_link = li.select_one("div.hd > a").attrs['href']
        # print(movies_link)

        # 上映年份
        # 国家
        # 类型
        """
        <p class="">
                            导演: 弗兰克·德拉邦特 Frank Darabont   主演: 蒂姆·罗宾斯 Tim Robbins /...<br/>
                            1994 / 美国 / 犯罪 剧情
                        </p>
        <class 'bs4.element.Tag'>
        先获取整个p标签内容，然后获取br标签到p标签之间的内容，然后拆分并赋值
        """
        # 获取整个p标签内容
        movies_time_country_type = li.select_one("div.bd > p")
        # print(movies_time_country_type)
        # print(type(movies_time_country_type))

        # 获取br标签的下一个兄弟节点的内容，并通过strip()去除多余的空白字符。
        br_tag = movies_time_country_type.select_one('br')
        # print(br_tag)
        movies_three = br_tag.nextSibling.strip()
        # print(movies_three)  # 1987 / 英国 意大利 中国大陆 法国 / 剧情 传记 历史

        # 拆分字符串
        movies_three_list = movies_three.split(" / ")

        # 解包赋值
        movies_time, movies_country, movies_type = movies_three_list
        # print(movies_time, movies_country, movies_type)

        # 评分
        movies_score = li.select_one("div.star > span.rating_num").text
        # print(movies_score)

        # 评论人数,父子选择器+相邻选择器
        movies_comment_num = li.select_one("div.star > span.rating_num + span +span").text
        # print(movies_comment_num)

        # 整合数据
        data_list.append(
            [movies_name, movies_link, movies_time, movies_country, movies_type, movies_score, movies_comment_num])
    # print(data_list)

    # 调用保存数据函数
    save_data(data_list)


# 3.保存数据
def save_data(data_list):
    name_file = "豆瓣电影Top250.xlsx"
    # 判断文件是否存在
    if not os.path.exists(name_file):
        print("文件不存在")
        # 创建工作簿,同时会自动创建一张工作表
        wb = openpyxl.Workbook()
        print(wb.sheetnames)
        # 修改工作表名
        sheet = wb["Sheet"]
        sheet.title = "豆瓣电影Top250"
        # 增加表头
        sheet.append(['电影名称', '详情页地址', '上映年份', '国家', '类型', '评分', '评论人数'])
        # 添加数据
        for item in data_list:
            sheet.append(item)
    else:
        print("文件存在")
        # 加载已存在的文件
        wb = openpyxl.load_workbook(name_file)
        # 获取最后活动表
        sheet = wb.active
        # 写入内容
        for item in data_list:
            sheet.append(item)
    # 最后保存工作簿
    wb.save('豆瓣电影Top250.xlsx')


if __name__ == '__main__':
    num_list = [0, 25, 50, 75, 100, 125, 150, 175, 200, 225]
    for item in num_list:
        get_data(item)

```

