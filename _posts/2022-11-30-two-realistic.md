---
layout: post
title:  "vue操作"
categories: vue
tags: 组件
author: 熊叩
---

* content
{:toc}


# vue创建一个vue cli项目


1.运行一个新项目
	```//hello-world(这个是文件名，可以自己取)
	vue create hello-world
	```
2.第二步默认

3.cd到你安装的文件夹

4.运行npm run serve  npm uninstall后面接要 删除的内容 
	npm i 每次修改了pack.age.json里面的依赖的内容。都要更新下
	ctrl+c停止脚手架服务


5.创建一个组件在components里面新建一个ButtonCounter.vue(新的文件名)
	```
	<script>
	import ButtonCounter from './ButtonCounter.vue'

	export default {
	  components: {
		ButtonCounter
	  }
	}
	</script>

	<template>
	  <h1>Here is a child component!</h1>
	  <ButtonCounter />
	</template>
```
6.使用组件 创建在src文件下面
7.props属性和data是平级的,位置随意调整，created初始化
	props:["title参数"]

8.（1）定义一个data里面的对象声明对象
	（2）created初始化,加this
	（3）template ：kk="name"
	绑定vue里面的对象
	

9. 变大的字体
	$emit调用父级组件上的方法
	第一步：子组件定义click方法  @click="fontLarge"
	第二步: 子组件的click方法，调用父组件的方法     fontLarge(){this.$emit('enlarge-text'}
	第三部：父组件在子组件html上定义方法 ,一定要和子组件调用的方法一样 , 这里叫 @enlarge-text="doSomething"
	第四部：在父组件里面实现doSomething方法的具体细节
	
10.通过插槽来分配内容
		<AlertBox>组件
		  Something bad happened.
		</AlertBox>
		定义插槽<slot />
		
11.is就是他的属性，不就：就是一个字符串

12.只有一行显示多个的时候用flex布局。
	单个元素用text-align=center,针对行内块级，或者文字，或者行内元素，只能设置在父级上，并且父级有足够的宽度

13.文件路径
	当前目录   ./
	上一级   ../
14.vue三部曲，先





