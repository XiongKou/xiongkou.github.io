---
layout: post
title:  "axios"
categories: axios
tags: axios安装
author: 熊叩
---

* content
{:toc}


# 1.创建一个json文件夹，json文件夹内新创一个data.json
在这里json数据如下：
```
{"list":[
{"id":1,
 "name":"吃饭"},
{"id":1,
 "name":"吃饭"},
{"id":1,
 "name":"吃饭"}
]}
```
# 2.安装json-server的两个依赖



```
npm -g i json-server

npm install -g json-server
```	
# 3.安装axios依赖
```
npm i axios
```
# 哪个页面要用就导入axios使用
```
import axios from 'axios';
```




# 4.新建终端选择json文件夹运行：


```
json-server --watch --port 8081 data.json
```	
# 复制运行得到的链接进浏览器运行出结果如下


```
resources
http://localhost:8081/list

home
http://localhost:8081
```	
# 8.接下来就是App.vue里的代码


```
第一步再data.json 创建传入得函数
{   
    "xiongReport":{
        "num":12,
        "shang":200,
        "shi":100
    }
        
      
    
}
第二步在你需要用的页面
export default{
created(){
//定义个接收函数
return{
xiongReport:{};},
created(){
//获得权限
axios.get("http://localhost:8081/list").then(res=>
{this.xiongReport=res.data;
});
}
第三步就可以在template需要的地方用了{{ xiongReport.num}}
```	