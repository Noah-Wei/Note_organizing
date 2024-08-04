# Gitlab分支新建与合并

为了避免混乱，案例中自己创建的分支名为`feature_pcy`

## 1.创建分支

> 有两种方式

### 方式一：项目仓库中新建

直接点击`+` 号，然后点击`new branch`后跳转到如下界面，其中`branch name`为自己命名，建议采用`feature +后缀`的方式，例如`feature_pcy`

![image-20240612124219334](https://img.noahwei.com/2024/08/04/66af047b674a8.png)

![image-20240612124252037](https://img.noahwei.com/2024/08/04/66af047eb79c3.png)

### 方式二：侧边栏菜单中选择创建

Gitlab网页侧边栏选择 `Repository` 下的 `Branches` ，然后点击`new branch`后跳转到如下界面，其中`branch name`为自己命名，建议采用`feature +后缀`的方式，例如`feature_pcy`

![image-20240612124356536](https://img.noahwei.com/2024/08/04/66af04824c0c8.png)

![image-20240612124252037](https://img.noahwei.com/2024/08/04/66af047eb79c3.png)

## 2. 创建工作目录

本地新建一个文件夹，并进入，然后右键选择`Open Git Bash here`

![image-20240612124904003](https://img.noahwei.com/2024/08/04/66af048805d52.png)

## 3. 克隆代码仓库到本地

**注：若本地已有历史开发的代码，执行 `git pull origin feature_pcy`，将远程代码仓库更新到本地，跳过第 4、5 步**

语法：

```bash
git clone [URL]
```

这里的 `[url]` 是远程仓库的URL，获取位置为下图

示例：

```bash
git clone http://gitlab.htzq.htc.com.cn/fic/data-development.git
```

![image-20240612125307313](https://img.noahwei.com/2024/08/04/66af048b2559c.png)

## 4. 进入项目仓库根目录，重新打开Git命令窗口

> 或者直接 cd 命令切换

![image-20240612125729709](https://img.noahwei.com/2024/08/04/66af048e91aad.png)

## 5. 切换分支

创建一个名为 `desired_branch` 的本地分支，并且这个新分支的起点与远程仓库 `origin` 中的 `desired_branch` 分支保持一致，随后立即将工作目录切换到这个新创建的本地分支上

语法：

```
git checkout -b desired_branch origin/desired_branch
```

示例

```
git checkout -b feature_pcy origin/feature_pcy
```

**注：如果切换失败，则需要冲远程仓库手动更新一下仓库信息，即 `git fetch origin` 然后重新执行上面的语句**

## 6. 开发者开发了新的代码，放在本地仓库的分支feature_pcy中的某个文件夹中

`git status` ：查看状态，红色的文件即为本次修改的文件

## 7. 将修改的文件加入本地缓冲区

语法：

```
git add --all
```

## 8. 本地提交修改

**注：注释中需要加上#jire号码#否则无法推送**

语法：

```
git commit -m "注释说明"
```

示例

```
git commit -m " #MARKETDATA-2258# 【CAMS】COM_CREDITBANKCLASSIFY 表上采编"
```

## 9. Push到远程仓库分支

语法：远程仓库（默认为 `origin`）：

```
git push [远程仓库名] [本地分支名]:[远程分支名]
```

示例

```
git push origin feature_pcy
```

## 10. 合并分支

- Gitlab网页新建`merge request`

- 选择将 `feature_pcy` 合并到 `develop` 分支

- 提交之后需要由项目**管理员同意**

![image-20240612134141608](https://img.noahwei.com/2024/08/04/66af04960f6f9.png)

## 11. 删除自己的分支

在历史的开发工作的 merge request 被同意后，分支即进行了合并，之后需要删除自己建立的分支 feature_pcy，删除不可撤销，请确认已经提交修改合并分支。

![image-20240612134338016](https://img.noahwei.com/2024/08/04/66af049a3c51e.png)

![image-20240612134608011](https://img.noahwei.com/2024/08/04/66af049d2fdc9.png)