# Python操作PDF

## 一、PDF简介

PDF是Portable Document Format的缩写，这类文件通常使用`.pdf`作为其扩展名。在日常开发工作中，最容易遇到的就是从PDF中读取文本内容以及用已有的内容生成PDF文档这两个任务。

在Python中，可以使用名为`PyPDF2`的三方库来读取PDF文件，可以使用下面的命令来安装它。

安装最新版本

```powershell
pip install PyPDF2
```

![image-20231031103255124](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/31/6540675eb15c0.png)

安装指定版本

```powershell
pip install PyPDF2==2.3.1 
```

## 二、从PDF中提取文本和加密PDF文件

### 2.1 提取文本

`PyPDF2`没有办法从PDF文档中提取图像、图表或其他媒体，但它可以提取文本，并将其返回为Python字符串。

- 导入包

```python
import PyPDF2
```

- 创建读取文件的对象

```python
reader = PyPDF2.PdfReader(r"data/XGBoost.pdf")
```

- 获取指定页，结果返回一个字典

```python
page = reader.pages[0]
print(page)
```

- 获取指定页的文本内容

```python
text = page.extract_text()
print(text)
```

### 2.2 加密pdf文件

使用`PyPDF2`中的`PdfWrite`对象可以为PDF文档加密，如果需要给一系列的PDF文档设置统一的访问口令，使用Python程序来处理就会非常的方便。

加密pdf文件会创建新的对象

- 导入包

```python
import PyPDF2
```

- 创建读取文件的对象

```python
reader = PyPDF2.PdfReader(r"data/XGBoost.pdf")
```

- 创建写入文件的对象

```python
writer = PyPDF2.PdfWriter()
```

- 获取已知文件的每一页，添加到新的文件对象中

```python
for i in range(len(reader.pages)):
    writer.add_page(reader.pages[i])
```

- 给PDF文件加密，设置密码

```python
writer.encrypt("1234")
```

- 保存新创建的文件

```python
with open(r"data/XGBoost-加密.pdf", "wb") as f:
    writer.write(f)
```

## 三、旋转和创建空白pdf文件

### 3.1 旋转页

- 导入包

```python
import PyPDF2
```

- 创建读取文件的对象

```python
reader = PyPDF2.PdfReader(r"data/XGBoost.pdf")
```

- 创建写入文件的对象

```python
writer = PyPDF2.PdfWriter()
```

- 获取已知文件的每一页，添加到新的文件对象中

```python
for i in range(len(reader.pages)):
    # 获取每一页的对象
    page = reader.pages[i]
    if i % 2 == 0:
        # 偶数页顺时针选择90°
        page.rotate(90)
    else:
        # 奇数页顺时针选择90°
        page.rotate(-90)
    writer.add_page(reader.pages[i])
```

- 保存新创建的文件

```python
with open(r"data/XGBoost-旋转.pdf", "wb") as f:
    writer.write(f)
```

### 3.2 创建空白页

```python
import PyPDF2

# 1.旋转
# 创建读取文件的对象
reader = PyPDF2.PdfReader(r"data/XGBoost.pdf")
# 创建写入文件的对象
writer = PyPDF2.PdfWriter()

for i in range(len(reader.pages)):
    # 获取每一页的对象
    page = reader.pages[i]
    if i % 2 == 0:
        # 顺时针旋转90
        page.rotate(90)
    else:
        # 逆时针旋转90
        page.rotate(-90)
    writer.add_page(page)

# 2.创建空白页
blank_page = writer.add_blank_page()

with open(r"data/XGBoost-空白.pdf", 'wb') as f:
    writer.write(f)
```

## 四、批量添加水印

​	给文件添加水印，需要准备两个文件，一个需要添加水印的文件`XGBoost.pdf`，一个是水印文件`watermark.pdf`。通过`merge_page`方法将两个PDF页面进行叠加，通过这个操作，我们很容易实现给PDF文件添加水印的功能。

​	同样，该操作会创建一个新的对象

```python
# 导入包
import PyPDF2

# 创建读取文件对象
# 原文件对象
src_reader = PyPDF2.PdfReader(r"data/XGBoost.pdf")
# 水印文件对象
water_reader = PyPDF2.PdfReader(r"data/watermark.pdf")

# 创建写入文件对象
writer = PyPDF2.PdfWriter()

# 获取水印文件的page对象
water_page = water_reader.pages[0]
# 遍历需要添加水印的原文件的每一页对象，将其和水印文件的页对象进行合并
for i in range(len(src_reader.pages)):
    src_page = src_reader.pages[i]
    # 合并
    src_page.merge_page(water_page)
    # 添加到writer文件对象中
    writer.add_page(src_page)
    
# 保存文件
with open(r"data/XGBoost-水印.pdf", "wb") as f:
    writer.write(f)
```