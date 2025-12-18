# Q1

> 项目部署

## 本地部署（开发环境）

这是最简单、最常见的方式：在你自己的电脑上运行项目。

### 1.1.安装必要工具

确保你已经安装了：

- **Node.js（建议 v18 或更高）**（自带 npm）
- **Git**

如果还没安装，可以去：

- Node.js 官网下载安装：https://nodejs.org/

------

### 1.2.克隆仓库

打开终端（Terminal / PowerShell / Git Bash），运行：

```
git clone https://github.com/moleculemmeng020425/christmas-tree.git
cd christmas-tree
```

------

### 1.3.安装依赖

在项目根目录运行：

```
npm install
```

这会安装项目所需全部库（React、Three.js、R3F、MediaPipe 等）。

------

### 1.4.启动开发服务器

运行：

```
npm run dev
```

执行后，你会看到类似：

```
Local: http://localhost:5173
```

把这个地址复制到浏览器打开，就能看到圣诞树应用在本地运行了 

------

## 常见问题 & 提示

✅ **如果运行出错**

- 确保 Node.js 版本 >= 18
- 删除 `node_modules` 并重新 `npm install`

------

## 简单总结

| 步骤         | 命令            |
| ------------ | --------------- |
| 克隆项目     | `git clone ...` |
| 安装依赖     | `npm install`   |
| 启动本地预览 | `npm run dev`   |
| 生产打包     | `npm run build` |

# Q2:

> 我在本地有同一个前端项目的两份拷贝，分别位于 **A 目录** 和 **B 目录**。
>  A 目录中已经执行过 `npm install` 和 `npm run dev`。
>
> 如果我在 B 目录中对源码进行了修改并运行项目，是否需要在 B 目录中再次执行 `npm install`？
>  不同目录下的项目运行是否会相互影响或产生冲突？

## 结论

> **不会冲突，但在 B 目录里通常需要重新 `npm install`，然后再 `npm run dev`。**

## 解析

- **A 目录 和 B 目录是完全独立的两个项目**
- Node / npm **不会“记住”你在别的目录装过依赖**
  - `npm install` 做的事是：
    - 根据 **当前目录的 `package.json`**
    - 在 **当前目录** 生成 `node_modules`
  - npm **不会**：
    - 共享 A 目录的 `node_modules`
    - 全局缓存给 B 用（即使包已下载）
- 👉 **B 目录第一次运行，一定要 `npm install`**
- 👉 **不会和 A 目录产生任何冲突**

## 那 A 和 B 会不会互相影响？

> 完全不会

| 项目         | 是否独立   |
| ------------ | ---------- |
| node_modules | ✅ 独立     |
| npm run dev  | ✅ 独立     |
| 端口         | ⚠️ 可能冲突 |
| 代码         | ✅ 独立     |

------

## 唯一可能“冲突”的地方：端口号

如果你 **同时** 在 A 和 B 运行：

```
npm run dev
```

而它们都用的是：

```
http://localhost:5173
```

### 会发生什么？

- 后启动的那个会提示：
  - 端口被占用
  - 或自动换成 5174、5175

✔️ **这是正常现象，不是冲突**

------

##  常见误区

### 误区 1：复制项目就不用 npm install

> 错
>  只要 **没有 node_modules**，就必须 `npm install`

------

### 误区 2：npm install 会影响别的项目

> 错
>  npm 只影响 **当前目录**

------

### 误区 3：改了代码要重新 npm install

> 错
>  **只有在你改了 `package.json`（加依赖）时才需要**

# Q3

> 我已经在 **B 目录** 中成功运行了项目（执行过 `npm install` 和 `npm run dev`）。
>
> 如果我之后只是修改了源码：
>  1️⃣ 是否还需要再次执行 `npm install` 和 `npm run dev`？
>  2️⃣ 之前这些命令生成的文件或目录（例如 `dist`、`node_modules` 等）是否需要删除？

## 结论

### 1.**你只是修改了代码（`src/` 里的内容）**

👉 **不需要再运行 `npm install`**
 👉 **不需要删除任何东西**
 👉 **如果 dev 正在跑，什么都不用做，直接刷新浏览器即可**

------

### 2.**你修改了代码，但 dev 已经停了**

👉 只需要：

```
npm run dev
```

❌ 不要再 `npm install`

------

### 3.**你没有新增/删除依赖**

👉 **永远不需要重新 `npm install`**

------

## 什么时候才需要重新 `npm install`？

**只有这一种情况：**

> **你改了 `package.json`（比如安装了新依赖）**

例如你做了：

```
npm install three
npm install @react-three/fiber
```

或者手动改了 `package.json` 里的 `dependencies`

👉 这时才需要（或已经自动）执行 `npm install`

------

## 为什么修改代码不需要 npm install？

### npm install 的作用只有一个：

> **根据 `package.json` 安装依赖到 `node_modules`**

而你改的是：

- `src/*.js`
- `src/*.jsx`
- `src/*.ts`
- `src/*.tsx`
- `index.html`
- `public/` 里的资源

👉 **这些和依赖无关**

------

## 关于 `npm run dev`（非常重要）

### Vite / React 项目特性：

- `npm run dev` 启动的是 **开发服务器**
- 它有 **热更新（HMR）**

### 所以：

| 情况         | 你要做什么       |
| ------------ | ---------------- |
| dev 正在运行 | **什么都不用做** |
| dev 已停止   | `npm run dev`    |
| 改了代码     | 自动生效         |

------

## 那 `dist`、`build` 要不要删？

### 明确回答：**完全不用**

### 原因：

- `dist/` 是 **`npm run build`** 生成的
- `npm run dev` **根本不使用 `dist`**
- 删除与否，对 dev 没任何影响

------

## 那什么时候需要管 `dist`？

只有两种情况：

### ① 你要重新打生产包

```
npm run build
```

👉 会自动覆盖旧的 `dist`
 👉 不需要手动删除

------

### ② 你发现生产包有异常（极少）

```
rm -rf dist
npm run build
```

👉 可选操作，不是必须

------

## 总结：用一句话帮你彻底区分

> **npm install 是“装工具”
>  npm run dev 是“开机器”
>  改代码只是在“用机器”，不用再装工具**

------

## 最终速查表

| 你做了什么  | 要不要 npm install | 要不要 npm run dev | 要不要删 dist |
| ----------- | ------------------ | ------------------ | ------------- |
| 改 src 代码 | ❌                  | ❌（运行中）        | ❌             |
| dev 停了    | ❌                  | ✅                  | ❌             |
| 新装依赖    | ✅                  | ✅                  | ❌             |
| 打包上线    | ❌                  | ❌                  | ❌             |

