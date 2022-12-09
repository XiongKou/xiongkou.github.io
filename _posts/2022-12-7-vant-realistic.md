---
layout: post
title:  "vant手机"
categories: vant
tags: 手机版本
author: 熊叩
---

* content
{:toc}


# vue创建一个vant项目

1.npm i vant
	
2.在main.js加入
```
	import Vant from 'vant';
	import 'vant/lib/index.css';
	app.use(Vant);
```
# vue插槽的用法
```
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```