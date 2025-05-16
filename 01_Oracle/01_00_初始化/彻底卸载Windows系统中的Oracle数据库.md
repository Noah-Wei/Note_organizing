# 彻底卸载Windows系统中的Oracle数据库

## 准备工作

在开始卸载前，请确保：
1. 备份所有重要的Oracle数据库数据
2. 关闭所有正在使用Oracle的应用程序
3. 确保你有管理员权限

## 1. 停止所有Oracle相关服务

### 方法一：通过服务管理器
1. 按下`Win + R`键，输入`services.msc`并回车
2. 在服务列表中找到所有Oracle开头的服务（通常包括以下服务）：
   - OracleService[实例名]
   - OracleOraDb11g_home1TNSListener
   - OracleJobScheduler[实例名]
   - OracleMTSRecoveryService
   - OracleVssWriter[实例名]
3. 右键点击每个服务，选择"停止"

### 方法二：通过任务管理器
1. 按下`Ctrl + Alt + Delete`，选择"任务管理器"
2. 切换到"服务"选项卡
3. 找到所有Oracle开头的服务，右键点击选择"停止"

## 2. 删除注册表中的Oracle信息

**警告**：修改注册表前建议先备份注册表

1. 按下`Win + R`，输入`regedit`并回车
2. 导航到以下路径并删除相应项：

   - **服务项**：
     ```
     HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\
     ```
     删除所有以"Oracle"开头的项

   - **软件信息**：
     ```
     HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE
     ```
     删除整个ORACLE文件夹

   - **事件日志**：
     ```
     HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application
     ```
     删除所有以"Oracle"开头的项

3. (可选)如果安装了多个Oracle产品，还需检查：
   ```
   HKEY_LOCAL_MACHINE\SOFTWARE\ODBC
   ```
   删除与Oracle相关的ODBC数据源

## 3. 删除环境变量

1. 右键点击"此电脑" → "属性" → "高级系统设置"
2. 点击"环境变量"按钮
3. 在"系统变量"部分：
   - 编辑`PATH`变量，删除所有包含Oracle的路径
   - 删除`ORACLE_HOME`变量（如果存在）
   - 删除`TNS_ADMIN`变量（如果存在）
   - 删除`NLS_LANG`变量（如果存在）

**注意**：修改PATH时建议先将其内容复制到记事本中，编辑后再粘贴回去，避免误删其他重要路径。

## 4. 重启操作系统

完成上述步骤后，必须重启计算机以确保所有更改生效。

## 5. 删除剩余文件和目录

重启后，手动删除以下Oracle相关目录（根据你的实际安装情况）：

- 安装目录：
  ```
  C:\app\<用户名>\oracle
  ```
  或你自定义的Oracle基目录

- 程序文件目录：
  ```
  C:\Program Files\Oracle
  C:\Program Files (x86)\Oracle
  ```

- 系统目录：
  ```
  C:\Windows\System32\config\systemprofile\Oracle
  ```

- 用户目录：
  ```
  C:\Users\<用户名>\Oracle
  C:\Users\<用户名>\AppData\Local\Oracle
  C:\Users\<用户名>\AppData\Roaming\Oracle
  ```

- 临时文件：
  ```
  C:\temp\oracle
  C:\Windows\Temp\oracle
  ```

## 6. (可选) 清理开始菜单项

1. 打开文件资源管理器，导航到：
   ```
   C:\ProgramData\Microsoft\Windows\Start Menu\Programs
   ```
2. 删除Oracle相关的文件夹和快捷方式

## 验证卸载是否彻底

1. 检查服务列表中是否还有Oracle服务
2. 搜索整个磁盘，确认没有残留的Oracle文件
3. 尝试重新安装Oracle，确保不会出现冲突

## 注意事项

1. 如果计划重新安装Oracle，建议在卸载后使用专门的注册表清理工具进行深度清理
2. 对于企业环境，建议先在一个测试机器上执行卸载流程
3. 某些安全软件可能会阻止注册表或系统文件的删除，必要时临时禁用安全软件

完成以上所有步骤后，你的系统应该已经完全清除了Oracle数据库及其相关组件。
