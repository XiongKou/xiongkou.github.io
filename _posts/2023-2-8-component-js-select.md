---
layout: post
title:  "js下拉框"
categories: js
tags: component
author: 熊叩
---

* content
{:toc}

原生下拉框看起来比较丑,使用js封装了一个下拉框









# 调用

下拉框需要绑定页面的元素,例如我们定义了一个`test-akd-select`的div

```html
<div id="test-akd-select"></div>
```

现在给html标签绑定数据

```js
var select = akdSelect({
	id: "test-akd-select",
	placeholder:"请选择部门",
	selectValue:"1",         // 默认选中的值
	textField: "CodeName",           // 显示文本字段
	valueField:"CodeNo",          // 显示value字段
	array: [{
		CodeName: "部门1",
		CodeNo:1
	}, {
		CodeName: "部门2",
		CodeNo: 2
	}, {
		CodeName: "部门3",
		CodeNo: 3
	}],
	callback: function (res) {
		console.log(res);
	}
});

// 清空下拉框
select.clear();

// 重新设置下拉框选项
var array = [{
	CodeName: "哈哈1",
	CodeNo: 1
}, {
	CodeName: "哈哈2",
	CodeNo: 2
}, {
	CodeName: "哈哈3",
	CodeNo: 3
}];
select.setOptions(array);

// 设置下拉框的选中的值
select.setValue(2);

// 设置下拉框禁用
select.setDisabled();

// 设置下拉框可用
select.setEnable();

// 获得选中的value值
select.getValue();
```

根据实际项目的需要,下拉框做了好几个功能

1. 清空下拉框选项 `select.clear()`
2. 设置下拉框选项 `select.setOptions(array)`
3. 设置下拉框的选中的值 `select.setValue(2)`
4. 禁用下拉框 `select.setDisabled()`
5. 启用下拉框 `select.setEnable()`
6. 获得下拉框选中的值 `select.getValue()`

下拉框效果如下

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-04-01/js-select.jpg)



# js


```js
; (function (app) {
    function AkdSelectClass(options) {
        this.placeholder = options.placeholder||"";     // 文本框提示信息
        this.selectValue = options.selectValue;     	// 文本框默认选中
        this.text = options.textField||"text";      	// 文本框的文本值字段 , 默认为 text
        this.value = options.valueField||"value";   	// 文本框的value值字段, 默认为 value
        this.disabled = options.disabled;           	// 文本框是否禁用
        this.array = options.array;
        this.id = options.id;
        this.callback = options.callback;
        // 是否显示提示框
        this.showPop = false;
        // 是否在底部显示
        this.isInBottom = false;
        this.selected = null;

        this.init = function () {
            // 给当前的div添加样式 akd-select
            this.div = document.querySelector("#" + this.id);
            this.div.classList.add("akd-select");

            // 创建文本框
            this.createInput();

            // 创建选择项
            this.setOptions(this.array);

            // 默认选中
            this.setValue(this.selectValue);

            // 添加样式
            this.initStyle();

            // 强行渲染
            this.div.clientHeight;
        }

        this.createInput = function () {
            // 创建input文本框
            this.input = document.createElement("input");
            this.input.type = "text";
            this.input.autocomplete = "off";
            this.input.setAttribute("readonly", "readonly");
            if (this.disabled) {
                this.input.className = "disabled";
            }
            if (this.placeholder) {
                this.input.setAttribute("placeholder", this.placeholder);
            }
            // 绑定事件,箭头函数自身没有this,这里可以替代call
            this.input.addEventListener("click", (e) => {
                e.stopPropagation();
                if (this.disabled) {
                    return;
                }

                // 判断下拉框选项是否显示在最底部
                this.isInBottom = e.target.getBoundingClientRect().y > window.innerHeight / 2;
                if (this.isInBottom) {
                    this.optionsDiv.className = "in-bottom";
                } else {
                    this.optionsDiv.removeAttribute("class");
                }

                // 判断是否显示下拉框选项
                this.showPop = !this.showPop;
                if (this.showPop) {
                    this.optionsDiv.style.display = "block";
                } else {
                    this.optionsDiv.style.display = "none";
                }
            }, false);

            // 创建图标
            let downIcon = document.createElement("i");
            downIcon.className = "fa fa-angle-down";
            this.div.appendChild(downIcon);

            this.div.appendChild(this.input);
        }

        this.setOptions = function (tempArray) {
            this.array = tempArray;
            // 创建下拉框的选项
            this.optionsDiv = document.createElement("div");
            this.optionsDiv.innerHTML = "";
            for (let i = 0; i < this.array.length; i++) {
                let p = document.createElement("p");
                p.value = this.array[i][this.value]||"";
                p.innerHTML = this.array[i][this.text]||"";
                p.setAttribute("index", i);
                p.addEventListener("click", (e) => {
                    e.stopPropagation();
                    this.selected = this.array[e.target.getAttribute("index")];
                    this.input.value = this.selected[this.text] || "";
                    if (this.callback) {
                        this.callback(this.selected);
                    }
                    
                    this.input.click();
                }, false);
                this.optionsDiv.appendChild(p);

            }
            this.div.appendChild(this.optionsDiv);
        }

        this.setValue = function (tempSelectValue) {
            this.selected = null;
            // 初始化默认值
            if (tempSelectValue !== undefined) {
                for (let i = 0; i < this.array.length; i++) {
                    if (this.array[i][this.value] == tempSelectValue) {
                        this.selected = this.array[i];
                        this.input.value = this.selected[this.text];
                    }
                }
            }
            if (this.callback) {
                this.callback(this.selected);
            }
        }

        this.getValue = function () {
            if (!this.selected) {
                return this.selected;
            }
            return this.selected[this.value];
        }

        this.clear = function () {
            this.setOptions([]);
            this.setValue();
            this.input.value = "";
        }

        this.setDisabled = function () {
            this.disabled = true;
            this.input.className = "disabled";
                
        }

        this.setEnable = function () {
            this.disabled = false;
            this.input.className="";
        }

        this.initStyle = function () {
            if (!document.querySelector("#akdSelectStyle")) {
                var style = document.createElement('style');
                style.type = "text/css";
                style.id = "akdSelectStyle";
                style.innerHTML = `
                    .akd-select{
                        position: relative;
                        height: 36px;
                        line-height: 36px;
                        box-sizing: border-box;
                    }
                    .akd-select input{
                        height: 100%;
                        width: 100%;
                        border-radius: 3px;
                        padding: 0 10px;
                        border: solid 1px lightgray;
                        vertical-align: bottom;
                        font-size: 14px;
                        box-sizing: border-box;
                    }
                    .akd-select .disabled{
                        background-color: #f1f1f1;
                        box-sizing: border-box;
                    }
                    .akd-select i{
                        position: absolute;
                        pointer-events: none;
                        right: 0;
                        top: 0;
                        width: 38px;
                        height: 100%;
                        text-align: center;
                        line-height: inherit;
                        box-sizing: border-box;
                    }
                    .akd-select div{
                        position: absolute;
                        z-index: 99999;
                        padding: 0 5px;
                        top: 100%;
                        left: 0;
                        width: 100%;
                        max-height: 240px;
                        overflow: auto;
                        -webkit-overflow-scrolling: touch;
                        background-color: white;
                        box-shadow: 5px 5px 15px rgba(0, 0, 0, .3);
                        display:none;
                        box-sizing: border-box;
                    }
                    .akd-select .in-bottom{
                        top: auto;
                        bottom: 100%;
                        box-sizing: border-box;
                    }
                    .akd-select p{
                        margin: 0;
                        padding: 5px 15px;
                        font-size: 15px;
                        box-sizing: border-box;
                        line-height:36px;
                        text-align:left;
                    }
                    .akd-select p:not(:last-child){
                        border-bottom: solid 1px #efefef;
                    }
                `;
                document.body.appendChild(style);
            }
        }
    }

    app.akdSelect = function (options) {
        var temp = new AkdSelectClass(options);
        temp.init();
        return temp;
    }
})(window);
```


