---
layout: post
title:  "css样式"
categories: css
tags: 样式
author: 熊叩
---

* content
{:toc}


#  布局
1.父元素设置相对定位position：relative
2.input宽度100%铺满
3.图标要绝对定位position：absolute，高度由input撑开
4.input的padding，border要去掉
```
 <div class="zui-bian">
      <input type="text" value="拓号组" class="text-nai" />
      <van-icon name="arrow-down" />
    </div>
	
.text-nai {
  text-indent: 5px;
  border-radius: 3px;
  width: 100%;
  padding: 0;
  border: 0;
  height: 40px;
}
.zui-bian {
  position: relative;
  border: 1px solid gray;
  width: 200px;
}
.zui-bian i {
  position: absolute;
  right: 0;
  width: 40px;
  height: inherit;
  line-height: 40px;
}
```

#命名规则
```
text-nai 什么-什么
```