# 子比主题美化系列（三）：logo扫光特效

## 前言

我们浏览网站时经常看到一些网站的LOGO带有扫光效果，如网站左上角。该效果可由纯CSS实现，使用时可能需要根据你的网站自行修改参数

## 效果展示

![logo_gif](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/10/13/6528e09f3b3a1.gif)

## 使用教程

以下代码添加到zibll主题>自定义代码>自定义CSS里。（实际修改后可压缩再放入，理论提高访问效率）

```css
/* logo扫光特效 */
.navbar-brand {
	position: relative;
	overflow: hidden;
	margin: 0px 0 0 0px;
}

.navbar-brand:before {
	content: "";
	position: absolute;
	left: -665px;
	top: -460px;
	width: 200px;
	height: 15px;
	background-color: rgba(255,255,255,.5);
	-webkit-transform: rotate(-45deg);
	-moz-transform: rotate(-45deg);
	-ms-transform: rotate(-45deg);
	-o-transform: rotate(-45deg);
	transform: rotate(-45deg);
	-webkit-animation: searchLights 6s ease-in 0s infinite;
	-o-animation: searchLights 6s ease-in 0s infinite;
	animation: searchLights 6s ease-in 0s infinite;
}

@-moz-keyframes searchLights {
	50% {
		left: -100px;
		top: 0;
	}

	65% {
		left: 120px;
		top: 100px;
	}
}

@keyframes searchLights {
	40% {
		left: -100px;
		top: 0;
	}

	60% {
		left: 120px;
		top: 100px;
	}

	80% {
		left: -100px;
		top: 0px;
	}
}
```