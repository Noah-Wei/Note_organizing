# Excel基本操作

## 1、整体视图

- 软件版本：**Microsoft Office LTSC 专业增强版 2021**
  - WPS缺失部分功能
- 相关概念
  - 一个Excel文件是一个工作簿
  - 一个工作簿中有多个工作表
  - 一个工作表中有多个单元格
  - Power Query：数据加载-数据处理/清洗
- 视图

<img src="https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/09/659d16d2238cc.png" alt="image-20240109175005052" style="zoom:50%;" />

## 2、保护设置

### 2.1 保护工作簿

> 即Excel文件，需要密码才能打开Excel文件

- 操作步骤

  - **文件>信息>保护工作簿>用密码进行加密**

  <img src="https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/09/659d17f672ccd.png" alt="image-20240109175504347" style="zoom:50%;" />

### 2.2 保护工作簿结构

> 防止对工作簿结构进行更改，如插入/删除/重命名/移动或复制/隐藏工作表等

- 操作步骤

  - 审阅>保护工作簿

  ![image-20240109180015437](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/09/659d192dbe126.png)

### 2.3 保护工作表

> 当前工作表的所有单元格均无法操作

- 操作步骤

  - 审阅>保护工作表

  ![image-20240109180345562](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/09/659d19ffba24b.png)

### 2.4 保护单元格

> 在一个工作表中，只允许在制定的区域（即单元格）进行编辑，而非全部的单元格

- 操作步骤

  - 审阅>允许编辑区域>保护工作表
  - 然后执行`保护工作表`的步骤：审阅>保护工作表

  ![image-20240109181018069](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/09/659d1b8857c61.png)

## 3、快速输入数据

### 3.1 填充柄：鼠标左键输入

- 文字+数字型
  - `Ctrl+鼠标左键`下拉填充柄：**复制**
  - `直接鼠标左键`下拉填充柄：**递增**
- 纯数字型
  - `Ctrl+鼠标左键`下拉填充柄：**递增**
  - `直接鼠标左键`下拉填充柄：**复制**

- 等差数列
  - 先输入两个数字，构造等差数列的步长，再下拉填充柄

<img src="https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/09/659d2909563a8.png" alt="image-20240109190756346" style="zoom:50%;" />

### 3.2 填充柄：鼠标右键输入

- 鼠标右键下拉填充，点击序列，等比序列，步长值

<img src="https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/09/659d29879fd55.png" alt="image-20240109191002579" style="zoom:50%;" />

### 3.3 同时编辑多个单元格

> 分为连续和不连续两种

- 单元格连续
  - `鼠标左键`拖拉选中单元格，然后输入数据，在按`Ctrl+Enter`补全
- 单元格不连续
  - `Ctrl+鼠标左键`一次选中单元格，然后输入数据，在按`Ctrl+Enter`补全

## 4、数据类型

### 4.1 简单的数据类型分类

| 数据类型     | 距离                                          |
| ------------ | --------------------------------------------- |
| 文本(字符串) | 姓名、性别、住址、商品名称                    |
| 数字         | 100，0.1，1e5…                                |
| 逻辑值       | TRUE, FALSE                                   |
| 错误值       | #VALUE!、#DIV/0!、#NAME!、#N/A、#REF!、#NULL! |

**注：日期时间属于特殊的数字**

### 4.2 常见的错误值说明

| 错误值  | 解释               |
| ------- | ------------------ |
| #VALUE! | 文本与数字运算     |
| #DIV/0! | 两数相除，分母为零 |
| #NAME!  | 公式名称错误       |
| #N/A    | 查找值不存在       |
| #REF!   | 所引用单元格被删除 |
| #NULL!  | 两数组无交集       |
| #NUM!   | 使用无效数值       |
| ###     | 单元格宽度不足     |

### 4.3 自定义格式

- **优点：只改变显示格式，，不改变实际值，增强数据的可读性**

比如我们想要在单元格中存放一个数据为`001`，但是当我们输入后回车，会直接变成`1`，没有达到我们想要的效果，所以我们可以做一些调整

**方式一**：改变单元格格式为**文本**

![image-20240109195305182](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/09/659d339f0ea43.png)

但是假如我们想要实现显示的是`0001`，实际值为`1`时，这种方式就不行了

**方式二：自定义格式**

选中要改变格式的单元格（可以批量修改），然后邮件选择设置单元格格式，然后选择自定义，然后填写类型

**注：这里的0为占位符**

比如我们想要单元格中的实际值是`234`，但是显示内容是`￥234.00元`，我们可以自定义类型为`￥0.00元`，这样既改变了显示格式，又不影响单元格的实际运算，增强了数据的可读性

![image-20240109195511229](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/09/659d341d6ecb1.png)

## 5、文本型数值

### 5.1 文本类型

**如果单元格输入的数字超过12位，则以科学记数显示**

例：`123456789101223`会被表示成`1.2346E+14`

转变方式：

- 方式一：设置单元格格式为文本，再输入数字
- 方式二：先输入单引号(英文)即`'`，再输入数字

### 5.2 文本型数字转为数值（也适用于日期时间）

<img src="https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/10/659e42ec26687.png" alt="image-20240110151040045" style="zoom:50%;" />

可以使用以下的6个公式之一来完成

| 使用6个公式之一来转换 |
| --------------------- |
| 1、=A1*1              |
| 2、=A1/1              |
| 3、=A1+0              |
| 4、=A1-0              |
| 5、=--A1              |
| 6、=VALUE(A1)         |

### 5.3 逻辑值转数字

原理相同，任选其一完成

| 逻辑值转为数字 |
| -------------- |
| 1、=A1*1       |
| 2、=A1/1       |
| 3、=A1+0       |
| 4、=A1-0       |
| 5、=--A1       |
| 6、=N(A1)      |

## 6、日期标准化

**日期是特殊的数字**

EXCEL中规定了一些日期的格式

![image-20240110154445586](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/10/659e4ae9bcad0.png)

- 情况一：文本型的标准格式

这种情况虽然显示的是日期，但是属于文本型，我们可以直接选择上诉6个公式之一来直接完整转换

- 情况二：不标准格式1

这种情况，有的日期格式正常，有些日期格式不是规定中的，比如使用`.`作为分割，我们可以使用查找替换功能，将`.`换成`/`，然后再自定义格式，统一

- 情况二：不标准格式2

这种情况中，年份的前两位被省略，我们可以先借助`TEXT`函数+自定义格式实现，如：`=TEXT(G2,"2000-00-00")`，即可

- 情况二：不标准格式3

这种情况，需要使用数据>分列的功能

操作步骤：

1. 首先选中需要修改的那一列，然后点击`数据`，然后点击`分列`

   ![image-20240110155458339](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/10/659e4d500ac6b.png)

2. 然后选择`下一步`，`下一步`，在列数据列表中选择`日期`，然后按照需要选择格式，最后点击完成

   <img src="https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/10/659e4d7a3a478.png" alt="image-20240110155542006" style="zoom: 50%;" />

3. 最后，在自定义格式统一即可

## 7、快速求和

- 快速求和快捷键：`ALT+=`
- 操作步骤
  - 鼠标选择需要计算的单元格区域
  - `ALT+=`

## 8、快速选择区域

| 快捷键            | 说明                                                  |
| ----------------- | ----------------------------------------------------- |
| Ctrl+方向键       | 快速移动到边界单元格                                  |
| Shift+方向键      | 当前所选的单元格区域，按指定方向扩充/缩小一行或一列   |
| Ctrl+Shift+方向键 | 当前所选的单元格区域，按指定方向扩充/缩小至边界单元格 |
| Ctrl+A            | 选择全部单元格                                        |

## 9、冻结窗口

- 冻结首行

  - 操作步骤：点击`视图`————`冻结窗口`————`冻结首行`

  ![image-20240110160802683](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2024/01/10/659e505f689ab.png)

- 冻结首列

  - 操作方式同上

- 冻结单元格

  - 比如我们想要冻结**A列和1行**的数据，我们只需要点击`B2`单元格，然后冻结窗口皆可。
  - 选择的单元格前面有几行几列，即是冻结的区域