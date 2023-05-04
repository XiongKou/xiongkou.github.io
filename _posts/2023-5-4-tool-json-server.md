---
layout: post
title:  "json server"
categories: tool
tags: json-serve
author: 熊叩
---

* content
{:toc}
 
有时候需要模拟ajax请求数据，json-server能实现简单的ajax请求











# 安装json server


```
npm install -g json-server
```

# 创建一个 `db.json`的文件

```
{
  "posts": [
    { "id": 1, "title": "json-server", "author": "typicode" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 }
  ],
  "profile": { "name": "typicode" }
}
```
 
# 开启json server

```
json-server --watch db.json
```

现在你访问 `http://localhost:3000/posts/1` ，可以得到请求数据

```
{ "id": 1, "title": "json-server", "author": "typicode" }
```

# 路由

根据之前的db.json文件，使用下面的路由。

```
GET    /posts
GET    /posts/1
POST   /posts
PUT    /posts/1
PATCH  /posts/1
DELETE /posts/1
```

# 过滤

使用。访问深层次的属性

```
GET /posts?title=json-server&author=typicode
GET /posts?id=1&id=2
GET /comments?author.name=typicode
```

# 分页

使用 _page 和 可选的 _limit 来分页

```
GET /posts?_page=7
GET /posts?_page=7&_limit=20
```

# 排序

添加 _sort 和 _order来排序

```
GET /posts?_sort=views&_order=asc
GET /posts/1/comments?_sort=votes&_order=asc
```

多个字段排序，使用下面的格式

```
GET /posts?_sort=user,views&_order=desc,asc
```

# 参考 

[json server 地址](https://github.com/typicode/json-server)