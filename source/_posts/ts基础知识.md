---
title: ts基础知识
date: 2021-05-14 15:19:40
tags: [大前端,ts,js]
categories: ts
---

## 定义

* typescript 为js添加了类型系统，ts是js的超集，ts老大哥，js弟弟 ts>js
* node.js让js摆脱了浏览器束缚，可以实现服务端/桌面端编程
  微信小程序和微信小游戏都是基于node.js实现的

* ts定义： Type+JavaScript(为js添加了类型系统)、微软开发的开源编程语言，开发大型应用

## 优势
js的类型系统存在先天缺陷，绝大部分是类型错误。
优势一：类型化思维方式，开发严谨，提前发现错误，减少改bug时间
优势二：代码可读性好，维护和重构代码更容易
优势三：补充了接口、枚举等开发大型应用时的缺少功能

解析TS的工具包：
node.js只认识js代码，不认识TS代码，需要先将TS代码转换为JS，然后
就可以在node.js/浏览器中运行了

## 环境

在当前目录打开终端，输入命令tsc hello.ts，会生成一个hello.js文件
在Node.js环境中跑代码 node hello.js
麻烦，简化方式，使用ts-node包 npm i -g ts-node
ts-node hello.ts直接运行ts文件

单行注释 //
多行注释 /* */

输出语句console.log() 在终端中打印信息

## 知识点

### 变量使用

1：声明变量并指定类型 2：给变量赋值
let age:number = 1;

变量命名只能出现数字、字母、下划线和美元符号，不能以数字开头
变量区分大小写
变量的命名规范：1、有意义 2、驼峰命名法cityName

基本数据类型 number/string/boolean/undefined/null
对象数据类型 
number[] 类型为数字数组
number类型可包括 1 1.0 +1 -1
string类型可包括 "1" '1' " "
boolean类型可包括 true false
undefined类型的值为：undefined,声明但未赋值
let u:undefined = undefined;
null类型的值为：null，声明了变量并且赋值为null
let n:null = null;

* +可用于字符串拼接
  2-+'1' = 2 - 1 = 1
  +加字符串会将字符串转换为数值

* 匿名函数
  let person = {
      sayHi:function(){
          console.log('大家好，我是一个方法');
      }
  }
  对象是无序键值对的集合
  属性和方法的区别：值是不是函数

### 对象的类型注解：接口
TS中对象是结构化的，简单来说就是对象有什么属性或者方法
对象类型注解的语法类似对象自身的语法

```javascript
//对象类型注解
let person:{
    name:string;
    age:number;
}

//实例化
person = {
    name:"刘老师",
    age:18,
}

//对象方法的类型注解
//技巧：vscode中鼠标放在变量名称上，VSCODE会给出该变量的类型注解
let person:{
    sayHi:()=>void
    sing:(name:string)=>void
    sum:(num1:number,num2:number)=>number
}

person = {
    sayHi:function(){
        console.log("hi");
    },
    sing:function(name:string){
        console.log("sing:"+name);
    },
    sum:function(num1:number,num2:number){
        return num1+num2;
    }
}
//箭头左边小括号中的内容，表示方法的参数类型
//箭头右边的内容，表示方法的返回值类型
//方法类型注解的关键点：1参数 2返回值
//这样比较麻烦，代码不简洁，而且无法复用类型注解，因此这里引入接口

//接口：为对象的类型注解命名，并为你的代码建立契约来约束对象的接口
interface IUser{
    name:string
    age:number
    sayHi:()=>void
}

let pi:IUser = {
    name:"梨花",
    age:1,
    sayHi:function(){
        console.log("hi");
    }
}

console.log()可以有多个参数
console.log(str1,str2);

//内置对象，编程语言自带的对象
//文档--MDN W3School
//数组对象Push方法,push()方法将一个或多个元素添加到数组的末尾，
//并返回数组的length
//const 常量不能重新赋值

let animales:string[] = ["pigs","goats"];
animales.push("chicken","snake");
console.log(animales.length);
//forEach()方法遍历数组
animales.forEach(element =>{
    console.log(element);
})

animales.forEach(function(item,index){
    console.log(index,item);
})
//疑问，不需要为回调函数的参数或返回值指定类型注解吗？
//注意：回调函数是作为实参传入

//forEach性能问题，遍历整个数组，无法退出且效率低
//some方法：遍历数组，查找是否有一个满足条件的元素（如果有，就可以停止循环）
//循环特点：根据回调函数的返回值，决定是否停止循环，true就停止，false就继续
let nums:number[]=[1,2,123,23];
nums.some(function(num,index){
    if(num>10){
        console.log(index);
        return true;//停止循环
    }
    return false;
})

```

### 类型推论与类型断言

```javascript
//TS的类型推论
let num = 19 //num类型为number
let num2;num2 = 19;//num2类型为any

//浏览器中只能运行JS,无法直接运行TS，需要将TS转换为JS，然后再运行
//使用命令tsc将ts文件转换为js
//html页面引入js
//问题：每次修改ts过后，都要重新运行tsc命令转换为js
//解决方法，使用tsc的监视模式 tsc --watch tsLearn.ts

//获取一个DOM元素，只能获取到第一个，拿到的返回值元素类型都是Element,Element只包含所有元素的共有属性和方法
document.querySelector(selector)

let img = document.querySelector("#img1");
img.src报错，因为img是Element，没有src属性

//类型断言
let img = document.querySelector("#img1) as HTMLImageElement;

//console.dir打印Element原始对象，获取其细分类型

document.querySelectorAll(selector)  //获取所有与选择器参数匹配的DOM元素，返回值是一个列表


```

### 文本内容、样式操纵

```javascript
//操纵文本内容 dom.innerText = "";  dom必须是详细类型，不能是Element，需要进行类型断言

//操纵css样式，说明，所有的样式名称都与css相通，但命名规则为驼峰命名法
//ts/js中调用 style.fontSize   html/css中font-size   
//style.display
```

classList属性(类样式)：添加、移除、判断存在
p.classList.add('b')   b是css样式名
p.classList.remove('b')
let bexists:boolean = p.classList.contains('b')

### 操作事件


```javascript
//addEventListener，给dom对象绑定事件
//removeEventListener，给dom对象移除事件
//click点击事件
//mouseenter 鼠标进入事件
//回调函数
btn.addEventListener('click',function(event){
    console.log('点击了');
    console.log(event.type);    //事件类型
    console.log(event.target);  //获取点击对象
})

btn.addEventListener('click',funtion(){},{once:true})
//once:如果值为true，会在触发事件后，自动移除，只触发一次


```

### 函数声明形式的事件处理程序说明
//可以先使用函数，再声明函数,event使用时必须指定类型
btn.addEventListener('click',handclick)
function handclick(event:MouseEvent){

}

### 使用字面量进行类型声明

使用多个字面量进行声明可以理解为枚举类型，只能在这些数值之间取得
let b:"male"|"female";
b="male";   //正确
b="female"; //正确
b="1"       //编译器会提示报错

### any

一个变量类型设置为any相当于对该变量关闭TS的类型检测，使用TS时，不建议使用any类型
声明变量如果不指定类型和赋值，TS解析器会自动判断变量的类型为any（隐式的any)

### unknown

unknown表示未知类型的值
unknown和any的区别：unknown类型的变量不能赋值给别的变量，any可以
unknown是一个类型安全的any

```javascript
let e:unknown;
let s:string="1";
s=e;//报错
if(typeof e === "string"){
    s=e;
}//不报错
//可以使用类型断言
s = e as string;
```



### void

表示为空，以函数为例，就表示没有返回值的函数

```javascript
function fn():void{

}
```



### never

表示永远不会返回结果，一般用于抛出异常

```javascript
function fn2():never{
	throw new Error('报错了');
}
```

### 对象 

语法：{属性名:属性值,属性名:属性值}
在属性名后边加上?，表示属性是可选的

```typescript
let b:{name:string,age?:number};  //age可选

//{propName:string}:any表示任意类型的属性
let c:{name:string,[propName:string]:any};
c={name:"猪八戒",age:18,gender:"男"};  //只有name属性是必须的
```

### 函数

```typescript
let d:(a:number,b:number) => number;
d = function(num1:number,num2:number){
    return num1+num2;
}
```



### 数组与元组

```typescript
let e:string[]; //字符串数组
e=['a','b'];

let f:Array<number>;

//元组tuple，元组是固定长度的数组，效率高于普通叔祖
let h:[string,string];
```



### 枚举类型enum

JS中没有，TS新增的

```typescript
enum Gender{
    male,
    female
}
let i:{name:string,gender:Gender}

let j:{name:string} & {age:number};
j={name:'孙悟空',age:18};
```



### 类型别名

```typescript
type myType = 1|2|3|4|5;
let k:myType;
let l:myType;
```

## 配置

```json
//编译时监视文件夹下所有ts文件，在同级目录新建配置文件tsconfig.json，里面内容为{}，然后执行tsc即可将当前
//文件夹下所有ts文件编译成js文件
//tsc编译文件夹下所有文件 tsc -w监视所有文件
//tsconfig.json是ts编译器的配置文件
//tsconfig配置
{
    //指定要编译的文件，**表示任意目录 *表示任意文件
    "include":["","",""....],
    //指定不需要编译的文件,默认值["node_modules","bower_components","jspm_packages"]
    "exclude":["","",""....],
    //继承配置文件
    "extends":"",
    //指定被编译文件列表，只有需要编译的文件少才需要
    "files":[
        "",
        "",
        ""
    ],
    "compilerOptions":{
        //target指定ts被编译成的ES版本es3 es5 es6 es2015-es2020
        "target":"es2015",
        //指定使用的模块化规范(import语句之类)commonjs es6 es2015
        "module":"es2015",
        //lib指定要使用的库,一般情况下不需要设置
        "lib":["dom","",""....],
        //outDir用来指定编译后文件所在目录
        "outDir":"./dist",
        //outFile输出文件，用来将编程成的js代码合并成一个文件
        "outFile":"./dist/app.js",
        //allowJs,是否"对js文件进行编译。默认false
        "allowJs":true,
        //checkJs,是否检查JS代码规范，默认false
        "checkJs":true,
        //是否移除注释
        "removeComments":true,
        //不生成编译后的js文件,一般用来做语法检查
        "noEmit":false,
        //当有错误时不生成编译后js文件
        "noEmitOnError":true,
        //所有严格检查的总开关
        "strict":true,
        //alwaysStrict是否开始严格模式，默认false
        "alwaysStrict":true,
        //noImplicitAny，不允许隐式的any类型，比如函数的形参，一般人注意不到
        "noImplicitAny":true,
        //noImplicitThis,不允许隐式的this类型
        "noImplilcitThis":true,
        //strictNullChecks,严格的检查空值
        "strictNullChecks":true,
    }
}
```

一般很少只用ts编译。大型项目ts结合webpack打包工具编译ts代码

执行 npm init -y指令生成package.json文件，用来存储打包配置
安装webpack打包工具