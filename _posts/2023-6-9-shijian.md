---
layout: post
title:  "事件委托"
categories: tool
tags: 事件
author: 熊叩
---

* content
{:toc}
 
事件委托是指通过事件冒泡，只处理一个事件，来达到控制多个事件的目的，这种方法能提高性能，也能防止出现事件穿透等问题。










事件委托的函数如下：

```js
function getTarget(target, curr, parent) {
    return function run(t, c) {
        if (!c || !t || t === (parent || document)) {
            return false;
        } else if ((function contrast(t, c) {
            switch (c.slice(0, 1)) {
                case '.':
                    return Array.prototype.indexOf.call(t.classList, c.slice(1)) !== -1;
                case '#':
                    return t.getAttribute('id') === c.slice(1);
                default:
                    return t.nodeName.toLocaleLowerCase() === c.toLocaleLowerCase();
            }
        })(t, c)) {
            return t;
        } else if (t.hasAttribute && t.hasAttribute('stop')) {
            return false;
        } else {
            return run(t.parentNode, c);
        }
    }(target, curr);
}
```

事件定义在最外层就可以了
```html
<div @click='testEvent'>
	<i class='fa-save'></i>
	<span class='address'></span>
	<p>测试</p>
</div>
```

这个函数通过递归的方式，从事件源对象上冒泡往外查找原始，直到找到正确的元素，调用方法是这样的：

```js
function testEvent(e){
	let target = getTarget(e.target, ".fa-save");
	if (target) {
  		...     
		return;
	}
	target = getTarget(e.target, "#address");
	if (target) {
  		...
		return;
	}
	target = getTarget(e.target, "p");
	if (target) {
  		...
		return;
	}
}
```

事件委托函数需要获取到事件源来判断是哪一个元素，是用target 第二个参数来判断的，这个参数类似 jquery 语法，

1. 如果是类样式元素的事件，使用`.`加上class名称，`getTarget(e.target, ".fa-save")`
2. 如果是ID样式元素的事件，使用`#`加上id名称，`getTarget(e.target, "#address")`
3. 如果是标签元素的事件，例如`p`标签，使用标签名称来访问，`getTarget(e.target, "p")`