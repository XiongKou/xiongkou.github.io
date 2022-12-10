---
layout: post
title:  "箭头函数"
categories: es6
tags: 箭头
author: 熊叩
---

* content
{:toc}


#  认识箭头函数

```
(参数) => { 函数体 }
let sum = function(a, b) {
	return a + b;
}

// 箭头函数
let sum1 = (a, b) => {
	return a + b;
}
```	
# 嵌入函数
```
let arr = [1, 2, 3, 4, 5];
arr.map(val => val * 2); // [2, 4, 6, 8, 10]
```

# 箭头函数中this 指向

```
箭头函数内，this指向它外层的对象，它自身没有this，而fuction自身有this
let num = 11;
const obj1 = {
	num: 22,
	fn1: function() {
		let num = 33;
		const obj2 = {
			num: 44,
			fn2: () => {
				console.log(this.num);
			}
		}
		obj2.fn2();
	}
}
obj1.fn1(); // 22

```