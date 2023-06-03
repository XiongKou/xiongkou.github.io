---
layout: post
title:  "vue下拉框"
categories: vue
tags: select
author: 熊叩
---

* content
{:toc}
 
原生下拉框看起来比较丑,使用vue封装了一个下拉框










#调用

```html
<akd-select v-model="area">
	<p v-for="item in areaList" :key="item.value" :value="item">
		\{\{ item.DeptName \}\}
	</p>
</akd-select>
```

areaList 是下拉框的 option 项

# 效果

![](https://blogpackage.oss-cn-shenzhen.aliyuncs.com/vue-xiala/vue-xiala.png)

# html

```html

<template>
	<div class="akd-select">
		<input
			:class="{ disabled: disabled }"
			type="text"
			v-model="text"
			readonly
			:placeholder="placeholder"
			@click="togglePop"
		/>
		<i class="fa fa-angle-down"></i>
		<transition name="slide-up">
			<div
				@click="selectHandler"
				v-show="showPop"
				:class="{ 'in-bottom': isInBottom }"
			>
				<slot></slot>
			</div>
		</transition>
	</div>
</template>

<script>

export default {
	name: "akdSelect",
	props: ["placeholder", "value", "disabled"],
	data() {
		return {
			showPop: false,
			isInBottom: false,
			text: "",
		};
	},
	created() {
		// this.centerNotice.$on("close_all_pop", () => {
		// 	this.showPop = false;
		// });
	},
	methods: {
		setData(value, text) {
			this.text = text;
			this.$emit("input", value);
			this.$emit("change", value);
		},
		getTarget(target, curr, parent) {
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
		},
		selectHandler(e) {
			if (!this.showPop) return;
			const target = this.getTarget(e.target, "p");
			if (!target) return;
			const value = this._vnode.children[2].child._vnode.children.filter(
				(c) => {
					return c.elm === target;
				}
			)[0].data.attrs.value;
			if (this.value !== value) {
				this.setData(value, target.innerText);
			}
			this.showPop = false;
		},
		togglePop(e) {
			e.stopPropagation();
			if (this.disabled) {
				return;
			}
			this.isInBottom =
				e.target.getBoundingClientRect().y > window.innerHeight / 2;
			this.showPop = !this.showPop;
		},
	},
	watch: {
		value: {
			handler(newValue) {
				this.$nextTick(() => {
					let flag = false;
					const options = [
						...this._vnode.children[2].child._vnode.children,
					];
					options.some((o) => {
						if (o.data.attrs.value === newValue) {
							flag = true;
							this.setData(o.data.attrs.value, o.elm.innerText);
							return true;
						}
					});
					if (!flag) {
						this.setData(newValue, "");
					}
				});
			},
			immediate: true,
		},
	},
};
</script>

<style lang="scss">
.akd-select {
	position: relative;
	height: 42px;
	line-height: 42px;
	input {
		height: 100%;
		width: 100%;
		border-radius: 3px;
		padding: 0 10px;
		border: solid 1px lightgray;
		vertical-align: bottom;
		font-size: 14px;
		&.disabled {
			background-color: #f1f1f1;
		}
	}
	i {
		position: absolute;
		pointer-events: none;
		right: 0;
		top: 0;
		width: 38px;
		height: 100%;
		text-align: center;
		line-height: inherit;
	}
	div {
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
		box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.3);
		&.in-bottom {
			top: auto;
			bottom: 100%;
		}
	}
	p {
		margin: 0;
		padding: 5px 15px;
		font-size: 15px;
		&:not(:last-child) {
			border-bottom: solid 1px #efefef;
		}
	}
}
</style>

```
