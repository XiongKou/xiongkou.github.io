---
layout: post
title:  "vue导航栏"
categories: vue
tags: 导航
author: 熊叩
---

* content
{:toc}
 
移动端需要导航栏，如果使用vant的导航栏调整样式会非常麻烦，自己封装一个的话用起来比较方便










#调用

```html
<akdHeader>深圳澳康达
	<template #right>
		<i class="fa fa-search" style="font-size: 18px"></i>
	</template>
</akdHeader>
```
 
# 效果

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/daohang/daohang.png)

# html

```html

<template>
	<div class="akd-xk-header">
		<div class="left">
			<slot name="left">
				<i class="fa fa-angle-left" @click="goBack"></i>
			</slot>
		</div>
		<h3>
			<slot></slot>
		</h3>
		<div>
			<slot name="right">
				<i></i>
			</slot>
		</div>
	</div>
</template>

<script>
export default {
	name: "akdHeader",
	methods: {
		goBack() {
			this.$router.back();
		},
	},
};
</script>

<style  lang="scss">
.akd-xk-header {
	flex: none;
	height: 60px;
	background-color: goldenrod;
	color: white;
	display: flex;
	align-items: center;
	justify-content: center;
	div {
		flex: none;
		width: 60px;
		height: 100%;
		> i {
			height: 100%;
			width: 100%;
			display: inline-block;
			text-align: center;
			font-size: 28px;
			line-height: 60px;
		}
	}
	h3 {
		flex: auto;
		margin: 0;
		display: flex;
		align-items: center;
		justify-content: center;
	}
}
</style>

```
