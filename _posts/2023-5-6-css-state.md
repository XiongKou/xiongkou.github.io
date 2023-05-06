---
layout: post
title:  "css标记"
categories: css
tags: 标记
author: 熊叩
---

* content
{:toc}
 
移动端需要显示数据的状态，这里记录一下一个好看的数据状态css











 
# 效果

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-05-06/biaoji.png)

# html


```html

<i class="mf-status mf-bg-1">深圳澳康达</i>
<i class="mf-status mf-bg-2">深圳澳康达</i>
<i class="mf-status mf-bg-3">深圳澳康达</i>
<i class="mf-status mf-bg-4">深圳澳康达</i>
<i class="mf-status mf-bg-5">深圳澳康达</i>
<i class="mf-status mf-bg-6">深圳澳康达</i>
<i class="mf-status mf-bg-7">深圳澳康达</i>
<i class="mf-status mf-bg-8">深圳澳康达</i>
<i class="mf-status mf-bg-9">深圳澳康达</i>

```

# CSS

```
.mf-status {
	position: relative;
	font-style: normal;
	font-weight: 400;
	padding: 3px 15px 3px 20px;
	margin-bottom: 5px;
	display: inline-block;
	font-size: 15px;
	-webkit-clip-path: polygon(0 0, 10px 50%, 0 100%, calc(100% - 10px) 100%, calc(100% - 10px) calc(100% + 5px), 100% calc(100% + 5px), 100% 0);
	clip-path: polygon(0 0, 10px 50%, 0 100%, calc(100% - 10px) 100%, calc(100% - 10px) calc(100% + 5px), 100% calc(100% + 5px), 100% 0);
	color: white;
}

.mf-status::after {
	position: absolute;
	content: "";
	right: -10px;
	bottom: -5px;
	border-top: 0 solid transparent;
	border-bottom: 5px solid transparent;
	border-left: 10px solid #ababab;
	border-right: 10px solid transparent;
	width: 0;
	height: 0;
}

.mf-bg-1 {
	background: rgb(189, 195, 239);
}

.mf-bg-2 {
	background: #008021;
}

.mf-bg-3 {
	background: #ff5200;
}

.mf-bg-4 {
	background: #d0a56f;
}

.mf-bg-5 {
	background: orange;
}

.mf-bg-6 {
	background: cornflowerblue;
}

.mf-bg-7 {
	background: lightblue;
}

.mf-bg-8 {
	background: chocolate;
}

.mf-bg-9 {
	background: olivedrab;
}
```
