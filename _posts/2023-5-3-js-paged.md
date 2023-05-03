---
layout: post
title:  "js分页"
categories: js
tags: component
author: 熊叩
---

* content
{:toc}
 
经常重复写分页的代码，花了点时间封装一下











# 调用

```js
var count = 0;
akdPaged({
	view: document.querySelector(".container"),
	url: "http://localhost:3000/comments",
	pageSize: 3,
	callback: function(res) {
		for (let i = 0; i < res.length; i++) {
			count++;
			var p = document.createElement("p");
			p.innerHTML = "测试段落" + count;
			this.view.appendChild(p);
		}
	}
});
</script>
```

# 分页效果

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-05-04/paged.png)
 
# js

```js
/**
 * pageIndex: 起始页码，从0开始
 * pageSize:  每页条条数
 * url: 分页的Ajax，数据格式：{data:[],total:1000}
 * loading: 是否正在加载中
 * view: 需要分页的控件
 * callback: 回调函数
 * params：其他参数，需要拼接到ajax里面
 */
;
(function(app) {
    function AkdPaged(options) {

        this.init = function() {
            // 这里默认用http.post 请求数据
            // 查看是否有引入http请求
            if (typeof(http) == 'undefined' || !http.post) {
                this.createHttp();
            }

            if (!options.pageIndex) {
                options.pageIndex = 0;
            }
            if (!options.pageSize) {
                options.pageSize = 10;
            }

            options.view.addEventListener('scroll', (e) => {
                const target = e.target;
                if (!options.loading && target.scrollHeight - target.scrollTop - target.clientHeight < 50) {
                    this.loadMore();
                }
            }, false);


            // 传入参数执行ajax请求
            this.loadMore();
        }
        this.loadMore = function() {
            options.loading = true;
            this.initStyle();
            this.initBlock();
            this.loading();

            http.get(options.url, options.params).then((res) => {
                options.callback(res.data, options);
                if (res.data.length < options.pageSize) {
                    this.loadAll();
                } else {
                    this.deleteBlock();
                    options.pageIndex++;
                }
            }).finally(function() {
                options.loading = false;
            });
        }
        this.initBlock = function() {
            const loadMoreDiv = document.createElement("div");
            loadMoreDiv.className = "akd-paged-div";

            const notMoreDiv = document.createElement("div");
            notMoreDiv.className = "akd-paged-not-more";
            notMoreDiv.innerHTML = "已加载全部数据";
            loadMoreDiv.appendChild(notMoreDiv);

            const hasMoreDiv = document.createElement("div");
            hasMoreDiv.className = "akd-paged-has-more";
            let i = document.createElement("i");
            hasMoreDiv.appendChild(i);
            i = document.createElement("i");
            hasMoreDiv.appendChild(i);
            i = document.createElement("i");
            hasMoreDiv.appendChild(i);
            loadMoreDiv.appendChild(hasMoreDiv);

            options.view.appendChild(loadMoreDiv);
        }

        this.deleteBlock = function() {
            const loadMoreDiv = document.querySelector(".akd-paged-div");
            if (loadMoreDiv) {
                loadMoreDiv.parentNode.removeChild(loadMoreDiv);
            }
        }

        this.loadAll = function() {
            const hasMoreDiv = document.querySelector(".akd-paged-has-more");
            const notMoreDiv = document.querySelector(".akd-paged-not-more");
            hasMoreDiv.style.display = "none";
            notMoreDiv.style.display = "block";
        }

        this.loading = function() {
            const hasMoreDiv = document.querySelector(".akd-paged-has-more");
            const notMoreDiv = document.querySelector(".akd-paged-not-more");
            hasMoreDiv.style.display = "block";
            notMoreDiv.style.display = "none";
        }

        this.createHttp = function() {
            app.http = {
                get: get,
                post: post,
            };

            function get(url) {
                return send('GET', url);
            }

            function post(url, data) {
                return send('POST', url, data || {});
            }

            function send(type, url, data) {
                var http = initHttp(),
                    success, fail, over, ch, params = [];
                if (data) {
                    for (var d in data) {
                        params.push(d + '=' + data[d]);
                    }
                    data = params.join('&');
                } else {
                    data = null;
                }
                try {
                    http.open(type, url);
                    http.setRequestHeader("If-Modified-Since", "0");
                    http.setRequestHeader("Cache-Control", "no-cache");
                    http.setRequestHeader("Content-Type", "application/x-www-form-urlencoded; charset=UTF-8");
                    http.onreadystatechange = function() {
                        if ((http.readyState === 4) && (http.status === 200)) {
                            var response = JSON.parse(http.responseText);
                            success && success(response);
                            over && over();
                        } else if (http.readyState === 4) {
                            fail && fail(http);
                            over && over();
                        }
                    };
                    http.send(data);
                } catch (e) {
                    ch && ch();
                    over && over();
                }
                return {
                    then: function(a, b) {
                        success = a;
                        fail = b;
                        return this;
                    },
                    success: function(a) {
                        success = a;
                        return this;
                    },
                    fail: function(b) {
                        fail = b;
                        return this;
                    },
                    catch: function(d) {
                        ch = d;
                        return this;
                    },
                    finally: function(c) {
                        over = c;
                        return this;
                    }
                }
            }

            function initHttp() {
                var xmlHttp;
                if (window.ActiveXObject) {
                    xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
                } else if (window.XMLHttpRequest) {
                    xmlHttp = new XMLHttpRequest();
                }
                return xmlHttp;
            }
        }
        this.initStyle = function() {
            if (!document.getElementById("akdPagedStyle")) {
                var style = document.createElement('style');
                style.type = "text/css";
                style.id = "akdPagedStyle";
                style.innerHTML = `
                .akd-paged-not-more{
                    display: none;
                    text-align: center;
                    margin: 30px 0;
                }
                .akd-paged-has-more{
                    position: relative;
                    height: 20px;
                    margin: 30px 0;
                }
                .akd-paged-has-more i{
                    position: absolute;
                    left: 50%;
                    top: 0;
                    width: 12px;
                    height: 12px;
                    background-color: #666;
                    border-radius: 50%;
                }
                .akd-paged-has-more i:nth-child(1){
                    margin-left: -36px;
                    animation: akd-paged-scale 1s linear infinite;
                    -webkit-animation: akd-paged-scale 1s linear infinite;
                }
                .akd-paged-has-more i:nth-child(2){
                    margin-left: -6px;
                    animation: akd-paged-scale 1s .25s linear infinite;
                    -webkit-animation: akd-paged-scale 1s .25s linear infinite;
                }
                .akd-paged-has-more i:nth-child(3){
                    margin-left: 24px;
                    animation: akd-paged-scale 1s .5s linear infinite;
                    -webkit-animation: akd-paged-scale 1s .5s linear infinite;
                }
                @keyframes akd-paged-scale {
                    0% {
                        transform: scale(1);
                    }
                    25% {
                        transform: scale(.6);
                    }
                    75% {
                        transform: scale(1.4);
                    }
                    100% {
                        transform: scale(1);
                    }
                }
                @-webkit-keyframes akd-paged-scale {
                    0% {
                        -webkit-transform: scale(1);
                    }
                    25% {
                        -webkit-transform: scale(.6);
                    }
                    75% {
                        -webkit-transform: scale(1.4);
                    }
                    100% {
                        -webkit-transform: scale(1);
                    }
                }
                `;
                document.body.appendChild(style);
            }
        }
    }

    app.akdPaged = function(options) {
        let akdPagedInstance = new AkdPaged(options);
        akdPagedInstance.init();
        return akdPagedInstance;
    }
})(window);
```

