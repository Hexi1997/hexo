---
title: react基础+进阶
date: 2021-05-14 14:39:00
tags: [大前端,react,dva]
categories: react
---

---
typora-copy-images-to: assects

---

# 一、React简介

## React全家桶

* React-Router(路由)、PubSub(消息管理)、Redux(集中式状态管理) Ant-Design(UI组件库)

## React定义

* React是用户构建用户界面的JavaScript库，react只关注页面（视图）
  发送请求获取数据（axios、fetch、ajax）
  处理数据（过滤、整理格式）
  操作DOM呈现页面(react工作)
* React是一个将数据渲染为HTML视图的开源JS库

## React谁开发的

* Facebook开发，且开源，Facebook工程师Jordan Walke创建，2011年部署于newsfeed
* 2012年部署于ins,2013年开源
* React被阿里、腾讯等一线大厂使用

## 为什么要学习React？

* 原生js操作DOM效率低且繁琐 document.getElementById('btn1')
* DOM API操作UI(样式)
* 使用js直接操作DOM,浏览器会进行大量的重绘重排
* 原生js没有组件化（模块化）编码方案，代码复用率低

## React的特点

* 采用组件化模式、声明式编码，提高开发效率及组件复用率
* React-Native中可以使用React语法进行移动端开发
* 使用虚拟DOM+优秀的DOM Diffing算法，尽量减少了与真实DOM的交互

## React学习之前要掌握的JS基础知识

* 判断this的指向
* class类
* ES6语法规范
* npm包管理器
* 原型、原型链
* 数组常用方法
* 模块化


# 二、hello-react案例

## 官网

* 中文网 https://react.docschina.org/
* 官网 https://reactjs.org/

## React基本使用

* 相关js库
  react.js:React核心库
  react-dom.js:提供操作DOM的react扩展库
  babel.min.js:解析JSX语法代码转换为js代码的库
  引用顺序react>react-dom

# 三、虚拟Dom的两种创建方式

## jsx创键虚拟Dom

## js创建虚拟Dom(不用)

# 四、虚拟DOM与真实DOM

## 虚拟dom

* 本质上是object类型的对象
* 虚拟dom比较轻，属性少，真实dom重
* 虚拟dom是react内部用，无需真实dom上那么多属性
* 虚拟dom最终会被react转换为真实dom呈现在页面上

# 五、jsx语法规则

## jsx

* 全称是javascript xml
* react定义的一种类似于xml的js拓展语法
* 本质上是React.createElement(componment,props,...children)方法的语法糖
* 作用:用来简化虚拟DOM的创建

```javascript
    写法 const（var) ele = <h1>111</h1>
    注意1：他不是字符串，也不是HTML/XML标签
    注意2：它最终产生的就是一个js对象
```

## 语法规则

* 定义虚拟dom,不要写引号
* 标签中混入js表达式要用{}
* 样式的类名指定不要用class,要用className
* 内联样式要用`style={{key:value}}`的形式去写，key即属性名，value为属性值，属性名依照小驼峰命名法（例如fontSize)
* 虚拟dom必须只有一个根标签
* 标签必须闭合
* 标签首字母
  小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错
  大写字母开头，react就去渲染对应组件，若组件没有定义，则报错
* 注意区分js语句和js表达式
  一个表达式会产生一个值，可以放在任何一个需要值的地方
      a
      a+b
      .demo(1)
      arr.map
      function demo()
  语句（代码）
      if()
      for(){}
      switch(){case}

# 六、jsx小练习

  在React JSX语法中，有时需要对数组或对象等进行遍历，数组的遍历有2种方式：forEach和map；
   但是需要知晓，JSX语法最终是要return返回html标签或对应内容的，而数组的遍历方法中forEach没有返回值，重在执行；map可以设置返回值。所以，在jsx中只能使用map方法进行遍历，需要知晓！

```javascript
            <div>
                {dataArr.map((item,index)=>{
                    return <h1 key={index}>{item}</h1>
                })}
            </div>
```

* 需要设置key属性，这是虚拟dom实现diffing算法的重要标志

# 七、组件与模块

## 模块（js按照功能分为不同模块）

* 理解：向外提供特定功能的js程序，一般就是一个js文件
* 为什么要拆成模块：随着业务逻辑增加，代码越来越多和复杂
* 作用：复用js，简化js编写，提高js运行效率

## 组件（拼乐高）

* 理解：用来实现局部功能效果的代码和资源的集合（html/css/js/img)等
* why: 一个界面复杂且繁琐
* 作用：复写编码，简化项目编码，提高运行效率

## 模块化

* 当应用的js都以模块来编写，这个应用就是一个模块化应用

## 组件化

* 当应用是以多组件的方式实现，这个应用就是一个组件化的应用

# 八、开发者工具的安装

## google浏览器安装 React-Developer-Tools

* 安装完React-Developer-Tools插件以后chrome调试页面会多出componment和profiler选项卡，componments用来查看当前页面有多少组件，profiler用来查看页面性能

# 九、函数式组件:适用于简单组件定义

## 代码

```javascript
//创建函数式组件 --首字母必须大写 函数必须有返回值
function Demo(){
    console.log(this) //输出是undefined babel编译后是严格模式，this不允许指向window，指向undefined
    return <h2>这是一个函数式组件</h2>
}
//引用函数式组件
ReactDOM.render(<Demo/>,document.getElementById('test'))
```

## 过程

* 执行了ReactDOM.render之后发生了什么？
  1、react解析组件标签，找到了Demo组件
  2、发现组件是函数定义的，调用该函数，将返回的虚拟DOM转为真实DOM,随后呈现在页面中

# 十、类的基本指示复习

```javascript
    class Person{
        //构造器方法
        constructor(name,age){
            //构造器的this指向实例对象
            this.name = name;
            this.age = age;
        }

        //一般方法
        speak(){
            //speak方法放在哪里？ -- 类的原型对象
            //speak方法给谁用？  --供实例对象使用
            //通过person实例调用时speak时，speak方法中的this指向person实例
            console.log(`我叫${this.name},我的年龄是${this.age}`)
        }
    }

    class Student extends Person{
        constructor(name,age,grade){
            super(name,age);
            this.grade = grade;
        }

        speak(){
            console.log(`我叫${this.name},我的年龄是${this.age},我读的是${grade}年纪`)
        }

        study(){
            //study放在哪里？——类的原型对象上，供student类实例对象使用
            //通过Student实例调用stuy方法时，study中的this指向实例对象
            console.log('我很努力学习');
        }
    }

    const p1 = new Person('tom',18);
    pi.speak(); //我叫tom，我的年龄是18
    pi.speak.call({}) //我叫undefined，我都年龄是undefined
```

## 总结

* 类中的构造器不是必须要写，要对实例进行一些初始化操作，如添加指定属性时才写
* 如果A类继承了B类，且A类写了构造器，那么构造器中super()必须要调用
* 类中所定义的非static方法，都是放在了类的原型对象上，供实例去使用

# 十一、类式组件

```javascript
//创建类式组件
class MyComponent extends React.Component{
    //必须要继承React.Component
    render(){
        //render是放在那里的？ 类的原型对象，供实例使用
        //render中this指向是谁？ MyComponent的实例对象，MyComponent组件实例对象
        return <h2>这是一个函数式组件</h2>
    }
}
//渲染组件到页面
ReactDOM.rend(<MyComponent/>,document.getElementById('test'))
```

## 过程

* 执行了ReactDOM.render之后发生了什么？
  1、react解析组件标签，找到了Demo组件
  2、发现组件是类定义的，随后new出来该类的实例
  3、并且通过该实例调用到原型上的render方法
  4、将render返回的虚拟DOM转为真实DOM,随后呈现在页面中

# 十二、组件实例的三大属性之一：state

## 理解

* state是组件最重要的属性，值是对象（可以包含多个key value的组合）
* 组件被称为状态机，通过更新组件的state来更新对应的页面显示

## 注意

* 组件中render()方法中的this指向实例化对象
* 组件自定义方法中this指向为undefined，如何解决？
  强制绑定this:通过函数对象bind()
  箭头函数
* 状态数据：不能直接修改或更新

## 实例

```javascript
    <script type="text/babel" > /* 此处一定要写babel */
        class Weather extends React.Component{
			//构造器只调用1次
			constructor(props){
				//必须放在第一行，只有super()之后才能使用this
				super(props);
				//初始化状态
                this.state = {isHot:true};
                //解决changeWeather中this指向问题
                this.changeWeather = this.changeWeather.bind(this)
			}

			//render()调用1+N次
			//初始化时调用1次
			//每次setState重新render()一次
			render(){
				console.log(this);
				if(this.state.isHot){
					//render中this指向类的实例对象
					//changeWeather通过类的实例对象才可以调用，this.changeWeather
					//由于changeWeather是作为onClick的回调，所以不是通过实例调用的，是直接调用
					//类中的方法默认开启了局部的严格模式，所以changeWeather中的this指向为undefined
					//所以this丢失，使用箭头函数或者bind()解决
					return <h1 onClick={this.changeWeather}>热</h1>
				}else{
					return <h1 onClick={()=>{this.changeWeather()}}>不热</h1>
				}
			}

			//点几次，调用几次
			changeWeather(){
				//不能通过给this.state.isHot让页面刷新
				//状态里的数据不能直接更改，通过setState方法更改
				//setState是对state进行更新，一种合并，而不是替换
				if(this.state.isHot){
					this.setState({
						isHot:false
					})
				}else{
					this.setState({
						isHot:true
					})
				}
			}

			
		}

        
		ReactDOM.render(<Weather/>,document.getElementById('test'))
	</script>
```

## 简写

```javascript
    <script type="text/babel" > /* 此处一定要写babel */
        class Weather extends React.Component{
			//构造器方法可省略
			//初始化状态,赋值语句给Weather实例对象添加state属性并赋值
			state = {isHot:false}
			//渲染
			render(){
				return <h1 onClick={this.changeWeather}>{this.state.isHot?'热':'不热'}</h1>
			}
			//自定义方法，用赋值语句加箭头函数解决this指向问题
			changeWeather = ()=>{
				const isHot = this.state.isHot;
				this.setState({isHot:!isHot})
			}
		}

		ReactDOM.render(<Weather/>,document.getElementById('test'))
	</script>
```

# 十三、react三大组件之二：props

## 简单例子

```javascript
 <script type="text/babel" > /* 此处一定要写babel */
        //创建组件
        class Weather extends React.Component{
            render(){
                return <h1>{this.props.weather}</h1>
            }
        }

		ReactDOM.render(<Weather weather='晴天'/>,document.getElementById('test'))
		ReactDOM.render(<Weather weather='阴天'/>,document.getElementById('test2'))
    </script>
```

## 扩展运算符回顾

```javascript
<script>let arr1 = [1,3,5,7,9];let arr2 = [2,4,6,8,10];console.log(...arr1); //展开一个数组let arr3 = [...arr1,...arr2];  //连接数组//数组使用reduce方法求和//在函数中使用扩展运算符function sum(...numbers){    return numbers.reduce((preValue,currentValue)=>{        //函数体        return preValue+currentValue;    })}console.log([1,3,4,5])let person = {name:'kobe',age:18};console.log(...person);  //报错，对象类型没有iterator接口，不能展开//复制一个Person,直接赋值的话只拷贝了引用，用扩展运算符的话进行浅拷贝，对象属性值仍然是指向同一个let person2 = person;  //person2和person实例指向一个实例对象let person3 = {...person};  //字面量赋值，扩展运算符进行对象浅拷贝person.name = 'curry';console.log(person2.name) //打印curryconsole.log(person3.name) //打印kobe//复制对象的同时修改属性let person4 = {...person,name:'lebron',age:22}console.log(person4);</script>
```

## 批量传递props

```javascript
class Person extends React.Component{
    render(){
        console.log(this);
        //解构赋值
        const {name,age,sex} = this.props;
        return(
            <ul>
                <li>姓名:{name}</li>
                <li>年龄:{age}</li>
                <li>性别:{sex}</li>
            </ul>
        )
    }
}

//渲染组件到页面
ReactDOM.render(<Person name="kobe" age={18} sex="男"/>,document.getElementById("test1"));
//通过对象解构传递复杂对象
const data = {
    name : "curry",
    age : 20,
    sex : "男"
}
//babel+扩展运算符，让标签中可以使用{...data}展开对象，传递Props(标签属性)
ReactDOM.render(<Person {...data}/>,document.getElementById("test2"))
```

## props属性限制

```javascript
class Person extends React.Component{
    render(){
        console.log(this);
        //解构赋值
        const {name,age,sex} = this.props;
        return(
            <ul>
                <li>姓名:{name}</li>
                <li>年龄:{age}</li>
                <li>性别:{sex}</li>
            </ul>
        )
    }
}

//React 16 版本之前都是这样对属性进行限制
// Person.propTypes = {
//     name:React.PropTypes.string.isRequired,
//     age:React.PropTypes.number
// }
//React 16版本及之后版本都将属性限制抽离出一个js文件，叫prop-type.js，引用该文件
Person.propTypes = {
    name:PropTypes.string.isRequired, //字符串 必传
    age:PropTypes.number, //数值
    sex:PropTypes.string, // 字符串
    speak:PropTypes.func //限制属性为方法时，注意不是function
}
//设置默认属性
Person.defaultProps={
    sex:'不男不女',
    age:'年龄不详',
    name:'名字不详'
}

function speak(){

}
```

## props简写

* props是只读的，不能修改

```javascript
class Person extends React.Component{    //通过static关键字给类自身添加属性，而不是给类的实例对象添加属性    static propTypes = {        name:PropTypes.string.isRequired, //字符串 必传        age:PropTypes.number, //数值        sex:PropTypes.string, // 字符串        speak:PropTypes.func //限制属性为方法时，注意不是function    }    static defaultProps = {        sex:'不男不女',        age:'年龄不详',        name:'名字不详'    }    render(){        ...省略    }}
```

## 类中constructor构造器详解

```javascript
class Person extends React.Component{    constructor(props){        //构造器是否接收props，是否传递给super，取决于：是否希望在构造器中使用this访问props属性        //实际开发中很少写constructor()构造器        super(props);        console.log('constructor:',this.props)    }}
```

## 函数式组件中props属性

* 函数式组件因为没有this,不能使用state和refs
* 但是函数式组件是一个方法，可以接受参数，因此可以使用Props

```javascript
function Person(props){    const {name,age,sex} = props;    return(        <ul>            <li>姓名:{name}</li>            <li>年龄:{age}</li>            <li>性别:{sex}</li>        </ul>    )}//给函数式组件Person添加属性控制Person.propTypes = {    name:PropTypes.string.isRequired, //字符串 必传    age:PropTypes.number, //数值    sex:PropTypes.string, // 字符串    speak:PropTypes.func //限制属性为方法时，注意不是function}//给函数式组件Person添加属性默认值Person.defaultProps = {    sex:'不男不女',    age:'年龄不详',    name:'名字不详'}ReactDOM.render(<Person name="klay" age={26} sex="男"/>,document.getElementById('test1'))
```


# 十四、react三大之间之三：refs

##  例子

* 自定义组件，功能说明如下
  1、点击按钮，提示输入第一个输入框中的值，如果第一个输入框有值，则alert
  2、第二个输入框失去焦点时，如果输入框中有值。alert这个输入框中的值
* 组件内的标签可以定义ref属性来标识自己，类似原生js的id 

* 字符串形式ref 过时API
* 字符串类型ref效率不高
* 字符串形式ref已经不被官方推荐使用，官方也有可能在之后版本去除字符串形式ref使用

```javascript
		//创建组件		class Demo extends React.Component{			render(){				return(					<div>						<input ref="input1" id="input1" type="text" placeholder="点击按钮提示数据"/>&nbsp;						<button onClick={this.showText1}>点我提示左边的数据</button>&nbsp;						<input onBlur={this.showText2} ref="input2" type="text" placeholder="失去焦点提示数据" />					</div>				)			}			//展示左侧输入框的数据			showText1 = () =>{				//js写法				//alert(document.getElementById('input1').value)											//react写法				//this.refs.input1获取到真实的DOM input节点				alert(this.refs.input1.value);			}			//失去焦点提示右侧输入框数据			showText2 = ()=>{				alert(this.refs.input2.value);			}		}		ReactDOM.render(<Demo />,document.getElementById('test1'))		ReactDOM.render(<Demo />,document.getElementById('test2'))
```

## 回调函数形式ref

* <input ref={(currentNode)=>{this.input1=currentNode}}/>
* 这样就可以通过this.input1获取input节点

```javascript
        //创建组件		class Demo extends React.Component{			render(){				return(					<div>						<input ref={(currentNode)=>{this.input1=currentNode}} id="input1" type="text" placeholder="点击按钮提示数据"/>&nbsp;						<button onClick={this.showText1}>点我提示左边的数据</button>&nbsp;						<input onBlur={this.showText2} ref={(currentNode)=>{this.input2=currentNode}} type="text" placeholder="失去焦点提示数据" />					</div>				)			}			//展示左侧输入框的数据			showText1 = () =>{						//this.input1可以直接获取到第一个输入框真实DOM				alert(this.input1.value);			}			//失去焦点提示右侧输入框数据			showText2 = ()=>{				alert(this.input2.value);			}		}		ReactDOM.render(<Demo />,document.getElementById('test1'))		ReactDOM.render(<Demo />,document.getElementById('test2'))
```

## 回调函数形式ref中调用次数问题

* 如果 ref 回调函数是以内联函数的方式定义的，在更新过程中它会被执行两次，第一次传入参数 null，然后第二次会传入参数 DOM 元素。这是因为在每次渲染时会创建一个新的函数实例，所以 React 清空旧的 ref 并且设置新的。通过将 ref 的回调函数定义成 class 的绑定函数的方式可以避免上述问题，但是大多数情况下它是无关紧要的。

## ref回调函数定义成class的绑定函数(例如：saveInput1）避免ref在更新过程中被执行两次

```javascript
        //创建组件		class Demo extends React.Component{            state = {isHot:true}            render(){				return(                    <div>                        {/*<input ref={(currentNode)=>{this.input1=currentNode}} id="input1" type="text" placeholder="点击按钮提示数据"/>&nbsp;*/}                        <input ref={this.saveInput1} id="input1" type="text" placeholder="点击按钮提示数据" />&nbsp;						<button onClick={this.showText1}>点我提示左边的数据</button>&nbsp;						<input onBlur={this.showText2} ref={(currentNode)=>{this.input2=currentNode}} type="text" placeholder="失去焦点提示数据" />                        <h1>今天天气很{this.state.isHot?"热":"凉爽"}</h1>                        <button onClick={this.changeWeather}>点击切换天气</button>					</div>				)            }            //class方法形式绑定ref回调函数            saveInput1 = (currentNode) =>{                this.input1 = currentNode;            }                        //切换天气方法            changeWeather = ()=>{                const {isHot} = this.state;                this.setState({                    isHot: !isHot                })            }			//展示左侧输入框的数据			showText1 = () =>{				alert(this.input1.value);			}			showText2 = ()=>{				alert(this.input2.value);			}		}		ReactDOM.render(<Demo />,document.getElementById('test1'))
```

## createRef的使用

* React.createRef调用后可以返回一个容器，该容器可以存储被ref所表示的节点真实DOM,该容器是"专人专用的"
* jsx注释格式： {/**/}

```javascript
    class Demo extends React.Component{
        myRef1 = React.createRef();
        myRef2 = React.createRef();
        render(){
            return(
                <div>
                    <input ref={this.myRef1} placeholder="点击右边按钮弹出值" type="text" />&nbsp;
                    <button onClick={this.showText1}>点击显示左边输入框的值</button>&nbsp;
                    <input ref={this.myRef2} onBlur={this.showText2} placeholder="失去焦点弹出值" type="text"/>
                </div>
            )
        }

        showText1 = ()=>{
            const value = this.myRef1.current.value;
            if(value){
                alert(value);
            }
        }

        showText2 = ()=>{
            const value = this.myRef2.current.value;
            if(value){
                alert(value);
            }
        }
    }
```

# 十五、ref总结

* ref的四种进阶使用方式，推荐程度递增
  1、字符串ref  不推荐
  2、内联函数ref
  3、class绑定函数ref
  4、React.createRef方法创建ref (目前最为推荐的一种方式)

# 十六、react中的事件处理

* 通过onXXX属性指定事件处理函数(注意大小写)
  a. react使用的是自定义合成事件，而不是原生DOM事件 ——————为了更好的兼容性
  b. react中事件是通过事件委托方式处理的（委托给最外层的元素） ——————为了更高效
* 如果发生事件的元素刚好是你要操作的元素（例如下例input2），那么就可以不同ref，直接使用e.target可以获取到DOM。不要过度使用ref

```javascript
class Demo extends React.Component{
    render(){
        return(
            <div>
                <input type="text" placeholder="失去焦点弹出值" onBlur={this.changeText2} />
            </div>
        )
    }

    changeText2 = (e)=>{
        //可以回调获取e，e.target即代码要操作的元素，不需要用ref
        alert(e.target.value);
    }
}

ReactDOM.render(<Demo />,document.getElementById("test1"))
```

# 十七、非受控组件

* 要编写一个非受控组件，而不是为每个状态更新都编写数据处理函数，你可以 使用 ref 来从 DOM 节点(输入框、下拉菜单...)中获取表单数据。
* 在大多数情况下,我们推荐使用 受控组件 来处理表单数据，因为非受控组件要使用ref，不推荐使用过多ref
* 非受控组件，即用即取，表单的输入值不存储在state中
* 组件的状态不受React控制的组件

```javascript
        //非受控组件
        class Login extends React.Component{
			render(){
				return(
					<form action="http://www.atguigu.com/" onSubmit={this.handleSubmit}>
						用户名:<input type="text" ref={(c)=>{this.username = c}}/><br/>
						密  码:<input type="text" ref={(c)=>{this.password = c}}/><br/>
						<button>提交</button>
					</form>
				)
			}

			handleSubmit = (e) =>{
				e.preventDefault();  //阻止提交后跳转页面
				alert(`用户名：${this.username.value}，密码：${this.password.value}`);
			}

			changeText2 = (e)=>{
				//可以回调获取e，e.target即代码要操作的元素,不需要用ref
				alert(e.target.value);
			}
		}

		ReactDOM.render(<Login />,document.getElementById("test1"))
```

# 十八、受控组件

* 受控组件就是组件的状态受React控制。通过onChange方法实现和表单中元素的输入值和React中的状态实时联动更新
* vue默认就实现了双向绑定，react没有实现，需要结合onChange和setState实现绑定

```javascript
class Login extends React.Component{
    state = {
        username:"",
        password:""
    }

    handleSubmit = ()=>{
        alert(`用户名：${this.state.username}，密码：${this.state.password}`)
    }

    usernameChange = (e) =>{
        this.setState({
            username:e.traget.value
        })
    }

    passwordChange = (e) =>{
        this.setState({
            password:e.traget.value
        })
    }

    render(){
        return(
            <form action="http://www.atguigu.com" onSubmit={this.handleSubmit}>
                用户名:<input type="text" onChange={this.usernameChange} /><br/>
                密  码:<input type="text" onChange={this.passwordChange} /><br/>
                <button>提交</button>
            </form>
        )
    }
}

ReactDOM.render(<Login/>,document.getElementById('test1'));
```

# 十九、高阶函数--函数柯里化

## 对象属性名是变量时如何处理：用方括号表示

```javascript
    let a = "name";
    let b = {
        [a]:"111" //也可以这样写
    }
    //b[a] = "111"  可以这样写
    console.log(b);
```

## 高阶函数定义

* 如果一个函数符合下面2个规范中的任何一个，那该函数就是高阶函数
  1、若A函数，接受的参数是一个函数，那么A就可以称之为高阶函数
  2、若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数
* 函数的柯里化：通过函数调用继续返回函数的方式，实现多次接受参数最后统一处理的函数编码形式

## 高阶函数实例

```javascript
class Login extends React.Component{
    state = {
        username:"",
        password:""
    }

    handleSubmit = ()=>{
        alert(`用户名：${this.state.username}，密码：${this.state.password}`)
    }

    //高阶函数，return了一个function
    formdataChange(datatype){
        return (event) =>{
            this.setState({
                //datatype是变量，必须用[]框起来
                [datatype]:event.target.value
            })
        }
    }

    render(){
        return(
            <form action="http://www.atguigu.com" onSubmit={this.handleSubmit}>
                用户名:<input type="text" onChange={this.formdataChange("username")} /><br/>
                密  码:<input type="text" onChange={this.formdataChange("password")} /><br/>
                <button>提交</button>
            </form>
        )
    }
}

ReactDOM.render(<Login/>,document.getElementById('test1'));
```

## 不用高阶函数实现上面那个效果

```javascript
class Login extends React.Component{
    state = {
        username:"",
        password:""
    }

    handleSubmit = ()=>{
        alert(`用户名：${this.state.username}，密码：${this.state.password}`)
    }

    formdataChange(datatype,event){
            this.setState({
                //datatype是变量，必须用[]框起来
                [datatype]:event.target.value
            })
    }

    render(){
        return(
            <form action="http://www.atguigu.com" onSubmit={this.handleSubmit}>
                用户名:<input type="text" onChange={event=>this.formdataChange("username",event)} /><br/>
                密  码:<input type="text" onChange={event=>this.formdataChange("password",event)} /><br/>
                <button>提交</button>
            </form>
        )
    }
}

ReactDOM.render(<Login/>,document.getElementById('test1'));
```

# 二十、组件生命周期

## 例子

* 定义组件实现以下功能
  1、让指定的文本做显示/隐藏的渐变动画
  2、从完全可见，到彻底消失，耗时2s
  3、点击不活了按钮从界面中卸载组件

```javascript
        class Life extends React.Component{
            state = {opacity:1}

            //组件挂载完毕
            componentDidMount(){
                console.log(this);
                //将定时器保存到Life类实例对象，方便销毁
                this.timer = setInterval(() => {
                    let {opacity} = this.state;
                    opacity -= 0.1;

                    //浮点数相减有可能不一定为0，用Number.EPSILON判断
                    if(Math.abs(opacity) < Number.EPSILON){
                        opacity = 1;
                    }

                    this.setState({opacity})
                }, 200);
            }

            //组件即将要卸载
            componentWillUnmount(){
                //清除定时器
                clearInterval(this.timer)
            }

            goDead = () =>{
                //卸载组件
                ReactDOM.unmountComponentAtNode(document.getElementById("test1"));
           }

            render(){
                return(
                    <div>
                        <span style={{opacity:this.state.opacity}}>显示的文本</span><br/>
                        <button onClick={this.goDead}>不活了</button>
                    </div>
                )
            }
        }

        ReactDOM.render(<Life/>,document.getElementById('test1'));
```

## 生命周期（旧）——组件挂载流程

* 组件从创建到死亡会经历一些特定的阶段
* React组件中包含一系列钩子函数（生命周期回调函数），会在特定时刻调用
* 我们定义组件时，会在特定得阶段时执行生命周期回调函数，做特定的工作

```javascript
        //组件初次加载 、setState更新、forUpdate强制更新
        class Count extends React.Component{
            constructor(props){
                console.log("Count--constructor");
                super(props);
                //初始化状态
                this.state = {count:0};
            }

            //组件将要挂载的钩子
            componentWillMount(){
                console.log("Count--componentWillMount");
            }

            //组件挂载完毕的钩子
            componentDidMount(){
                console.log("Count--componentDidMount");
            }

            //组件将要卸载的钩子
            componentWillUnmount(){
                console.log("Count--componentWillUnmount")
            }

            //控制组件更新的阀门
            //如果返回值是true，可以更新
            //如果返回值是false，不需要更新
            //该函数一般不需要重写，如果执行setState()， react底层默认会比较state的变化，如果没有变化返回false，不进行更新
            //如果有变化返回true，进行更新
            shouldComponentUpdate(){
                console.log("Count--shouldComponentUpdate");
                return true;
            }

            //组件将要更新的钩子
            componentWillUpdate(){
                console.log("Count--componentWillUpdate")
            }

            //组件更新完毕的钩子
            componentDidUpdate(){
                console.log("Count--componentDidUpdate")
            }

            //加1按钮的回调
            add = () =>{
                let {count} = this.state;
                this.setState({
                    count:count+1
                })
            }

            //强制更新
            force = ()=>{
                this.forceUpdate();
            }

            //卸载组件的钩子
            goDead = ()=>{
                ReactDOM.unmountComponentAtNode(document.getElementById('test1'))
            }


            render(){
                console.log("Count--render")
                return (
                    <div>
                        <h2>当前求和为{this.state.count}</h2>
                        <button onClick={this.add}>点我+1</button>
                        <button onClick={this.goDead}>点我卸载组件</button>
                        <button onClick={this.force}>不更改任何状态中的数据，强制更新以下</button>
                    </div>
                )
            }
        }
        //渲染组件
        ReactDOM.render(<Count/>,document.getElementById("test1"))
```

```javascript
//父组件render
class A extends React.Component{
    state = {carname:"奔驰"}

    changeCar = () =>{
        this.setState({
            carname:this.state.carname==="奔驰"?"宝马":"奔驰"
        })
           
    }

    render(){
        return(
            <div>
                <h1>A标签内容</h1>
                <button onChange={this.changeCar}>换车</button>
                <B carname={this.state.carname}/>
            </div>
        )
    }
}

class B extends React.Component{
    //实际上是componentWillReceiveNewProps
    //组件将要接受到新的Props
    //初始化的时候如果也传了一个Props，但不会触发该回调
    componentWillReceiveProps(){
        console.log("componentWillReceiveProps");
    }

    //控制组件更新的阀门
    //如果返回值是true，可以更新
    //如果返回值是false，不需要更新
    //该函数一般不需要重写，如果更新props,那么底层默认会返回true
    //如果有变化返回true，进行更新
    shouldComponentUpdate(){
        console.log("shouldComponentUpdate");
        return true;
    }

    //组件将要更新
    componentWillUpdate(){
        console.log("componentWillUpdate");
    }

    //组件更新完毕
    componentDidUpdate(){
        console.log("componentDidUpdate");
    }

    render(){
        console.log("render");
        return(
            <h1>B标签内容:{this.props.carname}</h1>
        )
    }
}
```

## 常用的三个钩子函数

* componentDidMount  一般在这个钩子做一些初始化的事情，开启定时器、发送网络请求、订阅消息
* componentWillUnmount 一般在这个钩子中做一些收尾的事情，例如：关闭定时器、取消订阅消息
* render  必须用

## 新旧生命周期的对比

* 由于react之后计划推出异步渲染效果，因此新的生命周期相比旧的生命周期，废弃了三个钩子（componentWillMount componentWillUpdate componentWillReceiveProps)，这三个钩子在之前版本被滥用。新增了两个钩子getDerivedStateFromProps和getSnapshotBeforeUpdate
* 新版本(>React 17.0)中不可以再写componentWillMount，必须要加上UNSAFE前缀
  componentWillMount --> UNSAFE_componentWillMount
  componentWillReceiveProps --> UNSAFE_componentWillReceiveProps
  componentWillUpdate --> UNSAFE_componentWillUpdate

## 新钩子 getDerivedStateFromProps(props, state) 

* getDerivedStateFromProps会在调用render方法之前调用，并且在初始挂载及后续更新时都会被调用。他会返回一个状态来更新state，如果返回null则不更新任何内容
* 此方法用于罕见的案例，即state的值在任何情况下都取决于props
* 派生状态会导致代码冗余，并使组件难以维护

```javascript
//得到派生state来自props
class A extends React.Component{
    static getDerivedStateFromProps(props,state){
        console.log('getDerivedStateFromProps',props,state);
        //return props; //可以
        //return null; //可以
        //return this.state //可以
        return {count:1}  //可以
    }
}
```

## 新钩子getSnapshotBeforeUpdate

* 在更新之前获取快照
* getSnapshotBeforeUpdate()在最近一次渲染输出（提交到DOM节点）之前调用，它使得组件能在发生更改之前从DOM捕获一些信息（例如：滚动位置）。此生命周期的任何返回值将作为参数传递给componentDidUpdate(preProps,preState,snapshotValue)

```javascript
//preProps是更新之前的props，preState是更新之前的state
getSnapshotBeforeUpdate(preProps, preState){
    console.log(preProps,preState)
    return "aguigu";
}

//组件完成更新
componentDidUpdate(preProps,preState,snapshotValue){
    console.log(snapshowValue);  //aguigu
}
```

* getSnapshotBeforeUpdate的使用场景
* 实现一个滚动的新闻列表，滚动条最上方不变

```javascript
        class NewsList extends React.Component{
			state = {newlist:[]}

			//组件加载完毕
			componentDidMount(){
				//开启间歇调用
				setInterval(() => {
					let {newlist} = this.state;
					let newValue = `新闻${newlist.length+1}`;
					this.setState({
						newlist:[newValue,...newlist]
					})
				}, 1000);
			}

			//获取之前的scrollHeight属性
			getSnapshotBeforeUpdate(preProps,preValue){
				return this.refs.list.scrollHeight
			}

			//拿更新之后的scrollHeight-更新之前的scrollHeight 加给list的scrollTop属性即可保持不动
			componentDidUpdate(preProps,preValue,snapValue){
				this.refs.list.scrollTop += this.refs.list.scrollHeight - snapValue;
			}

			render(){
				return(
					<div className="list" ref="list">
						{this.state.newlist.map((value,index)=>{
							return <div className="news" key={index}>{value}</div>
						})}
					</div>
					
				)
			}
		}
		ReactDOM.render(<NewsList />,document.getElementById('test1'))
```

# 二十一、Dom的diffing算法详解

## 举个例子

```javascript
        class Time extends React.Component{
			state = {date:new Date()}

			//组件挂载完毕开启循环定时器，更新日期
			componentDidMount(){
				setInterval(()=>{
					this.setState({
						date:new Date()
					})
				},1000)
			}

			//render中只有span节点在更新
			//h1和input节点没有更新，这就是react的Dom Diffing算法起的作用
			//DOM Diffing的最小粒度是标签
			//DOM Diffing算法可以处理标签嵌套的情况
			render(){
				return(
					<div>
						<h1>hello</h1>
						<input type="text" />
						<span><input type="text"/>现在是{this.state.date.toTimeString()}</span>
					</div>
				)
			}
		}

		ReactDOM.render(<Time/>,document.getElementById("test1"))
```

## DOM Diffing算法中key的作用

* 面试题：
  1、react/vue中的key有什么作用？（key的内部原理是什么？）
  2、为什么遍历列表时，key最好不要用index?

* 回答：
* 1、虚拟DOM中key的作用
  1）简单地说，key是虚拟DOM对象的标识，在更新显示key起着极其重要的作用
  2）详细的说：当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】，然后react将【新的虚拟DOM】与【旧的虚拟DOM】进行Diff比较，比较规则如下
      a.旧虚拟DOM中找到了与新虚拟DOM相同的key
          (1)比较新旧虚拟DOM中的内容，如果内容没变，直接使用之前的真实DOM，如果虚拟DOM内容变了，则生成新的真实DOM，替换到页面中之前的真实DOM
      b.旧虚拟DOM中未找到与新虚拟DOM相同的key，会根据数据创建新的真实DOM，随后渲染到页面
* 2、用index作为key可能会引发的问题？
  （1）若对数据进行：逆序添加，逆序删除等破坏顺序操作：会产生没有必要的真实DOM更新 ==》界面效果没问题，但是效率低
  （2）如果结构中还包含有输入类DOM，例如input框，会产生错误的DOM更新 ==》界面有问题
  （3）注意：如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于前端渲染列表展示，使用index作为key是没有问题的
* 3、开发中如何选择key?
  数据唯一表示：id、手机号、身份证、学号
  如果确定只是用于简单的数据展示，使用index作为key也是可以的

* 虚拟DOM，输入类DOM，没有value值，只有真实DOM展现在页面上，用户才能输入，输入类虚拟DOM比较也只是一句标签结构进行比较



```javascript
/*
慢动作回放--使用index索引值作为Key
 数据：
    {id:1,name:'小张',age:18}
    {id:2,name:'小李',age:19}
 初始的虚拟DOM:
    <li key=0>小张----18</li>
    <li key=1>小李----19</li>
 更新后的数据：
    {id:3,name='小王',age:20}
    {id:1,name:'小张',age:18}
    {id:2,name:'小李',age:19}
 更新后的虚拟DOM
    <li key=0>小王----20</li>
    <li key=1>小张----18</li>
    <li key=2>小李----19</li>
 进行初始的虚拟DOM和更新后的虚拟DOM进行比对，发现3个标签都需要更新，其实前两个数据没有变化，本来是不需要更新的，只要将id属性设置为key就可以解决这个问题

 --------------------------------------------
 慢动作回放--使用id作为Key
 数据：
    {id:1,name:'小张',age:18}
    {id:2,name:'小李',age:19}
 初始的虚拟DOM:
    <li key=1>小张----18</li>
    <li key=2>小李----19</li>
 更新后的数据：
    {id:3,name='小王',age:20}
    {id:1,name:'小张',age:18}
    {id:2,name:'小李',age:19}
 更新后的虚拟DOM
    <li key=3>小王----20</li>
    <li key=1>小张----18</li>
    <li key=2>小李----19</li>
  只引起了一次页面真实DOM更新，效率高
*/

class Person extends React.Component{
    state = {
        persons:[
            {id:1,name:'小张',age:18},
            {id:2,name:'小李',age:19}
        ]
    }

    add = ()=>{
        const {persons} = this.state;
        const p = {id:persons.length+1,name:"小王",age:20};
        this.setState({
            persons:[p,...persons]
        })
    }

    render(){
        return(
            <div>
                <h2>展示人员信息</h1>
                <button onClick={this.add}>添加一个小王</button>
                <ul>
                    {this.state.persons.map((person,index)=>{
                        return <li key={index}>{person.name}----{person.age}</li>
                    })}
                </ul>
            </div>
        )
    }
}
ReactDOM.render(<Person/>,document.getElementById("test1"))
```

# 二十二 杂七杂八(create-react-app脚手架)

### vscode代码块快速生成

文件--首选项--用户代码片段，使用https://snippet-generator.app/可以快速辅助生成代码

* Boolean有toString()方法
* Number.toFixed(2)，将数字类型转换为带有两位小数的浮点数
* Array.filter((item,inex) => { return item.index>2 }) filter方法中回调要求返回值是布尔值
* Array.map方法返回值没有要求
* Array.slice(0,3) 选中index为 0 1 2的数据，前闭后开
* Array.splice(2,1,3) 在原数组的身上删除索引从2开始的一条数据并将3填入。返回值是被删除的数组
* undefined和null没有toString()方法，可以用String()方法转换
* typeof undefined结果是undefined      
* typeof null的结果是Object
* jsx可以显示数组类型数据
* div span a h2等元素都有title属性，用来提示信息
* jsx中不能使用js的关键字，类似class用className代替，label的for属性用htmlFor代替
* jsx动态修改添加类`className = {"box title " + this.state.active ? "active" : ""}`
* jsx样式  `<div style={{color:'red',fontSize:'50px'}}></div>` 属性名用小驼峰命名法，属性值必须是字符串，否则会被识别为变量
* 事件绑定
  * 原生：onclick
  * jsx中：onClick={this.handleClick},react合成事件
* {isLogin && "你好"}  js逻辑与，前面条件不成立，后面都不会判断，返回false，假如说最后一个值为真，将最后一个值作为整个表达式的结果返回

### react模拟vue中v-show方法

* vue中v-if等价于微信小城中wx:if

* vue中v-if和v-show的区别，v-if的频繁切换会引起DOM频繁变化，等价于js中的 `{this.state.login?<h2>登录成功</h2>:null}`。v-show只是动态切换display的属性值block/none。这样不会引起DOM的重新渲染，效率高。`<h2 style={{display:this.state.isLogin?'block':'none'}}>登录成功</h2>`

### react模拟vue中slot插槽

1、将jsx作为props属性传递 ， 子组件通过this.props.leftJsx获取

2、将jsx作为children传递，子组件通过this.props.children[index]获取

### react中列表渲染

* vue中使用v-for
* 微信小程序中使用wx:for
* react中使用js中map方法，不要用forEach

### 函数方式创建Jsx

jsx仅仅是React.createElement(component,props,...children)函数的语法糖

所有的jsx都最终会被转换为React.createElement函数调用

### 虚拟DOM的创建过程

jsx -->  createElement函数 --> ReactElement(虚拟DOM对象树) --> ReactDOM.render --> 真实DOM

* 为什么使用虚拟DOM?
  * 虚拟DOM轻，装填方便轻松
  * 原始DOM重，状态难以跟踪
  * 频繁操作真实DOM，会引起DOM的频繁回流和重绘，效率低
  * Virtural DOM是一种编程理念，在这个理念中，UI以一种理想化或者说虚拟化的方式保存在内存中，他是一个相对简单的js对象
  * 可以通过ReactDOM.render()方法将虚拟DOM映射到真实DOM，这个过程叫协调

### npm和yarn对照表

![image-20210404213039649](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141718.png)

### 脚手架使用

```shell
npx create-react-app hello-react   项目名称不能包含大写字母
cd hello-react
yarn start 运行项目
```

* 结构分析

![image-20210404215426067](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141729.png)
执行`yarn enjet`展开webpack配置，慎用

react中组件名必须大写，jsx对标签名大小写敏感，必须小写。html对大小写不敏感

函数组件没有this

props属性限制

```js
import PropTypes from 'prop-types'

//属性和必填验证
ChildCpn.propTypes = {
    name:PropTypes.string.isRequired
}
//默认值设置
ChildCpn.defaultProps = {
    name:'alice'
}
```



### 生命周期

![image-20210404224138505](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141735.png)

### text组件

小程序               view text (text中不能包含换行符，否则会原样解析，文字可以不包裹)

react-native     View Text（文字必须用Text组件包裹)

react                 div (文字可以不包裹)

### context

![image-20210405094040326](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141742.png)

![image-20210405094222522](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141747.png)

![image-20210405094314595](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141753.png)

![image-20210405095134109](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141759.png)

![image-20210405095450383](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141808.png)



### setState同步异步

![image-20210405100223392](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141814.png)

* 合成事件onClick，react核心库的运行环境不仅包括浏览器，还有手机端，浏览器和手机端的事件对象不同，为了统一和高效率，react做了事件合成

setState在setTimeout和dom原生事件中同步

setState在组件生命周期和合成事件中异步

setState是对象合并并覆盖，基于Object.assign({},{message:"aaa"},this.state)

![image-20210405103248089](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141819.png)

虽然做了三次+1，但是由于setState是批量异步处理，因此实际还是做了一次+1，那么怎么解决呢？

可以用，实现+3

```js
setTimeout(()=>{
    this.setState({
        count:this.state.count+1
    })
    this.setState({
        count:this.state.count+1
    })
    this.setState({
        count:this.state.count+1
    })
},0)
```

更好的方式是使用函数方式的setState，实现+3

```js
this.setState(preState=>{
    count:preState+1
})
this.setState(preState=>{
    count:preState+1
})
this.setState(preState=>{
    count:preState+1
})
```

函数方式执行setState的时候，不能用这种形式 ，state的索引没有变化，不会触发更新

```js
//错误
this.setState(state=>{
	words:state.words.concat(['mark'])
})

//正确
this.setState(state=>{
    words:[...state.words,'mark']
})
```



### react更新机制

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141826.png" alt="image-20210405103759547" style="zoom:67%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141832.png" alt="image-20210405104149082" style="zoom:67%;" />![image-20210405104431745](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141837.png)



<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141842.png" alt="image-20210405104637219" style="zoom:67%;" />



<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141850.png" alt="image-20210405104753739" style="zoom:67%;" />



![image-20210405104949859](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141855.png)

index作为key，如果数据做插入、删除、逆序等操作时，index对效率提高很少。只有对于静态的展示的数据，才可以用index作为key提高效率

### 嵌套组件的render调用以及优化

情况：点击按钮时，触发父组件的render，子组件Header Main Footer也会重新触发render渲染

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141900.png" alt="image-20210405105429092" style="zoom: 67%;" />

优化：

* 函数组件中使用memo()高阶组件包裹，类组件中使用PureComponent代替React.Component。

* 组件的方法属性可以用useCalllback包裹，省得每次渲染，都会生成一个不同的箭头函数地址值

* 注意：PuserComponent的子组件也必须时PureComponent

* 类组件

  * 子组件中使用shouldComponentUpdate进行浅比较，不建议用深比较（效率低）

  <img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141906.png" alt="image-20210405105945517" style="zoom: 80%;" />

  * 子组件继承自PureComponent，纯组件，会被对修改前后props和state对象进行浅比较

* 函数组件

  * 函数组件中没有shouldComponentUpdate方法和PureComponent组件使用，函数组件主要有三个hook用来实现优化，避免不必要的渲染和js方法运行

  * 使用memo方法避免不必要的渲染

    * 在类组件时代，常常会使用PureComponent，每次对props进行一次浅比较，当然除了PureComponent以外，还可以在shouldComponentUpdate中进行更深层次的控制

    * 所有使用 `React.PureComponent` 的子组件，也应该是纯组件或函数组件。

    * 在函数组件中，使用React.memo方法memo方法第一个参数是要优化的组件，第二个参数是一个回调参数，可以接收到prevProps和nextProps，进行比较返回布尔值，如果为true，则重新渲染。如果为false，在不重新渲染。第二个参数可以不填，默认对Props进行浅比较。

    * ```js
      function MyComponent(props) {
        /* render using props */
      }
      function areEqual(prevProps, nextProps) {
        /*
        return true if passing nextProps to render would return
        the same result as passing prevProps to render,
        otherwise return false
        */
      }
      export default React.memo(MyComponent, reEqual);
      ```

  * 使用useMemo方法避免不必要的js运行

    * 类组件没有useMemo这样隔绝细粒度渲染的接口
    * useMemo功能是判断组件中的函数逻辑是否重新执行，用来优化性能

    ```js
    import React, { useState, memo, useMemo } from 'react';
    // , PureComponent, memo, useState, useMemo, useCallback
    import './App.css';
    
    const Count = memo(function Count(props) {
      console.log('count render')
      return <div onClick={props.onClick}>子组件count值：{props.double}</div>
    })
    
    function App(props) {
    
      const [count, setCount] = useState(0);
    
      //第一个参数是要执行的函数
      //第二个参数是执行函数依赖的变量组成的数据
      //这里只有count发生变化double才会重新计算
      const double = useMemo(() => {
        return count * 2;
      }, [count === 3])
    
    //情况一、如果onClick是这样一个箭头函数。那么每次点击按钮+1时，App()函数会从头往下重新运行，onClick函数前后实例并不一致，虽然double值没有变化，但是<Count double={double} onClick={onClick} />组件的props前后浅比较仍然发生了变化，因此<Count />组件会重新渲染
    //const onClick = () =>{
    //	console.log('click')
    //}
      
    //情况二、这样返回的函数就会在组件重渲染时产生相同的句柄，因此点击按钮不会触发重新渲染
    //const onClick = useMemo(() => {
        //这里返回的依然是函数
    //	return () => {
    //		console.log('click')
    //	}
    //  }, []);
      
    //情况三、使用useCallback作为useMemo的变体，点击按钮不会触发重新渲染
    //useCallback使用
      const onClick = useCallback(() => {
        console.log('click')
      }, []);
        
      return (
        <div className="app">
          <p>父组件count值：{count}</p>
          <Count double={double} onClick={onClick}/>
          <button
            onClick={() => {
              setCount(count + 1)
            }}>
            Add
         </button>
          <div>double值：{double}</div>
        </div>
      )
    }
    
    export default App;
    ```

  * 使用useCallback方法避免不必要的js运行，useCallback就是useMemo的变体

### 全局事件传递

使用events库`yarn add events`

![image-20210405163947877](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141914.png)

```js
import React, { useState,useEffect,memo } from 'react'
import {EventEmitter} from 'events'

const eventBus = new EventEmitter()
const Header = () => {
  return (
    <div>
      <button onClick={()=>{
        eventBus.emit('changeText',"改变啦! "+Date.now())
      }}>点击改变文字</button>
    </div>
  )
}

const App = () => {
  const [text, settext] = useState("我是现在的文字")
  const handleChageText = (newText) => {
    settext(newText)
  }

  useEffect(() => {
    //componentDidMount
    //监听
    eventBus.addListener('changeText',handleChageText)
    return () => {
      //componentWillUnMount
      eventBus.removeListener('changeText',handleChageText)
    }
  }, [])

    return (
      <div>
        {text}
        <Header/>
      </div>
    )
}

export default memo(App) 
```

### ref

#### 类组件中

```js
1、字符串形式  不推荐
<h1 ref="ref1">测试</h1>
使用 this.refs.ref1.current
2、回调函数形式将节点挂接到this对象  不推荐
<h1 ref={(node)=>{this.node = node}}>测试</h1>
使用 this.node
3、React.createRef()   推荐
在constructor函数中
使用this.ref1.current
const class App = (props) => {
    constructor(){
        super(props)
        this.ref1 = React.createRef()
    }
    render(){
        return(
        	<h1 ref={this.ref1}>测试</h1>
        )
    }
}
```

#### 函数组件中

使用useRef hook

![image-20210405171605913](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141922.png)

类组件中可以使用forwardRef进行组件转发

https://www.cnblogs.com/yky-iris/p/12355243.html

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141930.png" alt="image-20210405195310840" style="zoom:67%;" />



### 高阶组件（High-Order Component)简称HOC

输入或者输出是组件

高阶函数的输入或者输出是函数

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141938.png" alt="image-20210405191320593" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141945.png" alt="image-20210405192012475" style="zoom:67%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514141958.png" alt="image-20210405192105004" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142002.png" alt="image-20210405192900564" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142006.png" alt="image-20210405194344685" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142015.png" alt="image-20210405194406943" style="zoom:50%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142020.png" alt="image-20210405194609099" style="zoom:67%;" />

### Portals的使用

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142027.png" alt="image-20210405195701960" style="zoom:67%;" />

很多第三方库都是基于Portals实现的，这里以一个简单的modal的实现为例

1、在html中添加一个id为modal的div，之后modal元素就渲染在这个div中，一般的第三方组件（例如ant-design中的modal) 没法要求你在index.html中新增一个div，他们是通过document.createElement('div')创建div然后document.body.appendChild()将他动态挂载到页面

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142035.png" alt="image-20210405200352355" style="zoom:50%;" />

2、设置居中css

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142044.png" alt="image-20210405200510537" style="zoom:80%;" />

3、定义modal组件并使用

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142050.png" alt="image-20210405200539616" style="zoom:80%;" />

### Fragment（片段）

不会被渲染的div容器，提高效率

Fragment还有一种短语法写法<></>，短语法不能设置key属性

### StrictMode严格模式

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142059.png" alt="image-20210405201607344" style="zoom: 67%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142107.png" alt="image-20210405202006926" style="zoom:80%;" />

### react中样式

#### 简介

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142115.png" alt="image-20210405202348010" style="zoom:50%;" />



<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142121.png" alt="image-20210405202538973" style="zoom:50%;" />

#### 内联样式

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142126.png" alt="image-20210405202922811" style="zoom:80%;" />

#### 普通css（包括Less,Sass)

![image-20210405203406097](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142130.png)

#### css modules

![image-20210405203935913](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142135.png)

create-react-app脚手架已经内置了css modules规则，使用时如下

CSS的规则都是全局的，任何一个组件的样式规则，都对整个页面有效。

产生局部作用域的唯一方法，**就是使用一个独一无二的`class`的名字**，不会与其他选择器重名。这就是 CSS Modules 的做法。其他选择器不会产生局部作用域

![image-20210405221648868](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142159.png)

![image-20210405221908537](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142209.png)

上面是在react中使用css modules的过程，很简单。但是注意，只有title类选择器实现了局部作用域。虽然其他选择器虽然也能显示样式，但是他们仍然和css一样，是全局作用域的，仍然有可能出现覆盖问题。因此前端工程化开发中一般都**推荐用类选择器**

#### css in js

![image-20210405204213827](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142223.png)

![image-20210405204427337](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142228.png)

* styled-components的原理：标签模板字符串，可以通过模板字符串的方式对一个函数进行调用

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142238.png" alt="image-20210405205447456" style="zoom:50%;" />

* vscode中安装vscode-styled-components来支持语法

* styled-components常用用法,偷懒的写法，当标签很多时，要是我们虽每个标签都要进行修饰，那岂不是要写好多的组件，但是在有些情况下我们没必要分这木多组件，那我们不妨可以试试以下的写法

```js
import React, { Component,Fragment} from 'react';
//引入styled-components
import styled from 'styled-components'

//修改了div的样式，整个页面用人StyleDiv包裹一下，里面基本可以用less语法写样式，当然实际开发中StyleDiv也可以用一个单独的js（注意：不是css)文件保存，到处StyleDiv，需要使用的地方引入即可
const StyleDiv = styled.div`
  &>h1{
    font-size:30px;
    color:red;
  }
  &>div>h3{
    font-size:20px;
    color:green;
  }
`

class App extends Component {

  render() {
    return (
    <StyleDiv>
      <h1>大红牛</h1>
      <div>
        <h3>小绿牛</h3>
      </div>
    </StyleDiv>
    )
  }
}
export default App;
```

* 在书写样式时，类似less，也可以使用各种选择器、伪类、伪元素
* 样式继承

```js
import React, { Component,Fragment} from 'react';
//引入styled-components
import styled from 'styled-components'

//初始组件
const Button = styled.button`
color: palevioletred;
font-size: 1em;
margin: 1em;
padding: 0.25em 1em;
border: 2px solid palevioletred;
border-radius: 3px;
`
//对组件进行二次样式修饰
const YellowButton = styled(Button)`
  background-color:yellow
`

class App extends Component {
  render() {
    return (
    <Fragment>
      <Button>红牛</Button>
      <YellowButton>枸杞</YellowButton>
    </Fragment>
    )
  }
}
export default App;
```

* 可以使用props传递参数

```js
import React, { Component,Fragment} from 'react';
//引入styled-components
import styled from 'styled-components'

//修改了div的样式
const StyleDiv = styled.div`
  &>h1{
    font-size:30px;
    color:red;
  }
  &>div>h3{
    font-size:20px;
    color:${props => props.smallcolor};
  }
`

class App extends Component {
  state={
    color:'green'
  }

  componentDidMount(){
    setTimeout(() => {
      //3秒后，绿色变成蓝色
      this.setState({
        color:'blue'
      })
    }, 3000);
  }

  render() {
    return (
    <StyleDiv  smallcolor={this.state.color}>
      <h1>大红牛</h1>
      <div>
        <h3>小绿牛</h3>
      </div>
    </StyleDiv>
    )
  }
}
export default App;
```

* 修改样式同时设置属性

```js
import React, { Component,Fragment} from 'react';
//引入styled-components
import styled from 'styled-components'

//props传递参数（根据参数的有无设置样式）
// 有参数背景会变为蓝色
// 无传递值背景会默认取黄色
const Button = styled.button.attrs({
  title:"aaa",
  id:'bbb',
  'data-role':(props)=>props.hello
})`
  padding: 0.5em;
  margin: 0.5em;
  border: none;
  border-radius: 3px;
`

class App extends Component {
  render() {
    return (
    <Fragment>
        <Button hello="hi">红牛啊</Button>
    </Fragment>
    )
  }
}
export default App;
```

### 动态添加类名

* vue中

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142248.png" alt="image-20210405222911711" style="zoom:80%;" />

* react中

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142254.png" alt="image-20210405223015928" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142304.png" alt="image-20210405223557802" style="zoom:80%;" />

### Ant Design组件库

#### 介绍

![image-20210405223906002](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142310.png)

#### 使用

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142317.png" alt="image-20210405225312979" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142324.png" alt="image-20210405231503391" style="zoom:80%;" />

注意:安装less 和 less-loader

![image-20210405231120524](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142330.png)

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142339.png" alt="image-20210405231053596" style="zoom:80%;" />



### 网络请求

短链接app请求

长连接直播请求（socket)

#### jquery ajax

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142345.png" alt="image-20210406105938896" style="zoom:80%;" />

#### fetch和axios

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142350.png" alt="image-20210406110124932" style="zoom:80%;" />

主流是axios库

测试地址 http://www.httpbin.org/

axios()    通用请求

axios.get()  get请求

axios.post()  post请求

axios.all() 本地就是Promise.all

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142356.png" alt="image-20210406120933274" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142401.png" alt="image-20210406120410695" style="zoom:80%;" />

默认配置

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142406.png" alt="image-20210406120531440" style="zoom:80%;" />

开发中有可能会遇到多个服务器地址的情况，这种时候baseURL该如何配置

创建多个实例axios.instance，分别配置

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142410.png" alt="image-20210406121014780" style="zoom:80%;" />



qs请求参数序列化

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142415.png" alt="image-20210406121814928" style="zoom:80%;" />

### react动画 react-transition-group

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142423.png" alt="image-20210406122928760" style="zoom:80%;" />



<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142428.png" alt="image-20210406123304194" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142432.png" alt="image-20210406130451004" style="zoom:80%;" />



<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142437.png" alt="image-20210406130520421" style="zoom:80%;" />

#### CSSTransition Demo

* Demo，结合antd和css-in-js实现卡片样式的刷新动画、出现动画、消失动画

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142444.png" alt="image-20210407214548388" style="zoom:50%;" />

```js
import React, { useState } from 'react'
import { CSSTransition } from 'react-transition-group'
import StyleDiv from './style'
import { Card, Button } from 'antd';
const { Meta } = Card;

export default function Demo() {
  const [inProp, setInProp] = useState(true);
  return (
    <StyleDiv>
      <CSSTransition 
        //组件是否显示，类似visibile属性，通过该属性值的变化触发enter exit各种css动画
        in={inProp}
        //inProp为false时，动画完成后卸载组件，不占用DOM，默认为false，需要手动添加
        unmountOnExit
        //unmountOnExit的时间，类添加的时间，不影响动画的时间
        timeout={250}
        //动画类名，css样式前缀
        classNames="card"
        //默认为false，要想实现用户初次进入网页、刷新网页动画，必须加上该属性
        //结合appear   appear-active  appear-done等属性使用
        appear
        //动画生命周期，el是被CSSTransition包裹的组件
        onEnter = {el => console.log('开始进入')}
        onEntering = {el => console.log('正在进入')}
        onEntered = {el => console.log('完成进入')}
        onExit = {el => console.log('开始退出')}
        onExiting = {el => console.log('正在退出')}
        onExited = {el => console.log('完成退出')}
      >
      <Card
        hoverable
        style={{ width: 240}}
        cover={
          <img alt="example" 
               src="https://os.alipayobjects.com/rmsportal/QBnOOoLaAfKPirc.png"   
          />}
      >
        <Meta title="Europe Street beat" description="www.instagram.com" />
      </Card>
      </CSSTransition>
      <Button type="primary" onClick={()=>setInProp(!inProp)}>显示/隐藏</Button>
    </StyleDiv>
  );
}
```

```js
//引入styled-components
import styled from "styled-components";

//修改了div的样式
const StyleDiv = styled.div`
  width: 200px;
  text-align: center;
  Button {
    margin-top: 20px;
  }
  .card-enter,.card-appear {
      opacity:0;
      transform: scale(0);
  }
  .card-enter-active,.card-appear-active {
      opacity : 1;
      transform:scale(1);
      transition : opacity 300ms,transform 300ms ease-in-out;
  }
  .card-enter-done, .card-appear-done {
  }
  .card-exit {
    opacity : 1;
    transform:scale(1);
  }
  .card-exit-active {
    opacity : 0;
    transform:scale(0);
    transition : opacity 300ms,transform 300ms ease-in-out;
  }
  .card-exit-done {
  }
`;

export default StyleDiv;
```

#### SwitchTransition Demo

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142452.png" alt="image-20210407215450236" style="zoom:80%;" />

#### TransitionGroup Demo

TransitionGroup主要用于给循环的列表数据添加动画

```js
import React, { useState } from 'react'
import { CSSTransition, TransitionGroup } from 'react-transition-group'
import StyleDiv from './style'
import { Button } from 'antd';

export default function Demo() {
  const [data,setData] = useState([
    "first-child",
    "second-child",
    "third-child",
    "forth-child"
  ])
  return (
    <StyleDiv>
      {/* 如果CSSTransition包裹在循环中，必须在循环外套上TransitionGroup，否则设置动画无效 */}
      <TransitionGroup>
        {
          data.map((v,i)=>{
            console.log(v)
            return (
              <CSSTransition 
                //注意key，如果key值相同则不会显示，而且key值不能为数组索引
                //否则逆序或者删除动画显示会出错，这里必须用不变的唯一值做key
                key={v}
                timeout={500}
                classNames="item"
              >
                <div>{v}</div>
              </CSSTransition>
            )
          })
        }
      </TransitionGroup>
      <Button type='primary' onClick={()=>{
        setData(["newData"+Date.now(),...data])
      }}>添加一条数据</Button>
    </StyleDiv>
  );
}
```

```js
//引入styled-components
import styled from "styled-components";

//修改了div的样式
const StyleDiv = styled.div`
    .item-enter{
        opacity:0;
    }
    .item-enter-active{
        opacity:1;
        transition:opacity 300ms;
    }
`;
export default StyleDiv;
```



### 纯函数

必须满足以下两点

1、确定的输入产生确定的输出（不能有文件IO、随机数等）

2、不能有副作用（修改外部变量）

React中纯函数

1、组件必须像纯函数一样，保护他的props不会变化

2、redux中reducer也必须是一个纯函数

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142501.png" alt="image-20210406195038501" style="zoom:80%;" />![image-20210406195512910](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142507.png)

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142513.png" alt="image-20210406195038501" style="zoom:80%;" />![image-20210406195512910](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142522.png)![image-20210406195716938](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142528.png)

### redux

#### 简介

![image-20210408101347068](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142557.png)

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142603.png" alt="image-20210407224901918" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142608.png" alt="image-20210407224953083" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142615.png" alt="image-20210407225129045" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142620.png" alt="image-20210407225155196" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142625.png" alt="image-20210407230228365" style="zoom:80%;" />

#### 基础使用（无需依赖react)

```js
    //定义初始状态
    const initState = {
      count: 0
    }
    //reducer纯函数
    function reducer(state = initState, action) {
      switch (action.type) {
        case 'INCREMENT':
          //纯函数中不能修改state，必须返回一个新对象，因此reducer里面有很多对象解构
          return { ...state, count: state.count + 1 }
        case 'DECREMENT':
          return { ...state, count: state.count - 1 }
        case 'ADD_NUMBER':
          return {...state, count: state.count + action.num}
        case 'SUB_NUMBER':
          return {...state, count: state.count - action.num}
      }
    }
    //创建store
    const store = Redux.createStore(reducer)
    //订阅更新，查看结果
    store.subscribe(() => {
      console.log('state发生了改变',store.getState().count)
    })
    const action1 = {
      type:'INCREMENT'
    }
    const action2 = {
      type:'DECREMENT'
    }
    const action3 = {
      type: 'ADD_NUMBER',
      num:3
    }
    const action4 = {
      type: 'SUB_NUMBER',
      num:3
    }
    //派发action
    store.dispatch(action1)
    store.dispatch(action2)
    store.dispatch(action3)
    store.dispatch(action4)
```

#### 进阶使用

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142633.png" alt="image-20210408101739946" style="zoom:80%;" />

将将redux拆分成

constant.js  //存储aciton.type，因为reducer和createActions页面都需要，保持一致

```js
export const ADD_NUMBER = 'ADD_NUMBER'
export const SUB_NUMBER = 'SBU_NUMBER'
export const INCREMENT = 'INCREMENT'
export const DECREMENT = 'DECREMENT'
```

createActions.js  封装方法创建不同类型action

```js
import { ADD_NUMBER, SUB_NUMBER, INCREMENT, DECREMENT } from './constant'
export const addAction = num => ({
    type: ADD_NUMBER,
    num
})

export const subAction = num => ({
    type: SUB_NUMBER,
    num
})

export const incrementAction = () => ({
    type: INCREMENT
})

export const decrementAction = () => ({
    type: DECREMENT
})
```

reducer.js  根据不同的action.type对数据做不用处理，返回新数据

```js
import { INCREMENT, DECREMENT, ADD_NUMBER, SUB_NUMBER } from "./constant"

//定义初始状态
const initState = {
    count: 0
  }
  //reducer纯函数
export default function reducer(state = initState, action) {
    switch (action.type) {
      case INCREMENT:
        //纯函数中不能修改state，必须返回一个新对象，因此reducer里面有很多对象解构
        return { ...state, count: state.count + 1 }
      case DECREMENT:
        return { ...state, count: state.count - 1 }
      case ADD_NUMBER:
        return {...state, count: state.count + action.num}
      case SUB_NUMBER:
        return {...state, count: state.count - action.num}
      default:
            return state
    }
  }
```

index.js  创建store并导出

```js
import * as Redux from 'redux'
import reducer from './reducer'
const store = Redux.createStore(reducer)
export default store
```

使用

```js
import React, { PureComponent } from 'react'
import store from '../demo/store'
import { addAction } from '../demo/store/createActions'

export default class Home extends PureComponent {
    state={
        count:0
    }
    componentDidMount() {
        //订阅更新，查看结果
        store.subscribe(() => {
            console.log('state发生了改变', store.getState().count)
            this.setState({
                //添加监听，每次redux中存储的count一变化就会触发该监听并且重新执行setState，页面刷新
                count:store.getState().count
            })
        })
    }

    handleClick = (num) => {
        //stroe派发action
        store.dispatch(addAction(num))
    }

    render() {
        const {count} = this.state
        return (
            <div>
                <h1>Home</h1>
                <div>当前计数：{count}</div>
                <button onClick={() => this.handleClick(1)}>+1</button>
                <button onClick={() => this.handleClick(5)}>+5</button>
            </div>
        )
    }
}
```

#### 进一步封装

每个页面中都要写state，组件挂载完毕后订阅监听，很多公共代码，封装一个connect函数

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142642.png" alt="image-20210408105332281" style="zoom:80%;" />

```js
import { PureComponent } from "react"
import store from '../demo/store/index'

export function connect(mapStateToProps, mapDispatchToProps) {
    //返回一个方法，这个方法执行后放回一个组件  connect(参数1，参数2)()
    return function enhanceHOC(WrappedComponent) {
        return class extends PureComponent {
            constructor(props) {
                super(props)
                this.state = {
                    storeData: mapStateToProps(store.getState())
                }
            }

            componentDidMount() {
                //订阅更新，查看结果
                this.unsubscribe = store.subscribe(() => {
                    this.setState({
                        storeData: mapStateToProps(store.getState())
                    })
                })
            }

            componentWillUnmount(){
                //卸载订阅
                this.unsubscribe()
            }

            render() {
                return <WrappedComponent {...this.props}
                    {...mapStateToProps(store.getState())}
                    {...mapDispatchToProps(store.dispatch)}
                />
            }
        }
    }
}
```

使用

```js
import React, { PureComponent } from 'react'
import { addAction } from '../demo/store/createActions'
import { connect } from '../utils/connect'

class Home extends PureComponent {
    render() {
        const {count,addNum} = this.props
        return (
            <div>
                <h1>Home</h1>
                <div>当前计数：{count}</div>
                <button onClick={() => addNum(1)}>+1</button>
                <button onClick={() => addNum(5)}>+5</button>
            </div>
        )
    }
}
const mapStateToProps = state => {
    return{
        count:state.count
    }
}

const mapDispatchToProps = dispatch => ({
    addNum:function(num){
        dispatch(addAction(num))
    }
})
//使用connect函数包裹，被包裹的是UI组件，包裹的是容器组件，容器组件负责与redux通信，获取数据，派发更新
export default connect(mapStateToProps,mapDispatchToProps)(Home)
```

#### 完全封装

函数内部还依赖import store from '../demo/store/index'中的store，需要用户手动导入文件，要想使用户可以配置，创建一个StoreContext包裹住根组件，让用户配置value为它自己的store即可。

1、index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import StoreContext from './components/utils/context'
import store from './components/demo/store/index'


ReactDOM.render(
  <StoreContext.Provider value={store}>
    <App />
  </StoreContext.Provider>,
  document.getElementById('root')
);
```

2、connect.js

```js
import { PureComponent } from "react"
import StoreContext from './context'

export function connect(mapStateToProps, mapDispatchToProps) {
    //返回一个方法，这个方法执行后放回一个组件  connect(参数1，参数2)()
    return function enhanceHOC(WrappedComponent) {
        return class extends PureComponent {
            
            static contextType = StoreContext

            //在constructor里面不能用this.context获取到store
            //因为constructor是类组件入口，此时context还没有挂载到组件
            //constructor第二个参数就是接收到的context
            constructor(props,context) {
                super(props)
                this.state = {
                    storeData: mapStateToProps(context.getState())
                }
            }

            componentDidMount() {
                //订阅更新，查看结果
                this.unsubscribe = this.context.subscribe(() => {
                    this.setState({
                        storeData: mapStateToProps(this.context.getState())
                    })
                })
            }

            componentWillUnmount(){
                //卸载订阅
                this.unsubscribe()
            }

            render() {
                return <WrappedComponent {...this.props}
                    {...mapStateToProps(this.context.getState())}
                    {...mapDispatchToProps(this.context.dispatch)}
                />
            }
        }
    }
}
```

3、context.js

```js
import React from 'react'
const StoreContext =  React.createContext()
export default StoreContext
```

4、使用

仍然同上

#### 使用react-redux

实际上我们已经实现了react-redux，了解其原理，这里进行使用。

`npm install react-redux`

使用react-redux的StoreContext和connect api取代自己封装的即可

1、index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import store from './components/demo/store/index'
import { Provider } from 'react-redux';


ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

2、使用

```js
import React, { PureComponent } from 'react'
import { addAction } from '../demo/store/createActions'
import { connect } from 'react-redux'

class Home extends PureComponent {
    render() {
        const {count,addNum} = this.props
        return (
            <div>
                <h1>Home</h1>
                <div>当前计数：{count}</div>
                <button onClick={() => addNum(1)}>+1</button>
                <button onClick={() => addNum(5)}>+5</button>
            </div>
        )
    }
}
const mapStateToProps = state => {
    return{
        count:state.count
    }
}

const mapDispatchToProps = dispatch => ({
    addNum:function(num){
        dispatch(addAction(num))
    }
})

export default connect(mapStateToProps,mapDispatchToProps)(Home)
```

#### redux处理异步

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142655.png" alt="image-20210408213407249" style="zoom:80%;" />![image-20210408215634561](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142701.png)

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142711.png" alt="image-20210408215800379" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142716.png" alt="image-20210408220006811" style="zoom:80%;" />

##### redux-thunk

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142723.png" alt="image-20210408220203725" style="zoom:80%;" />

0、添加新数据相关文件

```js
//constant.js
export const ADD_NUMBER = 'ADD_NUMBER'
export const SUB_NUMBER = 'SBU_NUMBER'
export const INCREMENT = 'INCREMENT'
export const DECREMENT = 'DECREMENT'
export const CHANGEIMAGE = 'CHANGEIMAGE'

//reducer.js
import { INCREMENT, DECREMENT, ADD_NUMBER, SUB_NUMBER, CHANGEIMAGE } from "./constant"

//定义初始状态
const initState = {
  count: 0,
  imageUrl: ''
}
//reducer纯函数
export default function reducer(state = initState, action) {
  switch (action.type) {
    case INCREMENT:
      //纯函数中不能修改state，必须返回一个新对象，因此reducer里面有很多对象解构
      return { ...state, count: state.count + 1 }
    case DECREMENT:
      return { ...state, count: state.count - 1 }
    case ADD_NUMBER:
      return { ...state, count: state.count + action.num }
    case SUB_NUMBER:
      return { ...state, count: state.count - action.num }
    case CHANGEIMAGE:
      return { ...state, imageUrl: action.url }
    default:
      return state
  }
}
```

1、store\index.js

```js
import * as Redux from 'redux'
import thunkMiddleware from 'redux-thunk'
import reducer from './reducer'

//应用异步中间件,applyMiddleware可以接收多个中间件
const storeEnhancer = Redux.applyMiddleware(thunkMiddleware)
//第二个参数是store提升
const store = Redux.createStore(reducer,storeEnhancer)

export default store
```

2、createActions.js

```js
export const changeImgAction = (url) => ({
    type:CHANGEIMAGE,
    url
})

export const imgAsyncAction = async (dispatch,getState) => {
    //getState是用来获取store中存储的数据，如果网络请求依赖其他数据，则可以在这里获得
    console.log('进入异步action')
    const state = getState()
    const res = await axios.get('https://yesno.wtf/api')
    //派发同步action修改图片
    dispatch(changeImgAction(res.data.image))
}
```

3、test\index.js

```js
import React, { useEffect,useState } from 'react'
import axios from 'axios'
import { connect } from 'react-redux'
import { imgAsyncAction } from '../demo/store/createActions'

const Test = (props) => {

    useEffect(()=>{
        props.getImage()
    },[])

    return (
        <div>
            <img src={props.imgUrl} style={{width:'100px',height:'50px'}}/>
        </div>
    )
}

export default connect(
    //mapStateToProps,是一个回调，返回值是对象
    state => ({
        imgUrl:state.imageUrl
    }),
    //mapDispatchToProps
    dispatch => ({
        getImage:() => dispatch(imgAsyncAction)
    })
)(Test)
```

##### redux-thunk结合redux-devtools插件使用

redux-devtool是一个谷歌浏览器，方便在浏览器里面查看redux存储的状态和派发的action。主要配置在store\index.js中

```js
import * as Redux from 'redux'
import thunkMiddleware from 'redux-thunk'
import reducer from './reducer'

//应用异步中间件,applyMiddleware可以接收多个中间件
const storeEnhancer = Redux.applyMiddleware(thunkMiddleware)
//第二个参数是store提升,配置redux-devtools
const composeEnhancers =
  typeof window === 'object' &&
  window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?   
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({
        trace:true
      // Specify extension’s options like name, actionsBlacklist, actionsCreators, serialize...
    }) : Redux.compose;


const store = Redux.createStore(reducer,composeEnhancers(storeEnhancer))

export default store
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142732.png" alt="image-20210408223357453" style="zoom:80%;" />

##### redux-saga处理异步

首先了解一下生成器、迭代器(async await 的底层原理)

```js
//简单使用,迭代器下一次调用next()方法的实参是上一个yield语句的返回值，
//如果不传，那么就是undefined
//it.next()第一次调用会截至到第一个yield语句之前
//it.next(param) 第二次调用会将param作为上一个yield的返回值，并且执行到第二个yield语句前
function* fun1(){
	console.log(1)
	const a = 2
	d = yield a
	console.log('d',d)
	e = yield b = 3
	console.log('e',e)
}

const it = fun1()
console.log(it.next())  //1
//将5传入作为yield a 的返回值被赋值给d，如果不传，那么d就是undefined
console.log(it.next()) // d undefined
console.log(it.next(6))  // e 6

//0-9逐个产生数字
function* fun2(){
	for(let i = 0; i<10;i++){
		yield i
	}
}
const it2 = fun2()
console.log(it2.next().value)
console.log(it2.next().value)
console.log(it2.next().value)
console.log(it2.next().value)

//结合promise使用
function* fun3(){
	console.log('进入fun3')
	const res = yield new Promise((resolve,reject)=>{
		setTimeout(()=>{
			resolve('data')
		},3000)
	})
	console.log('res',res)
}
const it3 = fun3()
it3.next().value.then(value => {
	console.log(value)  //3秒后接收到Promise的结果'data'
	it3.next(value)     //将'data'作为上一个yield语句的返回结果赋值给res
})
```

1、安装redux-saga

`npm install redux-saga`

2、store\index.js中

```js
import * as Redux from 'redux'
import thunkMiddleware from 'redux-thunk'
import createSagaMiddleware from 'redux-saga'
import reducer from './reducer'
import saga from './saga'

//创建redux-saga中间件
const sagaMiddleware = createSagaMiddleware()

//应用异步中间件,applyMiddleware可以接收多个中间件
const storeEnhancer = Redux.applyMiddleware(thunkMiddleware,sagaMiddleware)
//第二个参数是store提升,配置redux-devtools
const composeEnhancers =
  typeof window === 'object' &&
  window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?   
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({
        trace:true
      // Specify extension’s options like name, actionsBlacklist, actionsCreators, serialize...
    }) : Redux.compose;

const store = Redux.createStore(reducer,composeEnhancers(storeEnhancer))

//redux-saga，必须要run(),saga.js是一个生成器函数，里面存储了要处理的action
sagaMiddleware.run(saga)

export default store
```

2、saga.js

```js
import {takeEvery,put,all} from 'redux-saga/effects'
import { CHANGEIMAGESAGA } from './constant'
import axios from 'axios'
import { changeImgAction } from './createActions'

function* getImageData(action){
    const res = yield axios.get('https://yesno.wtf/api')
    console.log(res)
    yield all([
        //里面如果要触发多个同步action，再继续写yield put即可
        yield put(changeImgAction(res.data.image))
    ])
}

function* saga(){
    //第一个参数是要监听的action的type
    //第二个参数是生成器回调，定义用户如果dispatch该action该如何处理
    //takeEvery 和 takeLateset的区别
    //如果短时间内发送了多个action,在执行当前action时如果又来一个同名action
    //那么takeEvery会逐个执行，takeLateset会取消当前action，执行下一个
    yield all([
        //如果要监听多个action，继续写takeEvery即可
        takeEvery(CHANGEIMAGESAGA,getImageData)
        // takeEvery(CHANGEIMAGESAGA,getImageData)
    ])
}

export default saga
```

3、使用

```js
import React, { useEffect,useState } from 'react'
import axios from 'axios'
import { connect } from 'react-redux'
import { imgAsyncAction, imgSagaAsyncAction } from '../demo/store/createActions'

const Test = (props) => {

    useEffect(()=>{
        props.getImage()
    },[])

    return (
        <div>
            <img src={props.imgUrl} style={{width:'100px',height:'50px'}}/>
        </div>
    )
}

export default connect(
    //mapStateToProps,是一个回调，返回值是对象
    state => ({
        imgUrl:state.imageUrl
    }),
    //mapDispatchToProps
    dispatch => ({
        getImage:() => dispatch(imgSagaAsyncAction)
    })
)(Test)
```

##### redux-thunk与redux-saga的区别

1、thunk容易，只是将actionCreater从对象变成了一个回调函数，可以在回调函数中发送异步请求，更符合函数化编程的思想，缺点是异步代码存储在action中，耦合度较高

2、saga使用起来更复杂，基于生成器和yield的语法更难以理解。但是saga可以将异步请求与action分离，代码耦合度更低。action是普通对象，与redux相同，saga.js中集中处理了所有异步操作，方便接口测试

3、在小项目中，thunk基本足够使用。大项目中使用saga较多

#### reducer代码拆分

reducer和reduce语法有相似之处，所以才叫reducer，平时多逛逛MDN

reducer函数switch 分支过多，处理内容过多

1、第一步拆分，修改一下reducer文件

```js
import { INCREMENT, DECREMENT, ADD_NUMBER, SUB_NUMBER, CHANGEIMAGE } from "./constant"

//reducer按照组件拆分，每个组件reducer的数据放在一起
//home组件
const initHomeInfo = {
  count:0
}
const homeReducer = (state = initHomeInfo,action) => {
  switch (action.type) {
    case INCREMENT:
      //纯函数中不能修改state，必须返回一个新对象，因此reducer里面有很多对象解构
      return { ...state, count: state.count + 1 }
    case DECREMENT:
      return { ...state, count: state.count - 1 }
    case ADD_NUMBER:
      return { ...state, count: state.count + action.num }
    case SUB_NUMBER:
      return { ...state, count: state.count - action.num }
    default:
      return state
  }
}

//test组件
const initTestInfo = {
  imageUrl:''
}
const testReducer = (state = initTestInfo,action) => {
  switch(action.type){
    case CHANGEIMAGE:
      return { ...state, imageUrl: action.url }
    default:
      return state
  }
}

//reducer纯函数，总state下有根据组件划分的各个组件的Info,homeInfo、testInfo
export default function reducer(state = {}, action) {
  return {
    homeInfo:homeReducer(state.homeInfo,action),
    testInfo:testReducer(state.testInfo,action)
  }
}
```

2、第二步拆分独立文件

每个组件，对应自己的reducer

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142746.png" alt="image-20210409091452415" style="zoom:80%;" />

```js
//constant.js
export const ADD_NUMBER = 'ADD_NUMBER'
export const SUB_NUMBER = 'SBU_NUMBER'
export const INCREMENT = 'INCREMENT'
export const DECREMENT = 'DECREMENT'

//createActions.js
import { ADD_NUMBER, SUB_NUMBER, INCREMENT, DECREMENT} from './constant'
export const addAction = num => ({
    type: ADD_NUMBER,
    num
})
export const subAction = num => ({
    type: SUB_NUMBER,
    num
})
export const incrementAction = () => ({
    type: INCREMENT
})
export const decrementAction = () => ({
    type: DECREMENT
})

//reducer.js
import { INCREMENT, DECREMENT, ADD_NUMBER, SUB_NUMBER } from "./constant"
const initHomeInfo = {
  count:0
}
export const homeReducer = (state = initHomeInfo,action) => {
  switch (action.type) {
    case INCREMENT:
      //纯函数中不能修改state，必须返回一个新对象，因此reducer里面有很多对象解构
      return { ...state, count: state.count + 1 }
    case DECREMENT:
      return { ...state, count: state.count - 1 }
    case ADD_NUMBER:
      return { ...state, count: state.count + action.num }
    case SUB_NUMBER:
      return { ...state, count: state.count - action.num }
    default:
      return state
  }

    
//index.js，统一入口和导出出口
//统一入口文件，暴漏出去数据
import {homeReducer} from './reducer'
export default homeReducer  

```

总reducer中

```js
import homeReducer from "./home";
import testReducer from "./test";

//reducer按照组件拆分，每个组件reducer的数据放在一起
//reducer纯函数
export default function reducer(state = {}, action) {
  return {
    homeInfo:homeReducer(state.homeInfo,action),
    testInfo:testReducer(state.testInfo,action)
  }
}
```

3、使用redux提供的compineReducers函数进行reducer合并

总reducer中

```js
import homeReducer from "./home";
import testReducer from "./test";
import { combineReducers } from "redux";

const reducer = combineReducers({
  homeInfo:homeReducer, //传递方法即可，combineReducers方法会自动调用
  testInfo:testReducer
})

export default reducer
```

#### 到底用什么管理state

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142753.png" alt="image-20210409093126556" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142759.png" alt="image-20210409093249739" style="zoom:80%;" />



#### monkey patching

给内置对象拓展方法

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142809.png" alt="image-20210409082347718" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142816.png" alt="image-20210409082422904" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142822.png" alt="image-20210409083603170" style="zoom:80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142828.png" alt="image-20210409094346394" style="zoom:67%;" />

### react路由

#### 三大阶段

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142838.png" alt="image-20210409094811023" style="zoom:80%;" />![image-20210409095206653](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142844.png)



![image-20210409095402679](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142851.png)

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142856.png" alt="image-20210409095523318" style="zoom:80%;" />

![image-20210409095636593](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142903.png)

![image-20210409095850307](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142916.png)

![image-20210409095922453](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142927.png)

#### 前端路由的原理

![image-20210409100224967](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142932.png)

![image-20210409100718901](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142937.png)

![image-20210409110250782](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142943.png)

#### react-router

![image-20210409110404339](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142948.png)

![image-20210409110706402](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514142955.png)

##### 基本使用

```js
import 'antd/dist/antd.less'
import {
  HashRouter,
  Link,
  NavLink,
  BrowserRouter,
  Route
} from 'react-router-dom'
import Home from './components/react-router/home'
import Profile from './components/react-router/profile'
import About from './components/react-router/about'

export default function App() {
  return (
    // HashRouter路由有锚点
    // BrowserRouter没有锚点
    // HashRouter浏览器兼容新更好
    <HashRouter>
	  {/* Link标签被渲染成a标签 */}
      <Link to="/">home </Link>
      <Link to="/about">about </Link>
      <Link to="/profile">profile </Link>

      {/* 默认是做模糊匹配，由于Link中的to属性'/about'和'/profile'包含'/' */}
      {/* 因此触发模糊匹配，匹配到Home和About/Profile两个组件 */}
      {/* 给Route标签机上exact属性代表精确匹配，只有访问地址是/才会完全匹配 */}
      <Route path='/' exact component={Home}/>
      <Route path='/about' component={About}/>
      <Route path='/profile' component={Profile}/>
    </HashRouter>
  );
}
```

* Route放在什么地方，就渲染在什么地方



![image-20210409113638256](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143003.png)

![image-20210409114048958](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143009.png)

Demo

```js
import 'antd/dist/antd.less'
import './reset.css'
import {
  HashRouter,
  Link,
  NavLink,
  BrowserRouter,
  Route,
  Switch
} from 'react-router-dom'
import Home from './components/react-router/home'
import Profile from './components/react-router/profile'
import About from './components/react-router/about'
import NoMatch from './components/react-router/nomatch'
import User from './components/react-router/user'

export default function App() {
  return (
    // HashRoute路由有锚点
    // BrowserRouter没有锚点
    <BrowserRouter>
      {/* exact精确匹配，只有地址是/才会显示activeStyle和activeClassName对应的样式 */}
      <NavLink exact to="/" activeStyle={{ color: 'red' }} activeClassName="item-active">home </NavLink>
      <NavLink to="/about" activeStyle={{ color: 'red' }} activeClassName="item-active">about </NavLink>
      <NavLink to="/profile" activeStyle={{ color: 'red' }} activeClassName="item-active">profile </NavLink>
      <NavLink to="/id:111" activeStyle={{ color: 'red' }} activeClassName="item-active">user </NavLink>
      <NavLink to="/222" activeStyle={{ color: 'red' }} activeClassName="item-active">nomatch </NavLink>

	  //使用Switch包裹，每次都从上往下匹配，匹配到一个就退出，不会匹配多个
      <Switch>
        {/* 默认是做模糊匹配，由于Link中的to属性'/about'和'/profile'包含'/' */}
        {/* 因此触发模糊匹配，匹配到Home和About/Profile两个组件 */}
        {/* 给Route标签机上exact属性代表精确匹配，只有访问地址是/才会完全匹配 */}
        <Route path='/' exact component={Home} />
        <Route path='/about' component={About} />
        <Route path='/profile' component={Profile} />
        //匹配to'id:xxx'形式
        <Route path='/id:userid' component={User} />
        //上面的路由都没有匹配成功，返回一个提示页面。提示用户当前页面不存在
        <Route component={NoMatch} />
      </Switch>
    </BrowserRouter>
  );
}
```



![image-20210409114538041](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143015.png)

Demo

```js
import React, { useEffect,useState } from 'react'
import { Redirect } from 'react-router-dom'

export default function Home() {

    const [login, setLogin] = useState(false)
  
    //如果没有登录，重定向到登录页面
    if (!login) {
      return (
        <Redirect to='/login' />
      )
    }

    
    return (
        <div>
            Home
        </div>
    )
}
```



![image-20210409114622120](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143027.png)

Demo

```js
//about.js
import React from 'react'
import { NavLink, Switch, Route } from 'react-router-dom'

function aboutHostory() {
    return (
        <div>
            500年历史
        </div>
    )
}

function aboutCulture() {
    return (
        <div>
            源远流长
        </div>
    )
}

function aboutContact(){
    return (
        <div>
            电话：025-8888
        </div>
    )
}


export default function About() {
    return (
        <div>
            <NavLink to='/about'>企业历史</NavLink>
            <NavLink to='/about/culture'>企业文化</NavLink>
            <NavLink to='/about/contact'>联系我们</NavLink>
            <Switch>
                {/* 如果不加exact，访问/about，匹配的是App.js中的About组件 */}
                {/* 不再进行匹配。如果加上about，不仅会渲染About组件，也会 */}
                {/* 渲染aboutHistory组件 */}
                <Route exact component={aboutHostory} path='/about' />
                <Route component={aboutCulture} path='/about/culture' />
                <Route component={aboutContact} path='/about/contact' />
            </Switch>
        </div>
    )
}

//App.js
import 'antd/dist/antd.less'
import './reset.css'
import {
  HashRouter,
  Link,
  NavLink,
  BrowserRouter,
  Route,
  Switch,
  Redirect
} from 'react-router-dom'
import Home from './components/react-router/home'
import Profile from './components/react-router/profile'
import About from './components/react-router/about'
import NoMatch from './components/react-router/nomatch'
import User from './components/react-router/user'
import { useState, useEffect } from 'react'
import Login from './components/react-router/login'

export default function App() {

  return (

    <BrowserRouter>
      {/* exact精确匹配，只有地址是/才会显示activeStyle和activeClassName对应的样式 */}
      <NavLink exact to="/" activeStyle={{ color: 'red' }} activeClassName="item-active">home </NavLink>
      <NavLink to="/about" activeStyle={{ color: 'red' }} activeClassName="item-active">about </NavLink>
      <NavLink to="/profile" activeStyle={{ color: 'red' }} activeClassName="item-active">profile </NavLink>
      <NavLink to="/login" activeStyle={{ color: 'red' }} activeClassName="item-active">login </NavLink>
      <NavLink to="/id:111" activeStyle={{ color: 'red' }} activeClassName="item-active">user </NavLink>
      <NavLink to="/222" activeStyle={{ color: 'red' }} activeClassName="item-active">nomatch </NavLink>

      <Switch>
        {/* 默认是做模糊匹配，由于Link中的to属性'/about'和'/profile'包含'/' */}
        {/* 因此触发模糊匹配，匹配到Home和About/Profile两个组件 */}
        {/* 给Route标签机上exact属性代表精确匹配，只有访问地址是/才会完全匹配 */}
        <Route path='/' exact component={Home} />
        <Route path='/about' component={About} />
        <Route path='/profile' component={Profile} />
        <Route path='/login' component={Login} />
        <Route path='/id:userid' component={User} />
        <Route component={NoMatch} />
      </Switch>
    </BrowserRouter>
  );
}
```



##### props.history跳转路由

![image-20210409140149304](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143035.png)

* 注意：只有路由组件可以通过this.props.history进行路由跳转。非路由组件可以使用withRouter高阶组件包裹后，也可以使用this.props.history
* 函数组件也可以使用useHistory hook
* react-router的history api如下

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143042.png" alt="image-20210409130921113" style="zoom:50%;" />

##### HashRouter和BrowserRouter的区别

- **URL的表现形式不一样**
  BrowseRouter使用HTML5的history API，保证UI界面和URL同步。HashRouter使用URL的哈希部分来保持UI和URL的同步。哈希历史记录不支持location.key和location.state。
- **HashRouter不支持location.state和location.key**
  首先我们需要了解页面之间传值的方式，参考我之前写的文章
  [React页面传值的做法](https://www.jianshu.com/p/33ae1c9c992e)
  通过state的方式传值给下一个页面的时候，当到达新的页面，刷新或者后退之后再前进，BrowseRouter传递的值依然可以得到。

##### 路由传参

![image-20210409140256489](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143055.png)

1、动态路由传简单参数

通过Link的to属性传递通过this.props.match.params获取动态路由传递过来的参数，刷新不会丢失

```js
//传递
      <NavLink to="/user/1234" activeStyle={{ color: 'red' }} activeClassName="item-active">user </NavLink>
	  <Route path='/user/:userid' component={User} />

//接收
import React from 'react'
export default function User(props) {
    return (
        <div>
            {props.match.params.userid}
        </div>
    )
}
```

2、search传参(已经不推荐使用)

```js
//传递
      <NavLink to="/user?name=why&age=18" activeStyle={{ color: 'red' }} activeClassName="item-active">user </NavLink>
	  <Route path='/user' component={User} />
          
//接收
import React from 'react'
export default function User(props) {
    console.log(props.location.search) // 打印 ?name=why&age=18
    console.log(props.history.location.search) //打印 ?name=why&age=18
    //这两个都可以获取到数据，但是没有转换成object类型，可自己转换或者借用
    //querystring转换。
    return (
        <div>
            {props.location.search.slice(1).replace('&',' and ')}
        </div>
    )
}
        
```

3、Link（NavLink）标签的to属性传入一个对象(复杂参数推荐使用，但是要注意HashRouter包裹的话，刷新会丢失state。BrowserRouter包裹根组件则不会丢失）

```js
//传递
NavLink to={{
        pathname:'/user',
        search:'?test=1&test2=2',
        state:{
          name:'why',
          age:28
        }
      }} activeStyle={{ color: 'red' }} activeClassName="item-active">user </NavLink>
<Route path='/user' component={User} />

//接收
import React from 'react'
export default function User(props) {
    //可以获取到search
    console.log(props.location.search)
    //可以获取到对象类型的数据
    console.log(props.location.state)
    return (
        <div>
            {JSON.stringify(props.location.state)}
        </div>
    )
}
```

4、props.history.方法传参（但是要注意HashRouter包裹的话，刷新会丢失state。BrowserRouter包裹根组件则不会丢失）

```js
<div onClick={this.click.bind(this)}>About</div>

//js
click(){
    this.props.history.push({ pathname: "/about", state: { id } });
}

//子组件中获取参数
this.props.location.state.id
```

5、监听路由退出当前页面弹出提示框（多用于表单未填写完毕用户返回或者关闭窗口)

```js
<Prompt
  when={formIsHalfFilledOut}
  message="Are you sure you want to leave?"
/>
message：可以是回调函数或者字符串，提示信息
when：布尔型，true代表用户退出时会弹出提示框，false代表用户退出时不会弹出提示框
```

##### react-router-config 配置路由

`npm install react-router-config`

新增一个routes.js

```js
import Home from "../home";
import NoMatch from "../nomatch";
import {About,aboutContact,aboutHostory,aboutCulture} from "../about";
//路由配置文件
const routes = [
    {
        path:'/',
        exact:true,
        component:Home
    },
    {
        path:'/about',
        component:About,
        routes:[
            {
                path:'/about',
                exact:true,
                component:aboutHostory
            },
            {
                path:'/about/culture',
                component:aboutCulture
            },
            {
                path:'/about/contact',
                component:aboutContact
            }
        ]
    },
    {
        component:NoMatch
    }
]
export default routes
```

```js
//一级路由渲染
import routes from './components/react-router/routes';
jsx中{renderRoutes(routes)}

//二级路由渲染
{/* 只有使用renderRoutes渲染的路由才会有props.route属性 */}
{/*通过props.route获取到当前路由，也就是一级路由，通过props.route.routes获取二级路由*/}
{renderRoutes(props.route.routes)}
```

##### react-router路由拦截鉴权



##### react-router路由懒加载



### react hooks

![image-20210409195942685](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143112.png)

![image-20210409200126213](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143118.png)

![image-20210409200302300](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143123.png)

* 函数组件依然需要`import React from 'react'`
* 严格模式下有些组件会被调用两次来测试代码是否有问题

![image-20210409200929759](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143129.png)

![image-20210409201400028](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143134.png)

hook必须在顶层代码使用

![image-20210409201615518](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143139.png)

setXXX必须传入一个新数据，否则不会刷新。

setCount(preCount => preCount+1) 类似this.setState(preState => ({count:preState.count+1}))。都是为了防止多行setState同时运行修改某一项，只修改一次。传入回调函数的形式能确保当前回调执行完毕再执行下一行

#### useState

#### useEffect

```js
useEffect(()=>{
    //compontendDidMount+compontDidUpdate
})

useEffect(()=>{
    //componentDidMount
	return () => {
        //componentWillUnmout
    }
},[])

useEffect(()=>{
    //只有依赖项count变化才会进入这个回调，依赖项可以有多个
},[count])
```

#### useContext

以前往往需要用Context.Consumer，如果有多个还需要嵌套，使用useContext hook则非常简单

![image-20210409205129911](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143148.png)

![image-20210409210251904](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143154.png)

#### useReducer

只是useState的升级版，不能取代redux，没法共享数据。当state的逻辑处理过于复杂，可以使用useReducer

![image-20210409210944847](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143158.png)

![image-20210409211144124](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143205.png)

#### useCallback

![image-20210409212632774](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143212.png)

#### useMemo



#### useRef

![image-20210409223737556](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143218.png)

类组件可以通过ref拿到实例对象，函数组件不行，可以使用forwardRef包裹函数组件转发ref，然后给函数组件中的某个DOM设置ref。函数组件可以通过转发ref获取到具体的DOM节点

DEMO

```JS
import React,{useState,useEffect,useRef} from 'react'
export const Test = (props) => {
    const [count,setCount] = useState(0)
    const dataRef = useRef({
        count:0
    })
    useEffect(()=>{
        //当count数值发生变化时触发回调
        //count+1以后界面render，才会触发这里回调（类似componentDidUpate)，因此dataRef.current.count保存的就是上一次的值
        dataRef.current.count = count
    },[count])
    
    return (
    	<div>
        	<h1>之前的值：{dataRef.current.count}</h1>
			<h1>现在的值: {count}</h1>
        	<button onClick={()=>{setCount(count + 1)}}>+1</button>
        </div>
    )
}
```

#### useImperativeHandle

一般结合forwardRef+函数组件使用

类组件可以通过ref拿到实例对象，函数组件不行，可以使用forwardRef包裹函数组件转发ref，然后给函数组件中的某个DOM设置ref。函数组件可以通过转发ref获取到具体的DOM节点。

![image-20210409225247071](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143226.png)

如果只使用forwardRef，为了获得子组件的一个DOM节点，结果给整个子组件挂载了ref，可操纵子组件的内容过多，自由度太多

改造：子组件中使用useImperativeHandle方法，接收一个参数时ref,第二个参数是一个回调函数，返回值是一个可被父组件获取的方法/属性，这样当父组件调用inputRef.current.focus()时实际上执行的时useImperativeHanle的focus方法。再这个方法里面，通过子组件自身的ref获取到input节点，执行focus方法

```js
import 'antd/dist/antd.less'
import './reset.css'

import React,{useState,useEffect,useRef, forwardRef, useImperativeHandle} from 'react'


const HYInput = forwardRef((props,ref) => {
  const inputRef = useRef()
  //受控组件
  const [inputText,setInputText] = useState('')

  //由于要返回实时的输入值，因此这里依赖inputText
  useImperativeHandle(ref,()=>({
    focus:()=>{inputRef.current.focus()},  //方法
    value:inputText                        //值
  }),[inputText])

  return (
    <input value={inputText} onChange={(e)=>{setInputText(e.target.value)}} ref={inputRef} />
  )
})

const App = (props) => {
  const inputref = useRef()
  return (
    <div>
      <HYInput ref={inputref}/>
      {/* 通过inputRef.current调用的就是子组件中useImperativeHandle中第二个参数返回的对象 */}
      {/* 对象中可以存储input组件的方法或者值 */}
      {/* 使用useImperativeHandle更受控，不会对子组件做出额外的操作 */}
      <button onClick={()=>{inputref.current.focus()}}>获取焦点</button>
      <button onClick={()=>{console.log(inputref.current.value)}}>获取值</button>
    </div>
  )
}

export default App
```

#### useLayoutEffect

![image-20210409232008293](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143255.png)

### 自定义hook

### hooks原理

#### Fiber

React16推出，

电脑屏幕参数：刷新率，一秒钟刷新多少次，单位hz（赫兹）

GUI渲染和JS代码执行是在一个线程，互斥

### 网易云官网项目

#### 项目规范

![image-20210410153949567](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143301.png)

![image-20210410154215360](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143306.png)

1、项目创建

2、文件夹创建

3、重置样式reset.css和common.css

4、安装antd并配置

5、安装craco配置文件夹快捷路径，避免层层../

```js
//craco.config.js
const CracoLessPlugin = require('craco-less');
const path = require("path");
const resolve = dir => path.resolve(__dirname, dir);
module.exports = {
  plugins: [
    {
      plugin: CracoLessPlugin,
      options: {
        lessLoaderOptions: {
          lessOptions: {
            modifyVars: { '@primary-color': '#1DA57A' },
            javascriptEnabled: true,
          },
        },
      },
    },
  ],
  webpack: {
    alias: {
      '@': resolve("src")
    }
  }
};
```

6、页面划分AppHeader AppFooter

7、路由配置react-router-dom  react-router-config

8、导入规范

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143316.png" alt="image-20210410160640081" style="zoom:50%;" />

* 创建页面rmc，快捷键创建memo包裹的函数组件

9、使用styled-component编写AppHeader和AppFooter样式（背景图片用精灵图）

10、主页面路由优化 、 发现二级路由配置

11、封装axios
12、redux react-redux redux-thunk redux-devtool搭建，每一个子页面一个reducer

13、使用redux获取推荐页面轮播图数据

14、使用redux hooks替代connect mapStateToProps  useSelecter useDispatch  shallowEqual

```js
import React, { memo, useEffect } from 'react'
import { getBannersAction } from './store/actionCreator'
import { useSelector, shallowEqual } from 'react-redux'
import { useDispatch } from 'react-redux'

function Recommend(props){

    //useSelector存在性能问题，每次都会刷新，内部是做全等判断，对象的全等肯定false，因此这里传递第二个参数shallowEqueal作为比较方法进行浅比较，和connect函数相同
    const banners = useSelector(state => state.recommendInfo.banners,shallowEqual)
    
    const dispatch = useDispatch()

    useEffect(()=>{
        //获取轮播图
        dispatch(getBannersAction())
    },[dispatch])

    return (
        <div>
            Recommend:
            {
                banners && banners.length
            }
        </div>
    )
}

export default memo(Recommend)
```

15、ImmutableJS解决对象拷贝性能问题

 

![image-20210411222728645](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143323.png)

![image-20210411222839089](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143330.png)

![image-20210412141942401](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143336.png)

16、redux-immutable的combineReducer函数

```js
//recommend/store/reducer.js
import { CHANGEBANNER } from "./constant";
import {Map} from 'immutable'
//初始状态
//Map函数返回一个im对象，设置用set，获取用get
const initState = Map({
    banners:[]
})
//reducer中数据合并均采用Immutable
export function reducer(state=initState,action){
    switch(action.type){
        case CHANGEBANNER:
            // 不用es6对象解构，使用immutable，使用set方法设置其值
            return initState.set("banners",action.data)
        default:
            return state
    }
}

//stroe/index.js
import recommendReducer from "../pages/find-music/children-pages/recommend/store";
import { combineReducers } from "redux-immutable";
//reducer按照页面拆分，每个页面reducer的数据放在一起
//reducer纯函数
//使用redux-immutable的combineReducers提高效率
const reducer = combineReducers({
    recommendInfo:recommendReducer
})
export default reducer

//使用
const {banners} = useSelector(state => {
        return {
            // banners:state.recommendInfo.banners  对象形式
            // banners:state.get('recommendInfo').get('banners') Immutable get方法
            banners:state.getIn(['recommendInfo','banners'])  //Immutable.getIn语法糖
        }
    },shallowEqual)
```

17、antd轮播图(走马灯)，轮播图组件封装

18、热门推荐header组件封装

19、热门推荐数据请求

20、song-cover封装，优化，图片太大，图片后缀使用param=140*140

![image-20210413175944689](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143344.png)

新碟上架组件开发

21、播放器功能实现audio+Slider

![image-20210415145315670](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143349.png)

![image-20210415145703859](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143355.png)

# 二十三 dva

## 简介

dva是一个基于redux和redux-saga的数据流方案，同时为了方便开发，集成了react-router和fetch，也可以理解为一个轻量级的应用框架

## 特性

- **易学易用**，仅有 6 个 api，对 redux 用户尤其友好，[配合 umi 使用](https://umijs.org/guide/with-dva.html)后更是降低为 0 API
- **elm 概念**，通过 reducers, effects 和 subscriptions 组织 model
- **插件机制**，比如 [dva-loading](https://github.com/dvajs/dva/tree/master/packages/dva-loading) 可以自动处理 loading 状态，不用一遍遍地写 showLoading 和 hideLoading
- **支持 HMR**，基于 [babel-plugin-dva-hmr](https://github.com/dvajs/babel-plugin-dva-hmr) 实现 components、routes 和 models 的 HMR

## 快速上手

### 安装 dva-cli

通过 npm 安装 dva-cli 并确保版本是 `0.9.1` 或以上。

```bash
$ npm install dva-cli -g
$ dva -v
dva-cli version 0.9.1
```

### 创建新应用

安装完 dva-cli 之后，就可以在命令行里访问到 `dva` 命令（[不能访问？](http://stackoverflow.com/questions/15054388/global-node-modules-not-installing-correctly-command-not-found)）。现在，你可以通过 `dva new` 创建新应用。

```bash
$ dva new dva-quickstart
```

这会创建 `dva-quickstart` 目录，包含项目初始化目录和文件，并提供开发服务器、构建脚本、数据 mock 服务、代理服务器等功能。

然后我们 `cd` 进入 `dva-quickstart` 目录，并启动开发服务器：

```bash
$ cd dva-quickstart
$ npm start
```

几秒钟后，你会看到以下输出：

```bash
Compiled successfully!

The app is running at:

  http://localhost:8000/

Note that the development build is not optimized.
To create a production build, use npm run build.
```

在浏览器里打开 http://localhost:8000 ，你会看到 dva 的欢迎界面。

### 使用 antd

通过 npm 安装 `antd` 和 `babel-plugin-import` 。`babel-plugin-import` 是用来按需加载 antd 的脚本和样式的，详见 [repo](https://github.com/ant-design/babel-plugin-import) 。

```bash
$ npm install antd babel-plugin-import --save
```

编辑 `.webpackrc`，使 `babel-plugin-import` 插件生效。

```diff
{
+  "extraBabelPlugins": [
+    ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }]
+  ]
}
```

> 注：dva-cli 基于 roadhog 实现 build 和 dev，更多 `.webpackrc` 的配置详见 [roadhog#配置](https://github.com/sorrycc/roadhog#配置)

### 定义路由

我们要写个应用来先显示产品列表。首先第一步是创建路由，路由可以想象成是组成应用的不同页面。

新建 route component `routes/Products.js`，内容如下：

```javascript
import React from 'react';

const Products = (props) => (
  <h2>List of Products</h2>
);

export default Products;
```

添加路由信息到路由表，编辑 `router.js` :

```diff
+ import Products from './routes/Products';
...
+ <Route path="/products" exact component={Products} />
```

然后在浏览器里打开 http://localhost:8000/#/products ，你应该能看到前面定义的 `<h2>` 标签。

### 编写 UI Component

随着应用的发展，你会需要在多个页面分享 UI 元素 (或在一个页面使用多次)，在 dva 里你可以把这部分抽成 component 。

我们来编写一个 `ProductList` component，这样就能在不同的地方显示产品列表了。

新建 `components/ProductList.js` 文件：

```javascript
import React from 'react';
import PropTypes from 'prop-types';
import { Table, Popconfirm, Button } from 'antd';

const ProductList = ({ onDelete, products }) => {
  const columns = [{
    title: 'Name',
    dataIndex: 'name',
  }, {
    title: 'Actions',
    render: (text, record) => {
      return (
        <Popconfirm title="Delete?" onConfirm={() => onDelete(record.id)}>
          <Button>Delete</Button>
        </Popconfirm>
      );
    },
  }];
  return (
    <Table
      dataSource={products}
      columns={columns}
    />
  );
};

ProductList.propTypes = {
  onDelete: PropTypes.func.isRequired,
  products: PropTypes.array.isRequired,
};

export default ProductList;
```

### 定义 Model

完成 UI 后，现在开始处理数据和逻辑。

dva 通过 model 的概念把一个领域的模型管理起来，包含同步更新 state 的 reducers，处理异步逻辑的 effects，订阅数据源的 subscriptions 。

新建 model `models/products.js` ：

```javascript
export default {
  namespace: 'products',
  state: [],
  reducers: {
    'delete'(state, { payload: id }) {
      return state.filter(item => item.id !== id);
    },
  },
};
```

这个 model 里：

- `namespace` 表示在全局 state 上的 key
- `state` 是初始值，在这里是空数组
- `reducers` 等同于 redux 里的 reducer，接收 action，同步更新 state

然后别忘记在 `index.js` 里载入他：

```diff
// 3. Model
+ app.model(require('./models/products').default);
```

### connect 起来

到这里，我们已经单独完成了 model 和 component，那么他们如何串联起来呢?

dva 提供了 connect 方法。如果你熟悉 redux，这个 connect 就是 react-redux 的 connect 。

编辑 `routes/Products.js`，替换为以下内容：

```javascript
import React from 'react';
import { connect } from 'dva';
import ProductList from '../components/ProductList';

const Products = ({ dispatch, products }) => {
  function handleDelete(id) {
    dispatch({
      type: 'products/delete',
      payload: id,
    });
  }
  return (
    <div>
      <h2>List of Products</h2>
      <ProductList onDelete={handleDelete} products={products} />
    </div>
  );
};

// export default Products;
export default connect(({ products }) => ({
  products,
}))(Products);
```

最后，我们还需要一些初始数据让这个应用 run 起来。编辑 `index.js`：

```diff
- const app = dva();
+ const app = dva({
+   initialState: {
+     products: [
+       { name: 'dva', id: 1 },
+       { name: 'antd', id: 2 },
+     ],
+   },
+ });
```

刷新浏览器，应该能看到以下效果：

![img](https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143416.gif)

### 构建应用

完成开发并且在开发环境验证之后，就需要部署给我们的用户了。先执行下面的命令：

```bash
$ npm run build
```

几秒后，输出应该如下：

```bash
> @ build /private/tmp/myapp
> roadhog build

Creating an optimized production build...
Compiled successfully.

File sizes after gzip:

  82.98 KB  dist/index.js
  270 B     dist/index.css
```

`build` 命令会打包所有的资源，包含 JavaScript, CSS, web fonts, images, html 等。然后你可以在 `dist/` 目录下找到这些文件。

### 简单DEMO（不包含网络请求和路由)

* 入口文件index.js

```js
import dva from 'dva';
import './index.css';

// 1. Initialize 初始化
//第一种方式，在入口文件index.js中编写initialState，相当于是在合并reducer时赋默认值
//第二种方式，在model中给state=[]赋予默认值，相当于是给每个reducer赋默认值
const app = dva({
    initialState:{
        products:[
            {name:'dva',id:1},
            {name:'antd',id:2}
        ]
    }
});

// 2. Plugins 中间件
// app.use({});

// 3. Model 配置model
// app.model(require('./models/example').default);
app.model(require('./models/product').default);

// 4. Router 加载路由配置
app.router(require('./router').default);

// 5. Start 将单页面内容spa挂载在index.html中的root节点
app.start('#root');
```

* 路由配置router.js

```js
import React from 'react';
import { Router, Route, Switch } from 'dva/router';
import IndexPage from './routes/IndexPage/IndexPage';
import ProductPage from './routes/ProductPage/ProductPage';

function RouterConfig({ history }) {
  return (
    <Router history={history}>
      <Switch>
        <Route path="/" exact component={IndexPage} />
        <Route path="/product" component={ProductPage} />
      </Switch>
    </Router>
  );
}

export default RouterConfig;
```

* model文件

```js
//model/product.js
export default {
    //reducer合并的标识，一般一个page，一个reducer，以page名命名即可
    //注意page和component的区别
    namespace: 'products',
    state: [
        //可以在入口文件index.js中设置initialState设置默认值
        //也可以在此处设置默认值
        // { name: 'dva', id: 1 },
        // { name: 'antd', id: 2 }
    ],
    reducers: {
        //也可以给delete加上''，没区别
        delete(state, { payload: id }) {
            //{payload:id}是对action对象的解构
            return state.filter(item => item.id !== id);
        },
        //增加一位
        'add'(state,{data}){
            console.log(data)
            return [...state,data]
        }
        //对象中可以继续写reducer
    }
}
```

* ProductPage页面

```js
//routes/ProductPage/ProductPage.js
import React, { useCallback } from 'react';
import {v4 as uuid} from 'uuid'
import { connect } from 'dva';
import { Button } from 'antd';

import ProductList from '../../components/ProductList/ProductList';

//使用dva的connect，props中默认可以接收到dispatch，不需要mapDispatchToProps
const Products = ({ dispatch, products }) => {
  function handleDelete(id) {
    dispatch({
      //products与model的namespace保持一致，这个namespace就是合并reducer的标识
      //delete是reducer的方法名
      type: 'products/delete',
      //传递payload，model的reducer中可以使用
      //const {payload:id} = {type:'products/delete',payload:id}来解构获取id
      payload: id,
    });
  }

  //增加一位，信息使用uuid随机(确保唯一值)
  const handleAdd = useCallback(
      () => {
          dispatch({
              type:'products/add',
              data:{name:'随机姓名:'+uuid(),id:uuid()}
          })
      },
      [],
  )

  return (
    <div>
      <h2>List of Products</h2>
      <ProductList onDelete={handleDelete} products={products} />
      <Button type="primary" onClick={handleAdd}>添加</Button>
    </div>
  );
};

//connect函数和react-redux的connect函数基本一致，就是只需要传递mapStateToProps回调即可
//不需要传递mapDispatchToProps,dva的connect函数会给被connect包裹的组件的props上添加dispatch属性
export default connect(({ products }) => ({
  products,
}))(Products);
```

* ProductList组件

```js
import React,{memo} from 'react';
import PropTypes from 'prop-types';
import { Table, Popconfirm, Button } from 'antd';

const ProductList = memo(function ProductList({ onDelete, products }) {
    const columns = [{
        title: 'Name',
        dataIndex: 'name',
    }, {
        title: 'Actions',
        render: (_, record) => {
            return (
                <Popconfirm title="Delete?" onConfirm={() => onDelete(record.id)}>
                    <Button>Delete</Button>
                </Popconfirm>
            );
        },
    }];
    return (
        <Table
            dataSource={products}
            columns={columns}
        />
    );
})

ProductList.propTypes = {
    onDelete: PropTypes.func.isRequired,
    products: PropTypes.array.isRequired
}

export default ProductList
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143430.png" alt="image-20210416170232122" style="zoom:50%;" />



### 路由使用

dva中路由集成的是react-router，与react-router的使用一致，使用Link或者NavLink的to属性跳转到某个地址，router.js中通过设置component配置访问指定地址时要显示的page

```js
<NavLink to='/product'>跳转到product页面</NavLink>

<Switch>
    <Route path="/" exact component={IndexPage} />
    <Route path="/product" component={ProductPage} />
</Switch>
```

js方法调用，详见上面路由，如果是在dva脚手架中非路由组件中跳转，需要`import {withRouter} from 'dva/router'`对组件进行包裹，然后组件中`props.history.push('/product')`即可跳转

因为dva中react-router进行了一层封装，因此需要去dva/router中导入

### mock使用

```javascript
//mock/product.js
module.exports = {
    "GET /api/product":{"name":"高粱"}
}

//.roadhogrc.mock.js
export default {
    ...require('./mock/product')
};

//services/product.js
import request from '../utils/request'
export function getProduct(){
    return request("/api/product")
}

//使用
useEffect(()=>{
    //componentDidMount
    getProduct()
    .then(v=>{
      console.log(v)
    })
  },[])
```

使用mockjs很方便，可以快速模拟指定图片和自增信息，可以随机数字，可以快速生成城市、文字、颜色、图片地址等信息

`npm install mockjs`

```js
const Mock = require('mockjs')

module.exports = {
    "GET /api/product": { "name": "高粱" },
    [`GET /api/user`](req, res) {
        res.json(Mock.mock({
            // 20条数据
            "data|20": [{
                // 商品种类
                "goodsClass": "女装",
                // 商品Id,从1开始，递增，每次+1
                "goodsId|+1": 1,
                //商品名称
                "goodsName": "@ctitle(10)",
                //商品地址
                "goodsAddress": "@county(true)",
                //商品等级评价★
                "goodsStar|1-5": "★",
                //商品图片
                "goodsImg": "@Image('100x100','@color','小甜甜')",
                //商品售价
                "goodsSale|30-500": 30
            }]
        }))
    }
}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514143438.png" alt="image-20210416182435625" style="zoom:50%;" />

### 异步请求DEMO

* 页面中不能使用services中封装的方法请求数据，应该直接dispatch一个异步的action

```js
//ProductPage.js
  useEffect(()=>{
    //页面加载完毕，获取product数据并展示
    dispatch({
      type:'products/fetchProduct',
      payload:{}
    })
  },[])
```

* model/product.js

```javascript
import { getProduct } from "../services/product";

export default {
    //reducer合并的标识，一般一个page，一个reducer，以page名命名即可
    //注意page和component的区别
    namespace: 'products',
    state: [
        //可以在入口文件index.js中设置initialState设置默认值
        //也可以在此处设置默认值
        // { name: 'dva', id: 1 },
        // { name: 'antd', id: 2 }
    ],
    reducers: {
        //也可以给delete加上''，没区别
        //删除
        delete(state, { payload: id }) {
            //{payload:id}是对action对象的解构
            return state.filter(item => item.id !== id);
        },
        //增加一位
        'add'(state,{data}){
            console.log(data)
            return [...state,data]
        },
        //设置数组的值
        set(state,{payload}){
            console.log(payload)
            return [...payload]
        }
        //对象中可以继续写reducer
    },

    //异步action,根据页面派发action的名称分别进入同步的action还是异步的action
    effects:{
        //获取商品列表
        *fetchProduct({ payload }, { call, put }){
            //进入获取列表异步action
            //发送请求获取商品数据
            const res = yield call(getProduct,payload)
            //发送给同步set action，更新product的值
            yield put({
                type:'set',
                payload:res.data
            })
        }
    }
}
```

* services/product.js

```js
import request from '../utils/request'
export function getProduct(){
    return request("/api/product")
}
```

* mock/product.js

```javascript
const Mock = require('mockjs')

module.exports = {
    //简写方式
    // "GET /api/product": [{ name: 'dva', id: 1 },{ name: 'antd', id: 2 }],
    //回调参数的形式，可以获取用户传递的参数
    "GET /api/product":(req,res) => {
        const params = req.query  //获取传递过来的参数
        res.send([{ name: 'dva', id: 1 },{ name: 'antd', id: 2 }])
    }
}
```

### redux-actions使用

避免每次创建action的麻烦

```js
//使用redux-action之前
const increment = (num)=>({type:"increment",payload:num})
const decrement = () => ({type:'decrement',payload:num})
//使用
dispatch(increment(10))



//使用redux-action将action集中管理(一般与redux-saga结合使用)
import { createAction } from 'redux-actions';
export const increment = createAction('cincrement');
export const decrement = createAction('decrement');
//使用
dispatch(increment())  //{type:"increment"}
dispatch(increment(10)) //{type:"increment",paylaod:10}

```
