# 使用VS Code和Git进行代码管理

## 1. Git环境配置

### 1.1 安装Git
- 下载地址: [Git官方网站](https://git-scm.com/)
- 验证安装: 
  ```bash
  git --version
  ```

### 1.2 配置用户信息
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 1.3 配置SSH密钥(推荐)
```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
# 将公钥(~/.ssh/id_ed25519.pub)添加到GitHub SSH Keys
```

## 2. 本地代码提交到GitHub

### 2.1 初始化本地仓库
```bash
mkdir project_folder
cd project_folder
git init
```

### 2.2 关联远程仓库
```bash
git remote add origin https://github.com/username/repo.git
# 或使用SSH地址
git remote add origin git@github.com:username/repo.git
```

### 2.3 添加并提交文件
```bash
# 添加单个文件
git add filename.txt

# 添加所有文件
git add .

# 提交更改
git commit -m "Initial commit"
```

### 2.4 推送到远程仓库
```bash
# 首次推送需要设置上游分支
git push -u origin main

# 后续推送
git push
```

## 3. 同步远程仓库更改

### 3.1 拉取最新更改
```bash
git pull origin main
```

### 3.2 处理合并冲突
1. 打开冲突文件，查找`<<<<<<<`, `=======`, `>>>>>>>`标记
2. 编辑文件保留需要的代码
3. 标记冲突已解决:
   ```bash
   git add resolved_file.txt
   git commit
   ```

### 3.3 强制同步(谨慎使用)
```bash
git fetch origin
git reset --hard origin/main
```

## 4. 多人协作开发流程

### 4.1 标准协作流程
1. 拉取最新代码:
   ```bash
   git pull origin main
   ```
2. 创建特性分支:
   ```bash
   git checkout -b feature-branch
   ```
3. 开发并提交更改
4. 推送到远程:
   ```bash
   git push -u origin feature-branch
   ```
5. 创建Pull Request

### 4.2 代码审查后合并
1. 切换回主分支:
   ```bash
   git checkout main
   ```
2. 拉取最新代码:
   ```bash
   git pull origin main
   ```
3. 合并特性分支:
   ```bash
   git merge feature-branch
   ```
4. 解决可能的冲突
5. 推送更改:
   ```bash
   git push origin main
   ```

## 5. 常见问题解决

### 5.1 提交了错误的文件
```bash
# 从暂存区移除文件但保留工作目录文件
git reset HEAD filename.txt

# 完全丢弃工作目录的更改
git checkout -- filename.txt
```

### 5.2 撤销上一次提交
```bash
git reset --soft HEAD~1
```

### 5.3 恢复已删除的分支
```bash
git reflog
git checkout -b branch-name commit-hash
```

## 6. VS Code Git集成功能

### 6.1 基本操作
1. 源代码管理面板(CTRL+SHIFT+G)
2. 更改标记:
   - `U`: 未跟踪文件
   - `M`: 修改过的文件
   - `D`: 已删除文件

### 6.2 高级功能
1. 行级提交: 选择部分代码行进行提交
2. 暂存更改: 临时保存工作进度
3. 分支管理: 可视化创建/切换分支
4. 冲突解决工具: 可视化解决冲突

### 6.3 推荐扩展
1. GitLens: 增强的Git功能
2. GitHub Pull Requests: PR管理
3. Git Graph: 可视化提交历史

## 最佳实践建议
1. 频繁提交小更改
2. 编写清晰的提交信息
3. 定期从主分支拉取更新
4. 使用特性分支开发新功能
5. 推送前运行本地测试
6. 定期清理已合并的分支

## 附录: Git常用命令速查表

| 命令               | 描述             |
| ------------------ | ---------------- |
| `git clone <repo>` | 克隆远程仓库     |
| `git status`       | 查看仓库状态     |
| `git log`          | 查看提交历史     |
| `git diff`         | 查看更改内容     |
| `git branch`       | 列出所有分支     |
| `git stash`        | 临时保存工作进度 |
| `git tag`          | 管理版本标签     |
