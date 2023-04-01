---
layout: post
title:  "vue日期选择器"
categories: vue
tags: date
author: 熊叩
---

* content
{:toc}



vue项目如果没有引用其他的组件库,自己写一个组件也是很不错的选择


















# js


```js
import Vue from 'vue'

let vnode, dateTimeSelect = {};
let tpl = Vue.extend({
    template: `<div id="date-time-select-box">
                    <transition name="fade-form-large">
                         <div class="date-time-select" v-if="show">
                            <div class="date-time-pop">
                                <h4>{{ model === 1 ? '日期' : model === 2 ? '时间' : '日期时间' }}选择器</h4>
                                <div class="date-select" v-if="model !== 2">
                                    <div class="date-select-year">
                                        <ul ref="year" data-type="year" @touchstart.prevent="touchStartHandler" @touchmove.prevent="touchMoveHandler" @touchend.prevent="touchEndHandler" @touchcancel.prevent="touchEndHandler"
                                         v-tap @tap="liClickHandler($refs.year, $event)">
                                            <li></li>
                                            <li v-for="y in years" :key="y">{{y}}</li>
                                            <li></li>
                                        </ul>
                                    </div>
                                    <div class="date-select-month">
                                        <ul ref="month" data-type="month" @touchstart.prevent="touchStartHandler" @touchmove.prevent="touchMoveHandler" @touchend.prevent="touchEndHandler" @touchcancel.prevent="touchEndHandler"
                                         v-tap @tap="liClickHandler($refs.month, $event)">
                                            <li></li>
                                            <li v-for="m in months" :key="m">{{m}}</li>
                                            <li></li>
                                        </ul>
                                    </div>
                                    <div class="date-select-day">
                                        <ul ref="day" data-type="day" @touchstart.prevent="touchStartHandler" @touchmove.prevent="touchMoveHandler" @touchend.prevent="touchEndHandler" @touchcancel.prevent="touchEndHandler"
                                        v-tap @tap="liClickHandler($refs.day, $event)">
                                            <li></li>
                                            <li v-for="d in days" :key="d">{{d}}</li>
                                            <li></li>
                                        </ul>
                                    </div>
                                </div>
                                <div class="date-select" v-if="model !== 1">
                                    <div class="date-select-hours">
                                        <ul ref="hour" data-type="hour" @touchstart.prevent="touchStartHandler" @touchmove.prevent="touchMoveHandler" @touchend.prevent="touchEndHandler" @touchcancel.prevent="touchEndHandler"
                                        v-tap @tap="liClickHandler($refs.hour, $event)">
                                            <li></li>
                                            <li v-for="h in hours" :key="h">{{h}}</li>
                                            <li></li>
                                        </ul>
                                    </div>
                                    <div class="date-select-minutes">
                                        <ul ref="minute" data-type="minute" @touchstart.prevent="touchStartHandler" @touchmove.prevent="touchMoveHandler" @touchend.prevent="touchEndHandler" @touchcancel.prevent="touchEndHandler"
                                        v-tap @tap="liClickHandler($refs.minute, $event)">
                                            <li></li>
                                            <li v-for="m in minutes" :key="m">{{m}}</li>
                                            <li></li>
                                        </ul>
                                    </div>
                                </div>
                                <div class="footer" v-tap @tap="clickHandler">
                                    <button class="btn-clear">清除</button>
                                    <div>
                                        <button class="btn-cancel">取消</button>
                                        <button class="btn-sure">确认</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </transition>
                </div>`,
    data() {
        return {
            callback: undefined,
            date: undefined,
            show: false,
            type: '',
            years: [],
            months: [],
            days: [],
            hours: [],
            minutes: [],
            yearIndex: 0,
            monthIndex: 0,
            dayIndex: 0,
            hourIndex: 0,
            minuteIndex: 0,
            secondIndex: 0,
            lineHeight: 50,
            currUL: undefined,
            preY: {
                y: undefined,
                time: undefined
            },
            speed: 0,
        }
    },
    computed: {
        model() {
            return this.type === 'date' ? 1 : this.type === 'time' ? 2 : 3;
        }
    },
    methods: {
        init() {
            this.model !== 2 && this.initDate();
            this.model !== 1 && this.initTime();
        },
        initDate() {
            const date = this.date;
            let cy = date.getFullYear();
            let cm = date.getMonth() + 1;
            let cd = date.getDate();
            for (let i = cy - 4; i < cy + 5; i++) {
                this.years.push(i);
                if (i === cy) this.yearIndex = i - cy + 4;
            }
            for (let i = 1; i < 13; i++) {
                this.months.push(i);
                if (i === cm) this.monthIndex = i - 1;
            }
            for (let i = 1; i < 32; i++) {
                this.days.push(i);
                if (i === cd) this.dayIndex = i - 1;
            }
            this.setDate();
        },
        initTime() {
            const date = this.date;
            let ch = date.getHours();
            let cm = date.getMinutes();
            for (let i = 0; i < 24; i++) {
                this.hours.push(i);
                if (i === ch) this.hourIndex = i;
            }
            for (let i = 0; i < 60; i++) {
                this.minutes.push(i);
                if (i === cm) this.minuteIndex = i;
            }
            this.setTime();
        },
        setDate() {
            this.$nextTick(() => {
                this.$refs.year.style.transform = 'translateY(' + -1 * this.yearIndex * this.lineHeight + 'px)';
                this.$refs.month.style.transform = 'translateY(' + -1 * this.monthIndex * this.lineHeight + 'px)';
                this.$refs.day.style.transform = 'translateY(' + -1 * this.dayIndex * this.lineHeight + 'px)';
                this.calcDay();
            })
        },
        setTime() {
            this.$nextTick(() => {
                this.$refs.hour.style.transform = 'translateY(' + -1 * this.hourIndex * this.lineHeight + 'px)';
                this.$refs.minute.style.transform = 'translateY(' + -1 * this.minuteIndex * this.lineHeight + 'px)';
            })
        },
        touchStartHandler(e) {
            const cul = getTarget(e.target, 'ul');
            if (cul) {
                this.currUL = cul;
            } else {
                this.currUL = undefined;
            }
        },
        touchMoveHandler(e) {
            if (!this.currUL) return;
            if (this.preY.y || this.preY.y === 0) {
                const range = e.touches[0].clientY - this.preY.y;
                this.currUL.style.transform = 'translateY(' + ((getComputedStyle(this.currUL, null).transform.slice(22, -1) | 0) + range) + 'px)';
                this.speed = range / (new Date().getTime() - this.preY.time);
            }
            this.preY.y = e.touches[0].clientY;
            this.preY.time = new Date().getTime();
        },
        touchEndHandler() {
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
        },
        liClickHandler(dom, e) {
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
        },
        calcDay() {
            const date = new Date(this.years[this.yearIndex] + '/' + this.months[this.monthIndex] + '/1');
            date.setMonth(date.getMonth() + 1);
            date.setDate(0);
            const maxDay = new Date(date).getDate();
            if (this.dayIndex > maxDay - 1) {
                this.dayIndex = maxDay - 1;
            }
            const days = [];
            for (let i = 1; i <= maxDay; i++) {
                days.push(i);
            }
            this.$set(this, 'days', days);
            this.$nextTick(() => {
                this.$refs.day.style.transform = 'translateY(' + -1 * this.dayIndex * this.lineHeight + 'px)';
            })
        },
        clickHandler(e) {
            const target = getTarget(e.target, 'button');
            if (!target) return;
            let dateStr = '';
            switch (target.className) {
                case 'btn-clear':
                    this.callback();
                    close();
                    break;
                case 'btn-cancel':
                    close();
                    break;
                case 'btn-sure':
                    this.model !== 2 && (dateStr += this.years[this.yearIndex] + '/' + this.months[this.monthIndex] + '/' + this.days[this.dayIndex]);
                    this.model === 3 && (dateStr += ' ');
                    this.model !== 1 && (dateStr += this.hours[this.hourIndex] + ':' + this.minutes[this.minuteIndex]);
                    this.callback(new Date(dateStr));
                    close();
                    break;
            }
        }
    }
});

dateTimeSelect.install = (Vue) => {
    vnode = new tpl().$mount();
    document.body.appendChild(vnode.$el);
    Vue.prototype.$dateTimeSelect = open;
};

/**
 *
 * @param date          当前日期， Date格式
 * @param type          类型（date 仅日期；time 仅世间；datetime/其余所有 时间与日期）
 * @param callback      回调（返回日期时间 Date格式）
 */
function open({date = new Date(), type = 'datetime', callback}) {
    vnode.$set(vnode, 'show', true);
    vnode.$set(vnode, 'date', date);
    vnode.$set(vnode, 'type', type);
    vnode.$set(vnode, 'callback', callback);
    vnode.init();
}

function close() {
    vnode.$set(vnode, 'show', false);
    let obj = {
        callback: undefined,
        date: undefined,
        show: false,
        type: '',
        years: [],
        months: [],
        days: [],
        hours: [],
        minutes: [],
        yearIndex: 0,
        monthIndex: 0,
        dayIndex: 0,
        hourIndex: 0,
        minuteIndex: 0,
        secondIndex: 0,
        lineHeight: 50,
        currUL: undefined,
        preY: {
            y: undefined,
            time: undefined
        },
        speed: 0,
    };
    Object.keys(obj).map((o) => {
        vnode.$set(vnode, o, obj[o]);
    })
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

export default dateTimeSelect;
```

# css

```css
#date-time-select-box {
  position: fixed;
  z-index: 99999;
  left: 0;
  top: 0;
  width: 0;
  height: 0;
}

.fade-form-large-enter-active, .fade-form-large-leave-active {
  transition: all .3s ease;
}

.fade-form-large-enter, .fade-form-large-leave-to {
  transform: scale(1.2);
  opacity: 0;
}

.date-time-select {
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
  .date-time-pop {
    padding: 20px;
    background-color: white;
    border-radius: 3px;
    width: 90%;
    .date-select {
      margin-top: 20px;
      position: relative;
      display: flex;
      justify-content: space-around;
      height: 150px;
      overflow: hidden;
      &:after {
        position: absolute;
        content: '';
        left: 0;
        right: 0;
        width: 100%;
        height: 100%;
        pointer-events: none;
        background: linear-gradient(to bottom, white 0, white 5%, rgba(255, 255, 255, 0) 50%, white 95%, white 100%);
      }
      > div {
        position: relative;
        width: 33.3%;
        height: 100%;
        font-size: 15px;
        overflow: hidden;
        margin: 0;
        padding: 0;
        &:before {
          position: absolute;
          content: '';
          width: 70%;
          height: 2px;
          left: 15%;
          top: 33%;
          background-color: #333;
        }
        &:after {
          position: absolute;
          content: '';
          width: 70%;
          height: 2px;
          left: 15%;
          bottom: 33%;
          background-color: #333;
        }
        ul {
          list-style-type: none;
          margin: 0;
          padding: 0;
          height: 100%;
          text-align: center;
          line-height: 50px;
          li {
            height: 50px;
          }
        }
      }
    }
    .footer {
      margin-top: 20px;
      display: flex;
      justify-content: space-between;
      button {
        background-color: cadetblue;
        color: white;
        border: none;
        height: 42px;
        font-size: 15px;
        border-radius: 3px;
        padding: 8px 20px;
        &.btn-clear {
          background-color: #c1c1c1;

        }
        &.btn-cancel {
          background-color: burlywood;
          margin-right: 10px;
        }
      }
    }
  }
}
```

# 注册

全局注册日期选择器,也就是在main.js里面注册

`
import dateTimeSelect from './plugins/dateTime'
import './plugins/dateTime/index.scss'

Vue.use(dateTimeSelect);
`

# 调用

调用有三种方式

日期和时间的格式

```js
this.$dateTimeSelect({
	date: this.form.YJSSDate ? new Date(this.form.YJSSDate.replace(/-/g, '/').padEnd(19, ' 00:00:00')) : new Date(),
	type: 'datetime',
	callback: (date) => {
		this.$set(this.form, 'YJSSDate', date ? formatDate(date) : '');
	}
});
```

效果

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-04-01/js-datetime.jpg)


日期格式

```js
this.$dateTimeSelect({
	date: this.form.YJSSDate ? new Date(this.form.YJSSDate.replace(/-/g, '/').padEnd(19, ' 00:00:00')) : new Date(),
	type: 'date',
	callback: (date) => {
		this.$set(this.form, 'YJSSDate', date ? formatDate(date) : '');
	}
});
```

效果

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-04-01/js-date.jpg)

时间格式

```js
this.$dateTimeSelect({
	date: this.form.YJSSDate ? new Date(this.form.YJSSDate.replace(/-/g, '/').padEnd(19, ' 00:00:00')) : new Date(),
	type: 'time',
	callback: (date) => {
		this.$set(this.form, 'YJSSDate', date ? formatDate(date) : '');
	}
});
```

效果

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/2023-04-01/js-datetime.jpg)


