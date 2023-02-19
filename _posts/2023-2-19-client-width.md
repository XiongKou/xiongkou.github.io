---
layout: post
title:  "DOM元素尺寸和位置"
categories: js 
tags: dom
author: 熊叩
---

* content
{:toc}
 
这篇博客主要记录一下页面中某个元素的各种大小和各种位置的计算方式









# innerWidth 和 document.documentElement.clientWidth

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-2-19/1.png)

1. window.innerWidth : 浏览器的宽度,不包含边框,包含滚动条
2. document.documentElement.clientWidth : 浏览器的内容宽度,不包含边框,不包含滚动条
3. window.outerWidth : 浏览器的宽度,包含边框和滚动条
4. window.screen.width : 显示器的宽度

# clientWidth、scrollWidth、offsetWidth

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-2-19/2.png)

1. clientWidth: 元素内容+内边距
2. scrollWidth: 元素滚动内容+内边距
3. offsetWidth: 元素内容+内边距+元素边框

## 滚动条

当元素里面有滚动条的时候,clientWidth、scrollWidth、affsetWidth计算宽度有如下变化

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-2-19/4.png)

1. clientWidth: 元素内容+内边距-滚动条宽度
2. scrollWidth: 元素滚动内容+内边距-滚动条宽度
3. offsetWidth: 元素内容+内边距+元素边框

所以我感觉使用offsetWidth时比clientWidth更加准确

## clientLeft 和 clientTop

获得元素的左边框和上边框

## scrollTop 和 scrollLeft

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-2-19/3.jpg)

可以设置当前元素相对于父元素的位置,要记得给对应的父元素设置相对点`position: relative;`

## offsetLeft 和 offsetTop

获取当前元素相对于父元素的位置 , 如果多层的话，就必须使用循环或递归

```js
function offsetLeft(element) {
	var left = element.offsetLeft; //得到第一层距离
	var parent = element.offsetParent; //得到第一个父元素
	while (parent ! == null) { //如果还有上一层父元素
		left += parent.offsetLeft; //把本层的距离累加
		parent = parent.offsetParent; //得到本层的父元素
	} 
	return left;
}
```

# getBoundingClientRect()

这个方法返回一个矩形,包含4个属性:left、top、right和 bottom。分别表示元素各边与页面上边和左边的距离。

可以用在鼠标事件获得元素的位置,但是低版本的浏览器有兼容性的问题,下面这个方法能获得元素兼容性坐标

```js
function getRect(element) {
	var rect = element.getBoundingClientRect();
	var top = document.documentElement.clientTop;
	var left = document.documentElement.clientLeft;
	return {
		top : rect.top - top, 
		bottom : rect.bottom - top,
		left : rect.left - left,
		right : rect.right - left
	}
}
```

