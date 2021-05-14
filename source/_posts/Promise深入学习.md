---
title: Promise深入学习
date: 2021-05-14 13:53:22
tags: [大前端,js,Promise]
categories: js
---

## 剖析Promise

### 实例对象与函数对象

* function Fn(){ //Fn是函数}
* let fn = new Fn() //fn是实例对象
* console.log(Fn.prototype);  //Fn是函数对象
* Fn.bind({}); //Fn是函数对象，函数对象才有bind apply call方法
* Fn.call({}); //Fn是函数对象
* $('#test'); //jquery函数
* $.get('/test') //jquery函数对象

### 两种类型的回调函数

* 同步回调：数组的回调一般都是同步回调
* 异步回调：定时器 网络请求 文件IO

### JS的Error处理

* Error:所有错误的父类型
* ReferenceError:引用的变量不存在
* TypeError: 数据类型不正确
* RangeError: 数据值不存在
* SyntaxError:语法错误

```javascript
console.log(a);  //ReferenceError: a is not defined
let b = null;
console.log(b.xsx);  //TypeError: Cannot read property 'xsx' of null
function fn(){
     fn(); //递归
 }

 fn();  //RangeError: Maximum call stack size exceeded
 const c = """";  //语法错误
```

* 捕获错误 try{}catch(err){}
* 抛出错误 throw new Error('错误)

### Promise的理解和使用

#### Promise是什么？

* Promise是JS中进行异步编程的新的解决方案
* 最老的解决方式是：纯回调方式，会产生回调地狱问题
* 旧的解决方案是 生成器+迭代器+yield标识符
* 新的解决方案是 Promise + async + await

* 从语法上来说，Promise是一个构造函数
* 从功能上来说：Promise用来封装一个异步操作并可以获取结果

#### Promise的状态改变

* pending 到 resolved
* pending到 rejected
* 只有这两种，且一个promise对象只能改变一次
* 无论变为成功还是失败，都会有一个结果数据
* 成功的结果数据一般为value，失败的结果数据一般为reason
* Promise 执行异步操作 成功了执行resolve()返回resolved状态的Promise对象，然后Promise对象.then回调onResolved，返回新的Promise对象，可以做链式调用
* Promise 执行异步操作，失败了执行reject()返回rejected状态的Promise对象，然后Promise对象.then或者catch()回调onRejected()返回新的Promise对象

#### Promise的基本使用

```javascript
const  p = new Promise((resolve,reject)=>{
    //执行异步操作
setTimeout(() => {
    const time = Date.now();
    if(time % 2 === 0){
        //成功了，调用resolve()
        resolve('成功的数据,time='+time);
    }else{
        //失败了，调用reject()
        reject('失败的数据,time='+time);
    }
}, 1000);    
})

p.then(value=>{
    console.log('成功的回调',value);
},reason=>{
    console.error('失败的回调',reason);
})
```

#### 回调地狱

* 普通回调方式，必须在定义时指定回调，不能事后指定，而Promise可以在const p = new Promise()之后随时调用p.then()，很灵活
* Promise有异常传递,异常处理非常方便
* Promise支持链式调用，可以解决回到地狱问题
* 什么时回调地狱：回调地狱嵌套调用，外部回调函数异步执行的结果时嵌套的回调函数执行
* 回调地狱的缺点：不便于阅读/不便于异常处理
* 回调一般产生于多个串联的异步操作，第二个回调依赖第一个的结果为条件，第三个依赖第二个的结果为条件。。。

* 解决回调地狱的终极解决方案：async+await+promise

#### Promise的进阶使用

```javascript
//API
//Promise(executor)
//executor函数:同步执行 (resolve,reject)=>{}
//resolve函数:内部定义成功时我们调用的函数 value => {}
//reject函数:内部定义失败时我们调用的函数reason =>{}
//说明:executor会在Promise内部立即同步回调，异步操作在执行器中执行

//Promise.prototype.then方法:(onResolved,onRejected)=>{}
//onResolved函数：成功的回调函数 value =>{}
//onRejected函数: 失败的回调函数 reason => {}
//说明:指定用于得到成功value的成功回调和用于得到失败reason的失败回调都返回一个新的Promise对象

//Promise.prototypee.catch()方法： (onRejected) => {}
//onRejected函数:失败的回到函数(reason) => {}
//说明：then()的语法糖，相当于： then(undefined,onRejected)

//Promise.resolve方法：(value) => {}
//value:成功的数据或者Promise对象
//说明:返回一个成功/失败的Promise对象

//Promise.all方法： (promises)=> {}
//promises:包含n个promise的数组
//说明:返回一个新的promise,只有所有的promise都成功才成功，只要有一个失败了就直接失败

//Promise.race方法， (promises) => {}
//promises:包含n个promise的数组
//说明:返回一个新的promise，第一个完成的promise的结果状态就是最终的结果状态

// new Promise((resolve,reject) => {
//     setTimeout(() => {
//         resolve('成功的数据');
//         //reject('失败的数据'); //如果resolve后再执行reject，reject修改Promise状态失效，Promise状态只能修改一次
//     }, 1000);
// }).then(value=>{
//     console.log('onResolved()1',value);
// },reason=>{
//     console.log('onRejected()2');
// })

//产生一个成功值为1的promise对象
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(1);
    }, 100);

})

const p2 = Promise.resolve(2);
// const p3 = Promise.reject(3);

// p1.then(value => {console.log(value)});  //1 
// p2.then(value => {console.log(value)});  //2
// p3.then(null,reason => console.log(reason)); //3
// p3.catch(reason => console.log(reason));  //3

//只有都成功了，才获取数据去处理
// const pAll = Promise.all([p1,p2]);
// pAll.then(values=>{
//     console.log('all onResolved',values);  //values是个p1,p2的返回值数组[1,2]
// },reason=>{
//     console.log('all onReject()',reason); //p1 p2失败的那个reason
// })

//多个异步任务看谁先完成
// const pRace = Promise.race([p1,p2]);
// pRace.then(value=>{
//     console.log(value);  //谁先执行完获取谁的值
// },reason=>{
//     console.log(reason); //失败的数据
// })

//如何改变promise的状态 reoslve reject 抛出异常
const p = new Promise((resolve, reject) => {
    resolve(1)
        //reject(2)
        //throw new Error('出错了'); //reason就是Error对象，抛出的什么，reason就是什么
})

//一个Promise指定了多个成功/失败回调函数，都会调用么？
//当promise改变为对应状态时都会调用
// p.then(value=>{
//     console.log('1',value)
// },reason =>{
//     console.log(reason);
// })

// p.then(value=>{
//     console.log('2',value);
// },reason=>{
//     console.log(reason);
// })

//改变Promise状态和指定回调函数谁先谁后？
//都有可能

//get类型ajax请求封装
// function getGetAjax(api) {
//     return new Promise((resolve, reject) => {
//         const xhr = new XMLHttpRequest();
//         xhr.open('GET', api);
//         xhr.send();
//         xhr.onreadystatechange = function() {
//             if (xhr.readyState === 4) {
//                 if (xhr.status >= 200 && xhr.status < 300) {
//                     resolve(xhr.response);
//                 } else {
//                     reject('失败' + xhr.status);
//                 }
//             } //后面不要加else条件reject，readState改变有几步， 2 ==》4，只有为4才获取成功，在变为4之前会改变为2，触发readystatechange事件
//         }
//     })
// }

// const apirequest = getGetAjax('https://api.apiopen.top/getJoke2').then(value => {
//     console.log(value);
// }, err => {
//     console.log(err);
// })

//Promise的状态
//pending 未决定的状态，初始状态
//resolved/fullfilled 解决(成功)状态
//rejected 拒绝(失败)状态

//一个Promise对象只能改变一次状态，由pending变resolved或者pending变rejected
//无论成功或者失败，都只有一个结果数据value/reason
//Promise是异步编程的新的解决方案
//Promise支持链式调用
//Promise的回调函数指定方式/时间灵活

//Promise实例对象的属性
//PromiseState
//PromiseResult，保存着异步任务的结果。 对象的值，只可以通过resolve()和reject()方法改变PromiseValue的值

//如何改变Promise的状态
//resolve(value):如果当前是pending就会变为resolved
//reject(reason):如果当前是pending就会变为rejected
//抛出异常 throw err：如果当前是pending就会变为rejected

//当一个promise指定多个成功/失败回调函数时，都会调用么？   是的，会调用多个回调函数
// const p = new Promise();
// p.then(); //回调函数1
// p.then(); //回调函数2

//改变promise状态和指定回调函数谁先谁后
//这个问题本质上就是 resolve()/reject()函数先执行还是 .then()方法先执行
//不确定，按照一般的代码结构，是先指定回调函数，然后promise状态改变
//也可以先改变状态，然后指定回调函数进行回调。
//如何先改变状态，再指定回调，可以把resolve/rejecct写在同步回调中。 或者给.then语句使用定时器延长执行

//什么时候才能得到数据？
//如果先指定回调，当Promise状态改变时，回调函数就会调用，得到数据
//如果Promise状态先改变，那么在指定回调函数时，回调函数就会调用，得到数据

//Promise如何串联多个操作任务，then()链式调用
//Promise的then()返回一个新的Promise，可以写成then的链式调用
//通过then的链式调用串联多个同步/异步任务
//then()方法中没写返回值，那么这个then()方法的返回值是一个默认的Promise，PromiseState为resolved/fullfilled, PromiseResult为undefined

//Promise异常传透
//当使用Promise的then链式调用时，可以在最后指定失败回调，这样不用每一个then都写失败回调reason=>{}
//.then(value=>{}).then(value=>{}).then(value=>{}).catch(err){}
//前面的任何操作出现了错误，都会传递到最后的失败回调中处理

//中断Promise链
//当使用promise的then链式调用时，在中间中断，不再调用后面的回调函数
//方法：在回调函数中返回一个pending状态的promise对象 then(value=>{ return new Promise(()=>{}) })
//返回pending状态,promise状态未改变，因此不会调用后面的回调函数

// const p = new Promise((resolve, reject) => {
//     resolve('ok');
// })

// const result = p.then(value => {
//     //then()方法的返回结果是一个Promise对象，Promise对象的返回结果取决于其回调函数返回结果
//     // return '1';  //非Promise类型的对象，返回值会被转换为Promise对象，PromiseResult为返回值，PromiseState为fullfilled/resolved
//     // throw new Error('错误'); //抛出异常，返回值也会被转换为Promise对象，PromiseResult为 错误对象，PromiseState为rejected
//     return new Promise((resolve, reject) => { //返回值是Promise对象，外层的Promise对象的值取决于内层Promise的返回值
//         // resolve('ok2');  // PromiseResult为ok2, PromiseState为fullfilled/resolved
//         reject('false'); //PromiseResult为false, PromiseState为rejected
//     })
// })

// console.log(result);


// const p1 = new Promise((resolve, reject) => {
//     // resolve(2); //如果resolve没有写在异步任务中，则先改变状态，然后指定回调函数
//     setTimeout(() => {
//         console.log('Promise状态改变');
//         resolve(1);
//     }, 1000);
// })

// //
// console.log('指定回调函数');
// p1.then(value => {

// })


// const p1 = new Promise((resolve, reject) => {
//     setTimeout(() => {
//         reject(1);
//     }, 1000)
// })

// const p2 = new Promise((resolve, reject) => {
//     reject(2);
// })

// const p3 = new Promise((resolve, reject) => {
//     reject(3);
// })

// // const pResult = Promise.all([p1, p2, p3]);
// // console.log(pResult);

// const pResult2 = Promise.race([p1, p2, p3]);
// console.log(pResult2);



// const p = new Promise((resolve, reject) => {
//     //内部代码是同步调用
//     console.log('111'); //因为是同步调用，所以最先输出的是111
//     //内部代码的异步方法是异步调用，例如定时器，网络请求，文件IO
//     resolve(1); //虽然立即执行回调函数，但是是异步执行的。因此1是在最后打印
// }).then(value => {
//     console.log(value);
// })
// console.log(222);
// //打印顺序是 111 222 1;

// let p1 = Promise.reject('1');
// //如果传入的参数是一个非Promise对象的值，则返回一个成功的Promise
// //如果传入的参数是一个Promise对象，则外层Promise对象的返回值取决于内层Promise对象的返回值
// console.log(p1);

```

## Promise流程图

![image-20210514141506114](https://gitee.com/chen_hexi/image-source/raw/master/img/Promise%E6%B5%81%E7%A8%8B%E5%9B%BE)

## 自己实现Promise

```javascript
//先写ES5，然后转ES6 class

//第一步：搭建基本结构Promise 和 then
//第二步：执行器函数(executor)在内步是同步调用的,因此需要执行executor();
//调用时由实例化对象传递resolve和reject实参，executor()方法需要定义形参resolve 和 reject接收
//resolve 和 reject都是函数对象re，声明方法即可 function resolve() ;  function reject();
//resolve和reject需要形参data接收实参值

//第三步：
//完善resolve方法，resolve()方法执行时Promise对象的状态会由pending变为fullfilled
//另外可以设置Promise对象成功的结果为data值

//由于需要设置Promise对象状态和结果，所以需要定义PromisState和PromiseResult属性
//类内部声明 this.PromiseState = 'pending';
//          this.PromiseResult = null;
//注意this的指向问题，两个function嵌套，内层function的this指向window，可以把内层fucntion改为箭头函数
//也可以在外层function用一个常量记录this值 const self = this;
//self.PromiseState 
//self.PromiseResult

//第四步：执行器函数中包含 throw代码，如何处理
//一般的Promise对象内部如果throw 一个对象，那么这个Promise返回的Promise对象的PromiseState变成rejected
//ProsmieResult的值就是抛出的对象。
//所以直接给executor()函数包裹上try catch即可。一旦catch，那么直接reject(e);

//第五步：Promise对象状态只能修改一次
//执行多次resolve()或者reject(),返回值永远是第一次的值和状态
//PromiseState的改变，只有两种可能 pending到fullfilled   pending到rejected
//因此在resolve()和reject()函数修改判断条件即可
//只有PromiseState为pending才修改状态和结果，否则就不修改。
//这样就确保了Promise对象只能修改一次

//第六步：then()方法执行回调
//回调函数onResolved()和onReject()是同步回调，不过回调函数里面的代码是异步执行的
//成功的话回调onResolved()
//失败的话回调onRejected()
//那么如何在Promise.prototype.then方法里面判断PromiseState的值呢
//通过原型链prototype指定的方法都是由实例对象调用，而不是由函数对象调用的
//一定会有一个Promise对象实例调用.then()方法，那么在.then()方法里面this指向的就是实例对象
//那么就可以通过this.PromiseState获取状态值判断，如果是fulfilled，那么执行onResolved(this.PromiseResult)
//如果是rejected，那么执行onRejected(this.PromiseResult)
//通过PromiseResult可以获取Promise对象实例的返回值，将其作为参数传入onResolved()和onRejected()

//第七步: 执行器函数executor中异步任务回调的执行
//很多情况下resolve()函数和reject()函数是异步执行的
//那么.then方法里，如果判断状态为pending，是否该执行onResolve和onRejectd回调函数呢，肯定不应该在这里执行
//异步任务的回调函数onResolved和onRejected执行时机是 PromiseState发生改变。也就是resolve()和reject()函数内
//那么Promise类里面的resolve()函数如何获取实例对象.then()方法指定的回调onResolved和onRejected呢？
//需要给Promise函数对象定义callback属性保存回调函数，等执行resolve()和reject() PromsieState状态改变以后再执行回调函数

//第八步：通过p.then()方法指定多个回调功能实现，Promise支持通过多个p.then()方法实现绑定多个onResolved和onRejected函数
//一旦PromiseState状态发生改变，就会触发指定的多个回调函数
//因此这里需要将callback属性改为callbacks对象数组，保存多个回调，然后等resolve()或reject()执行以后循环启动异步执行多个回调函数

//第九步：同步任务下then方法结果的返回
//then()方法的返回值是Promise对象，对象的状态取决于then方法内部的返回值
//如果没写返回值，那么then的返回值应该是一个空的Promise对象，PromiseState为pending,PromiseResult为undefind
//如果return 了一个非Promise类型的值。包括字符串,undefind啥的，会被转换为Promise对象，这个Promise对象的PromiseState
//为fulfilled，PromiseResult为return 的对象
//如果return 了一个Promise类型的对象，那么外层的Promise的返回值取决于内层的Promise的值
//执行onResolved()和onRejected()函数，那么需要兼容内部throw error抛出异常的情况，此时用 try{}catch(e){reject(e)}

//第十步：异步修改状态then方法结果返回
//这句话的意思就是生成器executor如果有异步操作，resolve()和rejected需要一段时间执行
//那么一开始.then()的返回值是pending状态的Promise
//执行resolve()/reject()的回调函数没有修改返回值
//返回值应该是和 onResolved() / onRejected()的返回值相关联的
//.then方法中当PromiseState状态为pending时这里不能用原来的this.callbacks.push({onResolved,onRejected})，
//这样在resolve()/reject()函数执行回调函数的时候没有修改Promise的值，Promsie的PromiseState仍然为pending
//PromiseResult为undefined，无法获取到.then()方法的返回值，因此这里需要将onResolved/onRejected的值修改为function方法，
//方法内执行onResolved函数获取返回值，判断返回值类型，如果是Promise类型，外层Promise类型的返回值取决于内层Promise
//如果不是Promise类型，直接resolve(返回值)即可。
//注意：在获取function里面获取实例对象PromiseResult属性的时候this会失效，需要在外层保存this或者使用箭头函数
//如果onResolved()和onRejected()代码里包括抛出异常，那么需要try catch

//第十一步：提取公共方法callback(type)，封装try catch语句。注意callback里面this指向问题

//第十二步：catch方法与异常穿透
//添加Promise.prototype.catch方法，此方法可以通过调用 实例化对象.then()方法实现，onResolved传undefined即可
//异常穿透 p.then(value=>{}).then(value=>{}).catch  then()方法中的错误可以被catch
//实现：.then()方法允许不传递onReject,需要自己赋一个初始化的值
//Promise的另一个特性，值传递，p.then().then(value={console.log(value)})  第一个then可以不传递onResolved和onRejected
//所以也需要给onResolved赋初始值
//p的promiseresult通过中间.then()方法不会丢失，会继续传递到最后面一个then方法中，这就是值传毒
//因为then()方法的默认onResolved函数时 return value;

//第十三步：Promise.resolve()方法实现,返回值是一个Promise类型对象
//Promise.resolve(value),如果value是Promise对象，那么外层的Promise的状态和值由内层的Promise的执行结果决定
//如果不是Promise对象，那么直接resolve(value)设置PromiseState为fulfilled，PromiseResult为value值

//第十四步：Promise.reject(reason)方法实现，返回结果永远是一个失败的Promise
//如果reason值是Promise对象，那么返回的PromiseResult就是这个reason,也就是Promise对象
//如果reason值是不是Promise对象，那么返回的PromiseResult也是这个reason

//第十五步：Promise.all(promises)方法实现，返回值类型是个Promise
//promises数组中所有方法全部成功，才成功,PromiseResult是所有Promise的结果组成的数组，PromiseState为fulfilled
//如果有不成功的Promise，那么PromiseState为rejected,PromiseResult就是第一个不成功的Promise的结果

//第十六步：Promise.race(promises)方法实现，返回值类型是个Promise
//promises数组中第一个成功的方法的返回值就是Promise.race的返回值

//第十七步：then()方法回调的异步执行,onResolved和onRejected方法是异步执行的
//使用定时器实现异步
//在resolve()和reject()方法内部调用回调函数数组callbacks也是异步执行的，用定时器实现

//ES5 声明类
function Promise(executor) {
    //添加属性
    this.PromiseState = "pending";
    this.PromiseResult = null;
    //保存then方法指定回调函数数组
    this.callbacks = [];

    //保存this值
    const self = this;

    function resolve(data) {
        if (self.PromiseState === 'pending') {
            //修改对象状态(PromiseState)
            self.PromiseState = "fulfilled";
            //设置对象结果值(PromiseState)
            self.PromiseResult = data;
            //调用成功的回调函数onResolved()
            //异步启动指定多个通过.then方法指定的回调函数
            setTimeout(() => {
                self.callbacks.forEach(item => {
                    item.onResolved(data);
                });
            });


        }

    }

    function reject(data) {
        if (self.PromiseState === 'pending') {
            //修改对象状态(PromiseState)
            self.PromiseState = "rejected";
            //设置对象结果值(PromiseState)
            self.PromiseResult = data;
            //调用失败的回调函数onResolved()
            //异步启动指定多个通过.then方法指定的回调函数
            setTimeout(() => {
                self.callbacks.forEach(item => {
                    item.onRejected(data);
                });
            });

        }
    }

    //同步调用执行器函数
    try {
        executor(resolve, reject);
    } catch (e) {
        //try catch语句处理执行器函数中包含thorow Error的情况
        reject(e);
    }
}

//实例化对象的then方法,返回对象类型是Promise
Promise.prototype.then = function(onResolved, onRejected) {
    //保存this值，也可以用箭头函数解决
    const self = this;
    if (typeof onRejected !== 'function') {
        //如果用户没有传递onReject回调，自己初始化一个，防止异常穿透的时候报错
        onRejected = reason => { throw reason }
    }

    if (typeof onResolved !== 'function') {
        onResolved = value => value;
    }

    return new Promise((resolve, reject) => {
        //方法封装
        function callback(type) {
            try {
                //获取回调函数的执行结果
                let result = type(self.PromiseResult);
                //result是Promise类型对象
                if (result instanceof Promise) {
                    result.then(v => {
                        resolve(v);
                    }, r => {
                        reject(r);
                    });
                } else {
                    //结果对象状态为成功
                    resolve(result);
                }
            } catch (e) {
                reject(e);
            }
        }

        if (this.PromiseState === 'fulfilled') {
            setTimeout(() => {
                callback(onResolved);
            });
        }
        if (this.PromiseState === 'rejected') {
            setTimeout(() => {
                callback(onRejected);
            });

        }
        if (this.PromiseState === 'pending') {
            //执行器函数中是异步回调，这里需要保存回调方法
            this.callbacks.push({ //这里不能用原来的this.callbacks.push({onResolved,onRejected})，
                //这样在resolve()/reject()函数执行回调函数的时候没有修改Promise的值，Promsie的PromiseState仍然为pending
                //PromiseResult为undefined，无法获取到.then()方法的返回值，因此这里需要将onResolved/onRejected的值修改为方法，
                //方法内执行onResolved函数获取返回值，判断返回值类型，如果是Promise类型，外层Promise类型的返回值取决于内层Promise
                //如果不是Promise类型，直接resolve(返回值)即可。
                //注意：在获取function里面获取实例对象PromiseResult属性的时候this会失效，需要在外层保存this或者使用箭头函数
                onResolved: function() {
                    callback(onResolved);
                },
                onRejected: function() {
                    callback(onRejected);
                }
            });
        }
    })

}

//添加catch方法进行错误捕捉
Promise.prototype.catch = function(onRejected) {
    return this.then(undefined, onRejected); //this指向实例化Promise对象
}

//添加Promise.resolve()方法
Promise.resolve = function(value) {
    return new Promise((resolve, reject) => {
        if (value instanceof Promise) {
            value.then(v => {
                resolve(v);
            }, r => {
                reject(r);
            })
        } else {
            resolve(value);
        }
    })
}

//添加Promise.reject(reason)方法，包裹reason生成一个失败的Promise对象
Promise.reject = function(value) {
    return new Promise((resolve, reject) => {
        reject(reason);
    })
}

//添加Promise.all(promises)方法
Promise.all = function(promises) {
    return new Promise((resolve, reject) => {
        //使用for循环同时执行promise
        let susCount = 0;
        //定义数组保存返回值，数组索引应该和promise在promises中的索引位置保存一一对应
        let arrResult = [];
        for (let i = 0; i < promises.length; i++) {
            promises[i].then(value => {
                arrResult[i] = value;
                susCount++;
                if (susCount === promises.length) {
                    //全部成功
                    resolve(arrResult);
                }
            }, reason => {
                reject(reason);
            })
        }
    })
}

//添加Promise.race(promises)方法
Promise.race = function(promises) {
    return new Promise((resolve, reject) => {
        promises.forEach(promise => {
            promise.then(v => {
                resolve(v);
            }, r => {
                reject(r);
            })
        })
    })

}
```

