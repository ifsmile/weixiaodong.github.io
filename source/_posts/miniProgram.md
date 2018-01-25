---
title: 小程序入门
date: 2018-01-25 15:37:55
tags:
    - 小程序
cover_picture: images/mini_program.jpg
---
虽然小程序已经发布这么长时间了，但是一直没有机会学习，最近公司任务不是太重，终于可以静下心来好好学习一番。最终整理了一份新手入门教程，比较适合刚接触小程序的童鞋学习，有写的不对的地方还请各位大侠多多指点！

> 首先附上小程序[官方网站](https://mp.weixin.qq.com/debug/wxadoc/dev/index.html)

### 开发工具安装
- 根据自己的电脑配置进行[下载](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/download.html)

### 创建小程序项目
- 首先打开安装好的开发者工具，进入小程序
- ![](/images/wx_tool.png)
- 点击创建项目
- ![](/images/creat_miniProgram.png)
- 选择项目目录
- AppID不用写，点击 点此体验 按钮就可以
- 输入项目名称（随意填写）
- 勾选 建立普通快速启动模板
- 点击确定，项目创建完毕

### 项目结构介绍
通过上面的步骤会生成一个小程序项目，里面所有页面都会包含四种格式的文件（.js、.json、.wxml、.wxss）
相信大多数童鞋看到这种文件结构都会惊呼这是什么鬼，不用惊慌，这只是小程序的自定义文件格式，只要搞懂这几分文件是干什么的就没有问题了。

wxml: 专门写页面布局的文件，可以直接使用小程序自带的[组件库](https://mp.weixin.qq.com/debug/wxadoc/dev/component/)
wxss: 样式文件
js: 专门写js的文件
json: 页面配置文件，可以配置页面标题、标题颜色、标题背景色等

### 重点介绍一下json配置文件

```json
    {
        "pages": [
            "pages/index/index",
            "pages/logs/index"
        ],
        "window": {
            "navigationBarTitleText": "Demo"
        },
        "tabBar": {
            "list": [{
            "pagePath": "pages/index/index",
            "text": "首页"
            }, {
            "pagePath": "pages/logs/logs",
            "text": "日志"
            }]
        },
        "networkTimeout": {
            "request": 10000,
            "downloadFile": 10000
        },
        "debug": true
    }
```
- pages: 配置页面的路径，所有可以路由到的页面都必须把路径事先写到这里。注：写在第一个的路径为首页路径
- window: 配置小程序的状态栏、导航条、标题、窗口背景色
- tabBar: 配置小程序tab栏的数量、位置、激活状态等样式。
    注：
    - 其中的pagePath用于配置需要跳转的路径
    - 只有在配置pagePath的页面才会出现此tab栏
- networkTimeout: 用于设置请求或下载的超时时间
- debug: 调试

> 学了这篇博客，相信大家基本上都能独立创建属于自己的小程序，后续会深入讲解小程序组件库的一些基本用法，敬请期待！



