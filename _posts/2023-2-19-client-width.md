---
layout: post
title:  "DOM元素尺寸和位置"
categories: js 
tags: js
author: 熊叩
---

* content
{:toc}
 
这篇博客主要记录一下页面中某个元素的各种大小和各种位置的计算方式








# 流式布局

文档流是指css元素默认的布局方式 , 默认从上到下 , 从左到右 

# 绝对定位

绝对定位是打破了默认的布局方式 , 配合top、right、bottom、left进行定位 , 设置 z-index 来控制元素的位置


下面是一个绝对布局的例子

```html
.box{
	position:absolute;
	top:0;
	left:0;
	right:0;
	bottom:0;
	z-index:9;
	background-color:#e5e5e5;
}

<body>
    <div class="box"></div>
</body>
```

如果同时设置了 `top:0;left:0;right:0;bottom:0;` , 就是告诉浏览器 , 我需要和相对定位的父元素一样宽,一样高 , 
也就是这个 div要铺满整个屏幕

## 绝对定位的相对点

绝对定位比较重要的一个点就是它的相对点 , 每个有绝对定位的元素都会从里往外找它父级元素的position属性 , 直到找到 `position:relative` 这个父元素 , top/left/right/bottom 都是相对这个父元素来设置的 , 例如下面的例子

```html
.container {
	background: #607d8b;
	position: relative;
	width: 400px;
	height: 400px;
}

.box1 {
	position: absolute;
	top: 50px;
	left: 50px;
	right: 50px;
	bottom: 50px;
	z-index: 9;
	background-color: #e5e5e5;
}

<div class="container">
	<div class="box1"></div>
</div>
```

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-02-05/css-position-1.png)

# 漂浮

浮动元素会脱离文档流，元素脱离文档流以后，不会再占用文档流的位置，它下边的元素会立即向上移动

```html
.box {
	position: relative;
	width: 400px;
	height: 400px;
}
.float-left {
	float: left;
	background: #607d8b;
}
.float-right {
	float: right;
	background: #8bc34a;
}

<div class="float-left box"></div>
<div class="float-right box"></div>
```

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-02-05/css-float-1.png)

# flex 弹性布局

flex 大大简化了布局的代码 , 但是他对于低版本的浏览器支持是有问题的 , 所以一般开发电脑版页面 , 我还是会选择传统的布局方式, 但是对于移动端开发来说 , flex布局是最好的选择.

例如现在要实现一个移动端页面 , 顶部和脚部div固定,内容部分如果超出出现滚动条,如下图

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-02-05/css-flex-1.png)

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
    <style>
        body {
            margin: 0;
        }
        html,body{
            height:100%;
        }
        .container {
            display: flex;
            flex-direction: column;
            height: 100%;
        }

        .header {
            background: green;
            height: 60px;
            flex: none;
        }

        .content {
            flex: auto;
            background: #e5e5e5;
            height: 9999px;
            margin-top: 10px;
            margin-bottom: 10px;
            overflow-y: scroll;
        }

        .footer {
            flex: none;
            background: orange;
            height: 60px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header"></div>
        <div class="content">
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
            <p>测试段落</p>
        </div>
        <div class="footer"></div>
    </div>
</body>
</html>
```

