---
layout: post
title:  "javascript对象"
categories: javascript
tags: 详情
author: 熊叩
---

* content
{:toc}


#  对象的定义
对象是引用数据类型，它可以被理解为一个保存复杂数据类型的容器，ECMAScript中的对象其实就是一组数据(属性)和功能(方法)的集合，它允许自身的的属性被动态的添加和删除。

# 对象的创建
```
键值对
1.字面量创建对象
var obj={
name:"zhangsan",
age:18
}
2.构造函数创建对象
var obj = new Project()
obj.name = '张三'
obj.age = 18
```
# 通过属性名访问
```
obj.name //zhangsan
```
# 通过for-in遍历对象的属性
```
for(var key in obj){
 
    console.log(obj[key])
}
//输出结果为 zhangsan 18

```
# 随机数
```
猜数字游戏
        // 1.随机生成一个1~10 的整数  我们需要用到 Math.random() 方法。
        // 2.需要一直猜到正确为止，所以需要一直循环。
        // 3.while 循环更简单
        // 4.核心算法：使用 if  else if 多分支语句来判断大于、小于、等于。
        function getRandom(min, max) {
            return Math.floor(Math.random() * (max - min + 1)) + min;
        }
        var random = getRandom(1, 10);
        while (true) { // 死循环
            var num = prompt('你来猜？ 输入1~10之间的一个数字');
            if (num > random) {
                alert('你猜大了');
            } else if (num < random) {
                alert('你猜小了');
            } else {
                alert('恭喜你，猜对了');
                break; // 退出整个循环结束程序
            }
        }
        // 要求用户猜 1~50之间的一个数字 但是只有 10次猜的机会
```
#日期格式化
```
输出当前日期
// 格式化日期 年月日 
        var date = new Date();
        console.log(date.getFullYear()); // 返回当前日期的年  2019
        console.log(date.getMonth() + 1); // 月份 返回的月份小1个月   记得月份+1 呦
        console.log(date.getDate()); // 返回的是 几号
        console.log(date.getDay()); // 3  周一返回的是 1 周六返回的是 6 但是 周日返回的是 0
        // 我们写一个 2020年 5月 1日 星期三
        var year = date.getFullYear();
        var month = date.getMonth() + 1;
        var dates = date.getDate();
        var arr = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
        var day = date.getDay();
        console.log('今天是：' + year + '年' + month + '月' + dates + '日 ' + arr[day]);


		// 格式化日期 时分秒
        var date = new Date();
        console.log(date.getHours()); // 时
        console.log(date.getMinutes()); // 分
        console.log(date.getSeconds()); // 秒
        // 要求封装一个函数返回当前的时分秒 格式 08:08:08
        function getTimer() {
            var time = new Date();
            var h = time.getHours();
            h = h < 10 ? '0' + h : h;
            var m = time.getMinutes();
            m = m < 10 ? '0' + m : m;
            var s = time.getSeconds();
            s = s < 10 ? '0' + s : s;
            return h + ':' + m + ':' + s;
        }
        console.log(getTimer());

```
# 对象是引用数据类型，
```
https://blog.csdn.net/qq_44853882/article/details/123585822
```

# 对象
```
zhuzheng[
{
	zhu:3250
},
{
	zhengping:350
}
]
//把第一个元素改下数据
zhuzheng[1].zhengping=450;
//在第一个元素下面加条数据
zhuzheng[1].zhubajie="你是天使"；
```