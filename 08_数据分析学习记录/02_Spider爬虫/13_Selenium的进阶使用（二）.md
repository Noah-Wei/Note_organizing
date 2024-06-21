# Selenium的进阶使用（二）

## 二、Selenium进阶使用

### 2.1 窗口的切换

#### 2.1.1 切换到显式窗口

显式窗口：顾名思义，通过按钮的点击，可以直接打开一个新的窗口

前面提到点击事件`click()`，是在原窗口中更改网页，所以放打开一个新的窗口是，需要用到`browser.switch_to.window(browser.window_handles[n])`，这里`browser.window_handles`返回的是一个列表，其中的元素是逐次打开的窗口的标识，也就是说，只要打开一个新的窗口，就会将窗口标识追加到`browser.window_handles`列表中

##### 需求

从`王者荣耀的首页`，进入到`英雄列表界面`，再进入到`英雄详情页面`，获取英雄的技能信息，一位英雄获取完毕之后，在返回到`英雄列表界面`，再进入到下一位`英雄详情页面`，获取英雄的技能信息，直到所有英雄全部获取结束

##### 分析

- **第一步：驱动打开浏览器，加载首页 **

```python
url = "https://pvp.qq.com/"
browser = webdriver.Chrome()
browser.maximize_window()
# 加载王者荣耀首页
browser.get(url)
time.sleep(3)
```

- **第二步：定位到【游戏资料】标签，进入英雄列表**

这里就需要注意了，这里会新开一个窗口，所以我们要切换窗口，`click`不会自动切换窗口，需要手动切换，所以需要调用到`browser.switch_to.window(browser.window_handles[n])`，这里`browser.window_handles`返回的是一个列表，其中的元素是逐次打开的窗口的标识，也就是说，只要打开一个新的窗口，就会将窗口标识追加到`browser.window_handles`列表中

```python
game_info_tag = browser.find_element(by=By.XPATH, value="//ul[@class='main-nav clearfix']/li[1]/a")
# 进行点击行为
game_info_tag.click()
# 结果是一个列表，其中存储的是打开窗口的表示信息。新打开的窗口对应的标识信息会被追加到该列表的末尾
print(browser.window_handles)  

browser.switch_to.window(browser.window_handles[-1])
time.sleep(2)
```

- **第三步：获取所有的英雄列表**

进入到英雄列表页面，我们就能够获取所有的英雄标签了。

```python
hero_lists = browser.find_elements(by=By.CSS_SELECTOR, value="ul.herolist > li > a")
```

然后就可以遍历列表，准备获取英雄数据，之所以说是准备，是因为，在这里，还是新开一个窗口，我们我们还需要切换窗口

```python
for hero in hero_lists:
    # 4.点击英雄标签，进入英雄的详情界面
    hero.click()
    # 切换窗口
    browser.switch_to.window(browser.window_handles[-1])
    time.sleep(2)
```

- **第四步：获取英雄的技能名称**

这里技能的名称是隐藏数据，正好前面解释了如何获取隐藏数据，可以直接照搬之前的代码

```python
# 开始获取英雄的技能
# 获取浏览器的事件对象
action = ActionChains(browser)
# 定位到所有技能的图标标签
skill_img_tags = browser.find_elements(by=By.XPATH, value="//ul[@class=skill-u1]/li[not(@class='no5)']")
# 遍历每个技能图标标签，
for i in range(len(skill_img_tags)):
    # 获取每个图标的li
    skill_img_li = skill_img_tags[i]
    # 给鼠标添加移动操作
    action.move_to_element(skill_img_li).perform()
    time.sleep(2)
    # 获取技能名称
    skill_name = browser.find_element(by=By.XPATH,
                                      value=f"//div[@class='skill-show']/div[{i + 1}]/p[@class='skill-name']/b").text
```

- **第五步：关闭英雄详情页面的窗口**

```
browser.close()
```

- **第六步：切换窗口回去**

注意：此时我们在英雄详情页面中，窗口为第三个（第一个为：官网首页，第二个为：英雄列表页面）。我们需要返回英雄列表页面，在列表`browser.window_handles`对应的索引为1

```python
browser.switch_to.window(browser.window_handles[1])
time.sleep(2)
```

到这里，一次循环才结束

最后在关闭窗口即可

##### 完整代码

```python
import time

import openpyxl
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By


def get_data():
    # 1.通过驱动打开浏览器
    url = "https://pvp.qq.com/"
    browser = webdriver.Chrome()
    browser.maximize_window()
    # 加载王者荣耀首页
    browser.get(url)
    time.sleep(3)

    # 2.定位到【游戏资料】标签，进入英雄列表
    game_info_tag = browser.find_element(by=By.XPATH, value="//ul[@class='main-nav clearfix']/li[1]/a")
    # 进行点击行为
    game_info_tag.click()
    # 结果是一个列表，其中存储的是打开窗口的表示信息。新打开的窗口对应的标识信息会被追加到该列表的末尾
    print(browser.window_handles)  # ['4A10E2FD6D3CB39BEBD0BEAE83B1BDC3', '9C5CA0E171DC637B5C8694857E826FB8']
    # 浏览器打开了一个新的窗口，进入到新的窗口才会会去到英雄信息
    # 上面的click不会自动切换窗口，需要手动切换
    browser.switch_to.window(browser.window_handles[-1])
    time.sleep(2)

    # 3.获取所有的英雄列表
    # hero_lists = browser.find_elements(by=By.XPATH, value="//ul[@class='herolist clearfix']/li/a")
    hero_lists = browser.find_elements(by=By.CSS_SELECTOR, value="ul.herolist > li > a")

    # 遍历列表，准备获取英雄数据
    for hero in hero_lists:
        # 4.点击英雄标签，进入英雄的详情界面
        hero.click()
        # 切换窗口
        browser.switch_to.window(browser.window_handles[-1])
        time.sleep(2)

        # 开始获取英雄的技能
        # 获取浏览器的事件对象
        action = ActionChains(browser)
        # 定位到所有技能的图标标签
        skill_img_tags = browser.find_elements(by=By.XPATH, value="//ul[@class=skill-u1]/li[not(@class='no5)']")
        # 遍历每个技能图标标签，
        for i in range(len(skill_img_tags)):
            # 获取每个图标的li
            skill_img_li = skill_img_tags[i]
            # 给鼠标添加移动操作
            action.move_to_element(skill_img_li).perform()
            time.sleep(2)
            # 获取技能名称
            skill_name = browser.find_element(by=By.XPATH,
                                              value=f"//div[@class='skill-show']/div[{i + 1}]/p[@class='skill-name']/b").text

        # 5.英雄详情页数据获取完毕之后，则关闭窗口
        browser.close()

        # 6.一个英雄的数据获取完毕，则需要回归到英雄的列表页面，点击下一个英雄，直到所有的英雄获取完毕
        # 窗口切换回去，在当前的代码中
        browser.switch_to.window(browser.window_handles[1])
        time.sleep(2)

    # 关闭浏览器
    browser.close()


if __name__ == '__main__':
    get_data()
```

#### 2.1.2 隐式窗口

**隐式窗口**：就是一个窗口是在当前文档中显示的，无法通过find_element定位

`<ifram>`标签里面的内容捕捉不到，属于一个隐式窗口

##### 需求

获取qq邮箱的“密码登录”标签，并且点击进去，然后再回到初始页面

##### 分析

观察网页元素可以发现，“密码登录”标签被嵌套在二层iframe中，二iframe标签里是隐式窗口，所以我们要切换进入隐式窗口

##### 完整代码

```python
import time

from selenium import webdriver
from selenium.webdriver.common.by import By

browser = webdriver.Chrome()
url = "https://mail.qq.com/"
browser.get(url)
time.sleep(2)

# 密码登录
# 问题：通过当前文档对象无法定位到密码登录的标签，会报错
# pwd_login = browser.find_element(by=By.ID, value="switcher_plogin")
"""上诉代码报错，定位不到"""
# 原因：我们看到的虽然是一个窗口，但是实际上html文档不是同一个，所以如果要操作，则需要进行切换，找到内容所在的html文档【内嵌窗口】

# 获取第一个iframe
iframe_1 = browser.find_element(by=By.CSS_SELECTOR, value=".QQMailSdkTool_login_loginBox_qq_iframe")
# 手动切换到该文档上
browser.switch_to.frame(iframe_1)

# 在第一个iframe的基础上，获取第二个iframe
iframe_2 = browser.find_element(by=By.ID, value="ptlogin_iframe")
# 手动切换到该iframe中
browser.switch_to.frame(iframe_2)

# 定位指定的标签
pwd_login = browser.find_element(by=By.ID, value="switcher_plogin")
pwd_login.click()
time.sleep(2)

# 想要回到原始的html文档，也就是最外层的html文档
browser.switch_to.default_content()

# 测试：是否回到最外层
a_tag_text = browser.find_element(by=By.CSS_SELECTOR, value=".header_logo").text
print(a_tag_text)
```

### 2.2  滚动获取数据

滚动获取数据，这个涉及鼠标的操作

`ActionChains`：鼠标对象

#### 2.2.1 常用方法

一下方法都在**`ActionChains`：鼠标对象**中

|                           方法                            |                             描述                             |
| :-------------------------------------------------------: | :----------------------------------------------------------: |
|                  click(on_element=None)                   |                       左键点击某个元素                       |
|              click_and_hold(on_element=None)              |                    左键点击并按住某个元素                    |
|              context_click(on_element=None)               |                       右键点击某个元素                       |
|               double_click(on_element=None)               |                     鼠标左键双击某个元素                     |
|               drag_and_drop(source, target)               |   按住源元素上的鼠标左键，然后移动到目标元素并释放鼠标按钮   |
|     drag_and_drop_by_offset(source, xoffset, yoffset)     |  按住源元素上的鼠标左键，然后移动到目标偏移量并释放鼠标按钮  |
|             move_by_offset(xoffset, yoffset)              |               将鼠标移动到当前鼠标位置的偏移量               |
|                move_to_element(to_element)                |                    将鼠标移动到元素的中间                    |
| move_to_element_with_offset(to_element, xoffset, yoffset) |   将鼠标移动指定元素的偏移量。偏移量相对于元素的视图中心点   |
|                scroll_to_element(element)                 |      如果元素在视口之外，则将元素的底部滚动到视口的底部      |
|            scroll_by_amount(delta_x, delta_y)             | 将滚轮滚动指定偏移<br />delta_x：int，横向移动的距离<br />delta_y：int，纵向移动的距离 |
|                      pause( seconds)                      |              在指定的持续时间(秒)内暂停所有输入              |
|                         perform()                         |                      执行所有存储的操作                      |
|                 release(on_element=None)                  |                  释放在元素上按住的鼠标按钮                  |
|                      reset_actions()                      |                 除已存储在本地和远程端的操作                 |

#### 2.2.2 实例

有的网站，比如：今日头条，一部分数据必须通过鼠标滚动才能加载

```python
import time

from selenium import webdriver
from selenium.webdriver import ActionChains  # 鼠标对象
from selenium.webdriver.common.by import By


def get_data():
    browser = webdriver.Chrome()
    browser.get("https://www.toutiao.com/")
    time.sleep(2)

    """滚动之前获取新闻的数量"""
    news_list = browser.find_elements(by=By.XPATH, value="//div[@class='ttp-feed-module']/div[2]/div")
    print(f"滚动前：{len(news_list)}")

    time.sleep(2)

    """滚动之后获取新闻的数量"""
    """
    # 方式一：
    # 获取新闻列表区域
    news_div = browser.find_element(by=By.CSS_SELECTOR, value=".ttp-feed-module")
    # 获取div的坐标信息
    rect = news_div.rect
    print(rect)  # {'height': 2111, 'width': 676, 'x': 123.00000762939453, 'y': 338.25}

    # 点击，移动等都属于浏览器时间对象，都封装在ActionChains中
    action = ActionChains(browser)
    # delta_x：int，横向移动的距离
    # delta_y：int，纵向移动的距离
    # 横向不变，纵向为div的高度加上坐标
    action.scroll_by_amount(delta_x=0, delta_y=int(rect.get("height") + rect.get("y"))).perform()
    """

    # 方式二：执行js代码
    # execute_script("window.scrollBy(横向移动的距离,纵向移动的距离)")
    browser.execute_script("window.scrollBy(0,2400)")

    time.sleep(2)
    # 滚动后获取新闻的数量
    news_list2 = browser.find_elements(by=By.XPATH, value="//div[@class='ttp-feed-module']/div[2]/div")
    print(f"滚动后：{len(news_list2)}")

    time.sleep(5)


if __name__ == '__main__':
    get_data()
```

### 2.3 动作链

使用Selenium操作浏览器，除了获取元素之外，也可以控制其他的一些行为

比如鼠标的操作：

- 单击
- 双击
- 移动鼠标
- 拖转
- 滚动
- 。。。

这些行为都是在封装在`ActionChains`类中，所以使用的的时候，需要先创建一个`ActionChains`对象

```python
action = ActionChains(browser)
```

然后添加行为，并且执行

```python
action.行为方法().perform()
```

这里要注意的是`action.行为方法`这个只是添加了行为，想要让该行为执行，则必须调用`perform()`

**比如：**

将鼠标移动到某个元素，然后进行点击的行为

```python
action.move_to_element(标签对象).click().perform()
```

这种语法就可以叫做**动作链**

### 2.4 截图

```python
import time

from selenium import webdriver
from selenium.webdriver.common.by import By

browser = webdriver.Chrome()
browser.get("https://www.jd.com")

time.sleep(2)

# 1.全屏截图：截取整个网页
browser.save_screenshot("jd.png")

# 2.局部截屏：
ele = browser.find_element(by=By.ID, value='J_logo_extend')
ele.screenshot('logo.png')

```

