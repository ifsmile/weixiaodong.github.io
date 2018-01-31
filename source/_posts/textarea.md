---
title: 基于vue实现textarea高度自适应
date: 2018-01-27 14:32:01
tags:
    - vue
    - JavaScript
---
这次主要介绍一个基于vue做的指令，主要用来解决textarea不能高度自适应的问题!

在之前的项目中写过一个仿微信的聊天组件，当写到聊天输入框的时候遇到了输入框不能高度自适应的问题，使用input的话只能单行输入，使用textarea的话只能固定高，不管哪一种都不能满足需求。
在网上查了很多资料，发现有一种方法可以满足需求，简单介绍下：
- 将一个div和一个textarea定位在一块，宽度一样，div高度自适应，且颜色背景都设置为透明
- 当在textarea中输入内容的同时把输入的值写到div中
- 此时div的高度会发生变化，再用js把textarea的高度设置为div变化后的高度

这种方法基本上能满足需求，但是会出现卡顿的现象，并且整体用起来不够流畅，最后还是弃用了，所以才有了autoHeight指令的诞生。

## 代码

```js
class AutoHeight {
    constructor(el) {
        this.$el = el
        this.value = el.value
        this.borderWidth = window.getComputedStyle(el, null).borderWidth.replace('px', '')

        this.initDom()
        this.resize()
        this.bind()
    }

    initDom() {
        this.$el.style.resize = 'none'
        this.$el.style.boxSizing = 'border-box'
    }
    bind () {
        this.$el.addEventListener('input', this.resize)
    }

    unbind() {
        this.$el.removeEventListener('input', this.resize)
    }

    resize = e => {
        this.changeOverflow()
        this.setHeight()
    }

    changeOverflow() {
        const overflowY = window.getComputedStyle(this.$el, null).overflowY
        if (overflowY === 'hidden') {
            this.$el.style.overflowY = 'scroll'
            this.$el.style.overflowY = 'hidden'
            return
        }
        this.$el.style.overflowY = 'hidden'
    }

    setHeight() {
        const originalHeight = this.$el.style.height
        this.$el.style.height = 'auto'
        const endHeight = this.$el.scrollHeight
        if (endHeight === 0) {
            this.$el.style.height = originalHeight
            return
        }
        this.$el.style.height = `${endHeight + this.borderWidth * 2}px`
    }
}

const plugin = {
    inserted(el) {
        if(el.tagName !== 'TEXTAREA') return
        el.__AutoHeight = new AutoHeight(el)
    },
    unbind(el) {
        if(el.__AutoHeight) {
            el.__AutoHeight.unbind()
            delete el.__AutoHeight
        }
    },
    update(el) {
        if(el.__AutoHeight) {
            el.__AutoHeight.resize()
        }
    },
    install(Vue) {
        Vue.directive('autoHeight', plugin)
    }
}

export default plugin

```

## 引用
```js
import Vue from 'vue'
import autoHeight from 'plugins/autoHeight'

Vue.use(autoHeight)
```

## 用法
```html
<textarea v-autoHeight></textarea>
```