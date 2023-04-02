---
layout: post
title:  "js日期选择器"
categories: js
tags: component
author: 熊叩
---

* content
{:toc}

html5虽然有日期选择控件,但是低版本的浏览器和移动端都有兼容性的问题,最好的办法就是封装一个日期组件















# 调用

需要先定义文本框,在日期组件的回调事件里面回写数据到文本框

```html
<p>
	<input type="text" id="txtDateTime" onclick="testDateTimeClick()" placeholder="测试日期时间" readonly="readonly" />
</p>

<p>
	<input type="text" id="txtDate" onclick="testDateClick()" placeholder="测试日期" readonly="readonly" />
</p>

<script type="text/javascript">
	function testDateTimeClick(e) {
		akdTime({
			date: document.querySelector("#txtDateTime").value,
			callback: (res)=> {
				document.querySelector("#txtDateTime").value=res;
			}
		})
	}

	function testDateClick(e) {
		akdDate({
			date: document.querySelector("#txtDate").value,
			callback: (res) => {
				document.querySelector("#txtDate").value = res;
			}
		})
	}
</script>
```


日期时间选择器

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-04-01/js-datetime.jpg)

日期选择器

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-04-01/js-date.jpg)

# js


```js
; (function (app) {
    function AkdDateClass(options) {
        this.callback = options.callback;

        this.date = options.date;
        this.date = this.date.trim();
        if (!this.date) {
            this.date = new Date();
        }else if (typeof (this.date) == "string") {
            this.date = new Date(this.date);
        }
        this.show = false;
        this.type = options.type;
        this.years = [];
        this.months = [];
        this.days = [];
        this.hours = [];
        this.minutes = [];
        this.yearIndex = 0;
        this.monthIndex = 0;
        this.dayIndex = 0;
        this.hourIndex = 0;
        this.minuteIndex = 0;
        this.secondIndex = 0;
        this.lineHeight = 50;
        this.currUL = undefined;
        this.preY = {
            y: undefined,
            time: undefined
        };

        this.yearElement;
        this.monthElement;
        this.dayElement;
        this.hourElement;
        this.minuteElement;
        this.footer;

        this.init = function () {

            this.initElement();

            this.initHandler();

            this.initStyle();

            this.initData();
        }

        this.initDate = function () {
            const date = this.date;
            let cy = date.getFullYear();
            let cm = date.getMonth() + 1;
            let cd = date.getDate();
            let yearHtml = "<li></li>";
            for (let i = cy - 4; i < cy + 5; i++) {
                this.years.push(i);
                yearHtml += "<li>" + i + "</li>";
                if (i === cy) {
                    this.yearIndex = i - cy + 4;
                }
            }
            yearHtml += "<li></li>";
            this.yearElement.innerHTML = yearHtml;

            let monthHtml = "<li></li>";
            for (let i = 1; i < 13; i++) {
                this.months.push(i);
                monthHtml += "<li>" + i + "</li>";
                if (i === cm) {
                    this.monthIndex = i - 1;
                }
            }
            monthHtml += "<li></li>";
            this.monthElement.innerHTML = monthHtml;

            let dayHtml = "<li></li>";
            for (let i = 1; i < 32; i++) {
                this.days.push(i);
                dayHtml += "<li>" + i + "</li>";
                if (i === cd) {
                    this.dayIndex = i - 1;
                }
            }
            dayHtml += "<li></li>";
            this.dayElement.innerHTML = dayHtml;
            this.setDate();
        }

        this.initTime = function () {
            const date = this.date;
            let ch = date.getHours();
            let cm = date.getMinutes();

            let hourHtml = "<li></li>";
            for (let i = 0; i < 24; i++) {
                this.hours.push(i);
                hourHtml += "<li>" + i + "</li>";
                if (i === ch) {
                    this.hourIndex = i;
                }
            }
            hourHtml += "<li></li>";
            this.hourElement.innerHTML = hourHtml;

            let minuteHtml = "<li></li>";
            for (let i = 0; i < 60; i++) {
                this.minutes.push(i);
                minuteHtml += "<li>" + i + "</li>";
                if (i === cm) {
                    this.minuteIndex = i;
                }
            }
            minuteHtml += "<li></li>";
            this.minuteElement.innerHTML = minuteHtml;
            this.setTime();
        }

        this.setDate = function () {
            this.yearElement.style.transform = 'translateY(' + -1 * this.yearIndex * this.lineHeight + 'px)';
            this.monthElement.style.transform = 'translateY(' + -1 * this.monthIndex * this.lineHeight + 'px)';
            this.dayElement.style.transform = 'translateY(' + -1 * this.dayIndex * this.lineHeight + 'px)';
            this.calcDay();
        }

        this.setTime = function () {
            this.hourElement.style.transform = 'translateY(' + -1 * this.hourIndex * this.lineHeight + 'px)';
            this.minuteElement.style.transform = 'translateY(' + -1 * this.minuteIndex * this.lineHeight + 'px)';
        }

        this.initData = function () {
            if (this.type == "date") {
                document.querySelector("#akd-date-title").innerHTML = "日期选择器";
                this.initDate();
                this.hourElement.parentNode.parentNode.style.display = "none";
            } else if (this.type == "datetime") {
                document.querySelector("#akd-date-title").innerHTML = "日期时间选择器";
                this.initDate();
                this.initTime();
            }
        }

        this.touchStartHandler = (e) => {
            stopPro(e);
            const cul = getTarget(e.target, 'ul');
            if (cul) {
                this.currUL = cul;
            } else {
                this.currUL = undefined;
            }
        }

        this.touchMoveHandler = (e) => {
            stopPro(e);
            if (!this.currUL) return;
            if (this.preY.y || this.preY.y === 0) {
                const range = e.touches[0].clientY - this.preY.y;
                this.currUL.style.transform = 'translateY(' + ((getComputedStyle(this.currUL, null).transform.slice(22, -1) | 0) + range) + 'px)';
                this.speed = range / (new Date().getTime() - this.preY.time);
            }
            this.preY.y = e.touches[0].clientY;
            this.preY.time = new Date().getTime();
        }

        this.touchEndHandler = (e) => {
            stopPro(e);
            const type = this.currUL.getAttribute('data-type');
            const curr = getComputedStyle(this.currUL, null).transform.slice(22, -1) | 0;
            const index = Math.min(Math.max(Math.round((curr + this.speed * 150) / this.lineHeight), (this[type + 's'].length - 1) * -1), 0);
            const target = this.lineHeight * index;
            this[type + 'Index'] = Math.abs(index);
            if (~['year', 'month'].indexOf(type)) {
                this.calcDay();
            }
            scrollTo(curr, target, Math.min(Math.abs(Math.floor(this.speed * 50)), 10), 0, ty => {
                this.currUL.style.transform = 'translateY(' + ty + 'px)';
            });
            this.speed = 0;
            this.preY = {};
        }

        this.liClickHandler = (dom, e) => {
            stopPro(e);
            const target = getTarget(e.target, 'li', dom);
            if (target) {
                if (!target.innerText) return;
                const type = dom.getAttribute('data-type');
                const index = this[type + 's'].indexOf((target.innerText | 0));
                this[type + 'Index'] = index;
                if (~['year', 'month'].indexOf(type)) {
                    this.calcDay();
                }
                scrollTo(getComputedStyle(dom, null).transform.slice(22, -1) | 0, -1 * index * this.lineHeight, 10, 0, ty => {
                    this.currUL.style.transform = 'translateY(' + ty + 'px)';
                });
            }
        }

        this.isStop = function (e, callback) {
            setTimeout(() => {
                //console.log(e);
                if (this.preTop === e.scrollTop) {
                    this.preTop = -1;
                    callback();
                } else {
                    this.preTop = e.scrollTop;
                    this.isStop(e, callback);
                }
            }, 50);
        }

        this.calcDay = function () {
            const date = new Date(this.years[this.yearIndex] + '/' + this.months[this.monthIndex] + '/1');
            date.setMonth(date.getMonth() + 1);
            date.setDate(0);
            const maxDay = new Date(date).getDate();
            if (this.dayIndex > maxDay - 1) {
                this.dayIndex = maxDay;
            }
            const days = [];
            let dayHtml = "<li></li>";
            for (let i = 1; i <= maxDay; i++) {
                days.push(i);
                dayHtml += "<li>" + i + "</li>";
            }
            dayHtml += "<li></li>";
            this.days = days;
            this.dayElement.innerHTML = dayHtml;
            this.dayElement.clientWidth;
            //this.dayElement.scrollTop = this.dayIndex * this.lineHeight;
            this.dayElement.style.transform = 'translateY(' + -1 * this.dayIndex * this.lineHeight + 'px)';
        }

        this.clickHandler = (e) => {
            stopPro(e);
            const target = getTarget(e.target, 'button');
            if (!target) return;
            let dateStr = '';
            switch (target.className) {
                case 'btn-clear':
                    this.callback();
                    this.close();
                    break;
                case 'btn-cancel':
                    this.close();
                    break;
                case 'btn-sure':
                    if (this.type == "date") {
                        dateStr=formatDate( this.years[this.yearIndex] + '/' + this.months[this.monthIndex] + '/' + this.days[this.dayIndex]);
                    } else if (this.type == "datetime") {
                        dateStr = formatFullDate(this.years[this.yearIndex] + '/' + this.months[this.monthIndex] + '/' + this.days[this.dayIndex] + " "+ this.hours[this.hourIndex] + ':' + this.minutes[this.minuteIndex]);
                    }

                    this.callback(dateStr);
                    this.close();
                    break;
            }
        }

        this.close = function () {
            document.body.removeChild(this.div);
        }

        function formatDate(date) {
            if (!date) return '';
            if (typeof date === 'string') {
                date = new Date(date.replace(/T/g, ' ').replace(/-/g, '/').slice(0, 10));
            }
            return date.getFullYear() + '/' + (date.getMonth() + 1 + '').padStart(2, 0) + '/' + (date.getDate() + '').padStart(2, 0);
        }

        function formatFullDate(date) {
            if (!date) {
                return '';
            }
            if (typeof date === 'string') {
                date = new Date(date.replace(/T/g, ' ').replace(/-/g, '/').slice(0, 19));
            }
            let year = date.getFullYear() + '',
                month = date.getMonth() + 1,
                day = date.getDate(),
                hour = date.getHours(),
                minute = date.getMinutes(),
                second = date.getSeconds();

            month = month > 9 ? month : '0' + month;
            day = day > 9 ? day : '0' + day;
            hour = hour > 9 ? hour : '0' + hour;
            minute = minute > 9 ? minute : '0' + minute;
            second = second > 9 ? second : '0' + second;
            return year + '/' + month + '/' + day + ' ' + hour + ':' + minute ;
        }

        function scrollTo(curr, target, time, start, callback) {
            let currY = easeOut(start, curr, target - curr, time);
            requestAnimationFrame(() => {
                callback(currY);
                start++;
                if (start < time) {
                    scrollTo(curr, target, time, start, callback);
                } else {
                    callback(target);
                }
            })
        }
        function easeOut(t, b, c, d) {
            return -c * (t /= d) * (t - 2) + b;
        }

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

        function stopPro(evt) {
            var e = evt || window.event;
            window.event ? e.cancelBubble = true : e.stopPropagation();
        }

        this.initElement = function () {
            if (!document.querySelector("#date-time-select-box")) {

                this.div = document.createElement("div");
                this.div.id = "date-time-select-box";
                this.div.innerHTML = `
                    <div class="date-time-select" v-if="show">
                            <div class="date-time-pop">
                                <h4 id="akd-date-title"></h4>
                                <div class="date-select" v-if="model !== 2">
                                    <div class="date-select-year">
                                        <ul ref="year" id="akd-date-year" data-type="year" >
                                        </ul>
                                    </div>
                                    <div class="date-select-month">
                                        <ul ref="month" id="akd-date-month" data-type="month" >
                                        </ul>
                                    </div>
                                    <div class="date-select-day">
                                        <ul ref="day" id="akd-date-day" data-type="day" >
                                        </ul>
                                    </div>
                                </div>
                                <div class="date-select" v-if="model !== 1">
                                    <div class="date-select-hours">
                                        <ul ref="hour" id="akd-date-hour" data-type="hour" >
                                        </ul>
                                    </div>
                                    <div class="date-select-minutes">
                                        <ul ref="minute" id="akd-date-minute" data-type="minute" >
                                        </ul>
                                    </div>
                                </div>
                                <div class="akd-date-footer"  id="akd-date-footer">
                                    <button class="btn-clear">清除</button>
                                    <div>
                                        <button class="btn-cancel">取消</button>
                                        <button class="btn-sure">确认</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                `;
                document.body.appendChild(this.div);
            }
        }

        this.initHandler = function () {
            this.yearElement = document.querySelector("#akd-date-year");
            this.monthElement = document.querySelector("#akd-date-month");
            this.dayElement = document.querySelector("#akd-date-day");
            this.hourElement = document.querySelector("#akd-date-hour");
            this.minuteElement = document.querySelector("#akd-date-minute");
            this.footer = document.querySelector("#akd-date-footer");

            this.yearElement.addEventListener("touchstart", this.touchStartHandler, false);
            this.yearElement.addEventListener("touchmove", this.touchMoveHandler, false);
            this.yearElement.addEventListener("touchend", this.touchEndHandler, false);
            this.yearElement.addEventListener("touchcancel", this.touchEndHandler, false);
            this.yearElement.addEventListener("click", (e) => {
                this.liClickHandler(this.yearElement, e);
            }, false);

            this.monthElement.addEventListener("touchstart", this.touchStartHandler, false);
            this.monthElement.addEventListener("touchmove", this.touchMoveHandler, false);
            this.monthElement.addEventListener("touchend", this.touchEndHandler, false);
            this.monthElement.addEventListener("touchcancel", this.touchEndHandler, false);
            this.monthElement.addEventListener("click", (e) => {
                this.liClickHandler(this.monthElement, e);
            }, false);

            this.dayElement.addEventListener("touchstart", this.touchStartHandler, false);
            this.dayElement.addEventListener("touchmove", this.touchMoveHandler, false);
            this.dayElement.addEventListener("touchend", this.touchEndHandler, false);
            this.dayElement.addEventListener("touchcancel", this.touchEndHandler, false);
            this.dayElement.addEventListener("click", (e) => {
                this.liClickHandler(this.dayElement, e);
            }, false);

            this.hourElement.addEventListener("touchstart", this.touchStartHandler, false);
            this.hourElement.addEventListener("touchmove", this.touchMoveHandler, false);
            this.hourElement.addEventListener("touchend", this.touchEndHandler, false);
            this.hourElement.addEventListener("touchcancel", this.touchEndHandler, false);
            this.hourElement.addEventListener("click", (e) => {
                this.liClickHandler(this.hourElement, e);
            }, false);

            this.minuteElement.addEventListener("touchstart", this.touchStartHandler, false);
            this.minuteElement.addEventListener("touchmove", this.touchMoveHandler, false);
            this.minuteElement.addEventListener("touchend", this.touchEndHandler, false);
            this.minuteElement.addEventListener("touchcancel", this.touchEndHandler, false);
            this.minuteElement.addEventListener("click", (e) => {
                this.liClickHandler(this.minuteElement, e);
            }, false);

            this.footer.addEventListener("click", this.clickHandler, false);
        }

        this.initStyle = function () {
            if (!document.querySelector("#akdDateStyle")) {
                var style = document.createElement('style');
                style.type = "text/css";
                style.id = "akdDateStyle";
                style.innerHTML = `
                    #date-time-select-box{
                        position: fixed;
                        z-index: 99999;
                        left: 0;
                        top: 0;
                        width: 0;
                        height: 0;
                    }
                    #date-time-select-box .fade-form-large-enter-active, #date-time-select-box .fade-form-large-leave-active {
                      transition: all .3s ease;
                    }

                    #date-time-select-box .fade-form-large-enter, #date-time-select-box .fade-form-large-leave-to {
                      transform: scale(1.2);
                      opacity: 0;
                    }

                    #date-time-select-box .date-time-select {
                      position: fixed;
                      z-index: 99999;
                      width: 100%;
                      left: 0;
                      top: 0;
                      height: 100%;
                      display: flex;
                      align-items: center;
                      justify-content: center;
                      background-color: rgba(0, 0, 0, .5);
                    }

                    #date-time-select-box .date-time-select .date-time-pop{
                        padding: 20px;
                        background-color: white;
                        border-radius: 3px;
                        width: 90%;
                    }

                    .date-time-select .date-time-pop .date-select{
                        margin-top: 20px;
                          position: relative;
                          display: flex;
                          justify-content: space-around;
                          height: 150px;
                          overflow: hidden;
                    }

                    .date-time-select .date-time-pop .date-select:after {
                        position: absolute;
                        content: '';
                        left: 0;
                        right: 0;
                        width: 100%;
                        height: 100%;
                        pointer-events: none;
                        background: linear-gradient(to bottom, white 0, white 5%, rgba(255, 255, 255, 0) 50%, white 95%, white 100%);
                   }

                   .date-time-select .date-time-pop .date-select>div{
                        position: relative;
                        width: 33.3%;
                        height: 100%;
                        font-size: 15px;
                        overflow: hidden;
                        margin: 0;
                        padding: 0;
                    }

                    .date-time-select .date-time-pop .date-select>div:before {
                      position: absolute;
                      content: '';
                      width: 70%;
                      height: 2px;
                      left: 15%;
                      top: 33%;
                      background-color: #333;
                    }

                    .date-time-select .date-time-pop .date-select>div:after {
                      position: absolute;
                      content: '';
                      width: 70%;
                      height: 2px;
                      left: 15%;
                      bottom: 33%;
                      background-color: #333;
                    }

                    .date-time-select .date-time-pop .date-select>div ul {
                      list-style-type: none;
                      margin: 0;
                      padding: 0;
                      height: 100%;
                      text-align: center;
                      line-height: 50px;
                      
                    }
                    .date-time-select .date-time-pop .date-select>div ul li{
                        height: 50px;
                    }

                    #date-time-select-box .akd-date-footer{
                        margin-top: 20px;
                        display: flex;
                        justify-content: space-between;
                    }

                    #date-time-select-box .akd-date-footer button{
                        background-color: cadetblue;
                        color: white;
                        border: none;
                        height: 42px;
                        font-size: 15px;
                        border-radius: 3px;
                        padding: 8px 20px;
                        box-sizing: border-box;
                    }

                    #date-time-select-box .akd-date-footer .btn-clear{
                        background-color: #c1c1c1;
                    }

                    #date-time-select-box .akd-date-footer .btn-cancel{
                        background-color: burlywood;
                        margin-right: 10px;
                    }
                `;
                document.body.appendChild(style);
            }
        }
    }

    app.akdDate = function (options) {
        options.type = "date";
        var temp = new AkdDateClass(options);
        temp.init();
    }

    app.akdTime = function (options) {
        options.type = "datetime";
        var temp = new AkdDateClass(options);
        temp.init();
    }
})(window);
```







