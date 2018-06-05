---
title: Electron-vue
date: 2018-06-05 16:27:16
tags:
    - electron
    - webpack
    - vue
cover_picture: images/electron.png
summary: 本文主要讲解一款可以构建跨平台的桌面应用的框架--Electron。通过本文的学习就可以构建一款基于vue开发并由webpack进行打包的桌面应用程序。
---

> 通过Electron创建的应用可以运行在Mac、Windows、Linux系统中。你可以将它看做是一个小的Chromium浏览器，它支持一个css新特性，基本上不用考虑浏览器的兼容问题，它具有丰富的API可以直接访问底层的系统应用。

### 安装
```bash
    npm install electron --save-dev
    npm install vue-cli -g
```
### 创建应用
```bash
    vue init simulatedgreg/electron-vue my-project-name
    cd my-project-name
    npm install
```
这是项目已经构建成功，项目主进程以index.ejs为模板文件，webpack会以renderer文件夹下面的main.js为入口文件对项目进行构建。

### 创建窗口
项目会有一个主进程，每一个子窗口代表一个子进程。
在src/main下面有一个index.js文件，用来创建项目主进程（主窗口）。

```js
const {app, BrowserWindow} = require('electron')
let mainWindow

function createWindow () {
// 创建窗口
  mainWindow = new BrowserWindow({
    height: 600,
    width: 1000
  })

  mainWindow.on('closed', () => {
    mainWindow = null
  })
}

app.on('ready', createWindow)

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', () => {
  if (mainWindow === null) {
    createWindow()
  }
})
```
BrowserWindow用来创建和控制浏览器窗口，它含有丰富的参数，具体可[查看](https://electronjs.org/docs/api/browser-window)
这里介绍几个常用的参数：
 - title 窗口标题
 - width 窗口宽度
 - height 窗口高度
 - show 创建之后是否立即显示窗口（默认：true）
 - autoHideMenuBar 是否自动隐藏菜单栏（默认：false）

 ### 自定义菜单
 ```js
    import { remote } from 'electron'
    const { Menu } = remote
    // 创建菜单模板
    const menu = Menu.buildFromTemplate([{
        label: '文件',
        click () {
            console.log('文件')
        },
        }, {
        label: '关于我们',
        click () {
            console.log('关于我们')
        },
        // 子菜单
        submenu: [{
            label: '帮助',
            click () {
                console.log('帮助')
            }
        }, {
            label: '博客',
            click () {
                console.log('博客')
            }
        }]
    }])
    // 设置菜单
    Menu.setApplicationMenu(menu)
 ```
### 打包
```bash
    npm run build
```
build其实执行的是``.electron-vue/build.js && electron-builder``命令
项目生成的代码会打包到dist/electron目录下
项目生成的应用程序会打包到build/win-unpacked目录下,其中以.exe结尾的文件就是可运行的程序
> 大家可以不用特意计较electron-packager与electron-builder的区别，一般情况下都推荐大家使用electron-builder的打包方式。
> electron-builder就是有比electron-packager有更丰富的的功能，支持更多的平台，同时也支持了自动更新。除了这几点之外，由electron-builder打出的包更为轻量，并且可以打包出不暴露源码的setup安装程序。

