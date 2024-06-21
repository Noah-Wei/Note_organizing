# Anaconda3-2023.07-2安装和配置教程

## 前言

最近开始学习数据分析，所以会用到Anaconda，所以记录一下。

Anaconda，中文大蟒蛇，是一个开源的Python发行版本，其包含了conda、Python等180多个科学包及其依赖项。因为包含了大量的科学包，所以相对而言，Anaconda 的下载文件比较大。

这时候可能有朋友会纠结先安装python还是安装anaconda，这边的建议是装anaconda，就不需要单独装python了，因为anaconda自带python，且安装了anaconda之后，默认python版本是anaconda自带的python版本。

**所以，Anaconda的安装分两种情况：**

**情况一：电脑现在没有装python或者现在装的可以卸载掉（装Anaconda时先卸python，本文记录方式）**

**情况二： 电脑目前装了python，但想保留它 (比较复杂，本文未记录，请百度 !)**

## 一、彻底卸载python

> 电脑系统：win11
>
> Anaconda版本：Anaconda3-2023.07-2-Windows-x86_64

### 1、卸载python

- 首先在控制面板中将python卸载，并且**手动删除安装目录中的文件**

![image-20231006201330270](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/651ff9f371181.png)

### 2、删除环境变量

**检查环境变量中是否也被删除**

> 当初在安装的时候，有的人未勾选“自动添加环境变量”的选项，选择手动添加

- 点击计算机（右键）→属性→高级系统设置→环境变量

![image-20231006202105011](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/651ffbb2f2f8b.png)

- 查看**用户变量**和系统变量中的**path**栏，双击打开，查看是否还有和python有关的环境变量，如果有，则删除掉。

![image-20231006202518830](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/651ffcb1bdc58.png)

**注**：在退出时，不要忘记外面还有几个**确认**按钮，也需要**点击**，**不要直接叉掉**，否则并没有保存设置。

- 至此，python完全卸载完成，选择性可以重启电脑。

## 二、下载Anaconda

### 方式一：官网下载

官网地址：[Free Download | Anaconda](https://www.anaconda.com/download/)

- 进入官网，直接点击首页的"Download"即可。

> 如果官网文件下载速度很慢的话，可以考虑第二种方式下载：清华大学开源软件镜像站下载

![image-20231006203452366](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/651ffeeeb9af8.png)

### 方式二、镜像站下载

网站地址：[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)

- 选择合适的版本下载即可

![image-20231006203725699](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/651fff874f7f3.png)

## 三、安装Anaconda3

- 双击安装包打开，等待一下加载，然后点击**Next**

![image-20231006204045029](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/6520004fc4949.png)

- 然后点击 **I Agree**

![image-20231006204116733](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/6520006fa47aa.png)

- 然后选择**为谁安装**，按需选择，这里就默认了，然后点击**Next**

![image-20231006204224339](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/652000b2daefb.png)

- 这里是安装位置，按需选择即可（不建议装C盘，如图所示，要占用4.9G，此处为虚拟机演示，就未修改），然后点击**Next**

![image-20231006204303349](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/652000d9eada2.png)

- 这里是选择环境变量，建议将**第二项勾选上，自动添加环境变量，不选择可以，安装结束后，手动添加**。（如果当前安装版本没有这个选择，也后面自动添加）第四项是：完成后清除包缓存，按需选择。然后选择**Install**，等待安装结束

![image-20231006204715013](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/652001d59dd3c.png)

- 安装完成后，点击**Next**，然后点击**Finish**，弹出的页面直接关闭即可

![image-20231006210121932](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/65200524dc20d.png)

- 至此，安装结束。

## 四、配置环境变量

> 前面安装时，如果已勾选自动配置的用户，则可忽略

- 点击计算机（右键）→属性→高级系统设置→环境变量

![image-20231006202518830](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/652007aacdead.png)

- 在下面**系统变量**里，找到并双击**Path**打开

> 如果前面选择了“自动添加”和“为自己安装”，应该在用户变量中的path中可以查看到。
>
> 选择“为所有用户安装”，应该在用户变量中的path中可以查看到。

![image-20231006211609574](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/6520089aa8b1f.png)

- 在编辑环境变量里，点击**新建**

![image-20231006211754329](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/652009036af12.png)

- 依次添加下面的5行环境变量，新建完成后点击**确定**！然后依次**确定**，关闭弹窗

**注**：这里不是完全复制，其中的前缀**C:\Software\anaconda3**需要**修改**为你自己电脑的安装路径

```
C:\Software\anaconda3
C:\Software\anaconda3\Library\mingw-w64\bin
C:\Software\anaconda3\Library\usr\bin
C:\Software\anaconda3\Library\bin
C:\Software\anaconda3\Scripts
```

![image-20231006213438094](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/65200cef4b57d.png)

## 五、检验安装是否成功

### 1、查看anaconda版本

同时按**win + r**，在左下角的运行窗口中，输入**cmd**，在弹出的命令行中输入：**conda --version**，查看anaconda版本，输入 ：

```powershell
conda --version
```

若能显示出anaconda版本号，则正常

![image-20231006213638386](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/65200d67a733f.png)

### 2、查看python版本

输入**python**，查看python版本

```powershell
python
```

若能显示出python版本号，则正常

![image-20231006213709857](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/65200d871b7db.png)

### 3、查看Anaconda Navifator

点击**任务栏中的Windows键**这里，然后选择**所有应用**，选择**Anaconda3(64-bit)**文件夹中的**Anaconda Navifator**打开它

![image-20231006213745006](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/65200daa5ccb4.png)

若出现以下页面，能安装正常

![image-20231006214233696](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/65200ecb56b4d.png)

**至此，检验结束。**

## 六、优化设置：配置国内镜像源

> 参考前面在官网下载安装包的速度，因为官方服务器在国外，后续我们需要安装一些Python包时，速度可能会很慢，所以我们将源换成清华大学镜像源

### 1、配置镜像源

- 点击**任务栏中的Windows键**这里，然后选择**所有应用**，选择**Anaconda3(64-bit)**文件夹中的**Anaconda Prompt**打开它

![image-20231006215731039](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/6520124c63747.png)

- 依次输入以下代码，配置镜像源即可

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
 
//设置搜索时显示通道地址
conda config --set show_channel_urls yes
```

![image-20231006215926002](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/652012bf27e21.png)

### 2、其他相关命令

#### a.查看是否修改好通道

```
conda config --show channels
```

![image-20231006220217424](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/6520136a5db0d.png)

#### b.恢复默认源

```
conda config --remove-key channels
```

#### c.删除旧镜像源

```
conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/

```

#### d.添加新镜像源

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/
```

## 七、配置Pycharm解释器

- 打开Pycharm，新建项目，修改项目位置**Location**

  在**Python Interpreter: Previously configured interpreter**选项中选择**Previously configured interpreter**

![image-20231006223525880](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/65201b35bda50.png)

- 此时最下面提醒：**No Python interpreter selected**，没有Python解释器选择。所以点击右侧的 **Add Interpreter**，然后选择**Add Local Interpreter...**

![image-20231006223838478](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/65201bee608ba.png)

- 选择左侧的**Virtualenv Environment**，然后**Environment:**选择**Existing**，点击右侧的**...**，在弹窗中选择之前安装anaconda的目录，在其中找到**python.exe**，然后点击**OK**，返回上级目录再点击**OK**

![image-20231006224205580](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/65201cbdb5b2c.png)

**补充说明**

如果忘记anaconda忘记安装在哪了，在cmd命令行窗口中输入**where python**查找

```
where python
```

![image-20231006224653930](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/65201dddabea4.png)

- 这个时候**Interpreter**里面就有内容了，点击左下角的**Create**创建项目

![image-20231006224955170](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/65201e930391d.png)

- 等待加载完成，创建一个Python文件，输入代码**print("hello world")**，然后运行。

> 初次运行可能需要加载设置，可等待一下。

![image-20231006230001431](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/06/652020f207094.png)

**程序正常运行，自此，配置结束。**
