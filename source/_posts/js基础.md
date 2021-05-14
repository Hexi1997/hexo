---
title: js基础
date: 2021-05-09 23:50:17
type: tags
tags: [大前端,js]
categories: js
---

# 基础总结深入

## 数据类型

* 基本（值）类型：String Number Boolean undefined null

  ​		undefined : 定义了但是没有赋值

  ​		null：定义了并且赋值为null 

  ​				什么时候给变量赋值为null：1、定义变量还没获取到要赋值的数据的时候，先赋值为null  

  ​																 2、给对象设置为null，通过js GC来回收数据

* 对象（引用）类型：Object Function Array

* {} 和 null的区别 https://www.cnblogs.com/mmykdbc/p/9140756.html

* Function和Array都是特别的Object

* Function一种特别的对象，可以执行

* Array一种特别的对象，数值下标，内部数据是有序的

* 数据类型：基本类型 对象类型

* 变量类型：基本类型 引用类型（地址）

* 函数传参：对象传地址，基本数据类型传值

  ```javascript
  var a = {age:12}
  var b = a
  a = {name:'tom',age:18}   //在堆内存中新生成一个对象，将对象地址给a重新赋值，a变量存储的地址改变，指向新的对象，这样的话,a b 分别指向不同的对象，互相不影响
  b.age = 14
  console.log(b.age,a.name,a.age)
  ```

* js中内存管理机制  https://www.cnblogs.com/wc-z/p/13259844.html

  

## 判断对象相等或者实例

* typeof：返回数据类型的字符串表达
  				typeof undefied 结果是 'undefined' 

  ​				typeof null 结果是 object

* instanceof 判断对象是否是指定数据类型的实例

  ​				null instanceof Object 结果是false

  ​				{} instanceof Object 会报错  c = {};   c instanceof Object结果是true

* ===   不仅判断值，还需要判断类型，只有两者都相同时，才会输出true 

  ​				null === null 结果是true

  ​			    {} == {} 结果是false

  ​				undefined === undefined 结果是true

  

# 函数

## 回调函数

* dom事件的回调函数
* 定时器回调函数
* ajax请求回调函数
* 生命周期回调函数

## IIFT

* 全称：Immediately-Invoked Function Expression 即调函数表达式
* 作用：隐藏实现 不会污染外部（全局）命名空间  经常用它来编写js模块

```javascript
(function(){   //匿名函数自调用
    var a = 3
    console.log(a+3)
})()
var a = 4
console.log(a)  //function中a是匿名函数中局部变量，不会污染全局，这里仍然输出 4
//javascript中两个匿名函数立即执行函数连写会出现问题，这是由于;不能正确插入导致的，详情参见链接：https://blog.csdn.net/czh500/article/details/103500470

;(function(){
    var a = 1
    function test(){
        console.log(++a)
    }
    window.$ = function(){
        //通过向window对象中添加属性向外暴漏一个全局函数
    	return {
            test:test //返回一个对象，对象中test属性存储的是函数对象
        }
    }
})()

$().test()  //调用test方法，输出结果是 2
```



## this指向

* this是什么
* 任何函数本质上都是通过某个对象调用的，如果没有指定，就是window调用的
* 所有的函数内部都有一个变量this
* 它的值是调用函数的当前对象

```javascript
function Person(color){
    this.color = color
    console.log(this)
    this.getColor = function(){
        console.log(this)
        return this.color
    }
    this.setColor = function(color){
        console.log(this)
        this.color = color
    }
}

Person('red')  //新建实例对象，但是没有赋予变量 打印第一个this。this指向window
var p = new Person('yellow')  //this指向新建的实例对象p
p.getColor()0                 //this指向调用getColor方法的对象p
var obj = {}
p.setColor.call(obj,'blank')  //call可以改变this，this指向obj对象
var test = p.setColor         //将保存的函数赋值给test
test()                        //test是直接调用的，不是p调用的，所以指向window
```



## js分号探讨

* js一条语句后面可以不加分号

* 是否加分号是编码风格问题，加不加都可以

* 下面两种情况不加分号会有问题

  ​	1、小括号开头的语句 例如即调函数表达式(function(){})()

  ​	2、中方括号开头的语句 例如 【1，3】.forEach(function()=>{}) 会报错

  解决：在行首加分号



# 函数高级

## 原型与原型链

### 函数的prototype

* 任何函数都有一个默认属性prototype，它默认指向一个Object空实例对象（即称之为：原型对象）
* 原型对象中有一个属性constructor，它指向函数对象
* 给原型对象添加属性（一般都是方法）
* 作用：函数的所有实例对象自动拥有原型中的属性（方法）

### 显式原型和隐式原型

* 每个函数function都有一个属性prototype，即显式原型(属性)
* 每个实例对象都有一个```__proto__```属性，即隐式原型(属性)
* 对象的隐式原型的值为其对应构造函数的显示原型的值，即```fn.__proto__ === Fn.prototype```  
* 总结：函数的显式原型是定义函数时默认添加的，隐式原型是构造函数创建对象时自动添加的
* 程序员能直接操作显式原型，不能操作隐式原型（ES6之前）

## 原型链

* 访问一个对象的属性时：现在自身属性查找，如果没有找到去```__proto__```这条链依次向上查找，如果最终在Object身上也没有找到，则返回undefined
* 别名：隐式原型链
* 作用：查找对象的属性(方法)
* 构造函数的实例对象自动拥有构造函数原型对象的属性（方法），利用的就是原型链
* 所有函数function的```__proto__```属性都是一样的，都是new Function函数对象的实例
* function Foo(){}  等价于 Function = new Function; var Foo = new Function()  
* 函数的显式原型指向的对象默认是空Object实例对象（但Object函数不满足）
* 所有函数都是Function的实例（包含Function本身和Object)
* Object是原型链的尽头，Object的原型对象Object.prototype的proto属性为null
* 读取对象的属性值时：会自动到原型链中寻找
* 设置对象的属性值时：不会查找原型链，如果当前对象中没有此属性，直接添加此属性并设置其值
* 方法一般定义在原型中，属性一般通过构造函数定义在对象本身上

```javascript
function Fn(){
    console.log('Fn')
}

cnosole.log(Fn.prototype instanceof Object)       //true
console.log(Object.prototype instanceof Object)   //false
console.log(Object.prototype.__proto__)           //null
console.log(Function.prototype instanceof Object) //true
console.log(Function.__proto__ === Function.prototype) //true
```



![原型链图1](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510220911.JPG)

![原型链图2](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510221156.png)

![原型链图3](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510221204.png)

![原型链图4](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510221246.png)



![原型链原理图](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510221343.png)

## instanceof

* 判断左边的对象是否是右边的实例

```javascript
function Foo(){}
var f1 = new Foo()
console.log(f1 instanceof Foo)           //true
console.log(f1 instanceof Object)        //true
console.log(Object instanceof Function)  //true
console.log(Object instanceof Object)    //true
console.log(Function instanceof Function) //true
console.log(Function instanceof Object)   //true
console.log(Object instanceof Foo)       //false  Object只能沿着原型链向上找
```



## 原型链面试题

```javascript
//第一题
function A(){
    
}
A.prototype.n = 1
var b = new A()
A.prototype = {
    n:2,
    m:3
} //这里新建一个Object实例对象并将新地址赋予A的显式原型，实例对象b的__proto__指向的对象地址没变
var c = new A()
console.log(b.n,b.m,a.n,a.m)  //1 undefined 2 3

//第二题：根据上面那张图进行比较__proto__属性逐级寻找
function F(){}
Object.prototype.a = function(){
    console.log('a()')
}
Function.prototype.b = function(){
    console.log('b()')
}
var f = new F()
f.a()  //a()
f.b()  //报错
F.a()  //a()
F.b()  //b()
```



## 变量提升与函数提升

* 变量声明提升：通过var关键字定义的变量，在定义语句之前就可以访问到，值是undefined

* 函数声明提升：通过function声明的函数（而不是通过var fun = function(){}形式），在之前就可以直接正常调用，箭头函数不存在函数提升

* 变量提升和函数提升是如何产生的？

  执行上下文

## 执行上下文

* 代码分类：全局代码 局部代码

* 全局执行上下文

  ​	在执行全局代码前将window确定为全局执行上下文

  ​	对全局数据进行预处理

  ​		var定义的全局变量==>undefined，添加为window的属性

  ​		function声明的全局函数==>赋值(fun)，添加为window方法

  ​		this==>赋值window

  ​	开始执行全局代码

* 函数执行上下文

  ​	在调用函数，准备执行函数整体之前，创建对应的函数执行上下文对象

  ​	对局部变量进行预处理

  ​		形参变量==>赋值(实参)==>添加为执行上下文属性

  ​		arguments==>赋值(实参列表)，添加为执行上下文的属性

  ​		var定义的局部变量==>undefined，添加为执行上下文属性

  ​		function声明的函数==>赋值(fun)，添加为执行上下文方法

  ​		this==>赋值(调用函数的对象)

* 全局/函数执行上下文中队局部变量进行预处理才导致了变量提升的情况

* 函数执行上下文只有在函数执行的时候才会产生，函数定义时不会产生

```javascript
var a = 10
var bar = function(x){
    var b = 5
    foo(x+b)
}
var foo = function(y){
    var c = 5
    console.log(a+c+y)
}
bar(10)  //不会报错，前面只是进行了函数的赋值，并没有执行函数，虽然函数定义是foo在bar的下面。在bar函数执行时foo函数已经被定义，因此不会报错
```

* 在全局代码执行前, JS引擎就会创建一个栈来存储管理所有的执行上下文对象
* 在全局执行上下文(window)确定后, 将其添加到栈中(压栈)
* 在函数执行上下文创建后, 将其添加到栈中(压栈)
* 在当前函数执行完后,将栈顶的对象移除(出栈)
* 当所有的代码执行完后, 栈中只剩下window

```javascript
 /*
 测试题1: 
 */
 function a() {}
 var a;
 console.log(typeof a)  //先执行变量提升，后执行函数提升，然后按照代码顺序进行变量赋值，后面的覆盖前面的，也就是变量赋值会覆盖函数提升，函数提升会覆盖变量提升。结果是 function（这样理解可以得出正确结果，但是有一定争议），参见 https://www.cnblogs.com/snandy/archive/2012/03/01/2373237.html
 
 //测试题1.5
var a;
function a(){}
console.log(typeof a) //结果仍然是function

//测试题2
function a(){}
var a = 1
console.log(typeof a)  //结果是number

 /*
 测试题2: 
 */
 if (!(b in window)) {
 var b = 1;
 }
 console.log(b)  //if条件语句不是函数，其内部定义变量是公共变量 ，结果是 1 （这是错误理解）
				 //if条件语句中的b是全局变量，由于全局变量的提前声明特性，var b; 因此b in window永				     远为true，永远不会进入到if条件中，因此打印结果是undefined (这是正确理解)
 /*
 测试题3: 
 */
 var c = 1
 function c(c) {
     console.log(c) 
     var c = 3
 }
 c(2)
//测试题3等价于
var c
function c(c){
    console.log(c) 
    var c = 3
}
//以上是变量提升和函数提升
c = 1
c(2)  //这里相当于window.c(2)，js没有那么智能，会自动寻找到c函数并执行，实际上他会找到最近的变量c，因此报错，这里涉及到一个问题
//JS编程时应该尽量避免变量名和函数名同名,否则会发生相互覆盖的问题.从实际测试效果来看,这种覆盖可以分为两种情况:
	//定义变量时只使用var定义变量,不分配变量初始值,此时函数的优先级更高,函数会覆盖变量;
	//定以变量时为变量指定了初始值,此时变量的优先级更高,变量会覆盖函数.
```



# 作用域与作用域链

## 作用域

* 理解：就是一块地盘，一个代码所在的区域
* 它是静态的（相对于上下文对象），在编写代码时就确定了
* 分类：
  * 全局作用域
  * 函数作用域
  * 块作用域(ES6才有)

* 作用：隔离变量，不同作用域下同名变量不会有冲突

## 作用域和上下文对象的区别

* 区别1
  * 全局作用域之外，每个函数都会有自己的作用域，这个作用域是在函数定义时就确定了，而不是在函数调用时
  * 全局执行上下文环境是全局作用域确定之后，js代码马上执行之前创建
  * 函数执行上下文是在调用函数时，函数体代码执行之前创建
* 区别2
  * 作用域是静态的，只要函数定义好了就一直存在
  * 执行上下文是动态的，调用函数时创建，函数调用结束时会自动释放
* 联系
  * 上下文环境（对象）从属于所在作用域
  * 全局上下文环境==》全局作用域
  * 函数上下文环境==》对应的函数作用域

​        

## 作用域链

* 作用域的层层嵌套，从内层向外找

```javascript
//面试题
 var x = 10;
 function fn() {
 console.log(x);
 }
 function show(f) {
 var x = 20;
 f();
 }
 show(fn);  //10，fn函数作用域是全局作用域

 var fn = function () {
 	console.log(fn)  //打印function (){console.log(fn)}
 }
 fn()
 var obj = {
 fn2: function () {
 	console.log(fn2)  //fn2首先在function作用域内找，没找到，然后就去全局作用域找，还是没有，所以报错，对象的{}不是作用域
    console.log(this.fn2) //this指向调用的对象，这样就不会报错
 }
 }
 obj.fn2()
```



# 闭包

## 如何产生闭包

* 当一个嵌套的内部（子）函数引用了嵌套的外部（父）函数的变量时，就产生了闭包
* 嵌套+引用变量，缺一不可

## 闭包到底是什么

* 使用chrome调试查看
* 理解一：闭包是嵌套的内部函数  （绝大部分人）
* 理解二：包含被引用变量（函数）的对象 （极小部分人）
* 注意：闭包存在于嵌套的内部函数中



## 产生闭包的条件

* 函数嵌套
* 内部函数引用了外部函数的数据（变量/函数）

```javascript
function fn1(){
		var a = 2
		var b = 'abc'
		function fn2(){     //执行函数定义fn1就会产生闭包（不用调用内部函数fn2）
			console.log(a,this)  //引用外部的变量a，闭包，this指向window
			//如果只是console.log(this) ,没有引用外部变量a，所以没有产生闭包Closure
		}
		fn2()
}
fn1()
```



* 将函数作为另一个函数的返回值（高阶函数、函数柯里化概念）

```javascript
function fn1(){
    var a = 2
    function fn2(){
        a++
        console.log(a)
    }
    return fn2
}
var f = fn1()   //怎么看产生闭包的数量，就看它外部函数运行几次，这里只运行一次fn1()，所以只产生了一个闭包，通过运行f()只是对同一个闭包调用多次，从父级获取的a变量值是变化的
f()   //3
f()   //4
```

* 将函数作为实参传递给另一个函数调用

```javascript
function showDelay(msg,time){
    setTimeout(function(){
        alert(msg)
    },time)
}
showDelay('atguigu',1000)
```



* 使用函数内部的变量在函数执行完后仍然存活在内存中（延长了局部变量的生命周期）
* 让函数外部可以操作（读写）到函数内部的局部变量（变量/函数）



* 问题：
  * 函数执行完毕后，函数内部声明的局部变量是否还存在？   
    *  一般情况下除了闭包引用的变量，其他的局部变量都不存在
    * 闭包调用的a变量存在
    * fn2变量不存在，因为它没有人引用，是垃圾对象
    * fn3变量也不存在，因此将fn3的地址return出去之后赋值给f变量，所以闭包对象还存在，但是局部变量fn3不存在
  * 在函数外部能直接访问函数内部的局部变量么？
    * 不能，但是我们可以通过闭包让外部操作它

```javascript
function fn1(){
    var a = 1
    function fn2(){
        a++
        console.log(a)
    }
    function fn3(){
        a--
        console.log(a)
    }
    return fn3
}
var f = fn1()   //生成闭包
f()        //0
f()        //-1
```

## 闭包的生命周期

* 产生：在嵌套函数定义执行完就产生了（不是在调用）
* 死亡：在嵌套的内部函数中称为垃圾对象时

```javascript
function fn1(){
    //此时闭包fn2就已经产生（函数提升，内部函数对象已经创建了)
    var a = 2
    function fn2(){
        a++
        console.log(a)
    }
    var fn3 = function(){   //此时闭包fn3才产生，因为它时var定义的赋值语句，只会在进行fn3的变量提升，到这一行进行变量赋值时才会产生闭包，赋值语句即使不调用外部变量也会形成闭包Closure(原因不理解)
        console.log('fn3')
    }
    return fn2
}
var f = fn1()
f()  // 3
f()  // 4
f = null //闭包死亡（包含闭包的函数对象成为垃圾对象）
```



## 闭包的应用

* 闭包的应用：定义JS模块
  * 具有特定功能的js文件
  * 将所有的数据和功能封装在一个函数中
  * 只向外暴漏一个包含n个方法的对象或函数
  * 模块的使用者：只需要通过模块暴露的对象调用方法来实现对应的功能

* 第一个版本，使用function封装，比较麻烦

```javascript
//模块文件 module1.js
function myModule(){
    var msg = 'my atguigu'  //msg是私有数据
    function doSomething(){ //闭包
        console.log('doSomething'+msg.toUpperCase())
    }
    function doOtherthing(){  //闭包
        console.log('doOtherthing'+msg.toLowerCase())
    }
    return {  //向外暴露方法：触发对象简写方式
        doSomething,   
        doOtherthing
    }
}
```

```html
//使用module1.js中的方法
<script src='module1.js' type='text/javascript'></script>
<script>
    	var myModule1 = myModule()
        myModule1.doSomething()  //调用doSomething方法
    	myModule1.doOtherthing()  //调用doOtherthing方法
</script>
```



* 第二个版本，使用匿名自调用函数

```javascript
//module2.js
(function(window){
    var msg = 'my atguigu'  //msg是私有数据
    function doSomething(){ //闭包
        console.log('doSomething'+msg.toUpperCase())
    }
    function doOtherthing(){  //闭包
        console.log('doOtherthing'+msg.toLowerCase())
    }
    window.myModule2 = {  //匿名自调用函数向外暴露方法只能将方法挂载到全局对象window上
        doSomething,   
        doOtherthing
    }
})(window)
```

```html
//使用module2.js中的方法
<script src='module2.js' type='text/javascript'></script>
<script>
        myModule2.doSomething()  //调用doSomething方法,这样调用更方便
    	myModule2.doOtherthing()  //调用doOtherthing方法
</script>
```



* 第二种方法更优秀，使用起来更方便，匿名自调用函数+闭包实现模块，如果是对象方式，对象的属性是直接可见的，不太好



## 闭包的缺点

* 缺点
  * 函数执行完后，函数内的局部变量没有释放，占用内存事件会变长
  * 容易造成内存泄漏
* 解决
  * 能不用闭包就不用
  * 及时释放，赋值为null
* 内存溢出：程序运行需要的内存超过了剩余的内存，会抛出内存溢出的错误
* 内存泄漏：占用的内存没有及时释放，内存泄漏多了就容易导致内容溢出
  * 常见的内存泄漏
    * 意外的全局变量
    * 没有及时清理的计时器或回调函数
    * 闭包

```javascript
function fn(){
    a = 1
}
console.log(a)  //1, a就是意外的全局变量
```



## 练习题

```javascript
//代码片段一
var name = "The Window";
var object = {
 name : "My Object",
 getNameFunc : function(){
 return function(){  //不是闭包，没有引用父级function内的局部变量
 return this.name;
 };
 }
};
alert(object.getNameFunc()()); // The window


//代码片段二
var name2 = "The Window";
var object2 = {
 name2 : "My Object",
 getNameFunc : function(){
 var that = this;
 return function(){  //是闭包，嵌套+引用变量that
 return that.name2;   
 };
 }
};
alert(object2.getNameFunc()()); //My Object

//代码片段三
function fun(n,o) {
     console.log(o)
     return {
         fun:function(m){
            return fun(m,n);
         }
     };
 }
 var a = fun(0); a.fun(1); a.fun(2); a.fun(3);//undefined,0,0,0
 var b = fun(0).fun(1).fun(2).fun(3);//undefined,0,1,2
 var c = fun(0).fun(1); c.fun(2); c.fun(3);//undefined,0,1,1
```



# 面向对象高级

## 对象创建模式

* Object构造函数模式
  * 套路：先创建空的Object对象，再动态添加属性/方法
  * 适用场景：起始时不确定对象内部数据
  * 问题：语句太多

```javascript
var p = new Object()
p.name = 'Tim'
p.age = 12
p.setName = function(name){
    this.name = name
}
p.setName('JACK')
```



* 对象字面量形式
  * 套路：适用{}创建对象，同时指定属性/方法
  * 适用场景：起始时多个对象
  * 问题：如果创建多个对象，会有重复代码

```javascript
var obj = {
    name:'tom',
    age:12,
    setName:function(name){
        this.name =  name
    }
}
obj.setName('jack')
```



* 通过工厂函数创建

  * 套路：通过工厂函数创建对象并返回
  * 适用场景：需要创建多个对象
  * 问题：对象没有一个具体的类型，都是Object类型

  实际上工厂函数就是把创建函数的代码封装成一个函数，只要传参就可以生成Object对象

```javascript
//创建Person对象的工厂函数
function createPerson(name,age){
    return {
        name,
        age,
        setName:function(name){
            this.name = name
        }
    }
}
```



* 自定义构造函数模式
  * 套路：自定义构造函数，通过new创建对象
  * 适用场景：需要多个类型确定的对象
  * 问题：每个对象都有相同的数值，浪费内存

```javascript
function Person(name,age){
    this.name = name
    this,age = age
   // this.setName = function(name){  //每new一个Person实例都会创建该方法，因此放在原型链上
    //   this.name = name
    //}
}
Person.prototype.setName = function(name){
    this.name = name
}
var p1 = new Person('tom',18)  //p1是Person的实例

function Student(name,price){
    this.name = name
    this.price = price
    this.setName = function(name){
        this.name = name
    }
}
var s1 = new Student('tom',13300)  //s1是Student的实例
```



## 原型链继承

* 套路
  * 定义父类型的构造函数
  * 给父类型原型添加方法
  * 定义子类型的构造函数
  * 创建父类型的对象并赋予子类型的原型
  * 将子类型原型的构造属性设置为子类型
  * 给子类型原型添加方法
  * 创建子类型的对象，可以调用父类型的方法

![原型链图5](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510221408.png)



# 进程机制与事件机制

## 进程与线程

* 进程
  * 程序的一次执行，它占有的一片独有的内存空间
  * 可以通过window任务管理器查看进程
  * 进程之间相互独立
* 线程
  * 是进程的一个独立执行单元
  * 是程序执行的一个完整流程
  * 是CPU最小的调度单元
* 一个应用程序有1-N个进程，1个进程有1-M个线程
* 应用程序必须运行在某个进程的某个线程上
* 一个进程至少要有一个运行的线程：主线程，进程启动后自动创建
* 一个进程也可以同时运行多个线程，我们会说程序是多线程运行的
* 一个进程内的数据可以供其中的多个线程直接共享
* 线程池（Thread Pool)：保存多个线程对象的容器，实现线程对象的反复利用（开启、暂停、关闭）



* 单线程与多线程比较
  * 多线程
    * 优点：能够有限提升CPU的利用率
    * 缺点：线程创建、通信、切换开销，  死锁与状态同步问题
  * 单线程
    * 优点：顺序变成，简单移动
    * 缺点：效率低
* JS是单线程还是多线程
  * 异步单线程
  * 但是适用H5中的Web Worker可以多线程运行

* 浏览器运行是多线程的
* 浏览器运行时单进程还是多进程？
  * 有的单进程(Firefox)、有的多进程(Chrome 新版IE)

## 浏览器内核

* 支持浏览器运行的最核心程序
* 不同浏览器可能不同
  * Chrome Safari：Webkit
  * firefox : Gecko
  * IE : Trident
  * 360、搜狗等国内浏览器：Trident + Webkit(双核双驱动)
* 内核由很多模块组成
  * 主线程
    * js引擎模块：负责js程序的编译与运行
    * html/css文档解析模块：负责页面文本的解析
    * DOM/CSS模块：负责dom/css在内存中的相关处理
    * 布局和渲染模块：负责页面的布局和效果的绘制（内存中的对象）
    *  。。。。
  * 分线程
    * 定时器模块：负责定时器的管理
    * 事件响应模块：负责事件的管理
    * 网络请求模块：负责ajax请求

## 定时器思考

* 定时器真的是定时执行的么？
  * 定时器并不能保证真正定时执行
  * 一般会延迟一点点（可以接受），也有可能延迟很长时间（不能接受）
* 定时器回调函数是在分线程执行的吗？
  * 在主线程执行的，js是单线程
* 定时器如何实现的？
  * 时间循环模型

```javascript
document.getElementById('btn1').onclick = function(){
    var start = Date.now()
    console.log('启动定时器前')
    setTimeout(function(){
      console.log('定时器执行了',Date.now()-start)  
    },200)
    console.log('启动定时器后')
    //做一个长时间的工作，会导致定时器执行的时间增加，不等于自身定时的200ms
    for(let i = 0; i < 1000000000; i++){
        
    }
}
```



## JS线程思考

* 如何证明JS执行是单线程的？

  * setTimeout()的回调函数是在主线程执行的
  * 定时器回调函数只有在运行栈中的代码全部执行完后才有可能执行

* 为什么js要用单线程模式，而不用多线程模式

  * JS的单线程，与它的用途有关
  * 作为浏览器脚本语言，JS的主要用途是与用户交互，以及操作DOM，用户交互一般都是单线程的
  * 这代表了他只是单线程，否则会带来很复杂的同步问题

* 代码的分类

  * 初始化代码
  * 回调代码

* js引擎执行代码的基本流程

  * 先执行初始化代码：包含一些特别的代码，例如开启定时器、绑定事件监听、发送ajax请求
  * 后面某个时刻才会执行回调代码：多个回调代码的执行也是有先后顺序，而不是同时的

  回调函数（异步执行：等初始化代码执行完毕以后才能执行回调代码）

```javascript
	setTimeout(function(){
		console.log('tomeout 222','代码执行了',Date.now()-start)   //一般是代码执行了 4000
		alert(222)
	},5000)
	setTimeout(function(){
		start = Date.now()
		console.log('timeout 111')
		alert(111)
	},1000)
	setTimeout(function(){
	   console.log('timeout 000') 
	},0)
	function fn(){
		console.log('fn()')
	}
	console.log('alert之前')
	alert('-------------')     //暂停当前主线程的执行，同时暂停计时，点击确定后，恢复计时
	console.log('alert之后')


    //以上代码的输出结果顺序是
    //alert之前
    //alert之后
    //timeout 000
    //timeout 111
    //tomeout 222 代码执行了 3999

```



## 事件循环模型

## 模型原理图

![事件轮询模型图](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510221420.png)

* 执行栈
  * stack：所有的代码都是在此空间中执行
* 浏览器内核
  * browser core
  * js引擎模块（主线程处理)
  * 其他模块（在主/分线程处理)
* 回调队列callback queue
  * 任务队列
  * 消息队列
  * 事件队列
* 事件轮询 event loop
  * 从任务队列中循环取出回调函数放入执行栈中处理（一个接一个）
* 事件驱动模型：就是上面那张图



# H5 Web Workers(多线程)

* H5规范提供了js分线程的实现，取名为: Web Worker
* 相关API
  * Worker：构造函数，加载分线程执行的独立js文件
  * Worker.prototype.onmessage：用于接受另一个线程的回调函数
  * Worker.prototype.postMessage：向另一个线程发送消息
* 不足
  * worker内代码不能操作DOM(更新UI)，因为worker内全局上下文不是window
  * 不能跨域加载js
  * 不是每个浏览器都支持这个新特性
  * 相比于直接在主线程运行，效率慢

```html
<html>
    <head>
        <title>web worker测试</title>
    </head>
    <body>
        <input type='text' placeholder='数值'/>
        <button id='btn1'>
            计算
        </button>
        <script>
        	var btn = document.getElementById('btn1')
            var worker = new Worker('worker.js')
            function clickCallback(event){
                alert(event.data)
            }
            btn.onclick = function(){
                var input = document.getElementsByTagName('input')[0]
                const number = input.value
                worker.onmessage = clickCallback  //定义接受分线程结果的回调函数
                worker.postMessage(number)        //主线程向分线程传递参数number
            }
        </script>
    </body>
</html>
```

```javascript
//worker.js，一般都是固定写法 onmessage postMessage注意不用worker实例来调
function Fei(number){
    return number<=2?1:Fei(number-1)+Fei(number-2)
}
console.log(this)   //worker.js中全局上下文this不是window，因此没法进行dom操作
var onmessage = function(event){   //不能用类型声明
    var number = event.data * 1  //获取主线成传递过来的number
    var result = Fei(number)  //计算
    postMessage(result)       //将结果传递给主线程
}
```

## 图解

![WebWorker图解](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510221430.png)