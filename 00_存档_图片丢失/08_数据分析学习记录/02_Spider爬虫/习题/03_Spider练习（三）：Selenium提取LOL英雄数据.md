# Spider练习（三）：Selenium提取LOL英雄数据

## 一、需求

使用Selenium提取数据，从英雄联盟首页进入 https://lol.qq.com/main.shtml   点击英雄资料 ，进入到英雄列表，然后保存

"英雄名称", "英雄职业", "英雄技能", "英雄难度", "英雄胜率", "英雄Ban率", "英雄登场率", "英雄位置"等信息，

难度 ：一条线设置为容易  两条线设置为中等  三条线设置为难】

## 二、代码

```python
# 导入模块
import time

import openpyxl
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By


# 1.获取数据
def get_data():
    # (1)打开浏览器
    url = "https://lol.qq.com/main.shtml"
    browser = webdriver.Chrome()
    browser.maximize_window()
    browser.get(url)
    time.sleep(2)

    # (2)进入英雄列表
    game_info_tag = browser.find_element(by=By.XPATH, value="//ul[@class='head-nav']/li/a")
    game_info_tag.click()
    # 手动切换，显示窗口
    browser.switch_to.window(browser.window_handles[-1])
    time.sleep(2)

    # (3)获取所有的英雄列表
    hero_lists = browser.find_elements(by=By.XPATH, value="//ul[@class='hero-list']/li/a")
    print(len(hero_lists))

    # 调用解析函数
    parser_data(browser, hero_lists)

    browser.close()


# 2.解析数据
def parser_data(browser, hero_lists):
    hero_messages = []
    # 遍历英雄列表，准备获取每一位英雄的数据
    # for i in range(10):
    for i in range(len(hero_lists)):
        # (4)进入英雄详情界面
        hero_li = browser.find_element(by=By.XPATH, value=f"//ul[@class='hero-list']/li[{i + 1}]/a")
        hero_li.click()
        time.sleep(3)

        """开始获取英雄数据"""
        # 1:"英雄名称"
        hero_name = browser.find_element(by=By.XPATH, value="//p[@class='hero-name']").text
        print(hero_name)
        # 2:"英雄职业"
        hero_types_tags = browser.find_elements(by=By.XPATH, value="//div[@class='hero-type']")
        hero_types = "-".join([hero_type.text for hero_type in hero_types_tags])
        print(hero_types)
        # 3:"英雄技能"
        hero_skills_tags = browser.find_elements(by=By.XPATH, value="//div[@class='spells-list']/div/img")
        hero_skills = "-".join([skill_tag.get_attribute("alt") for skill_tag in hero_skills_tags])
        print(hero_skills)
        # 4:"英雄难度"
        hero_difficulty_tags = browser.find_elements(by=By.XPATH,
                                                     value="//ul[@class='info-list']/li[2]/div/i[@class='light']")
        if len(hero_difficulty_tags) == 1:
            hero_difficulty = "容易"
        elif len(hero_difficulty_tags) == 2:
            hero_difficulty = "中等"
        else:
            hero_difficulty = "困难"
        print(hero_difficulty)
        # 5："英雄胜率"
        hero_win = browser.find_element(by=By.XPATH, value="//ul[@class='info-list']/li[3]/div").text
        print(hero_win)
        # 6:"英雄Ban率"
        hero_ban = browser.find_element(by=By.XPATH, value="//ul[@class='info-list']/li[4]/div").text
        print(hero_ban)
        # 7:"英雄登场率"
        hero_show = browser.find_element(by=By.XPATH, value="//ul[@class='info-list']/li[5]/div").text
        print(hero_show)
        # 8:"英雄位置"
        hero_positions_tags = browser.find_elements(by=By.XPATH, value="//div[@class='filter-box']/div[2]/a")
        hero_positions = "/".join([position.text for position in hero_positions_tags])
        print(hero_positions)
        """获取结束"""

        # 整合数据
        hero_messages.append(
            [hero_name, hero_types, hero_skills, hero_difficulty, hero_win, hero_ban, hero_show, hero_positions])
        # 返回上一页
        browser.back()
        time.sleep(3)

    # 写入数据
    save_data(hero_messages)


# 3.保存数据
def save_data(data_list):
    for item in data_list:
        sheet.append(item)


if __name__ == '__main__':
    wb = openpyxl.Workbook()
    sheet = wb.active
    sheet.append(["英雄名称", "英雄职业", "英雄技能", "英雄难度", "英雄胜率", "英雄Ban率", "英雄登场率", "英雄位置"])
    get_data()
    wb.save("LOL英雄数据.xlsx")

```

