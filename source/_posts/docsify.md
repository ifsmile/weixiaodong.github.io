---
title: docsify
date: 2018-03-13 11:25:10
tags:
    - tool
cover_picture: images/docsify.png
summary: 今天给大家介绍一款无需构建就可以快速生成文档网站的插件--docsify
---

### 安装
```bash
npm install docsify-cli -g
```
再安装一个简单的webServer插件:anywhere

```
npm install anywhere -g
```

### 初始化项目
```
docsify init docs
```
docs是我随便起的项目名字
然后会生成一个名为docs的文件夹，里面有三个文件:
```
.nojekyll
index.html
README.md
```
### 使用
在docs文件夹下面运行命令:anywhere启动项目
```bash
anywhere
```

### 配置
在index.html中会有几行代码用来配置项目

```js
window.$docsify = {
    el: '#app',             // 项目挂载的节点
    name: 'title',          // 项目名称
    repo: '',               // 仓库地址，用于在页面右上角显示github图标
    loadSidebar: true,      // 加载左侧目录列表
    autoHeader: true,       // 自动加载每个文章标题
    search: {               // 检索
        noData: '没有结果!',
        paths: 'auto',
        placeholder: '搜索'
    },
    auto2top: true,         // 切换页面时自动回到顶部
    subMaxLevel: 2          // 左侧树组件最大层级
} 
```
### 配置左侧目录树
新建文件: _sidebar.md ， 放在根目录下和index.html同级
然后在_sidebar.md文件下配置目录:
```json
- 代码规范
    - [html](style/html)
    - [image](style/image)
    - [javascript](style/javascript)
- [系统文档](project/system)
    - [文件结构](project/new-built)
```
[]中是目录树节点名称，()中是文章路径。
### 文章书写
文章使用markdown语法编辑,并且会被解析为html。
> 该项目不具有模块热替换功能，代码做出修改后需要手动刷新浏览器。