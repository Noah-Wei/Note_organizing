# Selenium的进阶使用（一）

## 一、Selenium进阶使用

### 1.1 Selenium获取Scrape图书信息

需求：使用Selenium请求**Scrape图书**网站1-10页，使用`bs4`或`xpath`解析数据

获取`"书名", "评分", "标签", "价格", "作者", "发表时间", "出版商", "页数", "ISBM编号"`信息，并实现数据持久化

URL：https://spa5.scrape.center/page/1

#### 1.1.1 Requests获取图书数据

##### a. 分析

首先，打开网站，然后配合`F12`检查元素，可以发现，当前网站是一个动态数据，直接请求，拿不到数据

![image-20231207140243587](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/07/6571600ce90ce.png)

然后我们分析，网页请求`Fetch API`，在这里面有一个文件，里面存放着数据，并且是一个JSON格式

![image-20231207140305631](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/07/6571601bd37bb.png)

所以我们就可以直接请求该文件的URL

![image-20231207140509516](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/07/6571609604f77.png)

就能拿到数据，但是，我们需要更多的详细数据，这些数据存放在书籍的详情页中。在观察数据详情页的网页元素，可以发现，该网页也还是一个动态数据，文件同样存放在JSON中。但是通过刚才获取到的数据，其中有一个KEY——count，其对应的值，刚好为书籍详情信息的JSON的URL的最后一部分，从而可以拿到所有的详情页JSON的URL，从而从JSON中解析各个数据，最后保存到文件中，实现持久化。

**核心：Requests请求 + JSON解析**

##### b . 完整代码

```python
# 1.请求数据
def get_data():
    # 说明：通过给定的url发现，请求到的结果不对，从而判断当前网站的数据是动态的
    for i in range(10):
        # 动态数据，动态页面
        url = f"https://spa5.scrape.center/api/book/?limit=18&offset={0 * 18}"
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                          'Chrome/119.0.0.0 Safari/537.36'
        }
        # 请求数据
        resp = requests.get(url, headers=headers)
        if resp.status_code == 200:
            print(f"第{i + 1}页请求成功")
            books_list = resp.json().get("results")
            parse_data(books_list)


# 2.解析数据
def parse_data(data):
    book_messages = []
    # 需要的数据在详情页中，其中ID对应的值为URL的最后一部分。所以，获取详情页，在详情页中在请求数据，然后解析
    for book in data:
        # 还是动态数据，动态页面
        # 获取书籍详情页URL：实际URL前缀：https://spa5.scrape.center/api/book/
        book_url = f"https://spa5.scrape.center/api/book/{book.get('id')}"

        # 请求数据
        book_resp = requests.get(book_url)
        # 判断是否拿到数据
        if book_resp.status_code == 200:
            print(f"编号{book.get('id')}的详情页请求成功")
            book_dict = book_resp.json()

        # a.获取书名
        book_name = book_dict.get("name")
        # b.获取评分
        book_score = book_dict.get("score")
        # c.获取标签
        book_tags = "-".join(book_dict.get("tags"))
        # d.获取定价
        book_price = book_dict.get("price")
        # e.获取作者
        book_author = "/".join([item.strip() for item in book_dict.get("authors")])
        # f.出版时间
        book_published_at = book_dict.get("published_at")
        # g.获取出版社
        book_publisher = book_dict.get("publisher")
        # h.获取页数
        book_page_number = book_dict.get("page_number")
        # i.获取ISBM编号
        book_isbn = book_dict.get("isbn")

        # 整合数据
        book_messages.append(
            [book_name, book_score, book_tags, book_price, book_author, book_published_at, book_publisher,
             book_page_number, book_isbn])
    # 保存数据
    save_data(book_messages)


# 3.保存数据
def save_data(data_list):
    for item in data_list:
        sheet.append(item)


if __name__ == '__main__':
    wb = openpyxl.Workbook()
    sheet = wb.active
    sheet.append(["书名", "评分", "标签", "价格", "作者", "发表时间", "出版商", "页数", "ISBM编号"])
    get_data()
    wb.save("Scrape图书1-10页.xlsx")
```

#### 1.1.2 Selenium获取图书数据

##### a.分析

​	通过界面的分析，我么知道这是一个动态的网页数据，Selenium是web的自动化测试工具，模拟人操作浏览器的行为，我们要先打开浏览器，然后点击一本书的详情页，然后刷选数据信息，数据获取后，在返回到主网页中，继续点击下一本的数据操作，直到这一个网页中所有的图书信息获取完毕，然后点击下一页。

这里就涉及到了两个点击行为：进入下一页和返回上一页，对应的Selenium中有两个方法`click()`和`back()`

同时，每次在进入一个网站的时候，建议添加一句，`time.sleep(3)`，让程序休眠几秒，渲染网页

##### b.实现

首先程序整体可以分为三个部分和一个`mian`代码块

- 获取数据：`get_data()`
- 解析数据：`parse_data()`
- 保存数据：`save_data()`

下面对每一个部分具体分析

###### (1) 获取数据：`get_data()`

这里我们使用Selenium的方式请求数据，所以我们首先需要创建一个浏览器对象

```python
browser = webdriver.Chrome()
```

然后设置窗口最大化

```python
browser.maximize_window()
```

然后调用get()方法传入URL

```python
url = "https://spa5.scrape.center"
browser.get(url)
```

此时浏览器会执行操作，打开网页，所以我们要让程序休眠几秒，用于渲染网页

```python
time.sleep(3)
```

下面就可以调用获取数据函数：`parse_data()`，然后关闭浏览器即可

```python
parse_data(browser)
# 关闭窗口
browser.close()
```

###### (2) 解析数据：`parse_data()`

这里就负责解析数据，我们首先进入的是主网页，然后配个F12检查元素视角，定位每一本图书都在一个`div`中，

```

```

![image-20231207145126334](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/07/65716b7144d76.png)

但是，为了准确定位，我们要选择具有唯一标识的DIV，所以我们依次往上查看父类DIV，直到选择到合适的为止。

这里，我们选择的是XPATH方式解析，语法为

> 这里需要导入一个额外的模块
>
> ​	from selenium.webdriver.common.by import By

```py
"""find_element(by,value)：
find_elements(by,value):
其中by的值：
by:
    ID:id,通过标签的id属性值进行查找
    XPATH:表示根据xpath路径规则进行定位
    CSS_SELECTOR:根据css选择器进行定位
    LINK_TEXT:通过a标签的内容获取标签（只针对a标签）
value:方式对应的值"""
```

```python
# 获取所有的图书
books_list = browser.find_elements(by=By.XPATH,
                                   value="//div[@class='m-t el-row']/div/div/div")
```

获取到了每一本书的DIV，我们就可以使用for循环进行遍历，因为需要进入到详情页获取数据，所以需要点击div，

```python
# for循环，获取一页的数据
for pos in range(1, len(books_list) + 1):
    # 因为需要进入到详情页获取数据，所以点击div
    book_div = browser.find_element(by=By.XPATH,value=f"//div[@class='m-t el-row']/div/div/div[{pos}]")
    book_div.click()  # 效果相当于鼠标的左键单击
    # 休眠几秒，渲染页面
    time.sleep(3)
```

下面就可以配合检查视角，获取具体数据了

- 获取图书名称

![image-20231207162121232](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/07/6571808309ad2.png)

```python
book_name = browser.find_element(by=By.XPATH, value="//h2[@class='m-b-sm name']").text
```

- 获取图书评分

![image-20231207162201212](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/07/657180aa6519d.png)

```python
book_score = browser.find_element(by=By.XPATH, value="//span[@class='score m-r']").text
```

- 获取标签

这里需要注意，标签是多个，所以我们要使用`find_elements()`，返还是一个列表，然后使用字符串进行拼接

![image-20231207162338934](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/07/6571810c71290.png)

```python
book_tags = browser.find_elements(by=By.XPATH, value="//div[@class='tags']/button")
book_tags = "-".join([tag.text for tag in book_tags])
```

- 获取定价

![image-20231207162429244](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/07/6571813e86661.png)

```python
book_price = browser.find_element(by=By.XPATH, value="//p[@class='price']/span").text
```

- 获取作者

这里需要注意一下，标签中的文本有前缀，所以我们要去掉前缀

```python
book_author = browser.find_element(by=By.XPATH, value="//p[@class='authors']").text
book_author = book_author.split("作者：")[1].strip()
```

- 出版时间

```python
book_published_at = browser.find_element(by=By.XPATH, value="//p[@class='published-at']").text
book_published_at = book_published_at.split("出版时间：")[1].strip()
```

- 获取出版社

这里有的图书没有出版社信息，所以为了防止报错，使用异常

```python
try:
    book_publisher = browser.find_element(by=By.XPATH, value="//p[@class='publisher']").text
    book_publisher = book_publisher.split("出版社：")[1].strip()
except Exception as e:
    book_publisher = None
```

- 获取页数

这里有的图书没有页数信息，所以为了防止报错，使用异常

```python
try:
    book_page_number = browser.find_element(by=By.XPATH, value="//p[@class='page_number']").text
    book_page_number = book_page_number.split("页数：")[1].strip()
except Exception as e:
    book_page_number = None
```

- 获取ISBM编号

```python
book_isbn = browser.find_element(by=By.XPATH, value="//p[@class='isbn']").text
book_isbn = book_isbn.split("ISBM：")[1].strip()
```

到这里，数据已经获取结束，我们就可以整合数据了

```python
books_messages.append(
                [book_name, book_score, book_tags, book_price, book_author, book_published_at, book_publisher,
                 book_page_number, book_isbn])
```

然后我们需要返回上一页，继续下一次循环

```python
browser.back()
time.sleep(3)
```

到这里，一个网页中的所有的数据都获取结束，我么就需要进入到下一页中，我们可以定位，下一页的按钮

![image-20231207163818782](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/07/6571847cdbe72.png)

但是当我们采集到最后一页的时候，这个按钮就不能在使用了

![image-20231207163903158](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/07/657184a852a85.png)

所以我们可以使用一个死循环，将上面的代码放在循环中

```python
while True：
	....
	....
	....
    # 上面的for循环结束，则表示一页数据获取完毕，则需要继续进行下一页数据的获取
    next_btn = browser.find_element(by=By.XPATH, value="//button[@class='btn-next']")
    # 标签对象.get_property()：获取某个属性的值
    if next_btn.get_property("disabled"):
        print("数据获取完毕")
        break
    else:
        # 找到下一页的按钮标签，实现点击
        next_btn.click()
        time.sleep(3)
```

最后在调用保存函数即可

###### (3) 保存数据：`save_data()`

这个函数就负责保存数据，在上一个`parse_data()`函数中，我们最后获得了一个二维数组`book_messages`，所以我们只需要遍历其中的元素，并且添加到工作表中即可

```python
# 3.保存数据
def save_data(data_list):
    for item in data_list:
        sheet.append(item)
```

###### (4)`mian`代码块

> 要写入的文件一定要是关闭状态

这里就存放创建和写入文件的一些代码。首先我们要创建一张工作簿，在工作簿被创建的同时会自动创建一张工作表，然后我们获取当前活动的工作表，接着写入表单抬头信息，然后调用`get_html`函数，最后保存数据到一个路径中即可

```python
if __name__ == '__main__':
    wb = openpyxl.Workbook()
    sheet = wb.active
    sheet.append(["书名", "评分", "标签", "价格", "作者", "发表时间", "出版商", "页数", "ISBM编号"])
    get_data()
    wb.save("Scrape图书1-10页.xlsx")
```

##### c. 完整代码

```python
import time

import openpyxl
from selenium import webdriver
from selenium.webdriver.common.by import By

"""
获取多页的数据
"""


# 1.请求数据
def get_data():
    # 创建一个浏览器对象
    browser = webdriver.Chrome()
    # 设置窗口最大值
    browser.maximize_window()

    # 打开网页
    url = "https://spa5.scrape.center/page/502"
    browser.get(url)

    # 休眠几秒，渲染页面
    time.sleep(3)

    # 筛选数据
    parse_data(browser)

    # 关闭窗口
    browser.close()


# 2.获取数据
def parse_data(browser):
    books_messages = []
    # 要获取多页数据，每一个都是重复下面的操作，所以可以使用死循环解决
    while True:

        # 获取所有的图书
        books_list = browser.find_elements(by=By.XPATH,
                                           value="//div[@class='m-t el-row']/div/div/div")
        print(len(books_list))

        # 遍历本书书的div
        # for循环，获取每一本书的数据
        for pos in range(1, len(books_list) + 1):
            # 因为需要进入到详情页获取数据，所以点击div
            book_div = browser.find_element(by=By.XPATH,
                                            value=f"//div[@class='m-t el-row']/div/div/div[{pos}]")
            book_div.click()  # 效果相当于鼠标的左键单击
            # 休眠几秒，渲染页面
            time.sleep(3)

            """开始获取数据"""
            # a.获取图书名称
            book_name = browser.find_element(by=By.XPATH, value="//h2[@class='m-b-sm name']").text
            print(book_name)
            # b.获取图书评分
            book_score = browser.find_element(by=By.XPATH, value="//span[@class='score m-r']").text
            print(book_score)
            # c.获取标签，标签位多个
            book_tags = browser.find_elements(by=By.XPATH, value="//div[@class='tags']/button")
            book_tags = "-".join([tag.text for tag in book_tags])
            print(book_tags)
            # d.获取定价
            book_price = browser.find_element(by=By.XPATH, value="//p[@class='price']/span").text
            print(book_price)
            # e.获取作者
            book_author = browser.find_element(by=By.XPATH, value="//p[@class='authors']").text
            book_author = book_author.split("作者：")[1].strip()
            print(book_author)
            # f.出版时间
            book_published_at = browser.find_element(by=By.XPATH, value="//p[@class='published-at']").text
            book_published_at = book_published_at.split("出版时间：")[1].strip()
            print(book_published_at)
            # g.获取出版社,有的图书没有该信息
            try:
                book_publisher = browser.find_element(by=By.XPATH, value="//p[@class='publisher']").text
                book_publisher = book_publisher.split("出版社：")[1].strip()
            except Exception as e:
                book_publisher = None

            print(book_publisher)
            # h.获取页数，有的图书没有该信息
            try:
                book_page_number = browser.find_element(by=By.XPATH, value="//p[@class='page_number']").text
                book_page_number = book_page_number.split("页数：")[1].strip()
            except Exception as e:
                book_page_number = None
            print(book_page_number)
            # i.获取ISBM编号
            book_isbn = browser.find_element(by=By.XPATH, value="//p[@class='isbn']").text
            book_isbn = book_isbn.split("ISBM：")[1].strip()
            print(book_isbn)

            # 整合数据
            books_messages.append(
                [book_name, book_score, book_tags, book_price, book_author, book_published_at, book_publisher,
                 book_page_number, book_isbn])

            # 返回到上一页
            browser.back()
            time.sleep(3)

        # 上面的for循环结束，则表示一页数据获取完毕，则需要继续进行下一页数据的获取
        next_btn = browser.find_element(by=By.XPATH, value="//button[@class='btn-next']")
        # 标签对象.get_property()：获取某个属性的值
        if next_btn.get_property("disabled"):
            print("数据获取完毕")
            break
        else:
            # 找到下一页的按钮标签，实现点击
            next_btn.click()
            time.sleep(3)
    # 写入数据
    save_data(books_messages)


# 3.保存数据
def save_data(data_list):
    for item in data_list:
        sheet.append(item)


if __name__ == '__main__':
    wb = openpyxl.Workbook()
    sheet = wb.active
    sheet.append(["书名", "评分", "标签"])
    get_data()
    wb.save("Scrape图书数据.xlsx")
```

### 1.2 selenium获取隐藏数据

​	不管是静态数据还是动态数据，selenium都是可以获取的，但是有一点，需要说明：selenium无法直接获取隐藏数据，需要通过鼠标事件操作

这里需要用到两个函数

- 获取浏览器的事件对象

```python
action = ActionChains(browser) 
```

- 鼠标移动到某个标签并执行

```python
action.move_to_element(标签对象).perform()
```

#### 1.2.1 实例

- **需求**

需求：获取王者荣耀某一英雄的四个技能

URL：https://pvp.qq.com/web201605/herodetail/hainuo.shtm

![image-20231207185755908](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/07/6571a534d59f3.png)

- **分析**

这里四个技能的名称，每次是鼠标放在图标上，显示响应的技能名字。每次只能显示一个，如果按照之前的操作

```python
# 获取所有的技能名称
skill_bs = browser.find_elements(by=By.XPATH, value="//div[@class='skill-show']/div/p[@class='skill-name']/b")
# 问题：进入到页面之后，默认只能显示第一个技能，其他未显示的技能获取不到
skill_texts = [skill_b.text for skill_b in skill_bs]
print(skill_texts)  # 只能看到第一个技能
```

程序运行输出结果

```python
['怒潮', '', '', '', '']
```

结果只会输出一个，这是因为其他的数据属于**隐藏数据**。进入到页面之后，默认只能显示第一个技能，其他未显示的技能获取不到。

如果要获取一个英雄的所有的技能，则需要将鼠标移动到对应技能的图标上，才能获取到相关的数据。只需要移动，不需要点击

所以我们要获取一个浏览器对象

```python
action = ActionChains(browser)
```

然后定位到表示所有技能的标签，因为这里有5个li标签，但是我们只需要前4ge

```python
skill_lis = browser.find_elements(by=By.CSS_SELECTOR, value="ul.skill-u1 > li:not(.no5)")
```

这里采用了`css`方式，`value`值的意思是：在`class='skill-u1'`的`ul`标签下找到`class='no5'`除外的所有`li`标签

然后我们遍历每一个li标签

```python
for i in range(1, len(skill_lis) + 1):
```

先获取每个图标的li

```python
img_li = skill_lis[i - 1]
```

然后给鼠标添加移动操作：`action.move_to_element(标签对象).perform()`

```python
action.move_to_element(img_li).perform()
time.sleep(2)
```

到这里，我们就可以获取到技能的名称，循环遍历，则可以获取到所有的技能名字

```python
skill_name = browser.find_element(by=By.XPATH,value=f"//div[@class='skill-show']/div[{i}]/p[@class='skill-name']/b").text
print(skill_name)
```

- 完整代码

```python
import time

from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By


# 1.获取数据
def get_data():
    url = "https://pvp.qq.com/web201605/herodetail/sunce.shtml"
    # 创建一个浏览器对象
    browser = webdriver.Chrome()
    # 设置窗口最大化
    browser.maximize_window()
    # 打开网页
    browser.get(url)
    # 程序休眠，渲染网页
    time.sleep(3)
    # 调用筛选函数方法
    parser_data(browser)
    # 关闭窗口
    browser.close()


# 2.解析数据
def parser_data(browser):
    # 获取所有的技能名称
    skill_bs = browser.find_elements(by=By.XPATH, value="//div[@class='skill-show']/div/p[@class='skill-name']/b")
    # 问题：进入到页面之后，默认只能显示第一个技能，其他未显示的技能获取不到
    skill_texts = [skill_b.text for skill_b in skill_bs]
    print(skill_texts)  # 只能看到第一个技能

    # 解决，如果要获取一个英雄的所有的技能，则需要将鼠标移动到对应技能的图标上，才能获取到相关的数据。
    # 只需要移动，不需要点击
    # 获取浏览器的事件对象
    action = ActionChains(browser)
    # 定位到表示所有技能的标签
    # ul.skill-u1 > li:not(.no5):在class='skill-u1'的ul标签下找到class='no5'除外的所有li标签
    skill_lis = browser.find_elements(by=By.CSS_SELECTOR, value="ul.skill-u1 > li:not(.no5)")
    for i in range(1, len(skill_lis) + 1):
        # 获取每个图标的li
        img_li = skill_lis[i - 1]  # 这里列表的索引从1开始，-1
        # 给鼠标添加移动操作：action.move_to_element(标签对象).perform()
        action.move_to_element(img_li).perform()
        time.sleep(2)
        # 获取技能名称
        skill_name = browser.find_element(by=By.XPATH,
                                          value=f"//div[@class='skill-show']/div[{i}]/p[@class='skill-name']/b").text
        print(skill_name)


if __name__ == '__main__':
    get_data()
```

