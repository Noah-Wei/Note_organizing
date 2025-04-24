# Python操作Word

## 一、Word简介

​	在日常工作中，有很多简单重复的劳动其实完全可以交给Python程序，比如根据样板文件（模板文件）批量的生成很多个Word文件或PowerPoint文件。Word是微软公司开发的文字处理程序，相信大家都不陌生，日常办公中很多正式的文档都是用Word进行撰写和编辑的，目前使用的Word文件后缀名一般为`.docx`。PowerPoint是微软公司开发的演示文稿程序，是微软的Office系列软件中的一员，被商业人士、教师、学生等群体广泛使用，通常也将其称之为“幻灯片”。在Python中，可以使用名为`python-docx` 的三方库来操作Word

​	我们可以先通过下面的命令来安装`python-docx`三方库。

```python
pip install python-docx
```

**注意事项：**

- 我们在安装此模块使用的是`pip install python-docx`，但是在导入的时候是 `docx`

​	

> ```python
># 一般情况下
> pip install python-docx
> 我们在安装此模块使用的是pip install python-docx，但是在导入的时候是 docx
> # 说明：只需要执行pip install python-docx，会自动安装lxml
> 
> # 可能出现问题：
>高版本lxml没有etree模块。有网友确定lxml4.2.5版本带有etree模块，且该版本lxml支持python3.7.4版本。安装命令：
> pip install lxml==4.2.5  (若python-docx 使用有问题,需要查看lxml版本)
> ```

## 二、向Word写入内容

### 2.1 导入模块

注：操作word，需要安装`python-docx`库，但是在代码中使用的时候导入`docx`

```python
from docx import Document
```

### 2.2 创建doc文档对象

```Python
doc = Document()
```

### 2.3 添加段落

```Python
doc.add_heading('在Python中操作Word文档',2)
```

注意：在Word中，一个段落可能有很多个组件组成

- 给段落添加组件

```Python
run1 = p.add_run('Python是可以跨平台的，')
run2 = p.add_run('Python可以做数据分析，')
run3 = p.add_run('Python还可以做web开发等。')

run1.bold = True  # 设置加粗
run2.underline = True   # 设置下划线
```

### 2.4 添加列表

- 有序序列

```Python
doc.add_paragraph('运营部',style='List Number')
doc.add_paragraph('行政部',style='List Number')
doc.add_paragraph('市场部',style='List Number')
doc.add_paragraph('财务部',style='List Number')
doc.add_paragraph('人事部',style='List Number')
```

- 无序序列

```Python
doc.add_paragraph('运营部',style='List Bullet')
doc.add_paragraph('行政部',style='List Bullet')
doc.add_paragraph('市场部',style='List Bullet')
doc.add_paragraph('财务部',style='List Bullet')
doc.add_paragraph('人事部',style='List Bullet')
```

### 2.5 添加图片

```Python
# 用于图片控制大小
from docx.shared import Cm

doc.add_picture(r'data/py.jpg',width=Cm(3))
```

### 2.6 保存文件

```python
doc.save(r'data/python操作word.docx')
```

## 三、读取Word内容

一个文本（document）可能有很多的段落（paragraphs）组成，一个段落可能有很多的组件（runs ）组成

```python
from docx import Document

# 1.创建doc对象
doc = Document(r"data/占勇辉的离职证明.docx")

# 2.获取doc中所有的段落，结果为列表，其中的元素为段落对象
print(doc.paragraphs)

# 3.遍历段落，获取段落的文本组成
for index, para in enumerate(doc.paragraphs):
    print(index, para.text)
    # 获取每个段落的组件
    # print(para.runs)
    # 获取每个组件的文本
    for run in para.runs:
        print(run.text)
```

## 四、批量生成Word文件

​	试想，我们如果把上面的离职证明制作成一个模板文件，把姓名、身份证号、入职和离职日期等信息用占位符代替，这样通过对占位符的替换，就可以根据实际需要写入对应的信息，这样就可以批量的生成Word文档。

**说明：如果需要批量生成文件，则需提前准备模板【一定要考虑到段落的组成】，就可以借助于下面代码的思想操作**

实现思路：

- **首先编辑一个离职证明的模板文件**
- **接下来我们读取该文件，将占位符替换为真实信息，就可以生成一个新的Word文档**

![image-20231030223856842](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/30/653fc00107be2.png)

```python
# 将员工信息保存在字典中
person_list = [
    {
        "name":"杨天偿",
        "id":"333222199909120987",
        "sdate":"2017年7月1日",
        "edate":"2021年11月1日",
        "department":"技术部",
        "position":"架构师",
        "company":"深圳华为技术有限公司"
    },
    {
        "name":"刘一奇",
        "id":"110120198909120937",
        "sdate":"2016年3月1日",
        "edate":"2020年10月1日",
        "department":"行政部",
        "position":"打手",
        "company":"黑龙江天上人间桑拿会所"
    },
    {
        "name":"张国涛",
        "id":"111222199908120987",
        "sdate":"2015年4月1日",
        "edate":"2019年11月1日",
        "department":"后厨部",
        "position":"试吃员",
        "company":"深圳市金威源餐饮有限公司"
    },
    {
        "name":"欧阳",
        "id":"112221199909120987",
        "sdate":"2016年7月1日",
        "edate":"2020年11月1日",
        "department":"后勤部",
        "position":"大茶壶",
        "company":"深圳市怡红院高级会所"
    },
]
```

```python
from docx import Document

# 将员工信息保存在字典中
person_list = [
    {
        "name": "杨天偿",
        "id": "333222199909120987",
        "sdate": "2017年7月1日",
        "edate": "2021年11月1日",
        "department": "技术部",
        "position": "架构师",
        "company": "深圳华为技术有限公司"
    },
    {
        "name": "刘一奇",
        "id": "110120198909120937",
        "sdate": "2016年3月1日",
        "edate": "2020年10月1日",
        "department": "行政部",
        "position": "打手",
        "company": "黑龙江天上人间桑拿会所"
    },
    {
        "name": "张国涛",
        "id": "111222199908120987",
        "sdate": "2015年4月1日",
        "edate": "2019年11月1日",
        "department": "后厨部",
        "position": "试吃员",
        "company": "深圳市金威源餐饮有限公司"
    },
    {
        "name": "欧阳",
        "id": "112221199909120987",
        "sdate": "2016年7月1日",
        "edate": "2020年11月1日",
        "department": "后勤部",
        "position": "大茶壶",
        "company": "深圳市怡红院高级会所"
    },
]

# 遍历列表，批量生成离职证明
for person in person_list:
    # 读取离职证明的模版文件
    doc = Document(r"data/离职证明模板.docx")
    # 数据处理
    for p in doc.paragraphs:
        # 过滤无需处理的段落
        if "{" not in p.text:
            continue
        # 处理带有{的段落
        for run in p.runs:
            # 过滤无需处理的组件
            if "{" not in run.text:
                continue
            # print(run.text)
            # 将模版中{xxx}数据进行替换
            # 通过切片，获取到xxx，就可以匹配到字典的key
            key = run.text[1:-1]
            # print(key)
            # 替换操作
            run.text = run.text.replace(run.text, person[key])

    # 将处理之后的文件进行保存
    doc.save(fr"data/{person['name']}的离职证明.docx")
```