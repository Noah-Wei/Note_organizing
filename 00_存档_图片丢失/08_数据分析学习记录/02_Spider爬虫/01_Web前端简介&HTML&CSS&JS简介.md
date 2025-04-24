# Web前端简介&HTML&CSS&JS简介

## 一、Web前端简介

### 1.1 基本知识

网页主要由三个部分组成：

- 结构：负责网页的结构和内容，如：标题，图片，段落等，由`html`实现
- 表现（样式）：设定网页的表现形式，如：标签的位置，大小，文字颜色等，由`css`实现
- 行为：控制网页的行为，可以和用户进行交互，如：按钮的点击等，由`javascrip`t实现

总的来说，**结构决定了网页是什么，表现决定了网页是什么样子，行为决定了网页能做什么**

网页文件：使用一种`html`的标记语言书写的纯文本文件，他可以在浏览器中按照指定的样式显示相容内容，支持的数据类型由文本，图片，音视频，图表，表格，列表等

### 1.2 开发模式

- B/S:Broswer  and  Server，浏览器服务器模式（web开发重点关注）
- C/S:Client  and Server，客户端服务端模式（爬虫重点关注）

## 二、HTML简介

### 2.1简介

HTML：HyperText  Markup Language，超文本标记语言，是最基础的网页语言，是解释性和编译型语言

其中：

- 超文本：超过文本的范畴
- 标记：html中所有的内容都是通过标签实现的，标签就是一个标记，如：`<a>hello</a>`
- 网页语言：制作网页的语言

采用的开发模式主要是B/S模式：浏览器/服务器开发模式，基于`http`（超文本传输协议）或者`https`（超文本传输安全协议）协议的实现向服务器发起请求`request`，html资源会传给服务器，服务器会根据浏览器传入的关键字返回一个响应`response`

所以流程可以简单表示为：

```
浏览器request ————> 服务器response -----> 浏览器根据响应内容作出布局(渲染)
```

**问题思考：**

- html是静态的还是动态的？
  - html主要是展示数据的，如果数据在某个行为发生时，改变原有的数据，则表示为动态的，如果没有任何的事件发生，则是静态的
- html指的是前端吗？
  - html只是前端中的一部分，前端包含网页，移动端，只要和用户直接交互的部分，被称为前端

### 2.2 html的规范

- 文件的后缀名为`.html`
- 一个html文件都有一个标签`<html></html>`,声明当前文件中书写的内容都是html标签
- html标签包含两部分：
  - head：设置html页面的属性和配置信息，如：标题，加载外部的文件
  - body：显示在浏览器中的内容

- 标签的形式
  - `<xxx></xxxx>`：开始标签和结束标签组成，如果结束标签省略，浏览器一般也会解析，title标签除外
  - `<xxxx>`：在标签内开始，在标签内结束，如：`<br />`、`<hr />`  

- 如果要对标签中的内容进行设置，则给该标签添加属性
  如`<font color="orange" size="5">hello world</font>，font`就是一个表示文本的标签，color和size是font的属性
- 标签的书写不区分大小写
- html中的标签几乎都是固定的，每种不同的数据类型有指定的标签进行表示，区别与XML，其中的标签可以自定义

- 标签之间的关系
  -  父子标签：直接嵌套关系的标签，如：html和head，html和body,head和title
  - 先辈后辈标签：出现了隔代的情况，如：html和title,html和meta
  - 兄弟标签：同级的标签，如：head和body,meta和title

**代码演示：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <title>认识html的结构</title>
</head>
<body>
第一个HTML程序

<!--
按下ctrl + /添加注释
-->
</body>
</html>
```

### 2.3 HTML常用的标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>demo2_常用的html标签</title>
</head>
<body>
<!--1.文本标签-->
<p>
    新华社北京10月30日电 <u>中共中央总书记、国家主席、中央军委主席</u><i>习近平30日下午</i>在中南海同全国妇联新一届领导班子成员集体谈话并发表重要讲话。习近平强调，<s>以中国式现代化全面推进强国建设</s>民族复兴伟业，需要全体人民团结奋斗，妇女的作用不可替代。要坚定不移走中国特色社会主义妇女发展道路，激励广大妇女自尊自信、自立自强，奋进新征程、建功新时代，为中国式现代化建设贡献巾帼智慧和力量
</p>
<p>
    <b>中共中央政治局常委、中央书记处书记蔡奇参加集体谈话。</b>
</p>
<p>
    10<sup>2</sup>
    log<sub>2</sub>
</p>

<!--2.标题标签-->
<h1>中共中央政治局常委</h1>
<h2>中共中央政治局常委</h2>
<h3>中共中央政治局常委</h3>
<h4>中共中央政治局常委</h4>
<h5>中共中央政治局常委</h5>
<h6>中共中央政治局常委</h6>

<!--3.图像标签-->
<!--可以加载本地图片，设置尺寸-->
<img src="data/风景1.jpg" width="500px" height="400px">

<!--默认为原尺寸-->
<!--<img src="data/风景1.jpg">-->

<!--还可以加载网络图片-->
<img src="https://www.baidu.com/img/bd_logo1.png" >

<br>
<br>
<br>
<br>

<!--4.超链接-->
<a href="http://tieba.baidu.com/">百度贴吧</a>

<!--5.块标签-->
<div>中共中央政治局常委</div>
<div>中共中央政治局常委</div>
<div>中共中央政治局常委</div>
<div>中共中央政治局常委</div>

<span>中共中央政治局常委</span>
<span>中共中央政治局常委</span>
<span>中共中央政治局常委</span>
<span>中共中央政治局常委</span>

<!--6.列表标签-->
<ol>
    <li>星期一</li>
    <li>星期二</li>
    <li>星期三</li>
    <li>星期四</li>
</ol>

<ul>
    <li>星期一</li>
    <li>星期二</li>
    <li>星期三</li>
    <li>星期四</li>
</ul>

<!--7.表格标签-->
<!--
快速生成
table>tr*4>td*3 然后按tab键，自动补全
-->
<table border="1" width="600px" cellspacing="0">
    <tr align="center">
        <td>姓名</td>
        <td>年龄</td>
        <td>地址</td>
    </tr>
    <tr>
        <td>张三</td>
        <td>10</td>
        <td rowspan="3" align="center">上海</td>
    </tr>
    <tr>
        <td>李四</td>
        <td>11</td>
    </tr>
    <tr>
        <td>王五</td>
        <td>12</td>
    </tr>
</table>
</body>
</html>
```

## 三、CSS核心语法

### 3.1 css和html的结合方式

**css使用的三种方式：**

- 方式一：行内样式
  - 直接在当前标签里面添加属性，只对当前标签起效果
- 方式二：内嵌样式
  - 在head标签里面创建一个style标签，在其中定义属性，对一系列的标签起效果
  - 如：`p{color:red;}`，表示对所有的p标签起效果
- 方式三：外部样式
  - 在外部定义一个css文件，然后通过link标签导入，对一系列的标签起效果
  - 如：`<link href="css/style1.css" type="text/css" rel="stylesheet">`

**代码演示：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
<!--    方式二-->
<!--    <style>-->
<!--        p{color:red;}-->
<!--    </style>-->
<!--    方式三-->
    <link href="css/style1.css" type="text/css" rel="stylesheet">
</head>
<body>
<!--方式一:行内样式 直接在标签内添加属性，只对当前标签起效果-->
<!--<p style="color:red;">-->
<!--    新华社北京10月30日电  中共中央总书记、国家主席、中央军委主席习近平30日下午在中南海同全国妇联新一届-->
<!--    领导班子成员集体谈话并发表重要讲话。习近平强调，-->
<!--    以中国式现代化全面推进强国建设民族复兴伟业，需要全体人民团结奋斗，妇女的作用不可替代。-->
<!--    要坚定不移走中国特色社会主义妇女发展道路，激励广大妇女自尊自信、自立自强，奋进新征程、建功新时代，-->
<!--    为中国式现代化建设贡献巾帼智慧和力量-->
<!--</p>-->
<!--<p>-->
<!--    中共中央政治局常委、中央书记处书记蔡奇参加集体谈话。-->
<!--</p>-->

<!--方式二：内嵌样式
<!--<p>-->
<!--    新华社北京10月30日电  中共中央总书记、国家主席、中央军委主席习近平30日下午在中南海同全国妇联新一届-->
<!--    领导班子成员集体谈话并发表重要讲话。习近平强调，-->
<!--    以中国式现代化全面推进强国建设民族复兴伟业，需要全体人民团结奋斗，妇女的作用不可替代。-->
<!--    要坚定不移走中国特色社会主义妇女发展道路，激励广大妇女自尊自信、自立自强，奋进新征程、建功新时代，-->
<!--    为中国式现代化建设贡献巾帼智慧和力量-->
<!--</p>-->
<!--<p>-->
<!--    中共中央政治局常委、中央书记处书记蔡奇参加集体谈话。-->
<!--</p>-->

<!--方式三：外部方式-->
<p>
    新华社北京10月30日电  中共中央总书记、国家主席、中央军委主席习近平30日下午在中南海同全国妇联新一届
    领导班子成员集体谈话并发表重要讲话。习近平强调，
    以中国式现代化全面推进强国建设民族复兴伟业，需要全体人民团结奋斗，妇女的作用不可替代。
    要坚定不移走中国特色社会主义妇女发展道路，激励广大妇女自尊自信、自立自强，奋进新征程、建功新时代，
    为中国式现代化建设贡献巾帼智慧和力量
</p>
<p>
    中共中央政治局常委、中央书记处书记蔡奇参加集体谈话。
</p>

</body>
</html>
```

### 3.2 css选择器和css属性

**css选择器的分类：**

- 元素选择器/标签选择器
- ID选择器：结合id属性使用,注意：id属性的值都是**唯一**的
- 类选择器：结合class属性使用，注意：class属性的值**不是唯一**的，所以查找的时候可能会查找出来多个
- 包含选择器：包含先辈后辈标签
- 兄弟标签：
  - +：只匹配相邻的一个标签
  - ~：匹配相邻的所有的标签

**代码演示：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        /*
        元素选择器/标签选择器
        */
        div {
            color: red;
        }

        /*
        ID选择器：结合id属性使用,注意：id属性的值都是唯一的
        */
        #second {
            color: cyan;
        }

        /*
        类选择器：结合class属性使用，注意：class属性的值不是唯一的，所以查找的时候可能会查找出来多个
        */
        .cls {
            color: purple;
        }

        /*
        包含选择器,包含先辈后辈标签
        #box p{color:blue;}
        */
        /*父子标签*/
        #box > p {
            color: yellow;
        }

        /*兄弟标签,
        +:只匹配相邻的一个标签
        ~：匹配相邻的所有的标签
        #p2+p{color:green;}
        */

        #p2 ~ p {
            color: green;
        }

    </style>
</head>
<body>
<div class="cls">
    新华社北京10月30日电 中共中央总书记、国家主席、中央军委主席习近平30日下午在中南海同全国妇联新一届
</div>
<div id="second">
    领导班子成员集体谈话并发表重要讲话。习近平强调
</div>
<div>
    以中国式现代化全面推进强国建设民族复兴伟业
</div>
<div class="cls">
    需要全体人民团结奋斗，妇女的作用不可替代
</div>

<div id="box">
    <p>hello~~~~11111</p>
    <div>
        <p id="p2">hello~~~~~22222</p>
        <p id="p3">hello~~~~~3333</p>
        <p>hello~~~~~44444</p>
        <p>hello~~~~~555555</p>
    </div>
</div>
</body>
</html>
```

## 四、Javascript基本语法

> 灰色地带：js逆向！！

### 4.1 概念

Javascript诞生于1995年，由Netspace（网景公司）和SUN（斯坦福大学网络公司）联合开发

Javascript是一门脚本语言，简称js，最初名字Mocha

**Javascript是基于对象和事件驱动的脚本语言，主要应用在客户端，简称js**

其中：

- 基于对象：提供好了一些对象，可以直接使用，如：document，window
- 事件驱动：html和css主要实现网页的静态效果，js负责和用户之间的交互（事件）
- 客户端：专门指的是浏览器

**Javascript的作用：负责网页的行为（和用户之间的交互），操作html和css**

**Javascript的特点：**

- 交互性：实现信息的动态交互
- 安全性：不可以直接访问本地磁盘
- 跨平台性：只要是可以解析js的浏览器都可以执行，与平台无关

**Javascript和java的区别：**

- 开发公司
  - JS：Netspace和SUN
  - Java：SUN
- 对象
  - JS：基于对象
  - Java：面向对象
- 变量
  - JS：弱类型`var num;`
  - Java：强类型`int num ;`
- 执行过程：
  - JS：解析就可以执行
  - Java：先编译，后运行

**Javascript的组成：**

- ECMAScript
  - 基本语法：变量，运算符，语句，函数

- BOM
  - Broswer Object Model，浏览器对象模型

- DOM（重点）
  - Document Object Model:文档对象模型

### 4.2 js基本语法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

</head>
<body>
<script>
    //单行注释
    /*
    多行注释
    */

    //1.变量
    let num = 10;
    var num1 = 20;

    var num2;
    num2 = 30;
    // alert(num1);
    //document.write(num1);
    console.log(num1, num2, num);

    num1++;
    console.log(num1);

    //2语句
    var num3 = 19;
    if (num3 % 2 == 0) {
        document.write('偶数');
    } else {
        document.write('奇数');
    }

    var total = 0;
    var i = 1;
    while (i <= 100) {
        total += i;
        i++;
    }
    document.write('<br>')
    document.write(total);

    for (var n = 0; n < 10; n++) {
        document.write(n + '<br>')
    }

    // 3.函数
    function func1(a, b) {
        var total = a + b;
        document.write(total);
    }

    func1(10, 20);

</script>

</body>
</html>
```
