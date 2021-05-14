---
title: Electron+React实战跨平台应用
date: 2021-05-14 12:19:27
tags: [electron,react]
categories: 实战项目笔记
---
# Electron+React实战跨平台应用

## 应用架构

md和xlsx文档编辑并自动上传保存到七牛云

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422205101724.png" alt="image-20210422205101724" style="zoom: 80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422205200618.png" alt="image-20210422205200618" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422205311162.png" alt="image-20210422205311162" style="zoom: 50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422205332813.png" alt="image-20210422205332813" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422205356869.png" alt="image-20210422205356869" style="zoom:50%;" />

![image-20210422205435395](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422205435395.png)

## DEMO

### 脚手架

```js
git clone https://github.com/electron/electron-quick-start
```

* 执行npm install安装node_modules会报错，按照如下命令安装并启动成功

```js
//使用淘宝镜像
npm config set ELECTRON_MIRROR https://npm.taobao.org/mirrors/electron/

//为了兼容后面的devtron开发者工具(最近一次更新是5年前），需要降级到5.0.6版本
npm uninstall electron
npm install -D electron@5.0.6

npm install
npm start
```

* Demo的图片

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422212452264.png" alt="image-20210422212452264" style="zoom: 50%;" />

### 进程和线程

线程是操作系统能够调度的最小单位，被包含在进程之中。一个进程可以有多个线程

一个应用程序可以有多个进程。

js是异步单线程

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422212801278.png" alt="image-20210422212801278" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422212912755.png" alt="image-20210422212912755" style="zoom:50%;" />

进程之间内存很难共享，通信也不方便（可以使用IPC实现）。

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422212954436.png" alt="image-20210422212954436" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422213101165.png" alt="image-20210422213101165" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422213132987.png" alt="image-20210422213132987" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422213156689.png" alt="image-20210422213156689" style="zoom:50%;" />

![image-20210422213212153](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422213212153.png)

### 创建BrowserWindow和代码流程分析

要是的nodejs支持热更新，需要使用nodemon

```js
1、安装nodemon
npm install -D nodemon

2、设置package.json中start命令,监听main.js变化并且运行程序
    "start": "nodemon --watch main.js --exec \"electron .\""
```

elecrton底层基于nodejs，因此main.js是commonjs规范，引入是require,导出时module.exports。

代码执行流程分析

1、执行npm start运行main.js文件开启主进程。

2、main.js流程分析

```js
// Modules to control application life and create native browser window
const {app, BrowserWindow} = require('electron')
const path = require('path')

function createWindow () {
  // 2.1创建一个浏览器窗口
  const mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      //2.2 执行预加载js,为window添加监听，当DOMContentloaded以后替换页面上的文字
      preload: path.join(__dirname, 'preload.js')
    }
  })

  //2.3 在这个窗口上挂载index.html，index.html中引入renderer.js，开启渲染进程
  mainWindow.loadFile('index.html')
}
app.whenReady().then(() => {
  createWindow()
  
  app.on('activate', function () {
    // On macOS it's common to re-create a window in the app when the
    // dock icon is clicked and there are no other windows open.
    if (BrowserWindow.getAllWindows().length === 0) createWindow()
  })
})

```

```js
2.4 renderer.js
//如果渲染这个js的窗口window的nodeIntegration没有设置或者设置为false
//那么这个文件里面不允许调用Nodejs api(例如require process等)，只可以调用DOM BOM等浏览器相关api
//当nodeIntegration为true时，可以混合调用

```

### 嵌套window

```js
// Modules to control application life and create native browser window
const {app, BrowserWindow} = require('electron')
const path = require('path')

function createWindow () {
  // Create the browser window.
  const mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js'),
      nodeIntegration:true
    }
  })
  mainWindow.loadFile('index.html')

  const secondWindow = new BrowserWindow({
    width:400,
    height:300,
    webPreferences: {
      nodeIntegration:true
    },
    //设置父窗口
    parent:mainWindow
  })
  secondWindow.loadFile('second.html')
()
}

app.whenReady().then(() => {
  createWindow()
  
  app.on('activate', function () {
    // On macOS it's common to re-create a window in the app when the
    // dock icon is clicked and there are no other windows open.
    if (BrowserWindow.getAllWindows().length === 0) createWindow()
  })
})
```

### DOMContentLoaded和onload的区别

他们的区别是，触发的时机不一样，先触发DOMContentLoaded事件，后触发load事件。

DOM文档加载的步骤为

解析HTML结构。

加载外部脚本和样式表文件。

解析并执行脚本代码。

DOM树构建完成。//DOMContentLoaded

加载图片等外部文件。

页面加载完毕。//load

在第4步，会触发DOMContentLoaded事件。在第6步，触发load事件。

### 进程间通信

https://www.cnblogs.com/baixinL/p/14276580.html

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422221902300.png" alt="image-20210422221902300" style="zoom:50%;" />

#### 安装 devtron，类型redux-dev-tools的浏览器插件，是electron的开发者工具方便查看和模拟进程间通信

```js
npm i -D devtron

//修改main.js的，添加两行代码
function createWindow () {
  // Create the browser window.
  //运行devtron浏览器插件  add 
  require('devtron').install()
  const mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js'),
      nodeIntegration:true
    }
  })
  mainWindow.loadFile('index.html')
  //ctrl+shift+i打开控制台面板
    globalShortcut.register('CommandOrControl+Shift+i', function () { currentWindow.webContents.openDevTools() });
  
}
```

开发者工具多了一个devtron面板

![image-20210422232332927](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422232332927.png)

#### 使用IPC进行进程通信

类似订阅监听模式，但是main process只能通过reply方式回复，不能调用ipcRenderer.send

```js
//from
const {ipcRenderer} = require("electron")
document.getElementById("sendBtn").addEventListener("click",()=>{
    //发送IPC
    ipcRenderer.send("message","hello from renderer")
    //监听回复
    ipcRenderer.on("reply",(event,arg)=>{
        console.log(arg)
    })
})

//to
const {ipcMain} = require('electron')
ipcMain.on("message",(event,arg)=>{
    //event是时间对象，arg就是传递过来的参数"hello from renderer"
    console.log(event,arg)
    //回复
    event.reply("reply","i receive you hello")
 })
```



![image-20210422233924132](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210422233924132.png)

#### 使用remote模块实现跨进程访问(由于安全性问题，最新版本已经被废弃)

ipcMain和ipcRenderer方式太麻烦。使用require('electron').remote可以获取到main process的独有的api.

例如

```js
const {BrowserWindow} = require('elecrton').remote

const newWindow = new BrowserWindow({
        width:400,
        height:300
    })
//也可以加载一个在线网址
newWindow.loadURL("https://www.baidu.com")
```

#### webContents.send发送

```js
// 在主进程中.
const { app, BrowserWindow } = require('electron')
let win = null

app.whenReady().then(() => {
  win = new BrowserWindow({ width: 800, height: 600 })
  win.loadURL(`file://${__dirname}/index.html`)
  win.webContents.on('did-finish-load', () => {
    win.webContents.send('ping', 'whoooooooh!')
  })
})

<!-- index.html -->
<html>
<body>
  <script>
    require('electron').ipcRenderer.on('ping', (event, message) => {
      console.log(message) // Prints 'whoooooooh!'
    })
  </script>
</body>
</html>
```

#### electron设置菜单不显示

https://blog.csdn.net/qq_42597536/article/details/116017264

### React学习

略

* npx npm cnpm yarn区别，详见https://blog.csdn.net/lixiaolong240035/article/details/98472531

### 调试

详见：https://www.cnblogs.com/vickylinj/p/14087734.html

#### 渲染线程

webContents.openDevTools即可

#### 主线程

使用inspect

## 真实项目

### 需求分析

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210423001658538.png" alt="image-20210423001658538" style="zoom:50%;" />



![image-20210423140553191](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210423140553191.png)

![image-20210423140753290](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210423140753290.png)

### 环境搭建

#### ts环境搭建

`npx create-react-app doc-editor --template typescript`

添加代码规范配置和git commit配置，规范代码结构和提交，详见ts实战项目.md

```js
{
  "name": "doc-editor",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/jest-dom": "^5.11.4",
    "@testing-library/react": "^11.1.0",
    "@testing-library/user-event": "^12.1.10",
    "@types/jest": "^26.0.15",
    "@types/node": "^12.0.0",
    "@types/react": "^17.0.0",
    "@types/react-dom": "^17.0.0",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "4.0.3",
    "typescript": "^4.1.2",
    "web-vitals": "^1.0.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest",
      "prettier"
    ]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  },
  "lint-staged": {
    "*.{js,css,md,ts,tsx}": "prettier --write"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "devDependencies": {
    "@commitlint/cli": "^12.1.1",
    "@commitlint/config-conventional": "^12.1.1",
    "electron": "^5.0.6",
    "eslint-config-prettier": "^8.2.0",
    "husky": "^4.2.3",
    "lint-staged": "^10.0.8",
    "prettier": "2.2.1"
  }
}
```

#### elecrton环境搭建

mainwindow加载localhost:3000网址

由于使用的是create-react-app脚手架，不能采用electron脚手架，需要手动安装electron和devtron并添加js文件

1、根目录下新建main.js文件，使用electron-is-dev来区分开发环境和生成环境

```js
const {app,BrowserWindow} = require('electron')
const isDev = require('electron-is-dev')
let mainWindow
app.on('ready',()=>{
    mainWindow = new BrowserWindow({
        width:1024,
        height:680,
        webPreferences:{
            nodeIntegration:true
        }
    })
    const urlLocation = isDev ? "http://localhost:3000" : "todo"
    mainWindow.loadURL(urlLocation)
})
```

2、设置package.json

```js
{
  "name": "doc-editor",
  "main": "main.js",  //设置入口为main.js
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/jest-dom": "^5.11.4",
    "@testing-library/react": "^11.1.0",
    "@testing-library/user-event": "^12.1.10",
    "@types/jest": "^26.0.15",
    "@types/node": "^12.0.0",
    "@types/react": "^17.0.0",
    "@types/react-dom": "^17.0.0",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "4.0.3",
    "typescript": "^4.1.2",
    "web-vitals": "^1.0.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "dev": "electron ."  //添加配置
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest",
      "prettier"
    ]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  },
  "lint-staged": {
    "*.{js,css,md,ts,tsx}": "prettier --write"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "devDependencies": {
    "@commitlint/cli": "^12.1.1",
    "@commitlint/config-conventional": "^12.1.1",
    "electron": "^5.0.6",
    "electron-is-dev": "^2.0.0",
    "eslint-config-prettier": "^8.2.0",
    "husky": "^4.2.3",
    "lint-staged": "^10.0.8",
    "prettier": "2.2.1"
  }
}

```

3、开两个终端，第一个终端先执行`npm start`，第二个终端后执行`npm run dev`

如此即可进行联动起来。

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210423142016562.png" alt="image-20210423142016562" style="zoom:50%;" />

4、上面有两个问题，需要开两个终端，而且要先启动服务，才能运行electron

使用concurrently可以同时运行这两个命令`npm install concurrently --save`

修改dev命令如下，使用\转移双引号

`  "dev": "concurrently \"electron .\" \"npm start\""`

这样只需要执行npm run dev就可以同时启动react和electron

但是由于react启动比较慢，所以electron启动会有白屏等待react

5、安装wait on解决白屏问题

`npm install -D wait-on`

修改命令

`  "dev": "concurrently \"wait-on http://localhost:3000 && electron .\" \"npm start\""`

等待http://localhost:3000加载完毕后再运行electron

6、每次都要打开浏览器，如何解决

react有个环境变量BROWSER可以设置

但是linux和window用到环境变量设置的书写机制不同，没法用一个命令去写。需要引入cross-env组件来解决环境变量跨平台问题

`npm install -D cross-env`

修改命令

`  "dev": "concurrently \"wait-on http://localhost:3000 && electron .\" \"cross-env BROWSER=none npm start\""`

这样一个electron和react结合的完美开发环境就搭建好了，支持热更新。愉快的敲代码吧

### 文件结构和代码规范

* 没有固定标准
* 不要超过5分钟思考
* 避免多层嵌套

### 样式库选择Bootstrap

`npm installl bootstrap`

在App.tsx上面引入bootstrap样式

`import "bootstrap/dist/css/bootstrap.min.css"`

Bootstrap基础知识

* loading图标使用

* ```js
  <div
          className="spinner-border"
          role="status"
          style={{ visibility: isLoading ? "visible" : "hidden" }}
        >
          <span className="sr-only">Loading...</span>
        </div>
  ```

* 

* d-flex

* align-items-center

* margin padding简写 p-0 pl-0 p-1 m-0 m-1 ml-1，详见https://blog.csdn.net/mp624183768/article/details/84685097

* 布局容器：Bootstrap 需要为页面内容和栅格系统包裹一个 `.container` 容器。我们提供了两个作此用处的类。注意，由于 `padding` 等属性的原因，这两种 容器类不能互相嵌套。

  `.container` 类用于固定宽度并支持响应式布局的容器。

  ```
  <div class="container">
    ...
  </div>
  ```

  `.container-fluid` 类用于 100% 宽度，占据全部视口（viewport）的容器。我们的软件视口是变化的，因此使用container-fluid

  ```
  <div class="container-fluid">
    ...
  </div>
  ```

* 栅格系统：栅格系统用于通过一系列的行（row）与列（column）的组合来创建页面布局，你的内容就可以放入这些创建好的布局中。下面就介绍一下 Bootstrap 栅格系统的工作原理：

  - “行（row）”必须包含在 `.container` （固定宽度）或 `.container-fluid` （100% 宽度）中，以便为其赋予合适的排列（aligment）和内补（padding）。

  - 通过“行（row）”在水平方向创建一组“列（column）”。

  - 你的内容应当放置于“列（column）”内，并且，只有“列（column）”可以作为行（row）”的直接子元素。

  - 类似 `.row` 和 `.col-xs-4` 这种预定义的类，可以用来快速创建栅格布局。Bootstrap 源码中定义的 mixin 也可以用来创建语义化的布局。

  - 通过为“列（column）”设置 `padding` 属性，从而创建列与列之间的间隔（gutter）。通过为 `.row` 元素设置负值 `margin` 从而抵消掉为 `.container` 元素设置的 `padding`，也就间接为“行（row）”所包含的“列（column）”抵消掉了`padding`。

  - 负值的 margin就是下面的示例为什么是向外突出的原因。在栅格列中的内容排成一行。

  - 栅格系统中的列是通过指定1到12的值来表示其跨越的范围。例如，三个等宽的列可以使用三个 `.col-xs-4` 来创建。

  - 如果一“行（row）”中包含了的“列（column）”大于 12，多余的“列（column）”所在的元素将被作为一个整体另起一行排列。

  - 栅格类适用于与屏幕宽度大于或等于分界点大小的设备 ， 并且针对小屏幕设备覆盖栅格类。 因此，在元素上应用任何 `.col-md-*` 栅格类适用于与屏幕宽度大于或等于分界点大小的设备 ， 并且针对小屏幕设备覆盖栅格类。 因此，在元素上应用任何 `.col-lg-*` 不存在， 也影响大屏幕设备。

  - ```html
    实例：从堆叠到水平排列
    使用单一的一组 .col-md-* 栅格类，就可以创建一个基本的栅格系统，在手机和平板设备上一开始是堆叠在一起的（超小屏幕到小屏幕这一范围），在桌面（中等）屏幕设备上变为水平排列。所有“列（column）必须放在 ” .row 内。
    
    .col-md-1.col-md-1.col-md-1.col-md-1.col-md-1.col-md-1.col-md-1.col-md-1.col-md-1.col-md-1.col-md-1.col-md-1
    .col-md-8.col-md-4
    .col-md-4.col-md-4.col-md-4
    .col-md-6.col-md-6
    <div class="row">
      <div class="col-md-1">.col-md-1</div>
      <div class="col-md-1">.col-md-1</div>
      <div class="col-md-1">.col-md-1</div>
      <div class="col-md-1">.col-md-1</div>
      <div class="col-md-1">.col-md-1</div>
      <div class="col-md-1">.col-md-1</div>
      <div class="col-md-1">.col-md-1</div>
      <div class="col-md-1">.col-md-1</div>
      <div class="col-md-1">.col-md-1</div>
      <div class="col-md-1">.col-md-1</div>
      <div class="col-md-1">.col-md-1</div>
      <div class="col-md-1">.col-md-1</div>
    </div>
    <div class="row">
      <div class="col-md-8">.col-md-8</div>
      <div class="col-md-4">.col-md-4</div>
    </div>
    <div class="row">
      <div class="col-md-4">.col-md-4</div>
      <div class="col-md-4">.col-md-4</div>
      <div class="col-md-4">.col-md-4</div>
    </div>
    <div class="row">
      <div class="col-md-6">.col-md-6</div>
      <div class="col-md-6">.col-md-6</div>
    </div>
    ```

  - 使用col-md col-xs col-lg可以实现类似媒介查询效果

  - 使用bg-xxx设置背景色

### 图标库选择

https://fontawesome.com/how-to-use/on-the-web/using-with/react

安装react-fontawesome

```js
npm i --save @fortawesome/fontawesome-svg-core
  npm install --save @fortawesome/free-solid-svg-icons
  npm install --save @fortawesome/react-fontawesome
```

使用

```js
import ReactDOM from 'react-dom'
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
import { faCoffee } from '@fortawesome/free-solid-svg-icons'

const element = <FontAwesomeIcon icon={faCoffee} />
1
ReactDOM.render(element, document.body)
```



### FileSearch组件开发

![image-20210423153637251](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210423153637251.png)

* 使用ctrl+shift+i可以打开控制台

* 默认最大化 `win = new BrowserWindow({show: false}) win.maximize() win.show()`
* 默认全屏 `win = new BrowserWindow({fullscreen: true})`

### FileList组件开发

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210423171105189.png" alt="image-20210423171105189" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210423171233911.png" alt="image-20210423171233911" style="zoom:50%;" />

使用Bootstrap的列表组件

markdown图标在fontawesome图标库的brands分类下

使用styled-components编写style

不同文件类型渲染不同

useKeyboward封装

### TabList组件开发

#### 需求



<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210423225558519.png" alt="image-20210423225558519" style="zoom:50%;" />

使用bootstrap nav-pills实现

### 编辑器

TinyMCE 、 Ueditor

支持预览模式

支持高亮显示不同内容

支持自定义工具栏

如果github某个库很久以前更新的，可以谷歌该库的fork，看看有没有fork并维护该项目的新仓库

我们这里使用EasyMDE的react封装，react-simplemde-editor

SimpleMDE需要设置key

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210423231920438.png" alt="image-20210423231920438" style="zoom:67%;" />

### 代码重构

state设计原则

下面这种设计是不好的，将打开状态、激活状态、保存状态都存储在文件的字段身上。更新比较麻烦而且冗余。可以把打开状态，激活状态、保存状态分别用数组保存对应文件id。这样设计更好。同时对象数组也不好使，建议转换成Flattern State

```js
export interface IFile {
  /**
   * id字符串
   */
  id: string;
  /**
   * 文件内容
   */
  body: string;
  /**
   * 文件名
   */
  title: string;
  /**
   * 文件创建时间戳
   */
  createAt: number;
  /**
   * 文件类型
   */
  fileType: FileType;
  /**
   * 是否已经在tab面板中打开
   */
  openStatus: boolean;
  /**
   * 是否是tab面板中正在编辑的文档，是则active，否则unactive
   */
  activeStatus: ActiveStatus;
  /**
   * 保存状态，编辑的文档内容是否保存，默认为false，一旦内容变化，则为true
   */
  saveStatus: SaveStatus;
  /**
   * 搜索状态，为true则显示，为false则不显示，默认为true
   */
  searchShow: boolean;
}
export type ActiveStatus = "active" | "unactive";
export type SaveStatus = "unsave" | "saved";
export type FileType = "md" | "doc" | "xls" | "xlsx" | "ppt";
```



Flatten State

* 解决对象数组冗余
* 数据处理更加方便

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210425161755754.png" alt="image-20210425161755754" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210425161826733.png" alt="image-20210425161826733" style="zoom:50%;" />

方法封装

```typescript
import { IFile, IFlattenFiles } from "../types";
/**
 * 对象数组扁平化为flatten obj，方便处理
 * @param arr 要Flatten的对象数组
 */
export const flattenArrToObj = (arr: IFile[]): IFlattenFiles => {
  return arr.reduce((map, item) => {
    map[item.id] = item;
    return map;
  }, <IFlattenFiles>{});
};
/**
 * 将flatten的obj转换为对象数组
 * @param obj flatten的obj
 */
export const objUnFlattenToArr = (obj: IFlattenFiles): IFile[] => {
  return Object.keys(obj).map((key) => obj[key]);
};
```

### 文件操作和数据持久化

使用electron-store进行数据持久化

### 导入文件

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210429150223671.png" alt="image-20210429150223671" style="zoom:50%;" />

### 添加上下文菜单

https://newsn.net/say/electron-context-menu.html

```typescript
useEffect(() => {
    const menu = new Menu();
    menu.append(
      new MenuItem({
        label: "重命名",
        click: () => {
          console.log("重命名");
        },
      })
    );
    const handleContextMenu = (e: any) => {
      menu.popup({ window: remote.getCurrentWindow() });
    };
    window.addEventListener("contextmenu", handleContextMenu);
    return () => {
      window.removeEventListener("contextmenu", handleContextMenu);
    };
  }, []);
```

hooks封装

```typescript
import { useEffect } from "react";
const { remote } = window.require("electron");
const { Menu, MenuItem } = remote;
const useContextMenu = (menuItems: any) => {
  useEffect(() => {
    const menu = new Menu();
    menuItems.forEach((item: any) => {
      menu.append(new MenuItem(item));
    });

    const handleContextMenu = (e: any) => {
      menu.popup({ window: remote.getCurrentWindow() });
    };
    window.addEventListener("contextmenu", handleContextMenu);
    return () => {
      window.removeEventListener("contextmenu", handleContextMenu);
    };
  }, [menuItems]);
};

export default useContextMenu;

//使用
  useContextMenu([
    {
      label: "重命名",
      click: () => {
        console.log("重命名");
      },
    },
  ]);
```

使用ref保存点击节点并且使得菜单只在文件区域出现

```typescript
import { useEffect, useRef } from "react";
const { remote } = window.require("electron");
const { Menu, MenuItem } = remote;
const useContextMenu = (menuItems: any, querySelectorString: string) => {
  const ref = useRef();
  useEffect(() => {
    const menu = new Menu();
    menuItems.forEach((item: any) => {
      menu.append(new MenuItem(item));
    });

    const handleContextMenu = (e: any) => {
      ref.current = e.target;
      //点击范围限制，使用原生dom的contains方法
      if (document.querySelector(querySelectorString)?.contains(e.target)) {
        menu.popup({ window: remote.getCurrentWindow() });
      }
    };
    window.addEventListener("contextmenu", handleContextMenu);
    return () => {
      window.removeEventListener("contextmenu", handleContextMenu);
    };
  }, [menuItems, querySelectorString]);
  return ref;
};

export default useContextMenu;

//使用，list-group是文件列表区域，只有点击文件列表区域才会出现右键菜单
//添加上下文菜单
  const nodeRef = useContextMenu(
    [
      {
        label: "重命名",
        click: () => {
          console.log("重命名");
          console.log(nodeRef.current);
        },
      },
    ],
    ".list-group"
  );
```

### 通过点击的dom获取到点击的区域

由于使用的是原生Dom，监听鼠标右键点击事件，只能获取到点击的原生Dom，不能获取到点击的是哪一个File。因此需要使用data-*进一步处理

1、在FileList循环时挂接上信息

```html
             <li
                style={{
                  cursor: "pointer",
                  display: item.searchShow ? "flex" : "none",
                }}
                key={item.id}
                onMouseEnter={(_) => setHoverFileId(item.id)}
                onDoubleClick={handleDoubleClick(item)}
                className="file-list list-group-item justify-content-between flex-nowrap"
                //储存信息方便原生DOM获取
                data-id={item.id}
                data-title={item.title}
              >
```

2、封装方法冒泡获取指定类的父节点

```typescript
/**
 * 逐层向上寻找指定类名父节点(一般用于点击事件的冒泡到指定父元素)
 * @param node 当前节点
 * @param parentClsName 要寻找的父节点的唯一类名
 */
export const getParentNode = (node: any, parentClsName: string) => {
  let current = node;
  while (current !== null) {
    if (current.classList.contains(parentClsName)) {
      return current;
    }
    current = current.parentNode;
  }
  return false;
};
```

3、使用

```typescript
  //添加上下文菜单
  const nodeRef = useContextMenu(
    [
      {
        label: "重命名",
        click: () => {
          const fileListNode = getParentNode(nodeRef.current, "file-list");
          if (fileListNode) {
            const editorFile = Files.find(
              (item) => item.id === fileListNode.dataset.id
            ) as IFile;
            handleEditor(editorFile)();
          }
        },
      },
      {
        label: "打开",
        click: () => {
          const fileListNode = getParentNode(nodeRef.current, "file-list");
          if (fileListNode) {
            const editorFile = Files.find(
              (item) => item.id === fileListNode.dataset.id
            ) as IFile;
            handleDoubleClick(editorFile)();
          }
        },
      },
      {
        label: "删除",
        click: () => {
          const fileListNode = getParentNode(nodeRef.current, "file-list");
          if (fileListNode) {
            const editorFile = Files.find(
              (item) => item.id === fileListNode.dataset.id
            ) as IFile;
            handleDelete(editorFile)();
          }
        },
      },
    ],
    ".list-group"
  );

```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210502180304619.png" alt="image-20210502180304619" style="zoom:50%;" />

### 添加原生应用菜单

MenuItem的accelerator属性可以设置快捷键

可以为MenuItem指定role属性

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210502181013775.png" alt="image-20210502181013775" style="zoom:50%;" />

windows和os快捷键不同

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210502180951935.png" alt="image-20210502180951935" style="zoom:50%;" />

![image-20210502181109726](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210502181109726.png)

```typescript
1、菜单封装到一个ts文件
const { app, shell } = require("electron");

export type FILE_OPERATE_TYPE =
  | "file-save"
  | "file-new"
  | "file-search"
  | "file-import";

/**
 * 发送消息
 * @param browserWindow 窗体
 * @param type 消息类型
 */
const sendMessage = (browserWindow: any, type: FILE_OPERATE_TYPE) => {
  browserWindow.webContents.send(type);
};

let template = [
  {
    label: "文件",
    submenu: [
      {
        label: "新建",
        accelerator: "CmdOrCtrl+N",
        click: (menuItem, browserWindow, event) => {
          sendMessage(browserWindow, "file-new");
        },
      },
      {
        label: "保存",
        accelerator: "CmdOrCtrl+S",
        click: (menuItem, browserWindow, event) => {
          sendMessage(browserWindow, "file-save");
        },
      },
      {
        label: "搜索",
        accelerator: "CmdOrCtrl+F",
        click: (menuItem, browserWindow, event) => {
          sendMessage(browserWindow, "file-search");
        },
      },
      {
        label: "导入",
        accelerator: "CmdOrCtrl+O",
        click: (menuItem, browserWindow, event) => {
          sendMessage(browserWindow, "file-import");
        },
      },
    ],
  },
];

export default template;


2、main.ts中设置菜单
//设置菜单
const menu = Menu.buildFromTemplate(template);
Menu.setApplicationMenu(menu);

3、封装useIpcRender
import { useEffect } from "react";
const { ipcRenderer } = window.require("electron");

type KeyCallbackType = {
  [key: string]: () => void;
};
const useIpcRenderer = (keyAndCallBackMap: KeyCallbackType) => {
  useEffect(() => {
    Object.keys(keyAndCallBackMap).forEach((key) => {
      ipcRenderer.on(key, keyAndCallBackMap[key]);
    });
    return () => {
      Object.keys(keyAndCallBackMap).forEach((key) => {
        ipcRenderer.removeListener(key, keyAndCallBackMap[key]);
      });
    };
  });
};

export default useIpcRenderer;

3、调用
  useIpcRenderer({
    "file-new": () => {
      type = "new";
      handleClick();
    },
    "file-import": () => {
      type = "import";
      handleClick();
    },
  });

```

### 设置窗口

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210502215001800.png" alt="image-20210502215001800" style="zoom:50%;" />

### 云同步

#### 初始化七牛云平台

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210502215148721.png" alt="image-20210502215148721" style="zoom: 33%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210502215232706.png" alt="image-20210502215232706" style="zoom:33%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210502215301673.png" alt="image-20210502215301673" style="zoom:33%;" />

1、注册、实名认证、登录

2、控制台中主要使用对象存储服务

![image-20210503104328782](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210503104328782.png)

3、新建桶（Bucket）可以把桶理解为对象存储的容器，系统已经自动分配了测试域名，每天访问限额10GB，30个工作日会自动回收该域名，如果要继续使用，需要绑定自己的域名

![image-20210503104218943](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210503104218943.png)

![image-20210503104148265](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210503104148265.png)

![image-20210503105204019](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210503105204019.png)

#### 查看七牛云nodejs文档并封装

![image-20210503105542203](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210503105542203.png)

由于electron支持使用nodejs，因此这里使用服务端直传

封装QiniuManager

```js
//引入qiniu的sdk
const qiniu = require("qiniu");
const path = require("path");
const axios = require("axios");
const fs = require("fs");

class QiniuManager {
  /**
   * 构造函数
   * @param {string} ak 公钥
   * @param {*} sk 私钥
   * @param {*} bucketname 桶名
   */
  constructor(ak, sk, bucketname) {
    this.bucketname = bucketname;
    //生成鉴权对象
    this.mac = new qiniu.auth.digest.Mac(ak, sk);

    //定义config,指定相关信息
    this.config = new qiniu.conf.Config();
    // 空间对应的机房，这里默认z0(华东)
    this.config.zone = qiniu.zone.Zone_z0;

    //桶管理器
    this.bucketManager = new qiniu.rs.BucketManager(this.mac, this.config);
  }

  /**
   * 自动获取当前桶的域名
   */
  getBucketDomain() {
    const reqUrl = `http://uc.qbox.me/v2/domains?tbl=${this.bucketname}`;
    const digest = qiniu.util.generateAccessToken(this.mac, reqUrl);
    return new Promise((resolve, reject) => {
      qiniu.rpc.postWithoutForm(
        reqUrl,
        digest,
        this.handleCallback(resolve, reject)
      );
    });
  }

  /**
   * 处理回调函数封装（使用高阶函数）
   * @param {*} resolve
   * @param {*} reject
   */
  handleCallback(resolve, reject) {
    return (respErr, respBody, respInfo) => {
      if (respErr) {
        reject(respErr);
      }
      if (respInfo.statusCode == 200) {
        resolve(respBody);
      } else {
        reject(respBody);
      }
    };
  }

  /**
   * 覆盖上传文件
   * @param {string} filePath 文件绝对路径
   */
  uploadFile(filePath) {
    return new Promise((resolve, reject) => {
      //根据文件路径获取文件名
      const fileName = path.basename(filePath);

      //动态生成覆盖上传凭证
      const options = {
        scope: `${this.bucketname}:${fileName}`,
      };
      const putPolicy = new qiniu.rs.PutPolicy(options);
      const uploadToken = putPolicy.uploadToken(this.mac);

      //执行上传
      const formUploader = new qiniu.form_up.FormUploader(this.config);
      const putExtra = new qiniu.form_up.PutExtra();
      formUploader.putFile(
        uploadToken,
        fileName,
        filePath,
        putExtra,
        this.handleCallback(resolve, reject)
      );
    });
  }

  /**
   * 生成下载链接
   * @param {string} key 要下载的文件名
   */
  generateDownloadLink(key) {
    return new Promise((resolve, reject) => {
      if (this.domain) {
        resolve(this.bucketManager.publicDownloadUrl(this.domain, key));
      } else {
        this.getBucketDomain()
          .then((domain) => {
            if (domain) {
              //获取到了域名
              this.domain = `http://${domain}`;
              resolve(this.bucketManager.publicDownloadUrl(this.domain, key));
            } else {
              reject("获取当前桶域名失败，请确认是否过期");
            }
          })
          .catch(reject);
      }
    });
  }

  /**
   * 下载文件到本地
   * @param {string} key 要下载的文件名
   * @param {string} savePath 保存路径(不包含文件名)
   */
  downloadFile(key, savePath) {
    return new Promise((resolve, reject) => {
      //1、生成下载地址
      this.generateDownloadLink(key)
        .then((url) => {
          //2、使用axios发送可读流
          const timeStamp = new Date().getTime();
          //加时间戳避免缓存
          const link = `${url}?timestamp=${timeStamp}`;
          return axios({
            url: link,
            method: "GET",
            //设置返回类型为stream
            responseType: "stream",
            //避免缓存
            headers: { "Cache-Control": "no-cache" },
          });
        })
        .then((response) => {
          //路径兼容性处理
          savePath = savePath.trimEnd("/").trimEnd("\\") + "\\" + key;
          const writer = fs.createWriteStream(savePath);
          //写入本地文件
          response.data.pipe(writer);
          //完成resolve,失败reject
          writer.on("finish", () => {
            resolve(savePath);
          });
          writer.on("error", reject);
        })
        .catch(reject);
    });
  }

  /**
   * 文件重命名
   * @param {string} originName 原始文件名
   * @param {string} currentName 目标文件名
   */
  renameFile(originName, currentName) {
    return new Promise((resolve, reject) => {
      this.bucketManager.move(
        this.bucketname,
        originName,
        this.bucketname,
        currentName,
        { force: true },
        this.handleCallback(resolve, reject)
      );
    });
  }

  /**
   * 删除文件
   * @param {string} key 文件名
   */
  deleteFile(key) {
    return new Promise((resolve, reject) => {
      this.bucketManager.delete(
        this.bucketname,
        key,
        this.handleCallback(resolve, reject)
      );
    });
  }
}

module.exports = {
  QiniuManager,
};
```

使用

```js
const path = require("path");
const { QiniuManager } = require("./qiniuManager");

const manager = new QiniuManager(
  "C6UvaUyTA4T43a4CeZFcUM-gOmE5bQlbbMbEvcep",
  "kDgqCo8wYzn9O5uoebZe7t-lT6M-yec-IY6hjjCM",
  "md-editor"
);
manager
  .downloadFile("test.md2", __dirname)
  .then((v) => {
    console.log(v);
  })
  .catch((e) => {
    console.error("下载该文件失败");
  });
// manager
//   .generateDownloadLink("test.md")
//   .then((v) => {
//     console.log(v);
//   })
//   .catch((err) => {
//     console.error(err);
//   });
// manager.getBucketDomain().then(
//   (v) => {
//     console.log(v);
//   },
//   (err) => {
//     console.error(err);
//   }
// );
// manager.uploadFile(path.resolve(__dirname, "test.md")).then(
//   (v) => {
//     console.log(v);
//   },
//   (err) => {
//     console.error(err);
//   }
// );

```



##### 流Stream

1、类似管道，边读边写，不会占用太多内存，流有事件，可以监听都或者写的生命周期

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210503124024362.png" alt="image-20210503124024362" style="zoom: 67%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210503124117380.png" alt="image-20210503124117380" style="zoom: 50%;" />

![image-20210503124234355](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210503124234355.png)

使用axios进行下载文件，因为它支持nodejs

* 七牛云上传文件不刷新问题解决，在cdn中刷新该文件即可

![image-20210504001924800](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210504001924800.png)

![image-20210504002757394](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210504002757394.png)

#### 自动同步和全部同步到云端

* 保存时自动同步到七牛云
* 打开文件时判断本地的updateTime与七牛云的putTime(上传时间)，如果putTime>updateTime代表服务器的文件比较新，需要将该文件下载到本地并且更新到store中
* 删除、重命名时都需要对七牛云进行同步：注意操作时要先对七牛云文件进行操作，然后才修改本地的updateTime，确保本地updateTime大于putTime，这样可以对比文件版本，因为用户如果通过其他方式对七牛云的文件进行了修改，那么putTime就会更新（变大）
* 上传或者下载比较慢，可以使用loading

### 打包electron

1、安装electron-builder `npm install electron-builder -D`

2、首先对react工程build `npm run build`

打包出一个静态资源文件夹

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210506191848133.png" alt="image-20210506191848133" style="zoom:50%;" />

3、更新main.ts中生产环境的加载地址

```javascript
  const urlLocation = isDev
    ? "http://localhost:3000"
    : `file:${path.join(__dirname, "./build/index.html")}`;
  mainWindow.loadURL(urlLocation);
```

4、在package.json中配置electron-builder打包配置

```json
{
  "name": "md-online-editor",
  "main": "main.js",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@electron/remote": "^1.1.0",
    "@fortawesome/fontawesome-svg-core": "^1.2.35",
    "@fortawesome/free-brands-svg-icons": "^5.15.3",
    "@fortawesome/free-regular-svg-icons": "^5.15.3",
    "@fortawesome/free-solid-svg-icons": "^5.15.3",
    "@fortawesome/react-fontawesome": "^0.1.14",
    "@testing-library/jest-dom": "^5.11.4",
    "@testing-library/react": "^11.1.0",
    "@testing-library/user-event": "^12.1.10",
    "@types/jest": "^26.0.15",
    "@types/react": "^17.0.0",
    "@types/react-dom": "^17.0.0",
    "axios": "^0.21.1",
    "bootstrap": "^4.6.0",
    "classnames": "^2.3.1",
    "concurrently": "^6.0.2",
    "electron-is-dev": "^2.0.0",
    "electron-store": "^4.0.0",
    "qiniu": "^7.3.2",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "4.0.3",
    "react-simplemde-editor": "^4.1.5",
    "styled-components": "^5.2.3",
    "typescript": "^4.1.2",
    "uuid": "^8.3.2",
    "web-vitals": "^1.0.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "dev": "concurrently  \"wait-on http://localhost:3000 && tsc main.ts && electron . --inspect\" \"cross-env BROWSER=none npm start\"",
    "prepack": "npm run build",
    "pack": "electron-builder --dir",
    "dist": "electron-builder",
    "predist": "npm run build"
  },
  "description": "Online MarkDown Editor Using React + Electron + Qiniu Cloud Service",
  "author": {
    "name": "Hexi 1997",
    "email": "18715156450@163.com"
  },
  "repository": {
    "url": "https://github.com/Hexi1997/doc-editor.git"
  },
  "homepage": "./",
  "build": {
    "appId": "mdOnlineEditor",
    "productName": "在线MarkDown编辑器",
    "copyright": "Copyright © year ${author}",
    "extends": null,
    "files":[
      "build/**/*",
      "node_modules/**/*",
      "nodejs/**/*",
      "package.json",
      "main.js",
      "menuTemplate.js",
      "assets/**/*"
    ],
    "directories":{
      "buildResources":"assets"
    },
    "mac":{
      "category":"public.app-category.productivity",
      "artifactName":"${productName}-${version}-${arch}.${ext}"
    },
    "dmg":{
      "background":"assets/appdmg.png",
      "icon":"assets/icon.icns",
      "iconSize":100,
      "contents":[
        {
          "x":380,
          "y":280,
          "type":"link",
          "path":"/Applications"
        },
        {
          "x":110,
          "y":280,
          "type":"file"
        }
      ],
      "window":{
        "width":500,
        "height":500
      }
    },
    "win":{
      "target":[
        "msi","nsis"
      ],
      "icon":"assets/icon.ico",
      "artifactName":"${productName}-Web-SetUp-${version}.${ext}",
      "publisherName":"Hexi 1997"
    },
    "nsis":{
      "allowToChangeInstallationDirectory":true,
      "oneClick":false,
      "perMachine":false
    }
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest",
      "prettier"
    ]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  },
  "lint-staged": {
    "*.{js,css,md,ts,tsx}": "prettier --write"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "devDependencies": {
    "@commitlint/cli": "^12.1.1",
    "@commitlint/config-conventional": "^12.1.1",
    "@types/node": "^12.20.10",
    "@types/styled-components": "^5.1.9",
    "@types/uuid": "^8.3.0",
    "cross-env": "^7.0.3",
    "devtron": "^1.4.0",
    "electron": "^5.0.6",
    "electron-builder": "^22.11.1",
    "eslint-config-prettier": "^8.2.0",
    "husky": "^4.2.3",
    "lint-staged": "^10.0.8",
    "prettier": "2.2.1",
    "wait-on": "^5.3.0"
  }
}

```

* 问题1：无法下载winCodeSign https://www.cnblogs.com/savokiss/p/9129956.html

打包：执行npm run dist即可，打包结果如下

![image-20210506233337597](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210506233337597.png)

#### 打包体积优化

![image-20210506233555560](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210506233555560.png)

![image-20210507003928065](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210507003928065.png)

### 自动更新(不做了，没有服务器)

* token

![image-20210507133919825](https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210507133919825.png)

* 

### 支持excel文件

使用LuckySheet

需要安装moment和引入Jq cdn

excel文件导入导出使用Luckyexcel

### 打包静态资源

https://www.cnblogs.com/mrwh/p/12961446.html
