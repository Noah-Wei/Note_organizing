# MAC软件设置

## 1.Pycharm

解释器选择

```
自定义环境——>生成新的——>conda——>自定义名称
删除项目不会产出这个【虚拟的独立conda环境】
```

安装插件

```
主题
	Catppuccin Theme
	One Dark Pro
图标插件
	Catppuccin Icons  搭配主题一起
美化
	Rainbow Brackets		-- 彩虹括号，不同层级括号用不同颜色，代码结构一目了然
	Rainbow CSV				-- CSV/TSV 文件高亮、查询、编辑像表格一样
	Indent Rainbow			-- Python 缩进用不同颜色区分层级，错误缩进标红
```

设置

```
字体：
大小：13
行高：1.3
鼠标改变大小：编辑器——>常规——>鼠标控制
```

## 2.Webstorm

安装插件

```
主题
	Catppuccin Theme
	One Dark Pro
图标插件
	Catppuccin Icons  搭配主题一起
美化
	Rainbow Brackets		-- 彩虹括号，不同层级括号用不同颜色，代码结构一目了然
	

```

## 3.Datagrip

安装插件

```
主题
	Catppuccin Theme
	One Dark Pro
图标插件
	Catppuccin Icons  搭配主题一起
美化
	Rainbow Brackets		-- 彩虹括号，不同层级括号用不同颜色，代码结构一目了然

```

## 4.sublime

window端配置文件

```json
{
    "font_face": "Maple Mono NL NFMono CN Regular",  
    "font_size": 13,                         
    "font_options": ["subpixel_antialias"],  
    "line_padding_top": 1,
    "caret_extra_width": 1,                  

    "color_scheme": "Catppuccin Frappe.sublime-color-scheme",
    "theme": "Default.sublime-theme",
    "ignored_packages":
    [
		"Package Control",
        "Vintage",
    ],
    "index_files": false,
    "update_check": false,
}
```

mac端

```
安装的插件：
	中文
	配色：			Catppuccin Frappe
	括号高亮：		BracketHighlighter
```



```json
{
    // 忽略不需要的包（禁用 Vim 风格）
    "ignored_packages": [
        "Vintage"
    ],

    // 界面主题
    "theme": "Default.sublime-theme",

    // 代码区配色（Catppuccin Frappe 暗色护眼）
    "color_scheme": "Packages/Catppuccin/Catppuccin Frappe.sublime-color-scheme",

    // 字体设置
    "font_face": "MapleMonoNL-NFMono-CN-Regular",
    "font_size": 13,
    "font_options": ["liga"],  // 开启连字功能

    // 代码行间距，视觉更舒适
    "line_padding_top": 2,
    "line_padding_bottom": 2,

    // 其它常用设置
    "highlight_line": true,		// 高亮当前光标所在的整行代码
    "caret_style": "smooth",	// 设置光标（Caret）的样式
    "show_minimap": true,		// 显示代码 迷你地图（Minimap）
    "word_wrap": false			// 控制代码 是否自动换行
}
```



