---
layout: post
title:  "vue router"
categories: vue
tags: 路由
author: 熊叩
---

* content
{:toc}
# vue删除eslint弹出警告的

1.package.jon里面找到eslint相关的全部删除

# vue创建一个vue router项目

# router主要用来页面跳转，并且要传递参数

1.npm install vue-router@4
	

2.在src下新建一个文件夹router，新建一个index.js

```js
import {createRouter,createWebHistory} from "vue-router";


// 1.导入router跳转的页面
import Home from '../Home.vue'
import About from '../About.vue'
import XiongKou from '../XiongKou.vue'

// 2. 定义路由访问地址
const routes = [
  { path: '/home', component: Home },
  { path: '/about', component: About },
  { path: '/xiongKou', component: XiongKou },
]

const router=createRouter({
    history:createWebHistory(),
    routes
 
})
export default router;

```

3.在main.js里面加载路由组件
```js
import { createApp } from 'vue'
import App from './App.vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

// 引入router文件夹
import router from './router'

const app = createApp(App)
app.use(ElementPlus)

// 使用路由
app.use(router)
app.mount('#app')
```

4.在app.vue里面可以使用定义的router了。
```
<template>
  <!-- 加载router跳转后的html,这一句必须写，不然没东西 -->
  <router-view></router-view>
</template>

<script>
export default {
  created(){
    this.$router.push('/home');

  },
  name: 'App',
  // components:{
  //   Navbar
  // }
}
</script>
```
5.引进element plus
```
1.npm install element
2.main.js里面加入
	import ElementPlus from 'element-plus'
	import 'element-plus/dist/index.css'
	app.use(ElementPlus)
```
6.第二点里面有几个path路径我们就要创建几个vue页面

```
<template>
//el-button,是引用element ul，npm install element-plus
  <el-button @click="toXiongkou">我老婆熊叩</el-button>
</template>
<script>
export default {
  methods: {
    toXiongkou() {
	//跳转去哪里的页面
      this.$router.push("/xiongKou");
	  
    },
  },
};
</script>
```




