---
layout: post
title:  "axios"
categories: axios
tags: axios安装
author: 熊叩
---

* content
{:toc}


# 1.安装json-server的两个依赖


```
npm -g i json-server

npm install -g json-server
```	
# 2.安装axios依赖
```
npm i axios
```
# 3.全局导入axios使用src目录下main.js文件内
```
import axios from 'axios';
```
# 4.配置全局默认地址：src目录下main.js文件内


```
axios.defaults.baseURL = 'http://localhost:8081'
```	

# 5.创建一个json文件夹，json文件夹内新创一个data.json
在这里json数据如下：
```
"list":[
{"id":1,
 "name":"吃饭"},
{"id":1,
 "name":"吃饭"},
{"id":1,
 "name":"吃饭"},
]
```

# 6.新建终端选择json文件夹运行：


```
json-server --watch --port 8081 data.json
```	
# 7.复制运行得到的链接进浏览器运行出结果如下


```
resources
http://localhost:8081/list

home
http://localhost:8081
```	
# 8.接下来就是App.vue里的代码


```
created(){
//获得权限
axios.get("http://localhost:8081/list").then(res=>
{this.xiongReport=res.data;
});
```	