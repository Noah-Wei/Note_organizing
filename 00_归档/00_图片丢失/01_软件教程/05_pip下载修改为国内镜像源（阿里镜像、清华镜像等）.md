# pip下载修改为国内镜像源（阿里镜像、清华镜像等）

## 1、前言

​	在学习Python的过程中，尤其是现在对爬虫的学习，经常会需要安装一些第三方库，但是这些第三方库默认都是在国外的服务器上，从而导致我们安装下载这些第三方库的时候，速度及其缓慢，并且还有可能导致会安装失败。

​	这时候我们就可以手动的将下载的镜像源修改为国内的镜像源，因为这些库的包都存储在国内的服务器上，所以下载素服会快很多。

## 2、修国内镜像源下载

> 这里以下载`ddddocr`库为例

#### 2.1 默认方式下载

```powershell
 pip install ddddocr
```

这样的话，默认链接到的是国外的无服务器，下载速度及其的缓慢，还有可能失败

![image-20231223153753502](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/23/65868e5666b45.png)

比如这个库，我们就需要一个半小时才能安装完成，实在是太影响效率了

#### 2.2 切换成国内镜像源下载

比如，修改为阿里云镜像源

```powershell
pip install ddddocr -i https://mirrors.aliyun.com/pypi/simple
```

![image-20231223153944252](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/12/23/65868ebdeb688.png)

修改之后，我们可以发现速度是非常的快，是之前的几百倍，效果还是很直观的

#### 2.3 切换成国内镜像源下载指定的版本

切换成国内镜像源下载，同样是可以下载指定的版本，这里依旧以阿里云镜像站为例

如：下载`1.4.10`版本的`ddddocr`

```
pip install  ddddocr==1.4.10 -i https://mirrors.aliyun.com/pypi/simple
```

## 3、常见的国内镜像源

#### 3.1 阿里云镜像站

```
https://mirrors.aliyun.com/pypi/simple/
```

#### 3.2 清华镜像

```
https://pypi.tuna.tsinghua.edu.cn/simple
```

#### 3.3 豆瓣镜像

```text
http://pypi.douban.com/simple/
```

#### 3.4 华中科大镜像

```text
http://pypi.hustunique.com/
```

#### 3.5 山东理工大学镜像

```text
http://pypi.hustunique.com/
```

#### 3.6 搜狐镜像

```text
http://mirrors.sohu.com/Python/
```

#### 3.7 百度镜像

```text
https://mirror.baidu.com/pypi/simple
```

## 4、修改默认镜像源

有时候，我们每下载一个包是，都是添加一次镜像站，会觉得有点麻烦，所以，我们可以直接修改默认的镜像源

如：修改默认的镜像源为阿里云镜像源

```
 pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
```

## 5、注意事项

- 有时候镜像网站也镜像挂掉，所以如果一个镜像网站感觉不行，就赶紧换一个。
- 针对不同的库，国内的镜像源可能有问题。比如很多c++的库用镜像源都下载不了
- 针对官方库下载慢，还可以借助梯子来加速。