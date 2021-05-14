---
title: react-native-tanhua
date: 2021-05-11 19:58:46
tags: [大前端,react,react-native]
categories: 实战项目笔记
---

为方便使用：react-native可简称为rn

## 一、环境搭建

参考：https://blog.csdn.net/weixin_43090018/article/details/114359937

安装过程中遇到下面几个问题

* npm设置淘宝镜像加快速度
* 安装yarn `npm install yarn -g`
* android studio加载速度慢，设置build.gradle为阿里镜像

```
buildscript{

    repositories{
        maven{ url'https://maven.aliyun.com/repository/gradle-plugin' }
        maven{ url'https://maven.aliyun.com/repository/google' }
        maven{ url'http://maven.aliyun.com/nexus/content/groups/public/' }
        maven{ url'https://maven.aliyun.com/repository/jcenter'}
    }

    dependencies{
        classpath"com.android.tools.build:gradle:4.0.1"
    }
}

allprojects{
    repositories{
        maven{ url'https://maven.aliyun.com/repository/gradle-plugin' }
        maven{ url'https://maven.aliyun.com/repository/google' }
        maven{ url'http://maven.aliyun.com/nexus/content/groups/public/' }
        maven{ url'https://maven.aliyun.com/repository/jcenter'}
    }
}
```

```shell
npm i -g create-react-native-app   //全局安装脚手架
npx create-react-native-app helloRN //使用脚手架初始化项目
npx react-native run-android  //运行app
```

* 如果运行app时卡在 kotlin-compiler-embeddable-1.3.50 下载，按照 https://blog.csdn.net/qq_40067488/article/details/104896201 中的步骤去maven上下载然后放到指定目录即可，这个文件35M，下载很慢，自己去maven上下载。

* 如果使用ndk23版本，会报错No toolchains found in the NDK toolchains folder for ABI with prefix: arm-linux-androideabi ，去https://developer.android.google.cn/ndk/downloads/ 下载22版本的替换到ndk和ndk-bundle目录即可

* 报如下错是因为没有安装Android sdk 29，在android studio中安装一遍即可

  ```
  * What went wrong:
  A problem occurred configuring project ':react-native-reanimated'.
  > Failed to install the following Android SDK packages as some licences have not been accepted.
       build-tools;29.0.2 Android SDK Build-Tools 29.0.2
       platforms;android-29 Android SDK Platform 29
    To build this project, accept the SDK license agreements and install the missing components using the Android Studio SDK Manager.
    Alternatively, to transfer the license agreements from one workstation to another, see http://d.android.com/r/studio-ui/export-licenses.html
  
    Using Android SDK: C:\Users\HASEE\AppData\Local\Android\Sdk
  
  * Try:
  Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.       
  
  * Get more help at https://help.gradle.org
  
  BUILD FAILED in 29s
  
  error Failed to install the app. Please accept all necessary Android SDK licenses using Android SDK Manager: "$ANDROID_HOME/tools/bin/sdkmanager --licenses". Run CLI with --verbose flag for more details.
  Error: Command failed: gradlew.bat app:installDebug -PreactNativeDevServerPort=8081
  ```

### 在模拟器中运行

安装完android studio以后，会自动新建一个模拟器。执行`npx react-native run-android`命令会直接打开模拟器运行。无需开启android studio

### 在安卓真机中运行

1、准备一台安卓手机，通过数据线连接到电脑，设置启用usb调试（需要首先打开开发者选项）

2、下载安卓投影工具https://gitee.com/Barryda/QtScrcpy，设置窗口置顶和保持唤醒。

模拟器或多或少有些坑，这里采用真机运行

### 截图工具使用

### 插件

#### Simple React Snippets

* 生成react代码的插件

#### Prettier - Code formatter

* 支持jsx代码格式化

### react-native是什么

使用js和react编写原生移动应用



## 二、基础知识

### JSX

* React中写组件的代码格式，全称是Javascript XML

```jsx
<View>
	<Text>JSX</Text>
</View>
```

* JS和标签代码混合在一起



### RN在java中打印输出

管理员打开cmd，输入monitor调用android studio日志窗口可查看java错误和输出，当安装react-native组件报错又找不到原因时，可以用这个看看

https://blog.csdn.net/qq_34801506/article/details/80704864

使用Log.d



### 对象的浅拷贝和深拷贝

Object.key(obj1)可以获得obj1的键集合

Object.values(obj1)可以获得obj1的值集合

array.map方法必须要有return，或者也可以使用简写形式return

array.filter，只要写筛选条件即可 `arr.filter(value=>value.index>1)`

array.splice(index,num,new)  在原来数组的基础上删除并替换，返回值是被删除的值组成的新数组，index是开始删除的所以，num是删除的数量，new如果有值，则将该值放入被删除的位置，如果没有值，代表仅删除

array.pop()方法用于删除并返回数组的最后一个元素，也是在原数组上进行操作

array.shift()方法用于删除并返回数组的第一个元素，也是在原数组上进行操作

array.unshift()方法用于向数组开头添加一个或多个元素，并返回新的长度，也是在原数组上进行操作

array.slice(start,end) 前开后闭，end可以使用负数代表倒数第几个，返回选择后的新数组，原数组不会变化

参见知乎专栏：https://zhuanlan.zhihu.com/p/86859874

浅拷贝的意思就是只复制引用，没有复制真正的值。

Object.assign 和 解构赋值都只能深拷贝一层，如果某一属性值是对象类型，那么拷贝后和拷贝前的对象的该属性值指向同一地址

数组的slice和contact也只能深拷贝一层，如果某一索引的值是对象类型，那么拷贝前后该对象指向仍然是同一地址

深拷贝目前有三种方法：

* JSON.parse(JSON.string(obj)) , 适用于数据和对象，但是当对象中出现函数的时候，则无法正确拷贝

* 手写递归方法 , 优点是通用所有情况，确实是如果嵌套层级高，效率低

  ```js
  function deepCopy(obj) {
    var newobj = obj.constructor === Array ? [] : {};
    if (typeof obj !== 'object') {
      return obj;
    } else {
    for (var i in obj) {
      if (typeof obj[i] === 'object'){ //判断对象的这条属性是否为对象
        newobj[i] = deepCopy(obj[i]);  //若是对象进行嵌套调用
      }else{
          newobj[i] = obj[i];
          }
      }
      }
      return newobj; //返回深度克隆后的对象
  }
  
  var obj1 = {
      name: 'shen',
      show: function (argument) {
          console.log(1)
      }
  }
  var obj2 = deepCopy(obj1)
  ```

* 借助第三方库，如jq的$.extend方法或者lodash库

```js
//jq使用
$.extend( true, object1, object2 ); // 深度拷贝
$.extend( object1, object2 );  // 浅拷贝

//loadash使用
var objects = [{ 'a': 1 }, { 'b': 2 }];
var deep = _.cloneDeep(objects);
console.log(deep[0] === objects[0]); // => false
```



### React Hooks

#### useRef

https://blog.csdn.net/hjc256/article/details/102587037

使用时，通过ref.current获取指向的对象

```js
import React, { useState, useEffect, useMemo, useRef } from 'react';

export default function App(props){
  const [count, setCount] = useState(0);

  const doubleCount = useMemo(() => {
    return 2 * count;
  }, [count]);

  const couterRef = useRef();

  useEffect(() => {
    document.title = `The value is ${count}`;
    console.log(couterRef.current);
  }, [count]);
  
  return (
    <>
      <button ref={couterRef} onClick={() => {setCount(count + 1)}}>Count: {count}, double: {doubleCount}</button>
    </>
  );
}
```

* 函数组件和类组件中变量的定义
  * 容易出现bug的地方是在路由退出和重新进入页面，
    * 类组件离开这个页面即卸载这个组件，进入这个页面会重新按照constructor componentWillMout render componentDidMount顺序执行（不一定）
    * 函数组件也是一样，一个离开这个页面，再重新进入useState,useRef,useEffect都会重新执行，导致数据初始化（不一定）
  * state中：不推荐，不符合state的定义，state只是给和页面刷新有关的数据定义的
  * 类组件中
    * this对象上挂载
      * constroctor方法时在this上挂载
      * 直接在类组件中data = '1111'，等价于this.data = '111'  不推荐，不知道render以后会不会重新赋值（需要测试）
      * 在类组件外面定义并初始化 ---- 不推荐，路由退出重新进入会重新执行componentDidMount中的函数和this.state={}函数，数据会被初始化，但是保存索引的一些变量没有初始化还是原来的值，对不上。。。
  * 函数组件中
    * 在函数组件外面定义，但是在useEffect中模拟的componentDidMount函数中进行初始化，因为很有可能用户退出又重新进入这个界面，componentDidMount事件会触发，初始化变量的值，显示正确，否则有可能显示错误  --不推荐，麻烦
    * 在函数组件外面定义并赋值---不推荐，有可能会产生bug
    * 在函数组件中直接定义---不推荐，有bug，每次页面刷新都会重新初始化
    * 在const index = (a = 2) => {} 中进行定义，多个变量不方便
    * 使用useRef保存数据，相当于把ref当作类的this使用，推荐使用这个（还没尝试，下次试试--好用，不会有bug，退出重进就重新初始化）
* 总结：**类组件在constructor中this.xxx进行定义，函数组件使用useRef进行定义**



* 针对函数组件，可以把不变的对象在 const index = ({navigaiont,route:{params}})在props中进行解构

```js
React.useEffect(()=>{
    //模拟componentDidMount 组件挂载完毕
},[])

React.useEffect(() => {
    //模拟componentDidUpdate 组件更新从新render后
})

React.useEffect(()=>{
    return ()=>{
    //模拟componentWillUnMount 组件卸载
  }
},[])

React.useEffect(()=>{
    //模拟componentWillReceiveProps
    //当props.value或者props.data数值浅比较变化时，会触发这个
    //那么如何让props.value和props.data同时变化时，才触发呢
},[props.value,props.data])
```

* componentWillMount生命周期模拟
* 第一种方法，使用useRef 和 current属性

```js
function RenderLog(props) {
    console.log('Render log: ' + props.children);
    return (<>{props.children}</>);
}

function Component(props) {

    console.log('Body');
    const [count, setCount] = useState(0);
    const willMount = useRef(true);

    if (willMount.current) {
        console.log('First time load (it runs only once)');
        setCount(2);
        willMount.current = false;
    } else {
        console.log('Repeated load');
    }

    useEffect(() => {
        console.log('Component did mount (it runs only once)');
        return () => console.log('Component will unmount');
    }, []);

    useEffect(() => {
        console.log('Component did update');
    });

    useEffect(() => {
        console.log('Component will receive props');
    }, [count]);


    return (
        <>
        <h1>{count}</h1>
        <RenderLog>{count}</RenderLog>
        </>
    );
}
```

* 第二种方法：useMemo方法

```js
const Component = () => {
   useMemo(() => {  //放在代码顶部，优先执行
     // componentWillMount events
   },[]);
   useEffect(() => {
     // componentDidMount events
     return () => {
       // componentWillUnmount events
     }
   }, []);
};
```

* useEffect中如何使用async和await https://my.oschina.net/u/4592353/blog/4692403



* useState不做状态合并，只做更新
* useReducer

### RN样式

* flex布局

  * rn中不存在div标签,View标签可替代,View标签可嵌套

  * 文本**必须**被Text标签包裹

  * rn中默认容器布局方式是flex，flex-direction默认是column

  * rn中样式没有继承

  * 样式使用 `style={{backgroundColor:"white",color:"black"}}`可以表示，样式是小驼峰命名法，和react一致

  * 单位

    * 不用加px，只写数字
    * vw vh单位也无法使用
    * 可以加百分比 `width:"50%"`

  * 获取屏幕宽度和高度

    ```js
    import {Dimensions} from 'react-native'
    const screenWidth = Math.round(Dimensions.get('window').width)
    const screenHeight = Math.round(Dimensions.get('window').height)
    ```

  * css动画 `style={{transform:[{translateY:200},{scale:2}]}}`

### 标签：可以理解为rn自定义的ui组件

  * View

    * 类似div
    * 不支持设置字体大小、样式（样式不继承）
    * 不能直接放文本内容，需要用text包裹
    * 不支持直接绑定点击事件，一般使用TouchableOpacity来代替

* ScrollView

  * 可以设置refreshControl实现下拉刷新

  * Text：文本标签

    * 文本标签可以设置字体颜色、大小
    * 支持绑定点击事件
    * 不支持textOverFlow whiteSpace等属性，需要设置省略号可以通过 numberOfLines 和 ellipsizeModel来设置

  * TouchableOpacity：可以绑定点击事件的块级标签

    * 相当于块级的容器
    * 支持绑定点击事件onPress
    * 可以设置点击时的透明度

    `<TouchableOpacity activeOpacity={0.5} onPress={this.handlePress}></TouchableOpacity>`

  * Image：图片标签

    * `<Image source={require("./imgs/1.jpg")}/> `  使用相对路径引入本地图片，可以不加宽度和高度属性
    * `<Image style={{width:100,height:100}} source={{uri:'https://xxxxxxxxx.jpg'}}></Image>`  引入在线图片时必须添加width和height属性，否则无法正常显示
    * 默认情况下Android不支持GIF和WebP格式图片，需要在android/app/build.gradle文件中dependencies手动添加以下模块(注意：修改完android文件夹里的内容以后要重新编译才能看到效果)

    ```
        // 如果你需要支持GIF动图
        implementation 'com.facebook.fresco:animated-gif:2.0.0'
    
        // 如果你需要支持WebP格式，包括WebP动图
        implementation 'com.facebook.fresco:animated-webp:2.1.0'
        implementation 'com.facebook.fresco:webpsupport:2.0.0'
    
        // 如果只需要支持WebP格式而不需要动图
        implementation 'com.facebook.fresco:webpsupport:2.0.0'
    ```

  * ImageBackground：类似带背景的div，可以通过source属性设置背景

  ```jsx
        <ImageBackground source={require("./imgs/demo.png")} style={{width:100,height:100}}/>
  
  ```

  * TextInput:文本输入框
    * 默认没有边框，看不见，可以加borderWidth让其可见
    * TextInput有两个事件onChangeText，传递参数是输入框的值，onChange传递参数是对象

* FlatList:高效率的列表组件，范例如下

* FlatList分页加载基于onEndReachedThreshold属性和onEndReached事件来实现，FlatList不需要做节流，它本身自带节流

```js
import React from 'react';
import { SafeAreaView, View, FlatList, StyleSheet, Text, StatusBar } from 'react-native';

const DATA = [
  {
    id: 'bd7acbea-c1b1-46c2-aed5-3ad53abb28ba',
    title: 'First Item',
  },
  {
    id: '3ac68afc-c605-48d3-a4f8-fbd91aa97f63',
    title: 'Second Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Third Item',
  },
];

const Item = ({ title }) => {
  return (
    <View style={styles.item}>
      <Text style={styles.title}>{title}</Text>
    </View>
  );
}

const App = () => {
  const renderItem = ({ item }) => (
    <Item title={item.title} />
  );

  return (
    <SafeAreaView style={styles.container}>
      <FlatList
        data={DATA}
        renderItem={renderItem}
        keyExtractor={item => item.id}
      />
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    marginTop: StatusBar.currentHeight || 0,
  },
  item: {
    backgroundColor: '#f9c2ff',
    padding: 20,
    marginVertical: 8,
    marginHorizontal: 16,
  },
  title: {
    fontSize: 32,
  },
});

export default App;
```



* 综合

  ```js
  import React from 'react'
  import {Text,View,TouchableOpacity,Image,ImageBackground,TextInput} from 'react-native'
  import {Dimensions} from 'react-native'
  const screenWidth = Math.round(Dimensions.get('window').width)
  const screenHeight = Math.round(Dimensions.get('window').height)
  
  const Index = () =>{
    const handlePress = () => {
      alert('你真点啊2')
    }
  
    const handleChangeText = (value) => {
      alert(value)
    }
  
    return(
      <View style={{backgroundColor:"white",flexDirection:"column"}}>
        <Text style={{color:"black"}}></Text>
        <Text style={{color:"black"}}></Text>
        <Text style={{color:"black"}}>JAX</Text>
        <TouchableOpacity onPress={handlePress} activeOpacity={0.5} style={{borderWidth:2}}>
        <TextInput onChangeText={handleChangeText}  style={{borderWidth:2}}/>
          <Text>点我啊444</Text>
        </TouchableOpacity>
        <Image source={require("./imgs/demo.png")}></Image>
        <ImageBackground source={require("./imgs/demo.png")} style={{width:100,height:100}}/>
      </View>
  
    )
  }
  
  export default Index
  ```

  

### 语法

#### 插值表达式

object类型不能直接展示，array可以，会平铺展示

```js
import React from 'react'
import {Text,View,TouchableOpacity,Image,ImageBackground,TextInput} from 'react-native'
import {Dimensions} from 'react-native'
const screenWidth = Math.round(Dimensions.get('window').width)
const screenHeight = Math.round(Dimensions.get('window').height)

const Index = () =>{
  const arr = ["111","222","333"]
  const obj = {name:'小明'}
  const a = 'xiaowan'

  return(
    <View style={{backgroundColor:"white",flexDirection:"column"}}>
      <Text>{a}</Text>
      <Text>{arr}</Text>
      {
        arr.map((v,index)=><Text key={index}>{v},{index}</Text>)
      }
    </View>

  )
}

export default Index
```

#### 对象深拷贝

* 使用JSON方法：obj2 = JSON.parse(JSON.stringify(obj1))
* 

#### 函数组件和类组件

函数生命周期可以通过hooks实现

#### 生命周期

constructor  构造器，调用时必须先super()，可以在其中初始化state和执行bind

render 视图渲染

componentDidMount  组件挂载完毕，发送网络请求、开启定时器

componentWillUnmount  组件将要卸载  清除定时器

#### 插槽(props.children)

```jsx
import React, { Component } from 'react'
import {View,Text} from 'react-native'

//export default本质是将default后面的值赋值给default，所以不能用 export Index = () =>{} 
export default () => {
    return(
        <View>
            <Text>我是父组件</Text>
            <Sub>
                <Text>通过props2223332.children传递</Text>
            </Sub>
        </View>
    )
}

const Sub = (props) => {
    return(
        <View>
            <Text>子组件</Text>
            {props.children}
        </View>
    )
}
```

#### 调试

android虚拟机屏幕上ctrl+m打开菜单，选择debug

* 谷歌浏览器调试
  * 不能查看标签结构
  * 不能查看网络请求
* react-native-debugger调试（推荐）
  * 可以查看标签结构
  * 不能查看网络请求

* 没法捕获模拟器发出的网络请求，可以在index.js中加入

```js
GLOBAL.XMLHttpRequest = GLOBAL.originalXMLHttpRequest || GLOBAL.XMLHttpRequest
```

#### 事件

绑定事件需要特别注意this指向，有以下几种解决方式

* 箭头函数 handleChange=()=>{}
* bind
* 匿名函数 onChangeText = {()=>{this.handleChange()}}
* 构造函数constructor中执行bind

### 连接夜神模拟器

在打开夜神模拟器后，第一次需要进入夜神模拟器安装目录的bin目录下执行下面的命令

nox_adb connect 127.0.0.1:62001

之后连接就可以输入adb connect 127.0.0.1:62001命令连接上模拟器，夜神的端口号是62001。

遇到问题： 如遇上端口占用，杀掉占用的端口。



传输到模拟器：
npx react-native run-android 将本地app打包成临时包，传输到模拟器。



### mobx 状态管理

全局数据管理库，比redux简单

#### 使用步骤

* mobx 核心库
* mobx-react 方便在react中使用mobx技术的库
* @babel/plugin-proposal-decorators 让rn项目支持es7的装饰器语法的库

```shell
npm install mobx mobx-react @babel/plugin-proposal-decorators
```

* 配置babel.config.js

```js
module.exports = function(api) {
  api.cache(true);
  return {
    presets: ['babel-preset-expo'],
    //添加以支持Mobx装饰器
    plugins:[
      ['@babel/plugin-proposal-decorators',{'legacy':true}]
    ]
  };
};
```

* 在项目根目录新建mobx文件夹内，文件夹内新建index.js文件用来存放全局数据

```js
import {observable,action, makeObservable, makeAutoObservable} from 'mobx'

class RootStore{
    //es7 装饰器语法 Object.defineProperty
    //mobx6版本之前----已不适用当前项目
    // @observable  //属性
    // name = '悟空'
    // @action  //行为，修改名称
    // changeName(name){
    //     this.name = name
    // }

    //mobx6版本--写法一 手动设置监测
    // constructor(){
    //     makeObservable(this)
    // }
    // @observable  //属性
    // name = '悟空'
    // @action  //行为，修改名称
    // changeName(name){
    //     this.name = name
    // }

    //mobx6版本--写法二 自动设置全部可监测
    constructor(){
        makeAutoObservable(this)
    }
    name = '悟空'
    changeName(name){
        this.name = name;
    }
}

export default new RootStore();
```

* 在根组件App.js挂载

```jsx
import React, { Component } from 'react'
import {Text,View} from 'react-native'
//引入全局mobx
import RootStore from './mobx'
//引入Porvider
import {Provider} from 'mobx-react'
import Btn from './components/Btn'

export default class App extends Component {
    render() {
        return (
            <View>
                <Provider RootStore={RootStore}>
                 {/* 组件都包裹在Provider之中，就能够使用全局变量 */}
                    <Btn></Btn>
                </Provider>
            </View>
        )
    }
}

```

* 类组件中使用mobx     components/Btn.js

```jsx
import React, { Component } from 'react'
import {Text,View} from 'react-native'
import {inject, observer} from 'mobx-react'

//注入RootStore全局变量，与App.js中Provider属性名保持一致即可
@inject("RootStore")
@observer  //观察，全局变量更新组件也会更新
export default class Btn extends Component {
    handlePress = () =>{
        //注意方法changeName不能使用解构赋值然后重新调用，changeName的this作用域会变化，无法实现更新后render
        let {name} = this.props.RootStore
        this.props.RootStore.changeName(name === '悟空'?'八戒':'悟空')
    }

    render() {
        return (
            <View>
                <Text onPress={this.handlePress}>Btn：{this.props.RootStore.name}</Text>
            </View>
        )
    }
}
```

* 函数组件中使用mobx

```js
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import Login from './pages/account/login'
import UserInfo from './pages/account/userinfo'
import Demo from './pages/demo'
import TabBar from './tabbar'
import {inject,observer} from 'mobx-react'

const Stack = createStackNavigator();

const Nav = (props) => {
  const [initRouteName] = React.useState(props.RootStore.token?"TabBar":"Login")

  return (
    <NavigationContainer>
        {/* initialRouteName:初始显示页面
          * headerMode:none  隐藏头部标题
        */}
      <Stack.Navigator headerMode="none" initialRouteName={initRouteName}>
        <Stack.Screen name="TabBar" component={TabBar} />
        <Stack.Screen name="Login" component={Login} />
        <Stack.Screen name="UserInfo" component={UserInfo} />
        <Stack.Screen name="Demo" component={Demo} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
//注入mobx
export default inject('RootStore')(observer(Nav));

//注入多个sotre
export default inject('UserStore','RootStore')(observer(Nav))
```

### VSCODE使用

* 如何格式化json数据
  * 安装json-tools
  * 新建一个空白文件，无需保存，将字符串json数据粘贴
  * ctrl+shift+m格式化json

## 三、探花交友项目

### **模块修改**

### jmessage-react-plugin

修改node_modules/jmessage-react-plugin/android/src/io/jchat/android/JMessageReactPackage.java

```java
package io.jchat.android;

import com.facebook.react.ReactPackage;
import com.facebook.react.bridge.JavaScriptModule;
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.uimanager.ViewManager;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class JMessageReactPackage implements ReactPackage {

    private boolean mShutdownToast;

    public JMessageReactPackage(boolean shutdownToast) {
        mShutdownToast = shutdownToast;
    }

    @Override
    public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {
        List<NativeModule> result = new ArrayList<>();
        result.add(new JMessageModule(reactContext, mShutdownToast));
        return result;
    }

    public List<Class<? extends JavaScriptModule>> createJSModules() {
        return Collections.emptyList();
    }

    @Override
    public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
        List<ViewManager> viewManagers = new ArrayList<>();
        viewManagers.add(new BubbleMsgManager());
        return  viewManagers;
    }
}
```

### aurora-imui-react-native

修改node_modules/aurora-imui-react-native/react-native-android/build.gradle

![image-20210323143346526](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514120819.png)

组件的glide为4.6.1时和项目的依赖不一致，报错NoSuchMethodError: No virtual method placeholder。。。，这里将组件的glide改为4.9.0解决报错

### 模块介绍

交友、圈子、消息、个人中心、其他

### 框架搭建

#### 安装react-navigation

```shell
1、安装react-navigation
npm install @react-navigation/native
2、安装依赖
npm install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
3、安装依赖
npm install @react-navigation/stack
```

#### 如何在非路由组件中使用路由功能进行跳转

* 从路由组件传递props.navigation属性到子组件（非路由组件）----多层不方便
* 基于context上下文实现的@react-navigation/native

```js
//函数组件中
import * as React from 'react';
import { Button } from 'react-native';
import { useNavigation } from '@react-navigation/native';

function MyBackButton() {
  const navigation = useNavigation();

  return (
    <Button
      title="Back"
      onPress={() => {
        navigation.goBack();
      }}
    />
  );
}


//类组件中
import { NavigationContext } from '@react-navigation/native';
class SomeComponent extends React.Component {
  //给当前的类的静态属性contextType赋值，这样就可以通过this.context获取到navigation
  static contextType = NavigationContext;

  render() {
    // We can access navigation object via context
    const navigation = this.context;
  }
}
```



#### 例子

```jsx
import * as React from 'react';
import { View, Text, Button } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import Login from './pages/acount/login'

function DetailsScreen() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Details Screen</Text>
      </View>
    );
  }

function HomeScreen(props) {
  //函数组件通过props参数解构获取导航
  //类组件通过this.props.navigation获取导航对象
  const {navigation} = props
  console.log(props,navigation)
  const handlePress = () => {
    navigation.navigate("Details")
  }
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Button onPress={handlePress} title='点我去Detail页面'></Button>
    </View>
  );
}

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
        {/* initialRouteName:初始显示页面
          * headerMode: none表示隐藏头部大标题
        */}
      <Stack.Navigator headerMode="none" initialRouteName="Login">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
        <Stack.Screen name="Login" component={Login} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```

#### react-navigation路由传参

https://blog.csdn.net/weixin_43294560/article/details/105214543

//发送 					{navigation.navigate('路由名',{键值对})}	

//接受                this.props.route.params.键名

react-navigation路由跳转navigate goPage方法跳转到一个旧路由，不会触发该路由的componentDidMount，所以有时候就会导致数据没有刷新，没法获取最新数据。这个时候可以用navigation.reset方法来重置路由实现让其刷新

例如

````js
//不带参数重置路由
this.props.navigation.reset({
                    routes:[{
                        name:'TabBar'
                    }]
                })

 //携带路由，重置参数
//发送
                props.navigation.reset({
                    routes: [{ name: 'TabBar',params:{tabname:'group'} }],
                  });
//接收，在TabBar页面
let initPageName = 'my'
    //链式判断ES6
    if( props?.route?.params?.tabname){
        initPageName = props.route.params.tabname
    }
````



### 后台服务搭建

* 安装mysql和navicat（需要破解）

* 后台代码启动补充:`npm install` 安装依赖

  下载myql并安装

  修改包hm-tanhua-api\server\middlewares\dbHandle 的password为安装时候设置的密码: ,如果改了用户名也要修改用户名

  启动mysql  :

  mysql -u root -p  

  回车后输入安装时候设置的密码

  创建tanhua数据库

  CREATE DATABASE 'tanhua' DEFAULT CHARACTER SET utf8 ;

  使用数据库

  use tanhua

  导入sql文件

   source   xxxx/tanhua 20200630 0858.sql;   

  `node app.js` 启动

  或者

  执行根目录下的  `start.bat` 启动服务器 ，注意要将 nodemon 安装为全局

* 部署在本地，安卓虚拟机也在本地，虚拟机中的localhost和本机的localhost不是一个东西，服务只部署在本机，虚拟机无法访问，可以将 localhost换成10.0.2.2来访问，端口号还是9089，详见https://blog.csdn.net/Ruffaim/article/details/80428887     https://www.oschina.net/question/163482_25146

* 跨域问题可以通过koa2-cors组件来解决

* 使用postman模拟请求 http://localhost:9089/user/login 判断接口是否本地部署成功，注意设置Content-Type为application/json

![image-20210311213613535](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514120845.png)

### vscode插件设置

安装Prettier-Code formatter实现保存jsx时正确保存，不格式化错误。

格式化jsx/less代码时可以右键格式化文档。

## 四、登录模块

### 登录页面

#### 填写手机号码

1、完成静态页面布局

2、用到的组件有

* StatusBar  ---- 手机顶部状态栏，使用StatusBar可以让背景图片显示在顶部，浑然一体

* Image --==rn组件，可以设置source={require('../../xxx.png')}

* Input(recat-native-elements)

  * 小型UI库
  * 安装步骤 https://reactnativeelements.com/docs/
    * npm install react-native-elements
    * npm i --save react-native-vector-icons
    * npx react-native link react-native-vector-icons
    * npx react-native link react-native-vector-icons
    * npx react-native link react-native-safe-area-context

* Icon(react-native-vector/FontAwesome5)

* react-native-linear-gradient

  * 安装  	
    * npm install react-native-linear-gradient --save

* teaset  

  * 轻量级UI库
  * 安装步骤
    * npm --save teaset
    * 封装
  * teaset封装

  ```js
  import React from 'react'
  import {ActivityIndicator} from 'react-native'
  import {Toast,Theme} from 'teaset'
  
  let toastKey = null;
  /**
   * 显示提示(带loading图标)
   * @param {String} text 提示的文本,默认为loading
   * @param {Number} duration 提示持续时间 ,默认10000毫秒
   * @param {String} position 提示的位置，默认center 
   */
  export const showToast = (text="loading",duration=10000,position='center') => {
    if(toastKey) return
    toastKey = Toast.show({
      text,
      icon: <ActivityIndicator size='large' color={Theme.toastIconTintColor} />,
      position,
      duration,
    });
  }
  
  /**
   * 隐藏提示
   */
  export const hideToast = () => {
    if(!toastKey) return
    Toast.hide(toastKey)
    toastKey = null
  }
  
  /**
   * 显示短消息提示（不带icon），默认2秒，2秒后自动消失
   * @param {String} text 提示的文本
   * @param {Number} duration 提示持续时间 ,默认2000毫秒
   * @param {String} position 提示的位置，默认center 
   */
  export const showShortTip = (text,duration=2000,position='center') => {
    Toast.show({
      text,
      position,
      duration,
    });
  }
  ```

  * axios封装包含请求加载loading图标

  ```js
  import React from 'react'
  import axios from 'axios'
  import { BASE_URI } from './pathMap'
  import { showToast, hideToast } from './toast';
  
  // axios.create()是添加了自定义配置的新的axios
  //可以简化路径写法,当基础路径发生变化时方便修改，有利于维护
  //不光如此还可以配置一些其他信息，例如headers，timeout。另外先创建实例再使用实例是一种规范
  const instance = axios.create({
      baseURL: BASE_URI
  })
  
  // 添加请求拦截器
  instance.interceptors.request.use(function (config) {
      // 在发送请求之前做些什么
  
      //显示加载中
      showToast("加载中")
  
      return config;
  }, function (error) {
      // 对请求错误做些什么
      return Promise.reject(error);
  });
  
  // 添加响应拦截器
  instance.interceptors.response.use(function (response) {
      // 对响应数据做点什么
      
      //隐藏加载中
      hideToast()
  
      return response;
  }, function (error) {
      // 对响应错误做点什么
      return Promise.reject(error);
  });
  export const axiosGet = instance.get
  export const axiosPost = instance.post
  ```

  

3、实现功能

* px单位转dp，实现自适应

```js
import {Dimensions} from 'react-native'
//原理，等比例缩放
//设计稿的宽度(px) / 元素的宽度(px) = 手机屏幕宽度(dp) / 手机中元素的宽度(px)
//前三个参数都是已知

//暴漏出去，这样以后可以直接用screenWidth和screenHeight
/**
 * 屏幕宽度
 */
export const screenWidth = Dimensions.get('window').width
/**
 * 屏幕高度
 */
export const screenHeight = Dimensions.get('window').height

//375px是设计稿的宽度
//输入设计稿中元素的宽度，返回结果是自适应不同屏幕的数值，单位是dp
/**
 * px转dp
 * @param {Number} elePx 元素的宽度(px)
 */
export const pxToDp = elePx => screenWidth * elePx / 375
```

* 填写手机号码

* 校验手机号码

* 获取验证码

  * 组件THButton封装
    * Text标签内文字垂直居中可以用textAlignVertical属性
  * 代码

  ```js
  import React from 'react'
  import PropTypes from 'prop-types'
  import { View, Text, StyleSheet, TouchableOpacity } from 'react-native'
  import LinearGradient from 'react-native-linear-gradient';
  import { pxToDp } from '../../utils/stylesKits';
  
  const THButton = (props) => {
      return (
          // 按钮设置100%宽度和高度，解构props传递的style属性来设置宽高，解构的属性放在后面，覆盖前面的
          <TouchableOpacity onPress={props.onPress} style={{ alignSelf: 'center', width: '100%', height: '100%', overflow: 'hidden', height: pxToDp(40), borderRadius: pxToDp(20), ...props.style }}>
              <LinearGradient start={{ x: 0, y: 0 }} end={{ x: 1, y: 0 }} colors={['#A36ED1', '#EA7588']} style={styles.linearGradient}>
                  <Text style={styles.buttonText}>
                      {props.children}
                  </Text>
              </LinearGradient>
          </TouchableOpacity>
      )
  }
  
  //函数组件设置
  //props输入属性字段类型、可选性进行设置
  THButton.propTypes = {
      style: PropTypes.object,
      onPress: PropTypes.func
  }
  //设置默认属性
  THButton.defaultProps = {
      style: {}
  }
  
  const styles = StyleSheet.create({
      linearGradient: {
          flex: 1,
          width: '100%',
          height: '100%',
      },
      buttonText: {
          fontSize: pxToDp(18),
          height: '100%',
          fontFamily: 'Gill Sans',
          textAlign: 'center',
          //Text内文字垂直居中
          textAlignVertical: 'center',
          color: '#ffffff',
          backgroundColor: 'transparent',
          alignItems: 'center'
      },
  });
  
  export default THButton
  ```

  

* 切换到验证码页面

  * 通过showLoginPage控制登录页面显示还是login页面显示

#### 填写验证码

* 实现验证码静态页面

  * 用到的组件有
    * react-native-confirmation-code-field
      * 安装 npm intall react-native-confirmation-code-field@next
      * 注意不要安装错误

* 实现功能

  * 获取验证码 5秒过后可重新获取验证码

  * 校验验证码

    * 校验成功并且是新用户，则跳转到用户信息完善页面
    * 校验成功并且是老用户，则跳转到交友页面
    * 如何实现跳转，由于Login组件是路由组件，可以直接使用this.props.navigation.navigate()方法实现跳转

    ![image-20210313182146493](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514120905.png)



### 用户信息完善页面

1、完成静态页面

* 用到的组件有

  * react-native-svg-uri 和 react-native-svg

    * npm install react-native-svg

    * npm install react-native-svg-uri

    * react-native link react-native-svg  （RN从0.60以后实现了autolink，不需要手动link，详见 https://www.jianshu.com/p/9641b3beae84）

    * 使用iconfont svg过程

      * 登录iconfont，选择图标添加到购物车，然后下载到本地
      * 在本地打开demo.html，得到你想要复制的图片的svg 的xml
      * ![image-20210313195403290](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514120912.png)
      * 新建iconSvg.js，专门用来保存svg格式的图片
      * ![image-20210313205851958](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514120919.png)
      * 调用即可
      * ![image-20210313205924783](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514120924.png)
      * 给svg上面加一个按钮容器，点击选中设置按钮的背景颜色标识选中

      

      

  * react-native-datepicker

    * 日期选择插件
    * npm install react-native-datepicker
    * 没啥好说的，按照官方文档整就完事了

  * react-native-picker

    * 用于选择城市，前端提供一个city.json作为数据源，组件加载完毕Picker.init，点击选择城市时弹出选择框Picker.show
    * 注意：json文件可以直接import `import CityJson from '../../res/citys.json'`

  * react-native-image-crop-picker

    * npm install react-native-image-crop-picker ， 安装过程可参考 https://blog.csdn.net/rongyuliu951080er/article/details/110822465，不过这个链接中的 ` maven { url 'https://maven.google.com' }` 可以用`maven{url 'http://maven.aliyun.com/nexus/content/groups/public/'}`国内的阿里镜像代替
    * 注意：react-native-image-crop-picker 0.36.0要求gradle版本>=0.35.0，使用create-react-native-app脚手架创建的项目必须要升级gradle版本，升级过程可参考：https://zhuanlan.zhihu.com/p/265655310  和   https://developer.android.google.cn/studio/releases/gradle-plugin#updating-plugin

  * teaset的overlay

    * 主要用于实现一个前端效果，刷来刷去审核
    * 将scan.gif和用户选择的图片叠在一起，实现审核的前端效果

* 完成功能

  * 选择性别：react-native-svg-uri组件使用
  * 填写昵称：react-native-element ui库的input组件
  * 选择生日
  * 自动定位和手动选择定位
  * 设置头像
    * 上传头像的接口地址和参数说明 `ACCOUNT_CHECKHEADIMAGE`
    * 参数说明
      * 需要使用formdata对象来传递图片
      * key为headPhoto
      * value值为对象{uri,type,name}
    * 请求头设置
      * 需要设置 "Content-Type":"multipart/form-data"
      * 需要携带token  `Authorization:Bearer`

* 其他

  * mbox共享数据，如手机号码、token和用户id

  * 使用高德地图来实现获取定位

    * react-native-amap-geolation

    * react-native获取高德地图的发布版和调试版sha1，参考 视频 https://www.bilibili.com/video/BV1F4411s7yd，react-native项目创建时会在android/app文件夹下新建一个debug.keystore，密码默认是android，读取该文件可以获得当前项目的调试版sha1。在android/app目录下使用命令创建一个my-release-key.keystore文件，获得发布版的sha1

    * 申请得到key以后要注意在AndroidManifest.xml中添加如下代码` <meta-data android:name="com.amap.api.v2.apikey" android:value="c0d86b7baa74b6dbb356eb6f9ffd3c00" ></meta-data>`

    * 封装Geo.js

    * ```js
      import { PermissionsAndroid, Platform } from "react-native";
      import { init, Geolocation } from "react-native-amap-geolocation";
      import axios from 'axios'
      
      class Geo{
          /**
           * 初始化
           */
          async initGeo(){
              console.log('initGeo()')
              if(Platform.OS === 'android'){
                  await PermissionsAndroid.requestMultiple([
                      PermissionsAndroid.PERMISSIONS.ACCESS_FINE_LOCATION,
                      PermissionsAndroid.PERMISSIONS.ACCESS_COARSE_LOCATION,
                  ]);
              }
              await init({
                  ios: "c0d86b7baa74b6dbb356eb6f9ffd3c00",
                  android: "c0d86b7baa74b6dbb356eb6f9ffd3c00"
                });
              return Promise.resolve();
          }
      
          /**
           * 获取当前经纬度
           */
          async getCurrentPosition(){
              return new Promise((resolve,reject)=>{
                  Geolocation.getCurrentPosition(({ coords }) => {
                      resolve(coords)
                  },reject);
              })
          }
      
          /**
           * 获取城市名
           */
          async getCityByLocation(){
              await this.initGeo()
              console.log('getCityByLocation()')
              const {longitude,latitude} = await this.getCurrentPosition()
              console.log(longitude,latitude)
              const res = await axios.get("https://restapi.amap.com/v3/geocode/regeo",{
                  params:{location:`${longitude},${latitude}`,key:'e1af550b8951e08ab753cd1f57d21dfb'}
              })
              return Promise.resolve(res.data)
          }
      }
      
      export default new Geo()
      ```

    * 注意：Geo.js的第一个key是android平台定位服务的key，第二个key是用来请求webapi进行地址编码的key

    * 根组件app.js修改，执行完initGeo方法以后再渲染组件，初始化一次，后面直接调用getCityByLocation方法即可，不需要初始化

    * ```js
      import React,{useState,useEffect} from 'react';
      import Nav from './src/nav'
      import {View} from 'react-native'
      import Geo from './src/utils/Geo'
      const App = () => {
        const [showApp, setShowApp] = useState(false) 
      
        useEffect( async ()=>{
          //模拟componentDidMount
          await Geo.initGeo()
          setShowApp(true)
        },[])
      
        return (
          // rn默认时flex布局，flex-direction为column
          // 这里使用flex:1使得最外层容器高度占满手机
          <View style={{flex:1}}>
            {showApp?<Nav></Nav>:<></>}
          </View>
        );
      }
      
      export default App;
      ```

    * 用户信息完善界面

    * ```js
          async componentDidMount(){
              const res = await Geo.getCityByLocation()
              console.log(res)
              if(res.status === "1"){
                  //定位请求成功
                  const {city} = res.regeocodes.addressComponent
                  console.log('定位到城市：'+city)
              }else{
                  showShortTip('自动获取定位失败')
              }
          }
      ```

    * 完善注册信息时调用getCityByLocation自动获取城市名

  * 封装request实现自动携带token

  ```js
  /**
   * 带Token的post请求
   * @param {String} uri 请求地址
   * @param {Object} data  post数据
   * @param {Object} options 配置
   */
  export const axiosPrivatePost = (uri,data={},options={}) => {
      const token = RootStore.token
      const headers = options.headers || {}
      return instance.post(uri,data,{
          ...options,  //解构options
          headers:{
              "Authorization":`Bearer ${token}`, //使用该请求post，自动携带token
              ...headers  //如果用户传递了headers参数，解构参数并且合并
          }
      })
  }
  ```

## 五、APP 底部menu搭建

tabbar结构

### 实现步骤

* 分析：tabbar结构上，存放着四个页面(模块)，分别是交友、圈子、消息、我的

* 用到的组件：react-native-tab-navigator（比原生的好，可以加小红点)

* 安装：npm install react-native-tab-navigator

* 编写tabbar.js

* ```js
  import React from 'react'
  import { Text,View } from 'react-native'
  import TabNavigator from 'react-native-tab-navigator';
  import SvgUri from 'react-native-svg-uri'
  import { friendSvgXml, groupSvgXml, groupSelectedSvgXml, messageSvgXml, messageSelectedSvgXml, mySvgXml, mySelectedSvgXml, frientSelectedSvgXml } from './res/font/iconSvg';
  import {theme_color} from './utils/varient'
  export default TabBar = () => {
      const [selectTab, setselectTab] = React.useState("friend")
      return (
          <View style={{ flex: 1 }}>
              <TabNavigator tabBarStyle={{backgroundColor:'#F8F8F8'}}>
                  <TabNavigator.Item
                      selected={selectTab === 'friend'}
                      title="交友"
                      renderIcon={() => <SvgUri width="20" height="20" svgXmlData={friendSvgXml}></SvgUri>}
                      renderSelectedIcon={() => <SvgUri width="20" height="20" svgXmlData={frientSelectedSvgXml}></SvgUri>}
                      // badgeText="1"
                      titleStyle={{color:'#212121'}}
                      selectedTitleStyle={{color:theme_color}}
                      onPress={() => setselectTab("friend")}>
                      
                      <Text>交友</Text>
                  </TabNavigator.Item>
                  <TabNavigator.Item
                      selected={selectTab === 'group'}
                      title="圈子"
                      renderIcon={() => <SvgUri width="20" height="20" svgXmlData={groupSvgXml}></SvgUri>}
                      renderSelectedIcon={() => <SvgUri width="20" height="20" svgXmlData={groupSelectedSvgXml}></SvgUri>}
                      titleStyle={{color:'#212121'}}
                      selectedTitleStyle={{color:theme_color}}
                      onPress={() => setselectTab('group')}>
                      <Text>圈子</Text>
                  </TabNavigator.Item>
                  <TabNavigator.Item
                      selected={selectTab === 'message'}
                      title="消息"
                      renderIcon={() => <SvgUri width="20" height="20" svgXmlData={messageSvgXml}></SvgUri>}
                      renderSelectedIcon={() => <SvgUri width="20" height="20" svgXmlData={messageSelectedSvgXml}></SvgUri>}
                      titleStyle={{color:'#212121'}}
                      selectedTitleStyle={{color:theme_color}}
                      onPress={() => setselectTab("message")}>
                      <Text>消息</Text>
                  </TabNavigator.Item>
                  <TabNavigator.Item
                      selected={selectTab === 'my'}
                      title="我的"
                      renderIcon={() => <SvgUri width="20" height="20" svgXmlData={mySvgXml}></SvgUri>}
                      renderSelectedIcon={() => <SvgUri width="20" height="20" svgXmlData={mySelectedSvgXml}></SvgUri>}
                      titleStyle={{color:'#212121'}}
                      selectedTitleStyle={{color:theme_color}}
                      onPress={() => setselectTab('my')}>
                      <Text>我的</Text>
                  </TabNavigator.Item>
              </TabNavigator>
  
          </View>
      )
  }
  ```



## 六、完善和优化已有权限和token业务

1、成功登录后，将用户信息存储到mobx中，同时将用户信息存储到本地缓存AsyncStorage中

2、moxb中存储的是临时数据，app退出就会丢失。如果不想让用户频繁登录，就需要将用户信息存储到本地缓存中

3、在nav界面对用户做权限校验：判断mbox中是否有userinfo，如果有值，那么就渲染tabbar页面，如果没值，就渲染login页面

4、在App.js中将本地存储中的用户数据赋值到mobx中

5、在tabbar页面进行极光登录

## 七、交友--首页

* 实现步骤

  * 功能点

    * 静态布局

    * 顶部图片吸附效果 react-native-image-header-scroll-view

    * 访客模块

    * 今日佳人模块

    * rn中使用普通的iconfont字体

      * 1、在字体图标网站上下载字体
      * 2、拷贝ttf文件到anroid\app\src\main\assets\fonts中，如果没有assets文件夹则新建一个
      * ![image-20210318163908022](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514120941.png)
      * 3、初级使用 `<Text style={{fontFamily:'iconfont',color:'red'}}>{'\ue82b'}</Text>`
      * 4、每次使用都要填写unicode编码，太麻烦，这里对其进行封装，首先设置一个名字有unicode编码对应的文件

      ```js
      export default {
        "iconlocation": "\ue618",
      
        "icondanao1": "\ue623",
      
        "iconuser": "\ue607",
      
        "iconmaozi": "\ue602",
      
        "iconboy-sel": "\ue601",
      
        "iconman-sel": "\ueb95",
      
        "iconxin-o": "\ue711",
      
        "iconxin": "\ue82b",
      
        "iconmeiyan": "\ue659",
      
        "iconchoosehandle": "\ue619",
      
        "iconfanhui": "\ue63b",
      
        "iconbianji": "\ue66d",
      
        "icontanhuanv": "\ue68b",
      
        "icontanhuanan": "\ue68d",
      
        "iconxihuan": "\ue634",
      
        "iconwenjian": "\ue68f",
      
        "iconservice": "\ue7bb",
      
        "icondianzan": "\ue686",
      
        "icontanhuayuyin": "\ue632",
      
        "icontanhuaxiaoxi": "\ue614",
      
        "iconshaixuan": "\ue631",
      
        "iconyuyinshuohuabao": "\ue667",
      
        "iconkaiguanclose": "\ue66b",
      
        "iconyuehoujifen": "\ue61b",
      
        "iconsuo": "\ue657",
      
        "iconxiala": "\ue617",
      
        "iconshuikanguowo": "\ue690",
      
        "iconsousuo": "\ue611",
      
        "iconguanbi": "\ue641",
      
        "icondianzan-o": "\ue65c",
      
        "icontupian": "\ue606",
      
        "iconfanzhuanjingtou": "\ue872",
      
        "icontanhuashipin": "\ue64e",
      
        "iconshipin": "\ue677",
      
        "iconbiaoqing": "\ue600",
      
        "iconselected": "\ue60d",
      
        "iconchenggong": "\ue669",
      
        "iconman": "\ue615",
      
        "iconnv": "\ue616",
      
        "iconnaozhong": "\ue61c",
      
        "iconyuyin": "\ue637",
      
        "iconhuxiangguanzhu": "\ue60f",
      
        "iconyibiaopan": "\ueb94",
      
        "icondingwei-o": "\ue620",
      
        "iconqianbao": "\ue621",
      
        "iconjia": "\ue794",
      
        "icontanhuaxiala": "\ue65b",
      
        "icontanhuatupian": "\ue6f5",
      
        "icongengduo": "\ue612",
      
        "iconpinglun": "\ue61e",
      
        "iconxiangji": "\ue7a5",
      
        "iconshengyin": "\ueae0",
      
        "iconshezhi": "\ue633",
      
        "iconshibai": "\ue60b",
      
        "iconrenshu": "\ue61d",
      
        "iconkefu": "\ue610",
      
        "icondingwei": "\ue638",
      
        "iconriqi": "\ue66a",
      
        "icondanao": "\ue61f",
      
        "iconxihuan-o": "\ue665",
      
        "icondongtai": "\ue613",
      
        "icontanhuakefu": "\ue62f",
      
        "icongonggao": "\ue6ff",
      
        "icontongxunlu": "\ue63a",
      
        "iconliaotian": "\ue75e",
      
        "iconxiaoxi": "\ue608",
      
        "icontanhuawode": "\ue609",
      
        "iconquanzi": "\ue60a",
      
        "iconyuyinshuohua": "\ue60c",
      
        "icontianjia": "\ue60e",
      
        "iconbuxihuan": "\ue61a"
      }
      ```

      

      * 5、封装IconFontText组件

      ```js
      import React from 'react'
      import {Text} from 'react-native'
      import iconMap from '../../res/icon'
      export default IconFontText = (props) => {
          return <Text style={{fontFamily:'iconfont', ...props.style}}>{iconMap[props.name]}</Text>
      }
      ```

      * 6、使用

      ```js
      <IconFontText style={{color:'red'}} name="iconuser" />
      ```



### 列表和筛选功能

1、获取数据渲染推荐列表

2、点击筛选时，使用teaset弹出层实现筛选界面的弹出

3、根据需求构造筛选界面的组件功能

4、点击确认时，将筛选的结果返回到父页面，继而加载新数据

### 探花页面

* 左滑右滑来切换用户
* 业务
  * 封装顶部tabbar
  * 加载数据实现渲染用户
  * 使用组件实现左滑右滑实现对用户的喜欢和不喜欢
  * 点击不同的按钮实现对用户的喜欢和不喜欢
  * 用到的接口
    * 获取需要滑动的数据
    * 喜欢和不喜欢
  * 用到的组件
    * react-native-deck-swiper
    * 滑动到底时分页功能时基于swipedAll时间实现的，一旦滑动到底，重新请求数据，不过数据变化时react-native-deck-swiper不会重新刷新，可以给他加一个key={Date.now()}让他强制刷新

### 搜附近

#### 业务

1、实现静态布局

2、获取附近好友 和 动态渲染最近好友

3、点击筛选按钮进行数据筛选

4、用到的接口

#### 点击筛选

同上面列表筛选

### 用户详情

#### 业务

1、获取数据动态渲染

2、顶部轮播大图 Teaset的 Carousel 组件

3、用户动态图片点击放大 react-native-image-zoom-viewer   https://blog.csdn.net/weixin_42512937/article/details/108195235

4、滚动分页加载数据

* 1、对ImageHeaderScrollView绑定onScroll事件
* 2、事件源中获取一些距离数据
  * nativeEvent.contentSize.height 列表内容的高度a
  * nativeEvent.layoutMeasurement.height 可视区域的高度b
  * nativeEvent.contentOffset.y  滚动条距离顶部的高度c
  * 当a-b-c<10时代表用户滚动到底，需要获取下一页数据，注意这里面需要做函数节流，防止重复在a-b-c=9.99时触发一次请求，在9.98时触发一次请求，当确定要进行分页时，设置一个boolean值为true，代表正在请求数据中，那么下一次重复触发时就不会请求

5、点击喜欢通过极光通讯给当前用户发送一条信息

* 建议使用新用户来完成聊天相关的功能
* 需要在tabbar组件中进行极光登录

6、点击聊一下跳转到聊天界面



## 八、聊天界面

### 业务

1、显示聊天界面 aurora-imui-react-native

* 安装组件

  * ```
    npm install aurora-imui-react-native --save
    npm install react-native-fs --save
    ```

2、获取极光历史消息

3、设置发送者和接收者位置显示

4、发送文字，配合极光发送文字

5、发送图片，配合极光发送图片

6、接受消息显示文字和图片

7、关键api

* constructNormalMessage 创建消息体的构造函数
* getHistoryMessage 获取历史聊天数据
* onSendText 发送文本消息
* onSendGalleryFiles 发送图片消息



## 九、圈子

### 圈子--首页

#### 业务

1. 使用组件 [react-native-scrollable-tab-view](https://www.npmjs.com/package/react-native-scrollable-tab-view) 搭建基本结构
2. 创建 **推荐** 和 **最新** 页面

#### 实现

1. 获取 `圈子-推荐`数据

2. 使用  `FlatList` 循环显示内容

   1. 使用到 [moment.js](http://momentjs.cn) 日期库 

3. 实现  `分页加载功能`

   使用onEndReached事件和onEndReachedThreshold属性，无需做节流

4. 实现  `点击图片-放大显示` 功能

   同用户详情页面

5. 实现 `点赞` 功能

   1. 同时也需要通过极光发送一条 **点赞消息**
   2. 点赞后立即更新页面，通过修改本地数据实现，使用lodash对对象进行深拷贝

6. 实现 `喜欢` 功能

7. 实现  `不感兴趣` 功能

   不感兴趣以后，会屏蔽当前用户的所有动态

8. 实现评论功能

   * 获取评论列表数据

   * 渲染页面

   * 实现评论点赞

   * 实现发表评论

   * 滚动分页

     * 用到是ScrollView的分页，不是FlatList，FlatList没法在不定高度中展现滚动条

   * 点击放大图片

   * 用到的接口

     1. 1. [推荐动态数据](http://api-tanhua-web.itheima.net/swagger.html#/qz/get_qz_recommend)

        2. [点赞-取消点赞](http://api-tanhua-web.itheima.net/swagger.html#/qz/get_qz_star__id_)

        3. [喜欢-取消喜欢](http://api-tanhua-web.itheima.net/swagger.html#/qz/get_qz_like__id_)

        4. [不感兴趣](http://api-tanhua-web.itheima.net/swagger.html#/qz/get_qz_noInterest__id_)

        5. [评论列表](http://api-tanhua-web.itheima.net/swagger.html#/qz/get_qz_comment__id_)

        6. [评论点赞](http://api-tanhua-web.itheima.net/swagger.html#/qz/get_qz_comments_star__id_)

        7. [评论-提交](http://api-tanhua-web.itheima.net/swagger.html#/qz/commentsSubmit)

     

### 圈子--最新



## 零、react-native组件集成

### 集成极光IM

1. 安装依赖

   ```js
   yarn add jmessage-react-plugin jcore-react-native
   ```

2. 配置

   1. `android\app\src\main\AndroidManifest.xml` 加入以下代码

      ```xml
            <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
            <!-- 极光的配置 -->
            <meta-data android:name="JPUSH_CHANNEL" android:value="${APP_CHANNEL}" />
            <meta-data android:name="JPUSH_APPKEY" android:value="${JPUSH_APPKEY}" />
            <!-- 极光的配置 -->
          </application>
      ```

   2. `android\app\build.gradle` 加入以下代码和按需修改

      ```js
      android {
          compileSdkVersion rootProject.ext.compileSdkVersion
      
          compileOptions {
              sourceCompatibility JavaVersion.VERSION_1_8
              targetCompatibility JavaVersion.VERSION_1_8
          }
      
          defaultConfig {
              applicationId "com.awesomeproject22"
              minSdkVersion rootProject.ext.minSdkVersion
              targetSdkVersion rootProject.ext.targetSdkVersion
              versionCode 1
              versionName "1.0"
              multiDexEnabled true // 新增的
              manifestPlaceholders = [
              JPUSH_APPKEY: "c0c08d3d8babc318fe25bb0c",	//在此替换你的APPKey
              APP_CHANNEL: "developer-default"		//应用渠道号
              ]
          }
      ```

      ---

      ```js
      dependencies {
          implementation fileTree(dir: "libs", include: ["*.jar"])
          implementation "com.facebook.react:react-native:+"  // From node_modules
          compile project(':jmessage-react-plugin') // 新增的
          compile project(':jcore-react-native')  // 新增的
          if (enableHermes) {
              def hermesPath = "../../node_modules/hermes-engine/android/";
              debugImplementation files(hermesPath + "hermes-debug.aar")
              releaseImplementation files(hermesPath + "hermes-release.aar")
          } else {
              implementation jscFlavor
          }
      }
      ```

   3. 根目录下新建文件和添加以下配置 `react-native.config.js`,注意传参为false表示显示提示，传参为true表示不显示提示

      ```js
      module.exports = {
        dependencies: {
          'jmessage-react-plugin': {
            platforms: {
              android: {
                packageInstance: 'new JMessageReactPackage(false)'
              }
            }
          },
        }
      };
      ```

   4. `android\settings.gradle` 加入如下配置

      ```js
      include ':jmessage-react-plugin'
      project(':jmessage-react-plugin').projectDir = new File(rootProject.projectDir, '../node_modules/jmessage-react-plugin/android')
      include ':jcore-react-native'
      project(':jcore-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/jcore-react-native/android')
      ```

   5. 修改node_modules/jmessage-react-plugin/android/src/io/jchat/android/JMessageReactPackage.java

      ```java
      package io.jchat.android;
      
      import com.facebook.react.ReactPackage;
      import com.facebook.react.bridge.JavaScriptModule;
      import com.facebook.react.bridge.NativeModule;
      import com.facebook.react.bridge.ReactApplicationContext;
      import com.facebook.react.uimanager.ViewManager;
      
      import java.util.ArrayList;
      import java.util.Collections;
      import java.util.List;
      
      public class JMessageReactPackage implements ReactPackage {
      
          private boolean mShutdownToast;
      
          public JMessageReactPackage(boolean shutdownToast) {
              mShutdownToast = shutdownToast;
          }
      
          @Override
          public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {
              List<NativeModule> result = new ArrayList<>();
              result.add(new JMessageModule(reactContext, mShutdownToast));
              return result;
          }
      
          public List<Class<? extends JavaScriptModule>> createJSModules() {
              return Collections.emptyList();
          }
      
          @Override
          public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
              List<ViewManager> viewManagers = new ArrayList<>();
              viewManagers.add(new BubbleMsgManager());
              return  viewManagers;
          }
      }
      ```

      

   6. Demo.js测试

   ```javascript
   import React, { Component } from 'react'
   import {View,Text} from 'react-native'
   import JMessage from 'jmessage-react-plugin'
   export default Demo = () => {
     React.useEffect(()=>{
       //componentDidMount
       JMessage.init({
         'appkey':'1d792822bca5fc39454acaae',
         'isOpenMessageRoaming':true,
         'isProduction':false,
         'channel':''
       })
   
       JMessage.login({
         username:'18715156459',
         password:'xxx'
       },(res) => {
         console.log('登录成功')
         console.log(res)
       },(err) => {
         console.log('登录失败')
         console.log(err)
       })
   
       return () => {
         //componentWillUnmout
       }
     },[])
   
     return (
       <View>
         <Text>Oh,我的上帝</Text>
       </View>
     )
   }
   ```