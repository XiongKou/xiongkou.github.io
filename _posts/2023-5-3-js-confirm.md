---
layout: post
title:  "confirm"
categories: js
tags: component
author: 熊叩
---

* content
{:toc}
 
记录一下简单的确认提示框代码











# 调用

```js
<button onclick="test();" style="padding: 10px 20px;">测试</button>
<script type="text/javascript">
	function test() {
		akdConfirm({
			message: "确定要删除吗？",
			callback: function(res) {
				if (res) {
					alert("Success");
					
					// 关闭提示框
					akdClose();
				}
			}
		});
	}
</script>
```

# 关闭提示框

如果用户点了提示框的确认按钮，一般情况下一执行一个ajax请求，ajax结束后才关闭提示框，所以关闭提示框需要手动自己调用，调用的js如下

```js
akdClose();
```

# 提示框效果

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-05-03/confirm.png)

# js

```js
/**
 * 确认选择框
 * 
 */
;
(function(app) {
    function AkdConfirmAlert(options) {

        let view = options.view || document.body;

        this.init = function() {
            this.confirmDiv = document.querySelector(".akd-confirm");
            if (!this.confirmDiv) {
                this.initBlock();
            }
            this.initStyle();
        }

        this.initBlock = function() {
            this.confirmDiv = document.createElement('div');
            this.confirmDiv.className = "akd-confirm";

            let box = document.createElement('div');
            box.className = "box";
            this.confirmDiv.appendChild(box);

            let h4 = document.createElement('h4');
            h4.innerHTML = "提示";
            box.appendChild(h4);

            let content = document.createElement('div');
            content.className = "content";
            box.appendChild(content);

            let p = document.createElement('p');
            p.innerHTML = options.message;
            content.appendChild(p);

            let btns = document.createElement('div');
            btns.className = "btns";
            box.appendChild(btns);

            let cancel = document.createElement('button');
            cancel.className = "cancel";
            cancel.innerHTML = "取消";
            cancel.addEventListener("click", () => {
                let confirmDiv = document.querySelector(".akd-confirm");
                if (confirmDiv) {
                    confirmDiv.classList.remove('akd-confirm-show');
                    setTimeout(() => {
                        confirmDiv.parentNode.removeChild(confirmDiv);
                    }, 300);
                }
            })
            btns.appendChild(cancel);

            let sure = document.createElement('button');
            sure.className = "sure";
            sure.innerHTML = "确认";
            sure.addEventListener("click", () => {
                options.callback(true);
            }, false);
            btns.appendChild(sure);

            view.appendChild(this.confirmDiv);
            setTimeout(() => {
                this.confirmDiv.classList.add('akd-confirm-show');
            }, 50);
        }

        this.initStyle = function() {
            if (!document.getElementById("akdMessageStyle")) {
                var style = document.createElement('style');
                style.type = "text/css";
                style.id = "akdMessageStyle";
                style.innerHTML = `
                    .akd-confirm{
                        position: fixed;
                        left: 0;
                        top: 0;
                        width: 100%;
                        height: 100%;
                        display:flex;
                        display:-webkit-flex;
                        background-color: rgba(0,0,0,.2);
                        align-items: center;
                        -webkit-align-items: center;
                        justify-content: center;
                        -webkit-justify-content: center;
                        opacity: 0;
                        transform: scale(1.5);
                        -webkit-transform: scale(1.5);
                        transition: all .3s ease;
                        -webkit-transition: all .3s ease;
                    }
                    .akd-confirm-show{
                        opacity: 1;
                        transform: scale(1);
                        -webkit-transform: scale(1);
                    }
                    .akd-confirm .box{
                        background-color: white;
                        width: 80%;
                        border-radius: 8px;
                        box-shadow: 10px 10px 30px rgb(0 0 0 / 30%);
                    }
                    .akd-confirm h4{
                        font-size: 18px;
                        padding: 0 20px;
                        margin: 0;
                        line-height: 50px;
                        border-bottom: solid 1px #efefef;
                    }
                    .akd-confirm .content{
                        min-height: 150px;
                    }
                    .akd-confirm p{
                        word-break: break-all;
                        padding: 20px;
                        text-indent: 2em;
                        margin: 0;
                    }
                    .akd-confirm .btns{
                        border-top: solid 1px #efefef;
                        height: 56px;
                        display: flex;
                        display: -webkit-flex;
                        align-items: center;
                        -webkit-align-items: center;
                        justify-content: space-around;
                        -webkit-justify-content: space-around;
                    }
                    .akd-confirm button{
                        width: 40%;
                        height: 42px;
                        outline: none;
                        border-radius: 3px;
                        font-size: 15px;
                    }
                    .akd-confirm .cancel{
                        color: #333;
                        background-color: white;
                        border: solid 1px #bcbcbc;
                    }
                    .akd-confirm .sure{
                        color: white;
                        background-color: cadetblue;
                        border: none;
                    }
                    .akd-confirm button{
                        width: 40%;
                        height: 42px;
                        outline: none;
                        border-radius: 3px;
                        font-size: 15px;
                    }
                `;
                view.appendChild(style);
            }
        }
    }


    app.akdConfirm = function(options) {
        let akdConfirmInstance = new AkdConfirmAlert(options);
        akdConfirmInstance.init();
    }
    app.akdClose = function() {
        let confirmDiv = document.querySelector(".akd-confirm");
        if (confirmDiv) {
            confirmDiv.classList.remove('akd-confirm-show');
            setTimeout(() => {
                confirmDiv.parentNode.removeChild(confirmDiv);
            }, 300);
        }
    }
})(window);
```

