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

# flex的用法
1.2要配合着一起用
（1.flex :auto 自动铺满剩余的空间
    2.flex:none  遵循flex布局，但是宽度就是自身元素的所占的宽度）

3.父元素设置高度子元素如果要用一样的，要用height:inherit继承父元素的高度

4.input有默认的padding要去掉

5.    height: calc(100% - 16px);
    高度等于 父级高度减掉16px

6.<input placeholder="请输入车架号或车名" >placeholder文本框提示
    outline: 0;外边框 要去掉
   