---
title: webpack深入学习
date: 2021-05-14 14:57:18
tags: [大前端,webpack]
categories: webpack
---
# webpack基础知识

## 定义

webpack是一种前端资源构建工具，一个静态模块打包器
在webpack看来，前端的所有资源文件js/json/css/img/less等都会作为模块处理。它将根据模块的依赖关系进行
静态分析，打包生成对应的静态资源bundle

引入js资源
引入样式资源
引入图片、字体等其他资源
一般是在一个文件中集中引入所需资源

在项目路径下执行 npm init，初始化项目生成package.json,,初始化之后才能安装webpack包，因为配置信息存储在package.json中 

## 打包图片

![webpack文件流向分析](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514145911.jpg)

![webpack打包流程](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514145920.JPG)

## Webpack五个核心概念

### Entry 

入口指示 Webpack以哪个文件为入口起点开始打包，分析构建内部依赖图

### Output 

输出指示 Webpack打包后的资源bundiles输出到哪里去，以及如何命名

### Loader 

让webpack能够去处理那些非javascript文件（webpack自身只理解js/json)

### Plugins 

可以用于执行范围更广的任务，插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量

### loader

只能翻译，不能压缩。Plugins可以压缩

### Mode(模式)

指示webpack使用相应模式的配置

* development 能让代码本地调试运行的环境
* production 能让代码优化上线运行的环境
* production对兼容性、效率有更高要求



## 树结构

在一个入口文件引入所有资源文件，形成所有依赖关系柱状图

## 模块

所有的资源都是模块
chunk:打包过程中被操作的模块文件叫做chunk,例如异步加载一个模块就是一个chunk
bundle:bundle是最终打包后的文件，最终文件可以和chunk一模一样，但大多数情况下是chunk的集合
为了优化最后生产出的bundle数量可能不等于chunk数量，因为可能多个chunk被组合到一个Bundle中

## 新建项目配置webpack

### 初始化项目

```shell
//初始化
cnpm init
//先全局安装,全局安装以后才可以通过npm指令调用。全局安装如果没有指定版本会自动获取最新的版本并覆盖老版本
cnpm i webpack@4.42.0 webpack-cli@4.2.0 -g  
//将webpack webpack-cli添加到开发依赖
cnpm i webpack@4.42.0 webpack-cli@4.2.0 -D
//打包
webpack --mode=development //package.json默认入口文件是src路径下的index.js(不用画蛇添足，添加src路径）。默认输出路径是dist/main.js
//以development模式的话输出的main.js中代码结构清晰，含注释，多行
//以production模式输出的main.js代码一行，不含注释
```

### 配置webpack

* 在工程目录下新建webpack.config.js，配置webpack
  cnpm i -D html-webpack-plugin@4.5.0 //引入插件打包html资源
  html-webpack-plugin插件可以帮忙正确引用打包编译生成的资源
  JS代码只需要设置成生产模式(production)，会自动压缩。压缩JS方法

* css-loader的作用是处理css中@import和url这样的外部资源
* style-loader的作用是把样式插入到DOM中，方法是在head中插入一个style标签，并且把样式写入这个标签的innerHHTML里
  cnpm i -D css-loader style-loader

* 安装less  cnpm i -D less less-loader

* 安装sass cnpm i -D node-sass sass-loader  。 sass文件的后缀名是scss，注意webpack.config.js中的配置

* 因为css打包后是包含在js文件中的，很抽象，如果想让他独立成css文件，需要安装 mini-css-extract-plugin插件 cnpm i -D mini-css-extract-plugin@0.9.0，然后在webpack.config.js中进行配置
  1、const引入
  2、将loader中的style-loader全部换成MiniCssExtractPlugin.loader
  3、plugins中new一个MiniExtractPlugin对象配置



* loader:下载 使用(配置)
* plugins:下载 引入(request) 使用

* 打包图片资源
  //url-loader和file-loader只能引入css/less/scss中的图片,url依赖file-loader
  cnpm i -D url-loader file-loader
  //html-loader处理html文件的img图片（负责引入图片从而能被url-loader打包处理）
  cnpm i -D html-loader 

* url-loader和html-loader不会重复打包同一图片

* 打包其他资源（不需要优化、压缩，不会变化的文件）
  例如字体、图标、音频
  打包其他资源使用file-loader，在配置module中写exclude排除其他资源

​       {
​            //处理其他资源。类似tiff
​            //排除css\js\html\less\scss\jpg\png\gif文件
​            exclude: /\.(css|js|html|less|scss|jpg|png|gif)$/,
​            loader: 'file-loader'
​        }

Live loading实现
使用webpack-dev-server实现，用来自动编译、自动打开浏览器、自动刷新浏览器
特点：只会在内存中打包，不会有任何输出（不会覆盖到输出目录）
启动devServer指令为:npx webpack-dev-server(或者webpack-dev-server)
如果全局安装了webpack则可以省略npx
cnpm i -D webpack-dev-server
其实使用webpack --watch指令搭配vscode中live server插件也可以实现live loading

如果使用mini-css-extract-plugin插件，那么可能会有css 图片url的路径问题，需要设置mini-css-extract-plugin的publicPath属性为../

css兼容性处理需要postcss-loader和postcss-preset-env插件
js兼容性处理需要用babel

cnpm i -D postcss-loader postcss-preset-env
//css兼容性处理
                    //帮postcss找到package.json中的browserslist配置，通过配置加载指定的css兼容样式
                    //使用loader的默认配置
                    //'postcss-loader'
                    //修改loader的配置

    "browserslist": {
        "development": [
            "last 1 chrome version",
            "last 1 firefox version",
            "last 1 safari version"
        ],
        "production": [
            ">0.1%",
            "not dead",
            "not op_mini all"
        ]
    }

//指定环境处理css兼容性(postcss根据环境访问package.json中对应browserslist中的环境属性，确定要如何兼容)
process.env.NODE_ENV = 'development';//或者production,指定css兼容性的环境，默认是production



压缩css代码,将css代码压缩成一行
cnpm i -D optimize-css-assets-webpack-plugin

js语法检查eslint，可以统一代码风格
语法检查：eslint-loader eslint
eslint的检查规则设置在package.json中eslintConfig中设置

eslint不认识window、navigator等全局变量
解决：增加env属性，设置值为{"browser":true}

推荐使用airbnb规则
要使用airbnb规则，需要安装 eslint-config-airbnb-base eslint eslint-plugin-import
需要设置exclude排除第三方库，只检查源代码
cnpm i eslint-loader eslint eslint-config-airbnb-base eslint-plugin-import -D

下一行所有eslint规则失效（下一行不进行eslint检查)
在代码的上一行添加注释// eslint-disable-next-line，那么当前代码行不进行eslint检查

webpack 打包生成新的文件时 自动 删除上次打包出来的文件
cnpm install clean-webpack-plugin -D

js兼容性处理
第一种解决方案 babel-loader  可以处理简单的语法转换，例如箭头函数，const let数据类型,Promise就不可以转换
cnpm i babel-loader @babel/preset-env @babel/core -D
babel-loader最新版本默认转换成es6语法，要配置options降阶

第二种解决方案是全部js兼容性处理 @babel/polyfill
cnpm i -D @babel/polyfill
使用：直接在入口文件index.js最上面 import '@babel/polyfill'
问题：文件大、污染代码、只要解决部分兼容性问题，但是将所有兼容性代码全部引入

第三种解决方案是按需加载 core-js
cnpm i -D  core-js
使用第三种就不能使用第二种方案，现在流行的是第1和第3种方案结合起来使用

正常来讲，一个文件只能被一个loader处理
当一个文件要被多个loader处理，一定要指定loader执行的先后顺序
例如js文件要先执行eslint进行语法检查，然后进行兼容性转换babel
给loader的enforce属性设置为'pre'即可让当前loader优先执行

# webback性能优化

* 开发环境性能优化
* 生产环境性能优化

## 开发环境性能优化

* 优化打包构建速度（代码编译能够更快看到结果），使用HMR实现(js/css/html热模块替换)
* 优化代码调试（能够指定源代码出错的位置），使用sourcemap技术实现（开发：eval-sorce-map  生产：source map）

## 生成环境性能优化

* 优化打包构建速度
  oneOf:减少文件正则匹配 oneOf中loader只匹配一个，找到匹配就停止
  babel缓存（优化打包构建速度）
  多进程打包：thread-loader，大项目需要，小项目考虑一下值得不

* 优化代码运行的性能
  文件缓存（hash-chunkhash-contenthash)
      强制缓存期间无法重新请求同一文件，使用contenthash修改文件名，文件内容一旦变化，contenthash也就变化，就会重新请求该文件。
  tree-shaking:树摇 ES6模块加production开启后压缩js，有可能会删除css文件，通过    sideEffects排除css文件

  code-split:代码分割
      1、单入口和多入口配置
      2、import动态引入
      3、设置optimization属性让node_modules打包为一个chunk

  externals技术：例如jquery，设置cdn映射

  dll技术：打包第三方库

  懒加载：点击时才import加载
  预加载：偷偷加载需要懒加载的js文件 兼容性差
  pwa技术：离线访问技术

## HMR(hot module replacement) 热模块替换/模块热替换

* webpack-dev-server如果没有开启HMR，那么一旦某个文件修改，全部文件都要重新编译，浏览器刷新，大型项目中运行效率低，开发效率低，因此要开启HMR
* HMR中一个模块发生变化，只会重新打包这一个模块（而不是打包所有模块），极大提升了构建速度
* devServer配置中hot属性值设置为true即可开启热模块替换(HMR)
* 修改webpack配置，一定要重启webpack服务，配置才会生效
* HMR默认只能实现less、css、scss样式文件的热模块替换，是因为style-loader实现了HMR功能，开发环境中一般用style-loader将样式内联嵌入到html中，方便样式热模块替换。生成环境中一般需要用到mini-css-extract-plugin插件提取css为单独文件。开发环境中不用mini-css-extract-plugin，它也不支持HMR功能
* js默认没有HMR功能
  解决办法：在js的入口文件（index.js中）添加if(module.hot)判断
  入口js（index.js)无法实现HMR
* html默认没有HMR功能，同时开始HMR会导致html不能热更新 解决办法：修改entry入口，将html文件引入 html不需要实现HMR功能，现在流行的都是单页面，通过js生成虚拟dom挂到html上

## source-map一种提供源代码到构建后代码映射技术

* 如果构建后代码出错了，根据映射可以追踪到源代码错误
* 在webpack.config.js中给module.export对象添加devtool属性，值为'source-map'即可
* source-map有很多种类型 [inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map

* source-map 外部 
  错误代码准确信息 和 源代码错误位置
* inline-source-map  内联--只生成一个内联source-
  错误代码准确信息 和 源代码错误位置
* hidden-source-map  外部 ，一般用于生产环境，只隐藏源代码，不隐藏构建后代码
  错误代码错误原因，但是没有错误位置，不能追踪源代码错误位置，只能提示到构建后错误代码位置
* eval-source-map  内联--每一个文件都生成对应的source-map，都在eval函数中
  错误代码准确信息 和 源代码错误位置
* nosources-source-map 外部  ,一般用于生产环境，全部隐藏代码
  错误代码准确信息 ，但是没有任何源代码信息
* cheap-source-map 外部 
  错误代码准确信息 和 源代码错误位置，只能精确到代码行
* cheap-module-source-map 外部
  错误代码准确信息 和 源代码错误位置
  module会将loader单独source-map加入

* 内联和外部的区别：外部生成单独sourcemap文件，内联效率高

开发环境：速度快、调试更友好
    速度快（eval>inline>cheap>...)
        eval-cheap-source-map (速度第一快)
        eval-source-map(速度第二快)
    调试更友好
        source-map(最友好)
        cheap-module-source-map（第二友好）
        cheap-source-map（第三友好）
结论：开发环境用eval-source-map / eval-cheap-module-source-map

生成环境：源代码要不要隐藏？ 调试要不要更友好？
    内联会让代码体积变大，生产环境不用内联
结论：生产环境用 source-map / cheap-module-source-map

## oneOf

* oneOf：以下loader只会匹配一个，减少文件匹配时间

## 缓存

* babel缓存
  代码中js代码最多，babel对js进行编译（兼容性）处理
  生产环境中没法用webpack-dev-sever实现HMR，因此用babel缓存实现
  babel-loader中设置cacheDirectory为true即可
  开启babel缓存后，第二次构建时会读取第一次的缓存，构建速度更快些
* 文件资源缓存
  hash:每次webpack构建时都会生成一个唯一hash值
      问题：因为js和css同时使用一个hash值
      如果重新打包，会导致所有缓存失效（可我却只能改动一个文件）
  chunkhash:根据chunk生成的hash值，如果打包来源于同一个chunk，那么hash值就一样
      问题：js和css hash值还是一样的，因为css是在js中被引入，同属于一个hash
  contenthash:根据文件内容生成hash值，不同文件hash值一定不一样
      让代码上线运行缓存更好使用

## tree shaking 去除没有使用的代码

* 针对js的tree shaking : 1、js必须使用ES6模块化 2、开启production环境
  作用：减少代码体积
  在package.json中配置
      "sideEffects":false //所有代码都没有副作用（都可以进行tree shaking)
      问题：可能会把css @babel/polyfill(副作用)文件干掉，不会输出
      "sideEffects":["*.css","*.less","*.scss"] //css文件不做tree shaking

## code split 代码分割

    第一种方案：单入口和多入口设置


​    
​    第二种方案：设置 optimization: {
​                        splitChunks: {
​                            chunks: 'all'
​                        }
​                    }
​        可以将node_modules中代码单独打包成一个chunk最终输出
​        自动分析多入口chunk中，有没有公共的文件，如果有会打包成单独的一个chunk，不会打包多个
​    

    第三种方案：import动态导入，能将js文件单独打包
    index.js中// import { n, demo } from './one'; 使用这种模块方法会导致打包时one.js和index.js在同一个包
    //如果想让one.js单独打包，则通过动态加载，魔法注释设置one.js打包输出的文件名为one.xxxx.js
    import ( /* webpackChunkName: 'one' */ './one').then(({ demo }) => {
        // eslint-disable-next-line
        console.log(demo("动态加载"));
    }).catch(() => {
        // eslint-disable-next-line
        console.log("文件加载失败");
    })

## 懒加载和预加载

    document.getElementById('btn').onclick = () => {
        //懒加载：当文件需要使用时才加载，即动态加载
        //预加载prefetch: 会在使用之前提前加载js文件
        //正常加载：并发加载js（同一时间加载多个js)
        //预加载prefetch: 等其他资源文件加载完毕，浏览器空闲，偷偷加载资源
        //预加载虽然非常好用，但是在IE、移动端兼容性非常差，慎用，懒加载兼容性较好，可以用
        import ( /* webpackChunkName:'lazy', webpackPrefetch:true */ './lazy').then(({ add2 }) => {
            console.log(add2(2, 3));
        })
    }

## PWA 渐进式网络开发应用程序（网站离线访问）

* workbox cnpm i -D workbox-webpack-plugin
  index.js中
  /* 
  注册serviceworker实现离线访问
  处理兼容性问题
  */
  if ('serviceworker' in navigator) {
    window.addEventListener('load', () => {
        navigator.serviceWorker.register('/service-worker.js').then(() => {
            console.log('sw注册成功')
        }).catch(() => {
            console.log('sw注册失败');
        })
    })
  }

webpack.config.js中写plugin配置

sw代码必须运行在服务器上
    --nodejs方法（不会，推荐用serve)
    --cnpm i -g serve 
    --serve -s build  //启动服务器，将build目录下所有资源作为静态资源暴漏出去

## 多进程打包

* 一般都只对babel-loader使用，因为js代码一般时较多的
* 多进程打包    进程启动时间大概为600ms，进程通信也有开销
* 只有工作消耗时间较大，项目较大，才需要多进程打包
* cnpm i -D thread-loader

## externals

* 防止将某些包输出到打包的bundles中，例如Jquery链接，希望从cdn中加载，cdn加载速度快
* webpack.config.js中配置externals: {
      //忽略库名 npm包名,拒绝Jquery被引入
      jquery: "jQuery"
  }
* 在index.html中加入<script src='https://cdn.bootcss.com/jquery/1.10.2/jquery.min.js'></script>，否则jqury失效

## dll技术打包node_modules

* node_modules一般不会变化，打包一次，后面就不用再打包。方便
* 减少打包代码体积，提高打包效率,使用dll技术，对某些库（第三方库：jquery react vue...) 进行单独打包
* 新建webpack.dll.js文件
* 运行 webpack--config webpack.dll.js生成固定dll，方便引用
* webpack.config.js中设置插件
      //告诉webpack哪些库不参与打包
      //同时使用时名称要改变
      new webpack.DllReferencePlugin({
          manifest: resolve(__dirname, 'dll/manifest.json')
      })
* cnpm i -D add-asset-html-webpack-plugin
      //将文件打包输出，并在html中自动引入该资源
      new AddAssetHtmlWebpackPlugin({
          filepath: resolve(__dirname, 'dll/jquery.js')
      })

## webpack配置详解 --entry篇

* 单入口：打包形成一个chunk，输出一个bundle文件，此时chunk默认输出名称是main
* array: ['./src/1.js','./src/2.js']
  数组单入口：数组中所有入口文件最终只会形成一个chunk，输出出去只有一个bundle文件
  一般只用在解决HMR导致html更新失效问题
* object: 多入口，有几个入口，就形成几个chunk,输出几个bundle
  特殊用法
      {
          //1.js和2.js形成一个chunk，输出一个bundle，add.js形成一个chunk，输出一个bundle
          index:['./src/1.js','./src/2.js'],
          add:'./src/add.js'
      }

## webpack配置xiangjie --output篇

output:{
    //文件名称(指定名称+目录)
    filename:'js/[name].js',
    //输出文件目录（将来所有资源输出的公共目录）
    path:resolve(___dirname,'build'),
    //所有资源引入公共路径前缀 --> imgs/a.jpg --> /imgs/a.jpg
    publicPath:'/',
    chunkFileName:'js/[name]_chunk.js'  //非入口chunk的名称（动态import的库、optimization中将node_modudles打包的chunk)
    //library:'[name]',  //整个库向外暴漏的变量名
    //libraryTargets:'window' //暴漏的变量添加到浏览器window对象
    //libraryTargets:'global' //暴漏的变量添加到node
    //libraryTargets:'commonjs'
    //library一般是暴漏一个库使用，一般结合dll单独打包某个库使用
}

## module配置

module:{
    rules:[
        {
            test:/\.css/,
            //排除node_modules
            exclude:/node_modules/,
            //只检查指定文件夹
            include:resolve(__dirname,"src"),
            //enforce数值执行先后顺序pre>不写>post
            enforce:'pre',
            //单个loader
            //loadr:'ealint-loader',
            //options:{
                

            //}
            //多个loader
            use:['style-loader','css-loader']
        },
        //以下loader配置只会生效1个,提高文件匹配效率
        oneOf:[
            {
                test:/\.js/,
                loader:''
            }
        ]
        
    ]

}

## module.exports的resolve属性

module.exports = {
    entry:'',
    output:'',
    modules:{},
    plugins:[],
    mode:'',
    resolve:{
        //解析模块的规则

        //配置解析模块路径别名:有点简写路径，缺点路径没有提示
        alias:{
            //指定一个全局变量$css指向src/css文件夹
            //这样在js文件中可以用$css来代替文件夹路径
            //这样是因为有可能文件层级目录多，写起来麻烦
            $css:resolve(__dirname,'src/css')
        },
        //配置省略文件后缀名
        //如果你没写后缀名，会有限匹配js>json>css
        //因此如果js和css文件同名，可能会匹配错误
        //不推荐使用
        extensions:['.js','.json','.css'],
        //告诉webpack解析模块去哪个目录去找
        //使用resolve绝对路径，减少寻找次数
        modules:[resolve(__dirname,'../../node_modules'),'node_modules]
    }

}

## devServer详细配置

devServer:{
    //运行代码的目录
    contentBase:resolve(__dirname,'build'),
    //监视contentBase目录，一旦变化就会reload
    watchContentBase:true,
    watchOptions:{
        //忽略文件目录
        ignored:/node_modules/
    }
    //启动gzip压缩
    compress:true,
    //端口号
    port:5000,
    //域名
    host:"localhost",
    //自动打开浏览器
    open:true,
    //开启HMR功能
    hot:true,
    //不要启动服务器日志记录
    clientLogLevel:'none',
    //除了基本启动信息外，其他内容都不显示
    quiet:true,
    //如果出现错误，不要全屏提示
    overlay:false,
    //服务器代理--解决开发环境跨域问题
    proxy:{
        //一旦devServer（5000）接收到/api/xxx的请求，就会把请求转发到另一个服务器（3000）
        '/api':{
            target:'http://localhost:3000',
            //发送请求时，请求路径重写：将 /api/xxx --> /xxx (去掉/api)
            pathRewrite:{
                '^/api':''
            }
        }
    }
}

## optimization配置详解

* optimization一般在生产环境中配置
  optimization:{
    //存在一个问题：当index.js通过import动态加载one.js的时候，编译后index.js存储了one.js的contenthash导致，one.js文件变化时，contentHash变化导致index.js的contenthash变化，这样就使得index.js的缓存失效
    //因此将contentHash配置信息独立提取出来一个Js文件
    //很重要，生产环境中一定要配置
    runtimeChunk:{
        name:entrypoint => `runtime-${entrypoint.name}`
    },

    minimizer:[
        //配置生产环境压缩方案：js和css
        new TerserWebpackPlugin({
            //开启缓存
            cache:true,
            //开启多进程打包
            parallel:true,
            //启用source-map
            sourceMap:true
        })
    ]

    splitChunks:{
        chunks:'all',
        //以下所有配置全是默认值，一般不做修改
        minSize: 30 * 1024, //分割的chunk最小为30kb
        maxSize:0,   //最大没有限制
        minChunks:1,  //要提取的chunk最小被引用1次
        maxAsyncRequests:5, //按需加载时并行加载的文件最大数量
        maxInitialRequests: 3,  //入口js文件最大并行请求数量
        automaticNameDelimiter: '~',  //名称连结符
        name:true,   //可以使用命名规则
        cacheGroups:{
            //分割chunk的组
            vendors:{
                //node_modules文件会被打包到vendors组的chunk中 --》 vendors~xxx.js
                //满足上面的公共规则，如：大小至少30kb，至少被引用1次
                test:/[\\/]node_modules[\\/]/,
                priority:-10,
            },
            default:{
                //要提取的chunk至少被引用2次
                minChunks:2,
                //优先级
                priority:-20,
                //如果当前要打包的模块和之前已经被提取的模块是同一个，会复用，而不是重新打包
                reuseExistingChunk:true
            }
        }
    }
  }