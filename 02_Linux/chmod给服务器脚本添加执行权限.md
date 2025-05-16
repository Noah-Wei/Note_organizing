# chmod 给服务器脚本添加执行权限

在 Linux/Unix 系统中，给服务器脚本添加执行权限的命令是 `chmod`（change mode）。以下是具体方法：

## 1. 基本命令格式
```bash
chmod +x 脚本文件名
```
示例：
```bash
chmod +x server.sh
```

## 2. 如果需要同时给所有者、组和其他用户执行权限：
```bash
chmod a+x 脚本文件名
```

## 3. 更精细的权限控制（数字模式）：
```bash
chmod 755 脚本文件名
```
- `7`（所有者）：读 + 写 + 执行（4+2+1）
- `5`（组和其他用户）：读 + 执行（4+0+1）

## 4. 如果需要递归给目录下所有脚本添加权限：
```bash
chmod -R +x 目录名/
```

## 注意事项：
1. 脚本首行需要指定解释器（如 `#!/bin/bash`）
2. 执行脚本时建议用 `./script.sh` 或绝对路径
3. 如果脚本涉及敏感操作，建议仅给必要用户权限（如用 `chmod u+x` 仅限所有者）

## 完整示例流程：
```bash
# 1. 创建脚本
echo '#!/bin/bash\necho "Hello World"' > server.sh

# 2. 添加执行权限
chmod +x server.sh

# 3. 执行脚本
./server.sh
```
