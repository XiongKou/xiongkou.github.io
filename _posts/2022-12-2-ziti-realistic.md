---
layout: post
title:  "vue引用工具"
categories: 字体
tags: 组件
author: 熊叩
---

# vue-lic脚手架中引入font-awesome
1.安装font-awesome

npm i font-awesome --production

2.在main.js中引用
import 'font-awesome/css/font-awesome.css'

3.应用
<i class="fa fa-camera-retro fa-lg"></i>


# vue-lic脚手架中引入element-plus
  
  1、 npm install element-plus --save
  2 main.ts
  
		import { createApp } from 'vue'
		import ElementPlus from 'element-plus'
		import 'element-plus/dist/index.css'
		import App from './App.vue'

		const app = createApp(App)

		app.use(ElementPlus)
		app.mount('#app')