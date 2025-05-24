# 兰空图床报错 PRC 时区问题解决方案

如果通过 `php -i` 命令已显示时区为 `Asia/Shanghai`，但 Lsky Pro 仍然报错 **`Timezone ID 'PRC' is invalid`**，说明 Lsky Pro 内部硬编码了 `PRC`，而没有动态读取 PHP 的系统时区配置。

此时，你需要手动修改 Lsky Pro 的源码或配置文件，将 `PRC` 替换为 `Asia/Shanghai`。以下是详细的解决步骤：

------

### **方法 1：修改 Lsky Pro 配置文件**

1. **进入容器：**

   通过以下命令进入 Lsky Pro 容器：

   ```bash
   docker exec -it lsky-pro-plus bash
   ```

2. **查找配置文件：**

   Lsky Pro 配置文件通常位于 `.env` 文件或 `config/` 目录下。首先，检查是否有 `.env` 文件，并进行全局搜索：

   ```bash
   cd /var/www
   ls -la .env  # 检查是否有 .env 文件
   grep -r "PRC" .  # 全局搜索 "PRC" 关键字
   ```

3. **修改时区配置：**

   如果找到了 `PRC`，将其替换为 `Asia/Shanghai`。可以使用 `sed` 命令批量替换：

   ```bash
   sed -i 's/PRC/Asia\/Shanghai/g' .env  # 修改 .env 文件
   sed -i 's/PRC/Asia\/Shanghai/g' config/*.php  # 修改 PHP 配置文件（如果有）
   ```

4. **重启 PHP-FPM 和 Nginx：**

   修改完成后，重启 PHP 和 Nginx 服务使改动生效：

   ```bash
   service php8.3-fpm restart
   service nginx reload
   exit
   ```

------

### **方法 2：修改 Lsky Pro 源码（若硬编码在代码中）**

如果 `PRC` 是硬编码在代码中的，按照以下步骤修改：

1. **进入容器：**

   ```bash
   docker exec -it lsky-pro-plus bash
   ```

2. **搜索硬编码时区：**

   使用 `grep` 搜索代码中硬编码的 `PRC`：

   ```bash
   cd /var/www
   grep -r "date_default_timezone_set('PRC')" .
   ```

   如果找到类似的代码（通常位于 `app/` 或 `bootstrap/` 目录），将 `PRC` 替换为 `Asia/Shanghai`：

   ```php
   date_default_timezone_set('Asia/Shanghai');  # 替换 PRC
   ```

3. **清理缓存：**

   Lsky Pro 可能会缓存配置，因此清理缓存是必须的。对于 Laravel 项目，可以使用以下命令：

   ```bash
   php artisan cache:clear
   php artisan config:clear
   ```

4. **重启服务：**

   修改完毕后，重启服务以应用更改：

   ```bash
   service php8.3-fpm restart
   service nginx reload
   exit
   ```

------

### **方法 3：通过 Docker 持久化修改（避免容器重启失效）**

如果希望避免每次容器重启后重新修改配置，可以在 `Dockerfile` 或 `entrypoint.sh` 中自动替换 `PRC`：

1. **修改 `docker/entrypoint.sh`：**

   在 `entrypoint.sh` 文件的最后，添加以下命令：

   ```bash
   # 替换 Lsky Pro 中的 PRC 时区
   sed -i "s/'PRC'/'Asia\/Shanghai'/g" /var/www/config/*.php 2>/dev/null
   sed -i "s/PRC/Asia\/Shanghai/g" /var/www/.env 2>/dev/null
   ```

2. **重新构建容器：**

   使用以下命令重建容器，使改动生效：

   ```bash
   docker-compose down
   docker-compose build
   docker-compose up -d
   ```

------

### **检查是否生效**

1. **进入网站后台**，查看是否仍然报错。

2. **查看日志：**

   查看容器日志，以确认是否仍然有问题：

   ```bash
   docker logs lsky-pro-plus
   ```

3. **如果问题依旧**，可能是缓存没有清理干净。尝试清理缓存：

   ```bash
   docker exec -it lsky-pro-plus bash
   cd /var/www
   rm -rf storage/framework/cache/*
   php artisan optimize:clear  # Laravel 项目适用
   exit
   ```

------

### **总结**

| 问题原因              | 解决方案                       |
| --------------------- | ------------------------------ |
| PHP 默认时区未设置    | 修改 `php.ini`（已做）         |
| Lsky Pro 硬编码 `PRC` | 修改 `.env` 或 `config/*.php`  |
| Lsky Pro 缓存未清理   | 运行 `php artisan cache:clear` |

在这种情况下，**PHP 时区已经正确设置**，但 **Lsky Pro 代码仍然使用 `PRC`**。因此，需要修改 Lsky Pro 的配置文件或源码。建议使用 `grep -r "PRC" /var/www` 查找具体位置进行修改。