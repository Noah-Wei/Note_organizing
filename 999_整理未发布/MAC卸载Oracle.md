卸载 Oracle Instant Client **非常简单**，因为它只是一个普通的文件夹，没有复杂的安装程序。

### 完整卸载步骤（推荐按顺序操作）

1. **删除 Instant Client 文件夹**（这是最主要的部分）  
   你的安装目录是：
   ```bash
   /Users/noah/Downloads/instantclient_19_16
   ```

   执行下面命令彻底删除：
   ```bash
   rm -rf /Users/noah/Downloads/instantclient_19_16
   ```

   删除后可以用 `ls /Users/noah/Downloads/` 检查是否已经没有这个文件夹了。

2. **清理环境变量**（防止以后报错）  
   编辑你的 `~/.zshrc` 文件，删除之前添加的那两行：

   ```bash
   nano ~/.zshrc
   ```

   找到并删除下面这两行（或类似内容）：
   ```bash
   export PATH=/Users/noah/Downloads/instantclient_19_16:$PATH
   export DYLD_LIBRARY_PATH=/Users/noah/Downloads/instantclient_19_16:$DYLD_LIBRARY_PATH
   ```

   保存退出后，执行：
   ```bash
   source ~/.zshrc
   ```

   **检查是否清理干净**：
   ```bash
   echo $PATH | grep instantclient
   echo $DYLD_LIBRARY_PATH | grep instantclient
   ```
   如果没有任何输出，就说明已经清理干净了。

3. **验证 sqlplus 已经无法使用**（可选，但建议检查）  
   ```bash
   sqlplus -v
   ```
   正常应该提示 `command not found: sqlplus`

### 额外建议

- **如果你以后可能还会用到**：不要直接删除文件夹，而是把整个 `instantclient_19_16` 文件夹移动到其他地方（比如 `/Users/noah/oracle-instantclient/`），需要时再移回来并重新设置 PATH 即可。
- **DMG 文件**：你下载的两个 `.dmg` 文件（instantclient-basic-... 和 instantclient-sqlplus-...）可以直接删掉或移到垃圾桶，不影响系统。
- **如果以后想重新安装**：直接重新运行那两个 DMG 里的 `install_ic.sh` 就行，过程和之前一样。

这样卸载就非常干净了，不会留下垃圾。

**小贴士**：  
以后如果你想更方便地管理，可以把 Instant Client 安装到固定位置，例如：
`/opt/oracle/instantclient_19_16`

需要我给你一个“一键清理”的命令组合（把删除文件夹 + 清理 zshrc 合并成几行命令）吗？还是你已经准备删除它了？