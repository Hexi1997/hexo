---
title: ES6知识点集合
date: 2021-05-14 12:27:48
tags: [大前端,js,es6]
categories: js
---

## 简介
* 定义：ECMAScript是由Ecma国际通过ECMA-262标准化的脚本程序设计语言
* Ecma国际制定了许多标准，而ECMA-262只是其中的一个，ES6就是ES2015
* ES6版本变动内容多，加入许多语法特性，是前端发展趋势
* ES6兼容性，可以用babel进行转换成ES5等更低级的标准以支持IE等浏览器
* ES6是JS语言的标准，是被浏览器支持的，只是支持程度不同，和TypeScript不同，TS需要被编译成JS才会被浏览器执行

## 语法
### let关键字
* *let不能重复声明同一变量
* let的作用域是块级作用域，只在作用域内生效
* var则不同，var没有块级作用域，如果在方法中声明类型为var的变量，则为局部变量；如果在全局中声明，则为全局变量
```javascript
function fn() {
    var a = 1;
    console.log(a);
}
fn(); //1
console.log(window.a); //undefined

if (1) {
    let girl = 'zhou';
    var boy = "luo"; //boy为全局变量，可以使用window.boy调用获取值
}
//可以输出var变量定义的值
console.log(boy);
//不可以获取到let定义的变量值
console.log(girl);

function get() {
    var test = "test";
}
console.log(test) //无法获取到test的值，因为是在方法中定义的

//let不存在变量提升
console.log(song); //不会报错,会打印出undefined，没定义的变量提升undefined
var song = '恋爱达人';
console.log(song2); //报错，无法继续执行
let song2 = '恋爱达人2';

//let不影响作用域链，当前块级作用域内没有找到变量，会去父级寻找
{
    let school = "11";
    function fn3() {
let school = '22';
fn();
    }

    function fn() {
console.log(school);
    }
    fn3();
}

//获取div元素对象
let items = document.getElementsByClassName('item');
for (let i = 0; i < items.length; i++) {
    items[i].onclick = () => {
console.log(this);
this.style.background = 'pink' //无论let还是var都生效,this指向item[i]，如果将function换成箭头函数，则失效，this指向window
//items[i].style.background = 'pink'; //不生效，如果将for循环中var变成let则生效
    }
}

//this
let i = 0;
{
     i = 0;
     items[0].onclick = () =>{
this.style.background = 'pink';
     }
}
{
    i = 1;
     items[1].onclick = function(){
this.style.background = 'pink';
     }
}
{
    i = 2;
     items[2].onclick = function(){
this.style.background = 'pink';
     }
}
```

### 声明常量const
```javascript
//const声明时一定要赋初始值
//一般常量名使用大写，不是规则
//不允许给常量再次赋值
//也是块级作用域
//对于数组和对象的元素修改，不算做对常量的修改，不会报错
const SCHOOL = "尚硅谷";
console.log(SCHOOL);

const TEAM = ['uzi', 'mlxg'];
TEAM.push('Hexi'); //不会报错
console.log(TEAM);
TEAM = '1'; //会报错
```

### 解构赋值
```javascript
const F4 = ['1', '2', '3'];
let [first, second, third] = F4;
// 声明和定义变量方便
const zhao = {
name: '赵本山',
age: '不详',
xiaopin: function() {
    console.log('我可以演小品');
}
    }
    // let {
    //     name,
    //     age,
    //     xiaopin
    // } = zhao;
    // xiaopin();
    // console.log(name, age, xiaopin);
let {
    xiaopin
} = zhao;
```

### 模板字符串``(反单引号)
```javascript
//引入了新的声明字符串的方式`` '' ""
let str = `111`;
console.log(str);
//模板字符串``可以直接出现换行符
str = `111
    222   `;
let love = '111';
//可用来拼接字符串
let out = `${love}222`;
console.log(out);
```

### 对象的简化写法
```javascript
let name = "1";
let change = function() {
    console.log('2');
}
const school = {
    name, //等价于 name:name
    change,
    //imporve :function(){console.log('3')} 可以被简写成下面格式
    improve() {
console.log('3');
    }
}
```

### 箭头函数
```javascript
//ES6中允许使用箭头定义函数
let fn = function() {}
let fn2 = (a, b) => {
    return a + b;
}
let result = fn2(1, 2);
console.log(result);

//箭头函数中this是静态的，this始终指向函数声明时所在作用域下的this的值
//箭头函数没有自己的this值，箭头函数中所使用的this都是来自函数作用域链，它的取值遵循普通普通变量一样的规则，在函数作用域链中一层一层往上找。
//不会被call()、reply()等方法改变
function getName() {
    console.log(this.name);
}
let getName2 = () => {console.log(this.name);
//设置window对象的name属性
window.name = '尚硅谷';
const school = {
    name: 'aguigu'
}
// getName();  //尚硅谷
// getName2(); //尚硅谷
getName.call(school); //aguigu
getName2.call(school); //尚硅谷

//不能作为构造实例化对象
let Person = (name, age) => {
    this.name = name;
    this.age = age;
}
let me = new Person('xiao', 30); //会报错,Person is not a constructor
console.log(me);

//不能使用arguments变量，可以使用rest变量
let fn = () => {
    console.log(arguments); //报错，arguments is not defined
}
fn(1, 2, 3);

//箭头函数的简写
//省略小括号（形参只有一个) let add = n => {return n+n;}
//省略花括号（代码体只有一条语句，此时return 必须省略 let add = n => n+n;
```

### 箭头函数的实践
```javascript
let ad = document.getElementById('ad');
ad.addEventListener('click', function() {
    // setTimeout(function() {
    //     //修改背景颜色
    //     this.style.background = 'black';  //修改失败，这个this是指向window的
    // }, 2000);
    setTimeout(() => {
        this.style.background = 'black'; //修改成功，这个this是指向ad的
    }, 2000);
});

var A = {
    name: 'B',
    sayHello: function() {
        console.log(this.name)
    }
}
A.sayHello(); //输出B,用箭头函数则打印出空

var PI = "a";
if (true) {
    console.log(PI); // ReferenceError: PI is not defined
    const PI = "3.1415926"; //如果此行存在，则报错，因为匹配的是代码块里面的const类型PI变量。ES6 明确规定，代码块内如果存在 let 或者 const，
    //代码块会对这些命令声明的变量从块的开始就形成一个封闭作用域。代码块内，在声明变量 PI 之前使用它会报错。
    //如果此行不存在，会打印a，因为匹配到的是var类型的全局变量PI
}
console.log(0.3 === (0.1 + 0.2)) //浮点数相加，返回false,需要用Number.EPSILON(容差)来判断
console.log(0.3 === 0.3) //判断成功
console.log(bl1); //打印不知道类型的变量会报错，如果后面再使用var bl1 = 1;进行变量定义，则会给变量提升为undefined类型，如果是let类型定义，仍然会报错

//数组筛选也可以用箭头函数
const arr = [1, 4, 8, 9, 10];
const result = arr.filter(item => item % 2 === 0)
console.log(result);

//箭头函数适合与this无关的问题，定时器，数组的方法回调
//箭头函数不适合与this有关的问题，事件回调，对象的方法（不适合用箭头函数，用function就可）
```

### 函数参数的默认值
```javascript
//形参初始值
//具有默认值的参数一般位置哟啊靠后
function add(a, b, c = 10) {
    return a + b + c;
}
let result = add(1, 2)
console.log(result);

//默认值可以与解构赋值结合
function connect({
    host = '127.0.0.1',
    username,
    password,
    port
}) {
    console.log(host, username, password, port);
}
connect({
    host: 'localhost',
    username: 'root',
    password: 'root',
    port: '3306'
})

//rest参数，ES6引入rest，用来获取函数的实参，用来代替arguments
//rest参数必须放到参数最后，不一定非要叫rest，也可以叫别的
//ES5获取实参的方式
function date() {
    console.log(arguments); //是object类型Arguments(2)
}
date('柏芝', '阿娇');
//ES6获取实参的方式
function date2(...args) {
    console.log(args); //输出['1','2']，是数组类型
}
date2('1', '2');
//箭头函数中也可以用...rest
let fn = (...args) => {
    console.log(args);
}
fn(1, 2, 3, 4)
```

### ES6扩展运算符
```javascript
//[...] 扩展运算符能够将数组转换为逗号分隔的参数序列
let arr = [1, 2, 3];
let ovj = {...arr
}; //ovj是对象 {0: 1, 1: 2, 2: 3}

const tfboys = ['1', '2', '3'];

function chunwan() {
    console.log(arguments);
}
chunwan(...tfboys);

//扩展运算符用于数组合并
let a = [1, 2];
let b = ['3', () => console.log(1)]
let c = [...a, ...b];
console.log(c);

//数组克隆,如果sanzhihua中有引用类型，包括对象、方法等，也是浅拷贝
const sanzhihua = ['E', 'D'];
const c = [...sanzhihua];
console.log(c);

//伪数组转换为数组
const divs = document.querySelectorAll('div');
console.log(divs); //NodeList类型，其_proto属性为NodeList，并且有Symbol.iterator迭代器，可以展开并转换为数组
const d = [...divs];
console.log(d); //输出为数组

//TS本来就是JavaScript的超集，所以用纯ES6写TS也是没问题的
```

### Symool
```javascript
//ES6中引入了一种新的原始数据类型Symbol，表示独一无二的值
//他是JS的第七种数据类型，是一种类似于字符串的数据类型
//Symbol的值是唯一的，用来解决命名冲突的问题
//创建Symbol
let s = Symbol();
console.log(s, typeof s);
let s2 = Symbol('尚硅谷');
let s3 = Symbol('尚硅谷');
console.log(s2 === s3); //false

//Symbol.for创建
let s4 = Symbol.for('尚硅谷');
let s5 = Symbol.for('尚硅谷');
console.log(s4 === s5); //true

//Symbol不能与其他数据进行运算
//USONB  JS数据类型总结
//U undefined
//S string symbol
//O object
//N null number
//B boolean

//symbol的属性
//向对象中添加方法up down
let game = {
    up: () => console.log('up'),
    down: () => cnosole.log('down')
}

//game.up = () => console.log('up1');  //会覆盖game中的up方法
let methods = {
    up: Symbol(),
    down: Symbol()
}
game[Symbol.for('up')] = () => console.log('newup');
game[Symbol.for('down')] = () => console.log('newdown');
console.log(game[Symbol.for('up')]); //()=>console.log('newup')
let youxi = {
    name: '狼人杀',
    [Symbol.for('say')]: () => console.log('我自爆')
}
console.log(youxi[Symbol.for('say')])

//Symbol内置值
```

### 迭代器
```javascript
//迭代器(Iterator)是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构
//只要实现了Iterator接口，就可以完成遍历操作
//ES6创造了一种新的遍历命令for ... of循环，Iteratro接口主要供for ... of消费
//原生具备iterator接口的数据 array arguments set map string typedarray nodelist
//工作原理
//第一步：创建一个指针对象，指向当前数据结构的起始位置
//第二步：第一次调用对象的next()方法，指针自动指向数据结构的第一个成员
//第三步：接下来不断调用next()方法，指针一直往后移动，直到指向最后一个成员
const a = ['唐僧', '孙悟空', '猪八戒', '沙僧'];
//使用for...of遍历数组
for (let v of a) {
    console.log(v); //of打印值，in打印索引
}
let iterator = a[Symbol.iterator]();
console.log(iterator);
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());

const banji = {
    name:'终极一班',
    students:['汪东城','罗志祥','金城武'],
    [Symbol.iterator](){
        let index = 0;
        return {
            next:()=>{
                if(index < this.students.length){
                    let result = {
                        value:this.students[index],
                        done:false
                    }
                    index++;
                    return result;
                }else{
                    return {
                        value:undefined,
                        done:true
                    }
                }
            }
        }
    }
}

//只要实现了Iterator接口的数据结构就可以用for of循环获取值
for(let v of banji){
    console.log(v);
}

//实现了Iterator接口的数据结构都可以用扩展运算符 ... 展开
let arr = [...banji];
console.log(arr); //["汪东城", "罗志祥", "金城武"]
```

### 生成器
```javascript
//生成其实就是一个特殊的函数
//异步编程 纯回调函数，一层层回调,回调地狱
function* gen() {
    console.log(arguments);
    let one = yield 111
    console.log(one);
    //console.log(2);
    let two = yield 222
    console.log(two);
    let three = yield 444
    console.log(three);
}
let iterator = gen('AAA');

console.log(iterator.next());
//next()在调用时可以传实参
console.log(iterator.next('BBB'));
console.log(iterator.next('CCC'));
console.log(iterator.next('DDD'));
//生成器函数中必须调用next进行逐步执行,分步骤关键字是yield
iterator.next(); //输出hello generator
iterator.next(); //输出first step

for (let v of gen()) {
    console.log(v);
}

//实例--使用场景
//整个JS是异步单线程,文件操作,网络操作(ajax,request) 数据库操作
//1s后控制台输出111 2秒后控制台输出222 3秒后控制台输出333
function fn1() {
    setTimeout(() => {
        console.log('111');
        iterator.next();
    }, 1000);
}

function fn2() {
    setTimeout(() => {
        console.log('222');
        iterator.next();
    }, 2000);
}

function fn3() {
    setTimeout(() => {
        console.log('333');
    }, 3000);
}

function* gen() {
    fn1();
    yield 1
    fn2();
    yield 2
    fn3();
    yield 3
}

let iterator = gen();
iterator.next();
iterator.next(); //如果写3个iterator.next(),则111 222 333出现只间隔1秒
iterator.next(); //因为settimeout方法是异步方法,执行完第一行iterator.next()以后会立即执行第二行iterator.next()
iterator.next(); //基本上相当于三个定时器同时开始计时,一个1秒,一个2秒,一个3秒,所以会间隔1秒输出一个
//因此可以iterator.next()方法放在settimeout函数之中

//实例 模拟获取用户数据 订单数据 商品数据
function getUsers() {
    setTimeout(() => {
        let data = '用户数据';
        iterator.next(data);
    }, 1000);
}

function getOrders() {
    setTimeout(() => {
        let data = '订单数据';
        iterator.next(data);
    }, 2000);
}

function getGoods() {
    setTimeout(() => {
        let data = '商品数据';
        iterator.next(data);
    }, 3000);
}

function* gen() {
    let userdata = yield getUsers()
    let orderdata = yield getOrders();
    let goodsdata = yield getGoods();
    yield '3'
}

let iterator = gen();
iterator.next();
```

### Promise
```javascript
//Promise是ES6引入的异步编程新的解决方案,语法上Promise是一个构造函数,
//用来封装异步操作并可以获取其成功或失败的结果
//Promise可以解决回调地狱问题
const p = new Promise(function(resolve, reject) {
    setTimeout(() => {
        let data = '数据库中的用户数据';
        let error = '数据读取失败';
        //resolve(data);  成功,进入then的第一个回调
        reject(error); //失败,进入then的第二个回调
    }, 2000);
})

//调用Promise对象的then方法
p.then(function(value) {
    console.log(value); //resolve
}, function(reason) {
    console.error(reason); //reject
})

//使用Promise封装
//见index.js文件,html中无法引入fs模块对文件进行操作

//使用Promise发送AJAX请求
const p = new Promise(function(resolve, reject) {
    const xhr = new XMLHttpRequest(); //1 创建对象
    xhr.open('GET', 'http://api.apiopen.top/getJ2oke') //初始化
    xhr.send() //发送
    xhr.onreadystatechange = function() {
        if (xhr.readyState === 4) {
            if (xhr.status >= 200 && xhr.status < 300) {
                //成功
                // console.log(xhr.response);
                resolve(xhr.response);
            } else {
                //失败
                // console.log(xhr.status);
                reject(xhr.status);
            }
        } else {
            //失败
            // console.log(xhr.status);
            reject(xhr.status);
        }
    }
}).then(value => {
    console.log(value);
}, reason => {
    console.log(reason);
})



//Promise.prototype.then
//创建Promise对象
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
reject('数据库连结失败');
    }, 2000);
})

// //catch方法,捕捉错误,reject
p.catch(err => {
    console.warn(err);
})

/*13 集合Set*/
//类似数组,但是集合中不包含重复的值
let set = new Set(['1', '2', '1']);
console.log(set);
console.log(set.size);
set.add('3');
console.log(set);
console.log(set.has('3'));
set.delete('2');
console.log(set);
set.clear();
console.log(set);

//Set实践
let arr1 = [1, 3, 4, 5];
let arr2 = [1, 4, 6, 7];
//数组合并加去重,,求并集
let set1 = new Set([...arr1, ...arr2]);
//求交集
let set2 = arr1.filter(item => {
    let s2 = new Set(arr2);
    if (s2.has(item)) {
        return true;
    } else {
        return false;
    }
});
console.log(set2);
//求差集,同求交集,只是条件略微不同
```

### Map字典
```javascript
//ES6提供了Map数据结构,它类似于对象,也是键值对的集合,但是键的范围不限于字符串,各种类型的值(包括对象)都可以当作键
//Map也实现了Iterator接口,所以可以使用[拓展运算符] 和 for...of...遍历.
//map.size  元素个数
//map.set 增加一个元素
//map.get 返回键名对象的键值
//map.delete 删除
//map.has 检测map中是否包含某个键,返回boolean值
let m = new Map();
m.set('key1', 'value1');
console.log(m.get('key1'));
console.log(m.has('key1'));
// m.clear();
for (let v of m) {
    console.log(v);
}
```

### class类
```javascript
//ES6提供了更接近传统语言的写法,引入了Class这个概念,作为对象的模板,通过Class关键字,可以定义类,基本上,ES6的class
//可以看作只是一个语法糖,它的绝大部分功能,ES5都可以做到,新的Class写法只是让对象原型的写法更加清晰 更像面向对象编程的语法而已
//知识点
//class声明类
//constructor定义构造函数初始化
//extends继承父类
//super调用父级构造方法
//static定义静态方法和属性
//父类方法可以重写

//ES5构造类
function Phone(brand, price) {
    this.brand = brand;
    this.price = price;
}
Phone.prototype.call = function() {
    console.log('我可以打电话');
}
Phone.prototype.game = function() {
    console.log('我可以玩游戏');
}

let huawei = new Phone('华为', 5999);
huawei.call();
huawei.game();
console.log(huawei);

//ES6写法
class Phone {
    constructor(brand, price) {
        this.brand = brand;
        this.price = price;
    }

    //class中方法必须用该语法，不能使用function...
    call() {
        console.log('我可以打电话');
    }

    game() {
        console.log('我可以玩游戏');
    }
}

let huawei = new Phone('华为', 5999);
huawei.call();
huawei.game();
console.log(huawei);


//ES6
class person {
    constructor(name, age) {
    this.name = name;
    this.age = age;
}
//class类中不能写成eat:fucntion(){....},必须用缩写或者箭头函数
    eat() {
        console.log('i can eat');
    }
}

let ajie = new person('ajie', 18);
console.log(ajie);

//ES5 class静态成员
function Phone() {

}
Phone.prototype.size = '5.5inch';
Phone.name = '手机';
Phone.change = function() {
    console.log('我可以改变世界');
}
let nokia = new Phone();
console.log(nokia.size)   //5.5inch
console.log(nokia.name);  //undefined
nokia.change();   //is not a function
//类（Phone）和实例化对象之间通过prototype沟通，给类设置class.prototype.属性或者方法，对象中才可以获取或者调用

//ES6
class Phone {

    //类中static修饰的方法不可以由实例化对象调用，可由自身类调用
    //Phone.name可以调用
    //huawei.name不可以调用
    //静态的方法只能写在class内，通过static关键字标识
    static change() {
        console.log('我可以改变世界');
    }

    static color = '#000';

    constructor(name, size) {
        this.name = name;
        this.size = size;
    }
}

//静态属性只能写在class外，通过 类名.属性名 = 属性值 声明
//ES6明确规定，静态属性无法通过在class类内static关键字声明
//现在有一个提案提供了类的静态属性，写法是在实例属性法的前面
//加上static关键字
Phone.color = '#fff';

let huawei = new Phone('华为', 100);
console.log(huawei);
console.log(huawei.color);

//ES5实现类的继承
function Phone(brand, price) {
    this.brand = brand;
    this.price = price;
}

Phone.prototype.call = function() {
    console.log('call');
}

Phone.prototype.text = function() {
    console.log('text');
}

function SmartPhone(brand, price, size, color) {
    Phone.call(this, brand, price); //call改变作用域实现继承Phone
    this.color = color;
    this.size = size;
}


SmartPhone.prototype = new Phone;
SmartPhone.prototype.constructor = SmartPhone;

//子类独有方法
SmartPhone.prototype.game = function() {
    console.log('game');
}

console.log(SmartPhone);
let sm = new SmartPhone('华为', 1000, '5.5inch', '#fff');
sm.call();
sm.game();
sm.text();
console.log(sm);

//ES6实现类的继承 extends
class Phone {
    constructor(brand, price) {
        this.brand = brand;
        this.price = price;
    }

    call() {
        console.log('call');
    }
}

class SmartPhone extends Phone {
    constructor(brand, price, size, color) {
        super(brand, price); //继承父类构造方法
        this.size = size;
        this.color = color;
    }

    photo() {
        console.log('photo');
    }

    call() {
        //重写父类方法
        console.log('视频通话')
    }
}

let xiaomi = new SmartPhone('小米', 1000, '5.5inch', '#fff');
xiaomi.call();
xiaomi.photo();
console.log(xiaomi);

//Class属性的get和set，访问控制
class Phone {
    get price() {
        console.log('价格属性被获取了');
        return '111'
    }

    //set必须要有一个形参，接受传入的值
    set price(value) {
        console.log('价格属性被设置');
    }

    constructor(brand, price) {
        this.brand = brand;
        this.price = price;
    }
}

let ph = new Phone('huawei', 100);
ph.price = 'free';
console.log(ph.price);

class Chef {　　
    constructor(food) {　　　　
        this.food = food;　　　　
        this.dish = [];　　
    }

    //getter
    get menu() {　　　　
        return this.dish　　
    }

    //setter　
    set menu(dish) {　　　　
        this.dish.push(dish)　　
    }
　
    cook() {　　　　
        console.log(this.food)　　
    }
}

let zhangsan = new chef();
console.log(zhangsan.menu = 'tomato'); //tomato
console.log(zhangsan.menu = 'pizza'); //pizza
console.log(zhangsan.menu); //["tomato","pizza"]
```

### 数值拓展
```javascript
//Number.EPSILON是JS表示的最小精度，可用来判断浮点数的相等
//判断浮点数是否相等方法封装
function floatNumberEquals(a, b) {
    if (Math.abs(a - b) < Number.EPSILON) {
        return true;
    } else {
        return false;
    }
}
console.log(floatNumberEquals(0.1 + 0.2, 0.3));
console.log(floatNumberEquals(0.3, 0.3));

//二进制和八进制
let b = 0b1010;
let o = 0o777;
let d = 100;
let x = 0xff;


//Number.isFinite 检测一个数值是否为有限数
console.log(Number.isFinite(100));
console.log(Number.isFinite(100 / 0));
console.log(Number.isFinite(Infinity));

//Number.isNaN 检测一个数值是否为NaN
console.log(Number.isNaN(1234));

//Number.parseInt Number.parseFloat字符串转整数
console.log(Number.parseInt('111'));
console.log(Number.parseFloat('111.111'));

//Number.isInteger 判断一个数是否为整数

//Math.trunc 将数字的小数部分抹掉
console.log(Math.trunc(3.4));

//Math.sign，判断一个数到底为正数、负数还是0
//正数返回结果为1，负数返回结果为-1，0的返回结果为0

//Object.is 判断两个值是否完全相等
//作用和全等号===类似，但有不同之处
console.log(Object.is(NaN, NaN)); //true
console.log(NaN === NaN); //false
console.log(Object.is(120, 120));

//Object.assign 对象合并
//用于做配置合并比较合适
const obj1 = {
    host: 1,
    name: 2,
    get() {
        console.log('get');
    }
}
const obj2 = {
    host: 2,
    test() {
        console.log(test);
    }
}
console.log(Object.assign(obj1, obj2));

//Object.setPrototypeOf 设置原型对象 Object.getPrototypeof
const school = {
    name: '尚硅谷'
}
const cities = {
    xiaoqu: ['北京', '上海', '深圳']
}

Object.setPrototypeOf(school, cities);
console.log(school);
```

### 模块化
```javascript
//模块化是指将一个大的程序文件，拆分成许多小的文件，然后
//将小文件组合起来

//模块化的好处：防止命名冲突、代码复用、高维护性

//ES6之前的模块化规范有
//CommonJS => NodeJS、Browserify
//AMD => requireJS
//CMD => sealJS

//历史上，JS一直没有模块(module)体系，无法将一个大程序拆分成互相依赖的小文件
//再用简单的方法拼装起来。在ES6之前，社区制定了一些模块加载方案，最主要的有CommonJS和AMD两种，前端用于服务器，后者用于浏览器。
//ES6在语言标准的层面上，实现了模块功能而且实现的相当简单，完全可以取代CommonJS和AMD规范，成为通用的模块解决方案

//ES6模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。
//CommonJS和AMD模块，都只能在运行时确定，比如CommonJS模块就是对象，输入时必须查找对象属性
//CommonJS模块
let {stat,exists,readFile} = require('fs');

//等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readFile = _fs.readfile;

//上面的代码实质上时整体加载fs模块（即加载fs的所有方法），生成一个对象（_fs）,然后再从这个对象上面读取三个方法，这种加载称为运行时加载，因为只有运行时才能得到这个对象，导致完全没办法在编译时做静态优化

//ES6模块不是对象，而是通过export命令显示指定输出的代码，再通过import命令输入
//ES6 模块
import {stat,exists,readFile} from 'fs';
//上面代码的实质是从人fs模块加载3个方法，其他方法不加载。这种加载称为编译时加载或者静态加载
//而ES6可以在编译时就完成模块加载，效率要比CommonJS模块的加载方式高。当然，这也导致没法引入ES6模块自身,因为它不是对象

//ES6的模块自动采用严格模式，不管你有没有在模块头部加载'user strict'

//ES6模块功能主要由两个命令构成import和export。export命令用于规定模块的对外接口
//import命令用于输入其他模块提供的功能

//一个模块就是一个独立的文件，该文件内部的所有变量，外部无法获取，如果你希望外部能够读取模块内部的某个变量,就必须使用export关键字输出该变量。
//profile.js
export var firstname = 'michael';
export var lastname = 'jackson';
export var year = 1958;
//上面代码时profile.js文件，保存了用户信息，ES6将其视为一个模块，里面用export命令对外补输出了三个变量

//ES6-babel对ES6模块进行代码转换,项目中一般用webpack打包
```

### ES7
Array.includes() 检查数组中是否包含某个值，返回值是true/false
### ES8
#### async+await
* async和await两种语法集合可以让异步代码像同步代码一样
* async函数的返回值为Promise对象
* Promise对象的结果由async函数执行的返回值决定
* await表达式
* await必须写在async函数中
* await右侧的表达式一般为promise对象
* await返回的是promise成功（resolve)的值
* await的promise失败了，就会抛出异常，需要通过try catch捕获处
```javascript
async function fn() {
    //return '尚硅谷'; //返回字符串，只要方法被async关键字修饰，字符串仍然会被转换为Promise类型
    //只要返回的结果不是Promise类型的对象，则函数返回结果就是成功的Promise

    //抛出错误
    //throw new Error('出错了');

    //返回结果是Promise对象,由promise对象决定函数的返回resolve或者reject
    return new Promise((resolve, reject) => {
        reject('错误');
    })
}

const result = fn();

result.then(value => {
    console.log(reason)
}, reason => {
    console.error(reason);
})
console.log(result);

//await
const p = new Promise((resolve, reject) => {
    reject('yonghu');
})

async function main() {
    try {
        let data = await p;
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}

main();
```

#### Object对象新方法
* Object.values()方法返回一个给定对象的所有可枚举属性值的数组
* Object.entries()方法返回一个给定对象自身可遍历属性`[key,value]`的数组
* Object.getOwnPropertyDescriptor()方法获取对象描述对象，可以进行深层次克隆
* Object.fromEntries二维数组 转 键值对对象
```javascript
const result = Object.fromEntries([
    ['name', '尚硅谷'],
    ['xueke', 'Java 前端']
])
console.log(result);
```

### ES9
* 扩展运算符与rest参数
```javascript
function connect({
    host,
    port,
    ...args
}) {
    console.log(host, port, args); // 1 2 {username:3,password:4}
}

connect({
    host: 1,
    port: 2,
    username: 3,
    password: 4
});

const skillone = {
        q: '天音波',
        w: '金钟罩'
    }
    // ...skillone => q:'天音波',w:'金钟罩'
const skillTwo = {
    e: "铁布衫"
}

//使用扩展运算符对对象进行展开与合并
const ms = {...skillone,
    ...skillTwo
};
console.log(ms);
```

### 正则扩展
#### 命名捕获分组
```javascript
let str = `<a href="http://www.atguigu.com">尚硅谷</a>`;
//提取Url与标签文本,用?<变量名>防止正则的字符串内容变化导致数字索引变化，直接用变量指定
const reg = /<a href="(.*)">(.*)<\/a>/;
//执行
const result = reg.exec(str);
console.log(result[1], result[2]);
const reg2 = /<a href="(?<url>.*)">(?<text>.*)<\/a>/;
const result = reg2.exec(str);
console.log(result.groups.url, result.groups.text);

```

#### 声明字符串
```javascript
//声明字符串
let str = 'JS5211314你知道么555啦啦啦';
const reg = /\d+(?=啦)/; //正向断言，根据后面的内容来查找
const result = reg.exec(str);
console.log(result);

const regfan = /(?<=么)\d+/;  //反向断言
let result2 = regfan.exec(str);
console.log(result2);
```

#### 正则扩展的dot模式
```javascript
//\s表示匹配任何空白字符，包括空格、制表符、换页符等等, 等价于[ \f\n\r\t\v]
//而"\s+"则表示匹配任意多个上面的字符

//dot . 元字符 除换行符以外的任意单个字符
```

### 字符串trimStart trimEnd 清除字符串左边和右边空白
```javascript
let str = '   ilove    ';
console.log(str);
console.log(str.trimStart());
console.log(str.trimEnd());
```

### ES10
```javascript
//flat和flatMap
//flat 将多维数组转换为低维数组
const arr = [1, 2, 3, [5, 6, [8, 9]]];
console.log(arr.flat(2)); //flat方法中可传递数字参数，代表转换深度

//Symbol.description属性
let s = Symbol('111');
console.log(s.description);
```

### ES11
* 私有属性 #属性名
* Promise allSettled
* 动态加载import
```javascript
btn.onclick = function(){
    import('src/index.js').then(module =>{
        module.hello();
    })
}
```
* 全局this(globalThis),支持浏览器和nodejs。取代window，可以在浏览器和nodejs中同时运行


### 字符串正则批量获取
```javascript
const result = str.matchAll(reg);
for(let v of result){
    console.log(v);
}
```

### 可选链操作符?.
```javascript
//应对对象类型参数，参数层级比较深
let a= {
    b:{
        c:{
           d:1
        }
    }
}

function main(config){
    //这样写判断前面对象是否存在，比较麻烦
    const result = config && config.b && config.b.c && config.b.c.d;
    //短路表达式 简写
    const result2 = config?.b?.c?.d;
}

main(a);
```

### BigInt类型，用于大数值计算
```javascript
let n  = 123;
console.log(BigInt(n));
n = Number.MAX_SAFE_INTEGER;
console.log(BigInt(n)+BigInt(100));
```
### 

