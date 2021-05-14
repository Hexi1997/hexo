---
title: css基础
date: 2021-05-09 23:50:17
type: tags
tags: [大前端,css]
categories: css
---

## css多种引用方式

* 内联样式

* head中使用style标签 

* link标签 

  

  link标签和style标签放在body中也可以

  ```html
  <h1 style='margin:0px'>
      xxx
  </h1>
  <style> ... </style> 
  <link rel='stylesheet' href='houdunren.css' type='text/css' />
  ```

  

* 导入样式@import

```css
@import url('common/menus.css');
body{
    houdunren:33px;
}
```



## less scss

* 使用less可以根据元素结构化写css
* vscode安装easy less插件，保存less自动生成同名css
* vscode安装live server插件，实现代码同步更新

## 选择器

分类: 

​	通配符选择器(*)

​	基本选择器（类、id、元素)

​	结构选择器（后代选择器(空格）、子元素选择器(>)、后面兄弟选择器(~)、相邻兄弟选择器(+)

​	属性选择器

​	伪类选择器(a:hover input:focus div:target :root :empty)

​	结构伪类选择器(first-child last-of-type nth-of-type(n) :not)

​	表单伪类： input:  enabled/disabled/checked/required/optional/valid/invalid

​	字符伪类:    ::first-letter  ::first-line   ::before   ::after

```css
*{
    //全局选择
}

h1{
    //标签选择器
}

#id{
    //id选择器
}

.classname{
    //类选择器
}

body div .classname{
    //后代选择器
}

body div>h2{
    //子元素选择器，只能选择body div结构的子元素，不能选择孙元素
}

body div~h2{
    //div后所有同级别的h2元素  
}

body div+h2{
    //div后紧挨着的h2元素  相邻兄弟选择器
}

h1[title][id]{
    //选择同时包含title和id属性的h1元素   属性选择器
}

h1[title="houdunren"]{
    //选择title属性为houdunren的h1元素  属性选择器
}

h1[title^="houdunren"]{
    //选择title属性的值是以houdunren开头的h1元素 起始属性选择器 <h1 title='houdunren'></h1>
}

h1[title$="com"]{
    //选择title属性的值是以com结尾的h1元素 结尾属性选择器 <h1 title='houdunren.com'></h1>
}

h1[title*="houdunren"]{
    //选择title属性的值包含houdunren的h1元素  包含属性选择器 <h1 title='www.houdunren.com'></h1>
}

h1[title|="houdunren"]{
    //选择title属性的值以houdunren开头或者以 -houdunren结尾的h1元素
    <h1 title='houdunren.com'></h1>
    <h1 title='com-houdunren'></h1>
}


a:hover{
    //伪类选择器，定义a链接悬浮的样式
}
a:visited{
    //伪类选择器，定义a链接浏览过后的样式
}
a:focus{
    //伪类选择器，定义a链接获得焦点的时候 鼠标松开时显示的颜色
}
a:active{
    //伪类选择器，当前活动元素 鼠标在元素上按下还没有松开
}

input:hover{
    //伪类选择器
}
input:focus{
    //伪类选择器
    outline:none  //清除input标签选中时的外边框
}

div:target{
    //target伪选择器，锚点选择器
    //<p><a href="#news1">跳转至内容 1</a></p>
    //<div id="news1"><b>内容 1...</b></div>
    backgroud:red
}

:root{
    //根元素伪类选择器
}

li:empty{
    //例如一个空li <li></li>，让他不显示
    //空元素选择器
    display:none
}
```



* 定义多个类选择器

```html
<h1 class='class1 class2'>
    xxx
</h1>
```



### 首尾元素伪类选择

#### first-child

选择属于父元素的第一个子元素的每个 <p> 元素 

![first-child](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215213.JPG)



----------------------------------------------------------------------------------------------------------------------------------------------------------

#### last-child

 选择属于其父元素最后一个子元素每个 <p> 元素 

![last-child](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215221.JPG)

-----------------------------------------------------------------------------------------------------------------------------------------------------------

#### only-child

 选择属于其父元素的唯一子元素的每个 <p> 元素 

![only-child](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215227.JPG)



#### first-of-type

选择属于其父元素的首个 <p> 元素的每个 <p> 元素。

![first-of-type](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215233.JPG)



#### last-of-type

 选择属于其父元素的最后 <p> 元素的每个 <p> 元素 

![last-of-type](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215237.JPG)



#### only-of-type

 选择属于其父元素唯一的 <p> 元素的每个 <p> 元素 

![only-of-child](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215241.JPG)



#### nth-child

 选择属于其父元素的第二个子元素的每个 <p> 元素 

![nth-child](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215254.JPG)



nth-child的参数n的妙用 https://blog.csdn.net/qq_20343517/article/details/82944842

n从0开始递增

n+4 >= 4 

-n+4<=4

2n+1 奇数（odd)

2n      偶数  (even)

* 选择倒数第二个 nth-last-child(2)

#### css后代选择器空格和>的区别

* 空格含子含孙
* 大于号只包含子



### not排除选择器

 选择非 <p> 元素的每个元素 

![not](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215301.JPG)



### 表单伪类：disabled和enabled和checked、required

input:disabled:选择每个禁用的 <input> 元素 

input:enabled: 选择每个启用的 <input> 元素

![disabled和enabled](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215309.JPG)



### 文本伪类

p::first-line   //第一行

p::first-letter  //第一个字符

伪类：a:hover a:visited 

伪元素： p::first-line p::first-letter ::after ::before

```
input不支持伪元素
```

* 伪类:和伪元素::的区别 : https://www.cnblogs.com/notNaN/p/11905360.html
* https://www.cnblogs.com/muzs/p/10484174.html



* 实现无form input框

```html
<html>
	<head>
		<style>
			*{
				margin:0;
				paddding:0;
			}
			div{
				width:140px;
				border:1px solid
			}
			
			input{
				border:0;
				width:120px;
				
				
			}
			
			input:focus{
				border:0;
				outline:none
			}
			
			input+span::before{
				content:'\2193';
				cursor:pointer
			}
		</style>
	</head>
	<body>
		<div>
			<input type='text' placeholder='请输入'/>
			<span></span>
		</div>
	</body>
</html>

```



## 权重问题

### 权重位：范围越广，权重越小

| 规则                                | 粒度 |
| ----------------------------------- | ---- |
| ID                                  | 0100 |
| class、类属性值、伪类(:not伪类除外) | 0010 |
| 标签,伪元素                         | 0001 |
| *                                   | 0000 |
| 行内样式                            | 1000 |

 内联》ID选择器》伪类=属性选择器=类选择器》元素选择器【p】》通用选择器(*)》继承的样式 

1000          100                          10                                      1                              0                     NULL

 注意：通用选择器（*），子选择器（>）和相邻同胞选择器（+）相邻后代选择器（~）并不在等级中，所以他们的权值都为0。 



权值比较 规则：

:not伪类不参与权重比较，可以理解为0

　　　　当两个权值进行比较的时候，是从高到低逐级将等级位上的权重值（如 权值 1,0,0,0 对应--> 第一等级权重值，第二等级权重值，第三等级权重值，第四等级权重值）来进行比较的，而不是简单的 1000*个数 + 100*个数 + 10*个数 + 1*个数 的总和来进行比较的，换句话说，**低等级的选择器，个数再多也不会越等级超过高等级的选择器的优先级的**;【为什么这么特别强调呢，因为为在网上查资料的时候，看到好多博客是把这个权重值理解成了所有等级的数字之和了】**，**说到这里 再 配合下图 大家应该就差不多理解了

![权重比较](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215315.png)





```css
#parent p                              =  0，1，0，1
div .a.b.c.d.e.f.g.h.i.j.k p           =  0，0，11，2
如果按照值来计算，那么应该112>101，实际上是逐位比较的权重 0,1,0,1 > 0,0,11,2,权重不能进位
```



复合选择器其权重也是组合的，例如 body .classname的权重为1+10= 0,0,1,1

​															   .classname[value] 的权重就是 类选择器+属性选择器 = 10 + 10 = 0,0,2,0





如果权重相同，后面的样式覆盖前面的



* 强制提升权重优先级到最高 ： !important 
  * 能不用就不用，会破坏css的权重计算规则
  * 一般用来修改第三方ui库的样式
* 元素继承权重详解
  * 例如在div中设置color，div中的某个子元素span如果没有自身的color，就会显示父级div的color设置
  * 元素继承没有权重(NULL)，一旦子元素设置样式，就不会继承
  * 继承也是基于文档树的，文档树中元素的某些属性可以被其子元素继承，每一个CSS属性都定义了它能否被继承。要设定文档的某些缺省样式属性，可以在文档树的根上设定该属性，如果这个属性可以继承，则其后代元素将继承这个属性，例如color、font-size等属性。
* 通配符的继承的博弈

```html
<html>
	<head>
		<style>
            /*
             *	对于span标签来说，继承了h2的样式，权重为NULL，而通配符*的权重为0，0>NULL,因此span标签内的元素的颜色为green，这就是通配符和继承之间的冲突，如果要解决这个问题，可以将通配符*换成html标签，这样的话span继承了html和h2的样式，由于继承h2在后覆盖前面，因此显示为red
            */
            *{
                color:green
            }
            
            h2{
                color:red
            }
		</style>
	</head>
	<body>
		<div>
			<h2>houdunren.com<span>hucms.com</span></h2>
		</div>
	</body>
</html>
```



### 使用预处理器less解决权重问题

less会自动根据结构生成css样式，避免为了权重问题而一个一个写



## 文本内容

### 字体

* 中文字体体积非常大，一般不用
* 现在有点字体提供按需加载
* 尽量不要使用商业字体

```html
<style>    @font-face{        font-family:'houdunren';        /*字体一般格式有：otf woff ttf*/        src:url("xxxxxx.ttf")    }    div{        font-family:'houdunren'   //使用定义的字体    }</style>
```

* font-weight
  * normal | bold | bolder | lighter | 100-900
  * 400对应normal，700对应bold，一般情况下使用bold 或者 normal更多
* font-size
  * xx-small | x-small | small | medium | large | x-large | xx-large
  * 字号也可以为具体数值，例如 font-size:14px
  * 字号也可以为百分比，相对于父级元素的字号百分比
  * 字号也可以为em单位

### 颜色和行高的声明

* 颜色
  * color:red
  * color:'#fff'
  * color:rgb(255,255,255)
  * color:rgba(255,255,255,0.5)
* 行高
  * font-size:15px
  * font-size:normal
  * font-size:1.5em
* 倾斜
  * font-style: italic  //倾斜
  * font-style: normal  //正常

### px em rem的区别

参见：https://www.runoob.com/w3cnote/px-em-rem-different.html

简单来说：

*  px像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的。 
*  em是相对长度单位。相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。 em的特点是：em会继承父级元素的字体大小
   *  *任意浏览器的默认字体高都是16px。所有未经调整的浏览器都符合: 1em=16px。那么12px=0.75em,10px=0.625em。为了简化font-size的换算，需要在css中的body选择器中声明Font-size=62.5%，这就使em值变为 16px\*62.5%=10px, 这样12px=1.2em, 10px=1em, 也就是说只需要将你的原来的px数值除以10，然后换上em作为单位就行了。* 
*  rem是CSS3新增的一个相对单位（root em，根em），这个单位引起了广泛关注。这个单位与em有什么区别呢？区别在于使用rem为元素设定字体大小时，仍然是相对大小，但相对的只是HTML根元素。这个单位可谓集相对大小和绝对大小的优点于一身，通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。目前，除了IE8及更早版本外，所有浏览器均已支持rem。对于不支持它的浏览器，应对方法也很简单，就是多写一个绝对单位的声明。这些浏览器会忽略用rem设定的字体大小。

### 字体样式组合定义

```css
p{    font:15px bold italic}
```



### 字符大小写转换

```css
font-variant:small-caps;   //小型大写，字号变化不大text-transform:uppercase   //大写，字号变化大一些text-transform:lowercase   //小写text-transform:capitalize  //首字母大写，逗号 . 空格都算首字母
```



### 文本线条

```css
p{	text-decoraion:underline;  //下划线    text-decoration: overline;  //上划线    text-decoration: line-through; //删除线}a{    text-decoration:none //去除超链接的下划线}
```



### 文本阴影

text-shadow: h-shadow     v-shadow      blur       color;

​						水平阴影       垂直阴影    模糊距离    颜色



```css
h2{    text-shadow:rgba(244,12,32,0.5) 5px 5px 1px;    /*			颜色																			*/	}
```



### 文本溢出与空白处理技巧

white-space: 

* normal默认。空白会被浏览器忽略。
* pre空白会被浏览器保留。其行为方式类似 HTML 中的 <pre> 标签。
* nowrap文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止。
* pre-wrap保留空白符序列，但是正常地进行换行。
* pre-line合并空白符序列，但是保留换行符。
* inherit规定应该从父元素继承 white-space 属性的值。 

```html
<style>    div{        border:1px solid #ddd;        width:300px;        white-space:nowarp;  //不允许换行        overflow:hidden;     //溢出内容隐藏        text-overflow:ellipsis;  //溢出的内容用...代替    }</style>
```



### 文本缩进

* text-indent : 文本缩进，一般用em单位表示缩进几个字符
  * text-indent:16px;
  * text-indent:2em;   //2个字符

### 文本对齐

```html
h2{	text-align:center;   //h2中文本居中对齐,right表示右对齐，left表示左对齐}
```

* 垂直对齐vertical-align

  * 该属性定义行内元素的基线相对于该元素所在行的基线的垂直对齐。允许指定负长度值和百分比值。这会使元素降低而不是升高。在表单元格中，这个属性会设置单元格框中的单元格内容的对齐方式。

  * **作用环境**：父元素设置line-height。

    **作用对象**：子元素中的inline-block和inline元素。

  | 值          | 描述                                                         |
  | :---------- | :----------------------------------------------------------- |
  | baseline    | 默认。元素放置在父元素的基线上。                             |
  | sub         | 垂直对齐文本的下标。                                         |
  | super       | 垂直对齐文本的上标                                           |
  | top         | 把元素的顶端与行中最高元素的顶端对齐                         |
  | text-top    | 把元素的顶端与父元素字体的顶端对齐                           |
  | middle      | 把此元素放置在父元素的中部。                                 |
  | bottom      | 把元素的顶端与行中最低的元素的顶端对齐。                     |
  | text-bottom | 把元素的底端与父元素字体的底端对齐。                         |
  | length      |                                                              |
  | %           | 使用 "line-height" 属性的百分比值来排列此元素。允许使用负值。 |
  | inherit     | 规定应该从父元素继承 vertical-align 属性的值。               |

![vertical-align](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215325.JPG)

![居中](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215725.JPG)

### 间距

* letter-space:20px;  字符间距
* word-space:30px;  单词间距
* 排版模式：writing-mode:vertical-lr;  文字垂直从左到右排版，很少用



## 盒子模型

margin: 上 右 下 左

margin：全部

margin： 上下   左右

margin-left:auto; margin-right:auto;  设置元素居中

边距（maring)合并特性上面div和margin-bottom和下面div的margin-top的边距会进行合并，只取最大值，这是css的特性

width宽度只包括content（内容区）的宽度，不包括padding border和margin，通过box-sizing:border-box可以指定设置width就是边框盒（内容区+padding+boder）的宽度。内容区的宽度是随着padding和border的变化而变化的

css盒子模型宽度问题： https://www.cnblogs.com/ada-blog/p/7252053.html

各种宽度的图片示意：https://www.jianshu.com/p/76ba0d71bd7c

https://blog.csdn.net/mr_fzz/article/details/53033877

 ![img](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215336.webp) 

 ![img](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215334.webp) 

 clientWidth=元素的width+padding 

 **offsetHeight=clientHeight+滚动条+border** 

min-width

min-height

max-width

max-height

可以在不规则图片统一显示中使用min-或者max-属性，让他们规则显示

```html
<style>    div{        width:300px;        height:300px;    }    div img{        max-width:90%;        min-width:60%;        max-height:90%;        min-height:60%;    }</style><div>    <img src='....'/></div>
```

盒子阴影：box-shadow:0px 0px 5px rgba(100,100,100,0.5) 

​					盒子阴影0px比较常用，四周发散



## 边框

border: 1px solid red;                   复合写法，顺序没有严格要求

border-top/right/bottom/left：2px solid green;  复合写法

border-style:solid;  //单独写法

boder-width:0;      //边框宽度

border-color:red;  //颜色

border:0;   //边框宽度为0，浏览器仍然会解析渲染边框，占用内存

border:none;  //边框宽度不存在，浏览器不会解析

一般推荐用border:none

 常用标签除了单选按钮和复选按钮都可以设置边框 ，例如p标签啥的

### 圆角

* border-radius:30px;
* border-radius:20%;  //以当前元素20%宽度显示圆角
* border-radius:50%;   //如果div是正方形，这样设置就显示成圆形
* border-top-left-radius:20px;  // boder-top/bottom-left/right，必须是这四种写法



### 轮廓线

outline:1px solid #fff

轮廓线不占用任何空间（和margin和border不同）

表单中input获取焦点时会有outline，可以通过outline:none设置不显示



## display

行内元素设置display:block，以块元素形式展现，独占一行

块元素一行显示：使用inline-block，常见应用：ul中li标签一行显示

```css
ul>li{    display:inline-block;}ul>li:hover{    cursor:pointer;    background:red;    color:green}
```

* display:none和visibility:hidden的区别？
  *  display:none 隐藏后的元素**不占据任何空间**，而 visibility:hidden 隐藏的元素**空间依旧存在**。 
  *  display:none 隐藏产生回流和重绘（reflow 和 repaint），而 visibility:hidden 只产生重绘。 
  *  display:none 就是“株连性”明显的声明：一旦父节点元素应用了 **display:none**，父节点及其子孙节点元素**全部不可见**，而且无论其子孙元素如何不屈地挣扎都无济于事。 
  *  若父元素引用**visibility:hidden**，但子孙元素应用了 **visibility:visible**，则这个孙元素不但不会隐藏，而且会显现出来 



  ## overflow溢出详解

| 值      | 描述                                                         |
| :------ | :----------------------------------------------------------- |
| visible | 默认值。内容不会被修剪，会呈现在元素框之外。                 |
| hidden  | 内容会被修剪，并且其余内容是不可见的。                       |
| scroll  | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。如果内容没有溢出，也会显示滚动条 |
| auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。如果内容没有溢出，不会显示滚动条 |
| inherit | 规定应该从父元素继承 overflow 属性的值。                     |

## css3 四个自适应关键字

* ![fill-available](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215344.JPG)fill-available是智能自动填充可用空间，不包括padding和margin

* fill-available 关键字值的价值在于，可以让元素的100%自动填充特性不仅仅在`block`水平元素上，也可以应用在其他元素 

* IE浏览器不支持，webkit内核浏览器需添加-webkit-前缀 (Chrome Safari)

* fill-available和100%的区别？ http://www.caotama.com/48195.html

  100%包括padding，可能会内容溢出，fill-available是智能计算的，不会溢出，fill-available不兼容IE浏览器



*  width:fit-content表示将元素宽度收缩为内容宽度 
   * IE浏览器不支持，webkit内核浏览器需添加-webkit-前缀 (Chrome Safari)

![fit-content](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215736.JPG)

*  width:max-content表示采用内部元素宽度值最大的那个元素的宽度作为最终容器的宽度。如果出现文本，则相当于文本不换行 
   * IE浏览器不支持，webkit内核浏览器需要添加-webkit-前缀

![max-content](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215743.JPG)

* width:min-content表示采用内部元素宽度值最小的那个元素的宽度作为最终容器的宽度

![min-content](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215352.JPG)



## 背景

### background-image

background-image:url('........') 

background-image 属性设置一个元素的背景图像。

默认情况下，background-image放置在元素的左上角，并重复垂直和水平方向。 

元素的背景是元素的总大小，默认包括填充(padding)和边界(border)，但不包括边距(margin)。

背景可以设置渐变: linear-gradient() 函数用于创建一个表示两种或多种颜色线性渐变的图片。

创建一个线性渐变，需要指定两种颜色，还可以实现不同方向（指定为一个角度）的渐变效果，如果不指定方向，默认从下到上渐变。

* `background-image: linear-gradient(direction, color-stop1, color-stop2, ...);`

* | 值                             | 描述                               |
  | :----------------------------- | :--------------------------------- |
  | *direction*                    | 用角度值指定渐变的方向（或角度）。 |
  | *color-stop1, color-stop2,...* | 用于指定渐变的起止颜色。           |

* ```css
  /* 从上到下，蓝色渐变到红色 */linear-gradient(blue, red); /* 渐变轴为45度，从蓝色渐变到红色 */linear-gradient(45deg, blue, red); /* 从右下到左上、从蓝色渐变到红色 */linear-gradient(to left top, blue, red); /* 从下到上，从蓝色开始渐变、到高度40%位置是绿色渐变开始、最后以红色结束 */linear-gradient(0deg, blue, green 40%, red);
  ```

* 在通常的网页设计中，往往不会用很纯的单一大色块，一般都会用到渐变效果，渐变范围很小，选择相近的颜色进行渐变，让他有层次感

### background-clip

如果要修改填充范围，可以使用background-clip熟悉，content-box(内容区域) padding-box  border-box

background-repeat:no-repeat/repeat/repeat-x/repeat-y/space(不显示一半的区域)



### background-attachment 

 设置背景图像是否固定或者随着页面的其余部分滚动。 

​	fixed: 当页面的其余部分滚动时，背景图像不会移动。 

​	scroll:  默认值。背景图像会随着页面其余部分的滚动而移动。 



### background-position

  这个属性设置背景原图像（由 [background-image](https://www.w3school.com.cn/cssref/pr_background-image.asp) 定义）的位置，背景图像如果要重复，将从这一点开始 

| 值                                                           | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| top left top center top right center left center center center right bottom left bottom center bottom right | 如果您仅规定了一个关键词，那么第二个值将是"center"。默认值：0% 0%。 |
| x% y%                                                        | 第一个值是水平位置，第二个值是垂直位置。左上角是 0% 0%。右下角是 100% 100%。如果您仅规定了一个值，另一个值将是 50%。 |
| xpos ypos                                                    | 第一个值是水平位置，第二个值是垂直位置。左上角是 0 0。单位是像素 (0px 0px) 或任何其他的 CSS 单位。如果您仅规定了一个值，另一个值将是50%。您可以混合使用 % 和 position 值。 |

### background-size

首页背景图通常用background-size:cover

 background-size属性指定背景图片大小 

| 值         | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| length     | 设置背景图片高度和宽度。第一个值设置宽度，第二个值设置的高度。如果只给出一个值，第二个是设置为 **auto**(自动) |
| percentage | 将计算相对于背景定位区域的百分比。第一个值设置宽度，第二个值设置的高度。如果只给出一个值，第二个是设置为"auto(自动)" |
| cover      | 此时会保持图像的纵横比并将图像缩放成将完全覆盖背景定位区域的最小大小。 |
| contain    | 此时会保持图像的纵横比并将图像缩放成将适合背景定位区域的最大大小。 |

background: red url('xxxxx') no-repeat center 复合写法

background可以指定多个，使用逗号分开

```css
div{    background-image:url('1.jpg'), url(2.jpg);    background-position: center right,center left;    background-repeat: repeat, no-repeat;}
```



## 表格

### css定制表格

```html
<html>	<head>		<link href="index.css" rel="stylesheet" type='text/css'/>	</head>	<body>		<div class="table">			<section>				<ul>					<li>编号</li>					<li>姓名</li>				</ul>			</section>			<section>				<ul>					<li>1</li>					<li>hexi</li>				</ul>			</section>			<section>				<ul>					<li>2</li>					<li>hedong</li>				</ul>			</section>		</div>			</body></html>
```

```less
.table{    display: table;    section{        &:nth-of-type(1){            display: table-header-group;            background-color: rgba(0,0,0,0.5);            color:white;        }        &:nth-of-type(2){            display:table-row-group;        }        &:nth-of-type(3){            display:table-footer-group;        }        ul{            display: table-row;            li{                display: table-cell;                border:1px solid #ddd;                padding: 10px;            }        }    }}
```

### 表格标题与对齐处理

caption 标题

text-align 水平对齐

vertical-align 垂直对齐

 border-collapse 属性设置表格的边框是否被合并为一个单一的边框，还是象在标准的 HTML 中那样分开显示

![border-collpase](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215756.JPG) 

empty-cells:  设置是否显示表格中的空单元格（仅用于"分离边框"模式） 

​	show: 显示  hide: 隐藏



## 列表符号

### 多种方式定义列表符号

```html
<style>    ul{        list-style-type: none;   //ul中li不显示编号        list-style-image: url(xxx.jpg);  //列表编号用图片显示    }    ul li{        background-iamge:url(yyy.jpg);   //li的背景    }</style>
```



## before+attr案例

![before+attr](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215408.png)

元素自定义属性一般用data-描述



## 浮动

浮动是一种布局

使用浮动可以控制相邻元素间的排列关系

没有设置浮动的块元素是独占一行的，浮动是对后面元素的影响，后面浮动对前面没有影响

 浮动元素边界不能超过父元素的padding 

 元素浮动后会变为块元素包括行元素如 `span`，所以浮动后的元素可以设置宽高 

* 浮动是否脱离文档流
  * 浮动并没有脱离文档流，浮动元素之间不会重叠
  * 定位设置absolute才会脱离文档流，且会重叠

* **清除浮动主要是为了解决，父元素因为子级元素浮动引起的内部高度为0的问题** （高度塌陷）

### 清除浮动的几种方式

* 额外标签法：在最后一个浮动标签后，新建一个标签，给其设置clear:both(不推荐)

  * 优点：通俗易懂，方便
  * 缺点：添加无意义标签，语义化差

* 父级元素添加overflow属性(hidden或者auto)  (不推荐)

  * 通过触发BFC方式， 父元素的高度计算会包括浮动元素的高度 ，实现清除浮动
  * 优点：代码简洁
  * 缺点： 内容增多的时候容易造成不会自动换行导致内容被隐藏掉，无法显示要溢出的元素 

* 使用after伪元素清除浮动（推荐）

  * 给父级元素的after伪元素设置 

  * ```css
    .father{    content:'';    display:block;    clear:both;}
    ```



## 页面布局

完成页面布局注意以下几点

1. 首先根据设计稿确定页面大小（主要指宽度，移动端不需要考虑），如 1200px 宽度
2. 水平分割页面主要区域
3. 每个区域中按以上两步继续细分

一组浮动元素要放在一个父容器中，如下图绿线指父容器，里面的红线为浮动子元素。

![image-20190820204502017](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215413.png)

下面是上图的布局结构代码

```html
<style>	article {      background: #f3f3f3;      width: 1020px;      height: auto;      overflow: auto;      padding: 20px;  }  article.hot section {      background: #fff;      box-shadow: 0 0 5px #777;      height: 300px;      width: 500px;  }  article.hot section:first-of-type {      float: left;  }  article.hot section:last-of-type {      float: right;  }  article.swiper section {      height: 200px;      background: #fff;      box-shadow: 0 0 5px #777;  }  article.swiper section:first-of-type {      width: 200px;      float: left;  }  article.swiper section:last-of-type {      width: 820px;      float: left;  }</style>... <article class="hot">        <section></section>        <section></section></article><article class="swiper">        <section></section>        <section></section></article>
```

## 形状浮动



### 距离控制

| 选项        | 说明       |
| ----------- | ---------- |
| margin-box  | 外边距环绕 |
| padding-box | 内边距环绕 |
| border-box  | 边线环绕   |
| content-box | 内容环绕   |

**外边距环绕**

![image-20190822133517672](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215418.png)

```text
<style>  span.shape {      float: left;      width: 100px;      height: 100px;      padding: 30px;      margin: 30px;      border: solid 30px green;      shape-outside: margin-box;  }</style>...<p>  <span class="shape"></span>  后盾人自2010年创立至今，免费发布了大量高质量视频教程，视频在优酷、土豆、酷六等视频网站均有收录，很多技术爱好者受益其中。  除了免费视频外，后盾人还为大家提供了面授班、远程班、公益公开课、VIP系列课程等众多形式的学习途径。后盾人有一群认真执着的老师，他们一心为同学着想，将真正的知识传授给大家是后盾人永远不变的追求。</p>
```

**边框环绕**

![image-20190822133612903](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215427.png)

```text
span.shape {    float: left;    width: 100px;    height: 100px;    padding: 30px;    margin: 30px;    border: solid 30px green;    shape-outside: border-box;}
```

### 显示区域clip-path

详细解说：https://blog.csdn.net/guolulu66/article/details/109043556?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&dist_request_id=6463c2c3-d0ff-4238-a7d6-2a996dcde5d9&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control

| 选项    | 说明   |
| ------- | ------ |
| circle  | 圆形   |
| ellipse | 椭圆   |
| polygon | 多边形 |

**圆形**

![image-20190822134841106](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215712.png)

```text
span.shape {    float: left;    width: 100px;    height: 100px;    padding: 30px;    margin: 30px;    background: red;    clip-path: circle(50% at center);}
```

**椭圆**

![image-20190822135121997](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215438.png)

```text
span.shape {    float: left;    width: 100px;    height: 100px;    padding: 30px;    margin: 30px;    background: red;    clip-path: ellipse(50% 80% at 100% 0);}
```

**多边形**

![image-20190822135238497](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215441.png)

```text
span.shape {    float: left;    width: 100px;    height: 100px;    padding: 30px;    margin: 30px;    background: red;    clip-path: polygon(50% 0, 100% 100%, 0 100%)}
```

### 内移距离inset

使用 `inset` 属性控制环绕向内移动的距离。

```text
span.shape {    float: left;    width: 100px;    height: 100px;    padding: 30px;    margin: 30px;    background: red;    shape-outside: inset(50px 30px 80px 50px) padding-box;}
```

### 环绕模式shape-outside

| 选项    | 说明     |
| ------- | -------- |
| circle  | 圆形环绕 |
| ellipse | 椭圆环绕 |
| url     | 图片环绕 |
| polygan | 多边环绕 |

**圆形环绕**

![image-20190822131434976](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215446.png)

```text
img {    padding: 20px;    float: left;    shape-outside: circle(50%) padding-box;}
```

**椭圆环绕**

![image-20190822131415117](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215452.png)

```text
img {    padding: 20px;    float: left;    shape-outside: ellipse(80px 70px) padding-box;}
```

**图片环绕**

![image-20190822131725161](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215501.png)

```text
img {    float: left;    shape-outside: url(xj.png);}
```

**多边环绕**

![image-20190822132344769](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215508.png)

```text
span.shape {    float: left;    width: 100px;    height: 100px;    background: red;    clip-path: polygon(50px 0px, 0 100px, 100px 100px);    shape-outside: polygon(50px 0px, 0 100px, 100px 100px);}
```













## 练习

### 后盾人页面模仿

```html
<html><head>    <link href="index.css" rel="stylesheet" type='text/css' /></head><body>    <header>        <nav>            <ul>                <li><a>系统学习</a></li>                <li><a>实战课程</a></li>                <li><a>最近更新</a></li>                <li><a>话题讨论</a></li>                <li><a>签到打卡</a></li>                <li><a>八点直播</a></li>                <li><a>在线文档</a></li>                <li><a>订阅会员</a></li>            </ul>        </nav>        <div class='btnGroup'>            <button>注册</button>            <button>登录</button>        </div>    </header>    <div class='content'>        <div class="content-left">            <h3>数据元素样式</h3>            <p>1、使用CSS定制表格</p>            <p>2、表格标题与对齐处理</p>            <p>3、表格颜色与背景处理</p>            <p>4、细线表格与间距与空白单元格处理</p>            <p>5、细线无边框表格设计</p>            <p>6、数据表格设计</p>            <p>7、多种方式自定义列表符号</p>            <p>8、多图背景控制与列表符号</p>            <p>9、after与before追加元素样式使用</p>            <p>10、使用after与before制作绚烂的表单</p>        </div>        <div class='content-right'>            <div>                <p>社区小帖</p>                <p>后盾人是一个主张友好、分享、自由的技术交流社区</p>                <div class="btnGroup2">                    <button>发帖交流</button>                    <button>签到打卡</button>                </div>            </div>            <div>                <p>最新课程</p>                <p>数据元素样式</p>                <p>背景与渐变</p>                <p>盒子模型详解</p>                <p>带你玩转CSS3文本，打牢前端开发基础</p>                <p>搞定CSS权重</p>            </div>        </div>    </div>    <footer>        <p>我们的使命：创博互联网前沿技术，帮助更多人实现梦想</p>        <p>Copyright © 2010-2018 houdunren.com All Rights Reserved 京ICP备12048441号</p>    </footer></body></html>
```

```less
body{    background-color: rgb(243,241,239);    header{            *{            margin:0;            padding:0;        }        border-bottom:3px solid green;        box-shadow: 0px 5px 5px #ddd;        overflow: hidden;        background-color: white;                nav{                ul{                font-weight: bold;                margin:0 auto;                width:800px;                li{                    float:left;                    margin-right: 5px;                    list-style: none;                    padding:10px;                    a{                        cursor:pointer;                        color:black;                        text-decoration: none;                    }                }                    li:last-child{                    a{                        color:rgb(40,154,63)                    }                }            }        }                div.btnGroup{            float:right;            padding:10px;            button{                margin-left:15px;                width:80px;                border-radius: 5px;                padding: 3px 6px;                cursor: pointer;            }            button:nth-of-type(1){                background-color:white;                border-color: rgb(24,151,179);                color: rgb(24,151,179);                border-width: 1px;                &:hover{                    background-color:  rgb(24,151,179);                    color:white;                }            }            button:nth-of-type(2){                background-color: rgb(24,151,179);                border-color:white;                border-width: 1px;                color: white;                                &:hover{                    background-color: white;                    color: rgb(24,151,179);                }            }        }    }        .content{        width:900px;        margin:30px auto;        overflow: hidden;        div.content-left{            background-color: white;            margin:30px 10px 20px 20px;            padding:10px;            width:500px;            float:left;            border:1px solid #ddd;            box-shadow: 0px 0px 3px #ddd;            h3{                color:rgb(129, 127, 127);            }            p{                font-size: 14px;                color:rgb(104, 104, 104);                padding-bottom: 6px;                border-bottom:1px solid #ddd;            }        }        div.content-right{                        float:right;            width:300px;            // border:1px solid red;            margin-top:30px;            margin-right: 30px;            div:nth-of-type(1){                color:rgb(104, 104, 104);                box-shadow: 0px 0px 3px #ddd;                background-color: white;                p:nth-of-type(1){                    padding: 10px 10px;                    margin-top:0px;                                       font-size: 15px;                    border-bottom:1px solid rgb(201, 199, 199);                }                p:nth-of-type(2){                    margin-top:0px;                    margin-bottom:0px;                    padding:10px 10px;                    border-bottom:1px solid rgb(201, 199, 199);                    font-size: 14px;                }                .btnGroup2{                    overflow: hidden;                    padding:20px;                    button{                        padding:3px 10px;                        width:100px;                        border-radius: 2px;                        border:1px solid;                    }                    button:nth-of-type(1){                        float:left;                    }                    button:nth-of-type(2){                        float:right;                    }                }            }            div:nth-of-type(2){                color:rgb(104, 104, 104);                box-shadow: 0px 0px 3px #ddd;                background-color: white;                margin-top:10px;                p:nth-of-type(1){                    padding: 10px 10px;                                       font-size: 15px;                    border-bottom:1px solid rgb(201, 199, 199);                }                p:nth-child(n+1){                    margin:0px;                    padding:10px;                    padding-top:-5px;                                       font-size: 14px;                    border-bottom:1px solid rgb(201, 199, 199);                }            }        }    }    footer{        background-color: white;        padding:10px;        p{                        margin:20px auto;            text-align:center;            padding-bottom:8px;            font-size: 10px;            &:first-child{                width:900px;                border-bottom:1px solid #ddd;            }            &:last-child{                width:600px;            }        }            }}
```



### 卡片布局实现（float)

 ---- 使用频率很高

```html
<html><head>    <link href="index.css" rel="stylesheet" type='text/css' /></head><body>    <div class='content'>        <div data-title="定位操作布局">            <img src='./素材2.JPG' />        </div>        <div data-title="浮动布局">            <img src='./素材2.JPG' />        </div>        <div data-title="定位布局">            <img src='./素材2.JPG' />        </div>        <div data-title="弹性布局">            <img src='./素材2.JPG' />        </div>        <div data-title="各种选择器">            <img src='./素材2.JPG' />        </div>        <div data-title="文本样式设置">            <img src='./素材2.JPG' />        </div>    </div></body></html>
```

```less
div.content div{    width:200px;    height:120px;    border:3px solid violet;    position: relative;    float:left;    margin:10px;    background-color: violet;    cursor: pointer;    &:hover img{        z-index:-1;    }    img{        width:200px;        height:120px;        position:absolute;        z-index:2;    }    &::before{        content:attr(data-title);        display: block;        position: absolute;        z-index: 1;        color:white;        width:100%;        height:100%;        line-height: 120px;        text-align: center;    }    &:after{        z-index: 3;        content:'热门';        display: block;        position:absolute;        top:0px;        left:0px;        font-size: 12px;        background-color:rgb(202, 108, 202);        width:30px;        height:30px;        border-radius: 15px;        text-align: center;        line-height: 30px;        margin:10px;        color:white;        box-shadow: 0 0 3px #ddd;    }}
```

### 卡牌布局实现（flex)

```css
.container{  width:300px;  height:200px;  border:1px solid;  display:flex;  flex-flow:row wrap;  justify-content:space-between;  padding:20px;  background-color:#ddd;  background-clip:content-box;  align-content:space-between;}.container>div{  width:60px;  height:50px;  border:1px solid green;}
```

### 微信公众号下拉菜单（Flex+absolute）

```html
 <div class="container">        <div class="content"></div>        <div class="menu">            <div>                <span>教程</span>                <div class="submenu">                    <div>PHP</div>                    <div>Linux</div>                    <div>Web</div>                </div>            </div>            <div>                <span>直播</span>                <div class="submenu">                    <div>户外</div>                    <div>游戏</div>                    <div>VTuber</div>                    <div>VTuber</div>                    <div>VTuber</div>                    <div>VTuber</div>                    <div>VTuber</div>                </div>            </div>            <div>                <span>直播</span>                <div class="submenu">                    <div>户外</div>                    <div>游戏</div>                    <div>VTuber</div>                </div>            </div>        </div>    </div>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;}.container{  margin:10px;  width:200px;  height:360px;  border:1px solid red;  div.content{    height:330px;    background-color: #eee;  }  div.menu{    display: flex;    flex-flow: row;    justify-content: space-between;    height:30px;    &>div{      flex:1;      position: relative;      display: flex;      flex-flow: column-reverse;      &:hover .submenu{          display: block;      }      span{        font-size: 14px;        display: block;        text-align: center;        border:1px solid;        cursor:pointer;        height:30px;        line-height: 30px;        color:white;        background-color: darkorchid;            }      div.submenu{        display: none;        width:100%;        div{          height:25px;          line-height:25px;          border-top:1px solid;          border-right:1px solid;          border-left:1px solid;          border-radius: 3px;          text-align: center;          cursor: pointer;          &:hover{            background-color: #ddd;          }        }        div:last-child{          border-bottom:1px solid;        }              }    }  }}
```

效果如下：

![微信下拉菜单模仿](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215517.png)



### 上下高度固定、中间高度自适应布局

```html
<body>    <div></div>    <div></div>    <div></div></body>
```

```less
*{  padding: 0;  margin: 0;}body{  display: flex;  flex-direction: column;  justify-content: space-between;  height:100vh;  div:nth-child(1){    background-color: rgb(47, 160, 98);    height:80px;  }  div:nth-child(2){    background-color: rgb(116, 111, 111);    flex-grow: 1;  }  div:nth-child(3){    background-color: rgb(177, 89, 89);    height:80px;  }}
```

### 栅格系统练习

```html
<article>        <header></header>        <nav></nav>        <main></main>        <footer>            <div></div>            <div></div>            <div></div>            <div></div>        </footer></article>
```

```less
*{  margin:0;  padding:0;}article{  width: 100vw;  height:100vh;  display: grid;  grid-template:     'header header' 60px    'nav main' 1fr    'footer footer' 60px/60px 1fr  ;    header,nav,main,footer{    background-clip: content-box;    background-color: blueviolet;    padding:6px;  }  header{    grid-area: header;  }  nav{    grid-area: nav;  }  main{    grid-area: main;  }  footer{    grid-area: footer;    display: grid;    grid-template-columns: repeat(4,1fr);    div{      background-clip: content-box;      background-color: #ddd;      padding:3px;    }  }}
```

![grid](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215522.JPG)

### 动画实现input动态下划线

```html
<main>        <div class='filed'>            <input placeholder="请输入后盾人账号" />        </div>        <div class='filed'>            <input placeholder="请输入密码" />        </div></main>
```

```less
*{  margin:0;  padding:0;}body{  background-color: #28527a;}main{  position: absolute;  top:50%;  left:50%;  transform:translate(-50%,-50%);  width:300px;  height:300px;  border:3px solid white;  display: flex;  flex-direction: column;  justify-content: center;  align-items: center;  position: relative;    div.filed{    position: relative;    overflow: hidden;    &::before{      content:'';      display: block;      position:absolute;      width:100%;      bottom:1px;      height:4px;      //linear-gradient只适用于background不适用于backgroud-color、color属性      background:linear-gradient(to right, #04b1a8,#025955, rgb(3, 209, 199));      transform: translateX(-100%);      transition: 2s;    }    &:nth-child(1):hover::before{      transform: translateX(100%);    }    &:nth-child(2):hover::before{      transform: translateX(100%);    }    input{      padding: 10px;      margin-bottom: 5px;      height:40px;      width:160px;      &:focus{        outline:none;      }    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215526.png" alt="input_color" style="zoom:50%;" />

### 动画实现菜单切换

```html
<main>        <div id='home'>            <i class="fa fa-home fa-5x" aria-hidden="true"></i>        </div>        <div id='video'>            <i class="fa fa-video-camera fa-5x" aria-hidden="true"></i>        </div>        <div id='live'>            <i class="fa fa-user-circle-o fa-5x" aria-hidden="true"></i>        </div>    </main>    <nav>        <a href="#home">home</a>        <a href="#video">video</a>        <a href="#live">live</a></nav>
```

```less
*{  margin:0;  padding:0;  box-sizing:border-box;}body{  width:100vw;  height:100vh;  display:flex;  flex-flow: column;  position:relative;  &::after{    content:'houdunren';    opacity: .5;    font-size: 38px;    position:absolute;    left:50%;    top:50%;    transform: translate(-50%,-50%);  }  main{    flex:1;    position: relative;    div{      //给div统一设置样式      position:absolute;      width:100%;      height:100%;      display:flex;      flex-direction: column;      justify-content: center;      align-items: center;      //先让它平移到上面      transform: translateY(-100%);      transition: 0.5s;      z-index: 999;    }    div:target{      //点击下面菜单命中锚点时才显示      transform: translateY(0px);    }    //设置div被锚点命中target时的样式    div:nth-of-type(1):target{      background-color: blueviolet;    }    div:nth-of-type(2):target{      background-color:brown;    }    div:nth-of-type(3):target{      background-color:darkgray;    }  }  nav{    height:60px;    background-color: rgb(126, 125, 125);    display: flex;    a{      flex:1;      text-decoration: none;      color:white;      text-transform: uppercase;      text-align: center;      line-height: 60px;      cursor: pointer;    }    a:nth-child(2){      border-right:1px solid white;      border-left:1px solid white;    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215531.JPG" alt="自适应菜单" style="zoom:50%;" />

### 动画实现动态菜单

```html
<main>        <ul>            <li>                <strong>video</strong>                <div>                    <a href="">hdcms</a>                    <a href="">php</a>                </div>            </li>        </ul>        <ul>            <li>                <strong>live</strong>                <div>                    <a href="">linux</a>                    <a href="">css3</a>                </div>            </li>        </ul></main>
```

```less
*{  padding:0;  margin:0;  box-sizing: border-box;  text-transform:uppercase;}body{  width:100vw;  height:100vh;  display: flex;  justify-content: center;  align-items:center;  main{    ul{      cursor: pointer;      float:left;      list-style: none;      li{        border:1px solid;        background-color: green;        color:white;        position: relative;        width:70px;        height:30px;        line-height: 30px;        text-align: center;        &:hover div{          transform: scale(1);        }        &:nth-child(1){          margin-right:10px;        }        &:nth-child(2){          margin-left:10px;        }        div{          transform: scale(0);          transition: .3s;          transform-origin: left top;  //设置缩放起始点          position:absolute;          margin-top:-1px;          a{            background-color: #ddd;            display: block;            border-top: none;            text-decoration: none;            color:black;            text-transform: uppercase;            font-size: 12px;            width:68px;          }        }      }    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215537.JPG" alt="动态菜单" style="zoom:50%;" />

### 图片选择模糊/清晰	效果实现

```html
<main>        <img src='./素材.png' />        <img src='./素材.png' />        <img src='./素材.png' /></main>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;}body{  width: 100vw;  height: 100vh;  display: flex;  justify-content: center;  align-items: center;  main{    transition:1s;    img{      width:20vw;      height:20vw;      filter: blur(0);      transition:0.8s;      margin:10px;      cursor: pointer;    }    &:hover img{      filter: blur(5px);      transform: scale(0.8);    }    &:hover img:hover{      filter: blur(0);      transform: scale(1.2);    }      }}
```

### 文字旋转效果

```html
<main>        <span>            <strong>h</strong>ou<strong>d</strong>unren        </span></main>
```

```less
*{  padding:0;  margin:0;}main{  width:100vw;  height:100vh;  display: flex;  justify-content: center;  align-items: center;    span{    font-size: 2em;    strong{      cursor: pointer;      display: inline-block;      width:1.5em;      height:1.5em;      border-radius: 50%;      text-align: center;      line-height: 1.5em;      color:white;      &:hover{        transform: rotate(360deg);        transition:2s;      }    }    strong:nth-child(1){      background-color: green;    }    strong:nth-child(2){      background-color:hotpink;    }  }}
```

### 按钮悬浮动态效果skew实现

```html
<a href=''>houdunren</a>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;  box-sizing: border-box;}body{  background-color: gray;  width: 100vw;  height: 100vh;  display: flex;  align-items: center;  justify-content: center;    a{        display: block;    text-decoration: none;    color:black;    font-size:4em;    color:white;    opacity: .8;    width:50vw;    height:60px;    line-height: 60px;    text-align: center;    border:2px solid #aa2b1d;    position:relative;    overflow: hidden;    &::after{      position:absolute;      content:'';      display: block;      width:0;      height:100%;      top:0;      left:50%;      transform: skewX(45deg);      background-color: #aa2b1d;      z-index:-1;      transition: 1s;    }    &:hover::after{      width:50vw;      transform:translateX(-50%);    }  }}
```

![动态按钮](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215541.JPG)

### 3D按钮制作(skew)

```html
<body>    <a href=''>houdunren</a></body>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;  box-sizing: border-box;}body{  background-color: gray;  width: 100vw;  height: 100vh;  display: flex;  align-items: center;  justify-content: center;    a{      display: block;    text-decoration: none;    color:black;    font-size:4em;    color:white;    opacity: .8;    width:50vw;    height:60px;    line-height: 60px;    text-align: center;    background-color:  #aa2b1d;    position:relative;    letter-spacing: .4em;    transform-style: preserve-3d;    transform:perspective(900px) skewX(25deg) rotate(-15deg);        &::after{      position:absolute;      content:'';      display: block;      width:100%;      height:15px;      top:100%;      left:-7px;      transform:skewX(-45deg);      background-color: black;      z-index: -1;    }    &::before{      position:absolute;      content:'';      display: block;      width:15px;      height:100%;      left:-15px;      top:8px;      transform:skewY(-45deg);      background-color: black;      z-index: -1;    }    &:hover{      font-weight: bold;    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215546.JPG" alt="3D按钮" style="zoom:50%;" />

### 3D新年贺卡

```html
<body>    <a href=''><span>荷西1997</span></a></body>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;  box-sizing: border-box;}body{  background-color: gray;  width: 100vw;  height: 100vh;  display: flex;  align-items: center;  justify-content: center;    a{    text-decoration: none;    color:white;    width: 200px;    height:120px;    background-color:rgb(192, 101, 15);    position: relative;    font-size: 3em;    transform-style:preserve-3d;    transform: perspective(900px) rotate3d(1,1,0,45deg);    span{      position:absolute;      z-index:1;      top:50%;      left:50%;      transform: translate(-50%,-50%);      font-size: 1em;    }    &::after,&::before{      position: absolute;      display: block;      width:50%;      height:100%;      display: flex;      align-items: center;      background-color: rgb(134, 24, 24);      transition: 1.2s;      z-index:2;    }    &::after{      content:'快乐';      left:50%;      transform-origin: right;    }    &::before{      content:'新年';      justify-content: flex-end;      transform-origin: left;    }    &:hover::before{      transform:rotateY(-180deg)    }    &:hover::after{      transform:rotateY(180deg)    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215549.JPG" alt="新年贺卡" style="zoom:67%;" />

### 动画实现静态时钟页面

```html
<div class="clock">        <div class="circle">            <div class="line"></div>            <div class="line"></div>            <div class="line"></div>            <div class="line"></div>            <div class="line"></div>            <div class="line"></div>            <div class='modal'></div>            <div id='hour'></div>            <div id='minute'></div>            <div id='second'></div>            <div id='point'></div>        </div></div>
```

```less
@bgcolor:#2c3e50;   //定义背景颜色变量body{  font-size: 62.5%;  margin:0;  padding:0;  box-sizing: border-box;  width: 100vw;  height: 100vh;  display: flex;  justify-content: center;  align-items: center;  background: @bgcolor;  div.clock{    width:250px;    height:250px;    border-radius: 50%;    background: linear-gradient(to right,#f1c40f,#e67e22,#e74c3c);    position:relative;    cursor: pointer;    div.circle{      position:absolute;      display: block;      width:90%;      height:90%;      background-color:@bgcolor;      border-radius: 50%;      top:50%;      left:50%;      transform: translate(-50%,-50%);      div.line{        position:absolute;        width:95%;        height:6px;        background-color: white;        top:50%;        left:50%;        margin-top:-3px;        margin-left: -47.5%;        transform-origin: center;        &:nth-child(1){          transform: rotate(0deg);        }        &:nth-child(2){          transform: rotate(30deg);        }        &:nth-child(3){          transform: rotate(60deg);        }        &:nth-child(4){          transform: rotate(90deg);        }        &:nth-child(5){          transform: rotate(120deg);        }        &:nth-child(6){          transform: rotate(150deg);        }      }      div.modal{        position:absolute;        background-color: @bgcolor;        width:85%;        height:85%;        top:50%;        left:50%;        border-radius: 50%;        transform:translate(-50%,-50%);      }      div#hour{        position:absolute;        background-color: white;        width:6px;        height:20%;        top:50%;        left:50%;        margin-left:-3px;        transition: 1s;        transform-origin: top;      }      div#minute{        position:absolute;        background-color: white;        width:4px;        height:30%;        top:50%;        left:50%;        margin-left:-2px;        transform: rotate(60deg);        transition: 1s;        transform-origin: top      }      div#second{        position:absolute;        background-color: white;        width:2px;        height:35%;        top:50%;        left:50%;        margin-left:-1px;        transform: rotate(190deg);        transform-origin: top;        transition: 1s;      }      div#point{        position:absolute;        background-color: white;        width:7%;        height:7%;        top:50%;        left:50%;        border-radius: 50%;        transform:translate(-50%,-50%);      }      &:hover div#minute{        transform: rotate(290deg);      }      &:hover div#hour{        transform: rotate(-90deg);      }      &:hover div#second{        transform: rotate(-90deg);      }    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215554.JPG" alt="css动画--时钟" style="zoom:50%;" />

### CSS动画实现动态时钟

```html
<body>    <div class='clock'></div></body>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;}body{  width: 100vw;  height: 100vh;  display: flex;  justify-content: center;  align-items: center;  div.clock{    width:300px;    height:300px;    background-color: cadetblue;    border-radius:50%;    position:relative;    &::after{      //设置中心点      background-color: white;      position: absolute;      content:'';      display:block;      width:16px;      height:16px;      top:50%;      left:50%;      border-radius: 50%;      transform:translate(-50%,-50%);    }    &::before{      //设置指针      background-color: white;      position:absolute;      content:'';      display: block;      width:8px;      height:45%;      margin-left:146px;      margin-top:16px;      transform-origin: bottom;      //设置指针回去的时间，2s      transition-duration: 2s;    }    &:hover::before{      //设置鼠标悬浮时一秒1deg前进      transition-duration: 60s;      transition-timing-function: steps(60,start);      transform: rotate(360deg);    }  }}
```



### 环绕菜单效果

```html
<body>    <div id="menu">        <div class="small"><span>1</span></div>        <div class="small"><span>2</span></div>        <div class="small"><span>3</span></div>        <div class="small"><span>4</span></div>        <div class="small"><span>5</span></div>        <div class="small"><span>6</span></div>        <div class="small"><span>7</span></div>        <div class="small"><span>8</span></div>        <div class="small"><span>9</span></div>    </div></body>
```

```less
*{  margin:0;  padding:0;  box-sizing: border-box;}body{  width: 100vw;  height: 100vh;  display:flex;  justify-content: center;  align-items: center;  color:white;  div#menu{    position:relative;    width:100px;    height:100px;    background-color: #e67e22;    border-radius: 50%;    cursor: pointer;    &::after{      content:'后盾人';      display: block;      position:absolute;      top:50%;      left:50%;      transform:translate(-50%,-50%);    }    div.small{      position: absolute;      background-color: #e67e22;      width:30px;      height:30px;      border-radius: 50%;      text-align:center;      line-height: 30px;      top:-30px;      left:-30px;      transition: 1s;      transform-origin: 80px 80px;  //小圆以大圆圆心旋转      span{        //只有block元素才能旋转        display: block;      }    }	//圆圈旋转    &:hover div:nth-child(1){      transform: rotate(0deg);    }    &:hover div:nth-child(2){      transform: rotate(40deg);    }    &:hover div:nth-child(3){      transform: rotate(80deg);    }    &:hover div:nth-child(4){      transform: rotate(120deg);    }    &:hover div:nth-child(5){      transform: rotate(160deg);    }    &:hover div:nth-child(6){      transform: rotate(200deg);    }    &:hover div:nth-child(7){      transform: rotate(240deg);    }    &:hover div:nth-child(8){      transform: rotate(280deg);    }    &:hover div:nth-child(9){      transform: rotate(320deg);    }    //文字倾斜纠正    &:hover div:nth-child(1)>span{      transform: rotate(0deg);    }    &:hover div:nth-child(2) span{      transform: rotate(-40deg);    }    &:hover div:nth-child(3)>span{      transform: rotate(-80deg);    }    &:hover div:nth-child(4) span{      transform: rotate(-120deg);    }    &:hover div:nth-child(5)>span{      transform: rotate(-160deg);    }    &:hover div:nth-child(6) span{      transform: rotate(-200deg);    }    &:hover div:nth-child(7)>span{      transform: rotate(-240deg);    }    &:hover div:nth-child(8) span{      transform: rotate(-280deg);    }    &:hover div:nth-child(9) span{      transform: rotate(-320deg);    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215600.JPG" alt="动态旋转菜单" style="zoom:50%;" />

### 3D旋转图集

```html
<div class="container">        <img src="./素材.png" />        <img src="./素材.png" />        <img src="./素材.png" />        <img src="./素材.png" /></div>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;  box-sizing: border-box;}body{  width:100vw;  height:100vh;  position: relative;  background:linear-gradient(90deg,#afabab,#dddddd);  display: flex;  justify-content: center;  align-items: center;  div.container{    width:150px;    height:150px;    position: relative;    transform-style: preserve-3d;  //加上才可以看到3d效果    transform:perspective(900px) rotateX(-15deg);    transform-origin: center center -120px;  //设置旋转中心点 x y z    transition: 1s;    &:hover{      transform: perspective(900px) rotateX(-15deg) rotateY(90deg);    }    img{      display: block;      position:absolute;      top:50%;      left:50%;      margin-left:-75px;      margin-top:-75px;      width:150px;      height:150px;      transform-origin: center center -120px;    }    img:nth-child(1){      transform:rotateY(90deg);    }    img:nth-child(2){      transform:rotateY(180deg);    }    img:nth-child(3){      transform:rotateY(270deg);    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215603.JPG" alt="3D旋转图集" style="zoom:50%;" />

### 3D立方体

```html
<body>    <div></div>    <div></div>    <div></div>    <div></div>    <div></div>    <div></div></body>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;  box-sizing: border-box;}body{  width:100vw;  height:100vh;  position: relative;  transform-style: preserve-3d;  transition: 2s;  transform: perspective(900px) rotateX(-45deg) rotateY(45deg);  div{    background-color: darkcyan;    position: absolute;    width:100px;    height:100px;    border:1px solid;    top:50%;    left:50%;    margin-top:-50px;    margin-left:-50px;    //设置旋转中心点为立方体中心，分别对6面旋转    transform-origin:center center -50px;    transition:1.2s;  }  div:nth-child(1){    background-color: rgb(43, 151, 151);    transform:  rotateY(90deg);  }  div:nth-child(2){    background-color: rgb(67, 77, 77);    transform:  rotateY(180deg);  }  div:nth-child(3){    background-color: rgb(172, 182, 29);    transform:  rotateY(270deg);  }  div:nth-child(4){    background-color: red;    transform:  rotateX(90deg);  }  div:nth-child(5){    background-color: green;    transform:  rotateX(270deg);  }  &:hover{    transform: perspective(900px) rotateX(45deg) rotateY(-45deg);  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215608.JPG" alt="3D立方体" style="zoom:50%;" />

### 3D长方体，长宽高可在less变量中随时修改

```html
<body>    <div class="container">        <div></div>        <div></div>        <div></div>        <div></div>        <div></div>        <div></div>    </div></body>
```

```less
@WIDTH:180px;  //长方体的长@HEIGHT:100px;  //长方体的高@ZLENGTH:80px;  //长方体在Z轴的长度//less变量减法运算两边必须空起来 @HEIGHT-@ZLENGTH的写法是错误的 - 两边要空格@C:@HEIGHT - @ZLENGTH;*{  margin:0;  padding:0;  box-sizing: border-box;}body{  widows: 100vw;  height: 100vh;  display: flex;  justify-content: center;  align-items: center;  div.container{    position:relative;    transform-style: preserve-3d;    transform:perspective(900px) rotate3d(1,1,0,45deg);    transition: 1s;    &:hover{      transform:perspective(900px) rotate3d(1.2,1.3,1,190deg);    }    div{      position:absolute;    }    div:nth-child(1){      width: @WIDTH;      height: @HEIGHT;      background-color: #28527a;    }    div:nth-child(2){      width: @WIDTH;      height: @HEIGHT;      transform: translateZ(-@ZLENGTH);      background-color: #8ac4d0;    }    div:nth-child(3){      width: @WIDTH;      height: @ZLENGTH;      top:@C;      transform-origin: bottom;      transform: rotateX(90deg);      background-color: #f4d160;    }    div:nth-child(4){      width: @WIDTH;      height: @ZLENGTH;      transform-origin: top;      transform: rotateX(-90deg);      background-color: #fbeeac;    }    div:nth-child(5){      width: @ZLENGTH;      height: @HEIGHT;      transform-origin: left;      transform:rotateY(90deg);      background-color: #025955;    }    div:nth-child(6){      width: @ZLENGTH;      height: @HEIGHT;      transform-origin: right;      left:@WIDTH - @ZLENGTH;      transform:rotateY(-90deg);      background-color: #e36bae;    }  }}
```

### 卡片切换效果

```html
<body>    <div class="container">        <div></div>        <div></div>    </div></body>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;}body{  width:100vw;  height:100vh;  display: flex;  justify-content: center;  align-items: center;  div.container{    width:200px;    height:200px;    position: relative;    backface-visibility: hidden;    border:1px solid;     cursor: pointer;    transform: perspective(900px) rotateX(45deg);    transform-style: preserve-3d;    &:hover div:nth-child(1){      transform: rotateY(0deg);    }    &:hover div:nth-child(2){      transform: rotateY(180deg);    }    div{      position:absolute;      width:100px;      height:100px;      top:50%;      left:50%;      margin-left:-50px;      margin-top:-50px;      text-align: center;      line-height: 100px;      //设置为hidden，当div 翻到背面时隐藏该div显示      backface-visibility: hidden;      transition: 2s;    }    div:nth-child(1){      background-color: #314e52;      transform: rotateY(180deg);    }    div:nth-child(2){      background-color: #eaac7f;    }  }}
```

### 翻转的登录/注册页面

```html
<body>    <div class="cards">        <div id='login'><span>注册</span></div>        <div id='register'><span>登录</span></div>    </div>    <div class="btns">        <a href="#login" onclick='change("login")'>注册</a>        <a href="#register" onclick='change("register")'>登录</a>    </div>    <script>        function change(type) {            let cardNode = document.getElementsByClassName('cards')[0];            switch (type) {                case 'login':                    //切换到登录页面                    let classNames = cardNode.className                    classNames = classNames.replace(' showlogin', '')                    classNames = classNames.replace(' showregister', '')                    classNames += ' showlogin'                    cardNode.className = classNames                    break;                case 'register':                    //切换到注册页面                    let classNames2 = cardNode.className                    classNames2 = classNames2.replace(' showlogin', '')                    classNames2 = classNames2.replace(' showregister', '')                    classNames2 += ' showregister'                    cardNode.className = classNames2                    break;                default:                    break;            }        }    </script></body>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;}body{  width:100vw;  height:100vh;  position:relative;  transform: perspective(900px);  transform-style: preserve-3d;  transform-origin: center center;  div.cards{          div{      position:absolute;      width:100%;      height:100%;      z-index: -1;      backface-visibility: hidden;      transition: 2s;      color:white;      display: flex;      justify-content: center;      align-items: center;      span{        font-size: 60px;        z-index: -1;      }    }    div:nth-child(2){      background-color: #eb5e0b;    }    div:nth-child(1){      background-color: #00917c;      transform:rotateY(-180deg);    }  }  div.showlogin{    //显示login页面，隐藏register页面    div:nth-child(1){      transform:rotateY(0deg);    }    div:nth-child(2){      transform:rotateY(-180deg);    }  }  div.showregister{    //显示register页面，隐藏login页面    div:nth-child(1){      transform:rotateY(180deg);    }    div:nth-child(2){      transform:rotateY(0deg);    }  }  div.btns{        position:absolute;    bottom:0px;    width:200px;    left:50%;    transform:translateX(-50%);    display: flex;    justify-content: space-around;    a{      z-index:9999;      display: inline-block;      text-decoration: none;      margin-bottom: 40px;      color:white;      font-size: 5em;      width:85px;      height:2em;      line-height: 2em;      text-align: center;      background-color: #314e52;      cursor: pointer;    }  }}
```

### CSS动画实现简易轮播图

* 将所有图片一行显示，逐个translateX即可，overflow:hidden

```html
<body>    <div class='tuji'>        <div class='imgs'>            <img src="./素材2.JPG" />            <img src="./素材.png" />        </div>    </div></body>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;  box-sizing: border-box;}body{  width: 100vw;  height: 100vh;  display: flex;  justify-content: center;  align-items: center;  div.tuji{    overflow: hidden;    width:200px;    height:120px;    &:hover div.imgs{      transform: translateX(-200px);    }    div.imgs{      transition-duration:2s;  //动画时间2s      transition-delay: 0s;  //延迟，实际切换需要1s延迟+2s动画时间=3s时间      transition-timing-function:cubic-bezier(0.075, 0.82, 0.165, 1);  //动画进行速度控制      display: flex;      cursor: pointer;      img{        width:100%;        height:100%;        display: block;        border:1px solid;      }    }  }}
```

### CSS实现完整轮播图

```html
<body>    <div class="container">        <div class='imgs'>            <div><img src="./素材2.JPG" /></div>            <div><img src="./素材4.png" /></div>            <div><img src="./素材5.png" /></div>            <div><img src="./css动画--时钟.JPG" /></div>            <div><img src="./素材2.JPG" /></div>        </div>        <ul>            <li>1</li>            <li>2</li>            <li>3</li>            <li>4</li>        </ul>    </div></body>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;}body{  display: flex;  justify-content: center;  align-items: center;  background-color: #28527a;    div.container{      width:200px;      height:150px;      position:relative;      overflow: hidden;            div.imgs{        animation-name: imgmove;            display:flex;        animation-duration: 4s;        animation-timing-function: steps(4,end);        animation-iteration-count: infinite;        @keyframes imgmove {          to{            transform: translateX(-800px);          }        }        div{          img{            width:200px;            height:150px;            background-size: cover;          }        }      }      &:hover div.imgs{        animation-play-state: paused;      }      &:hover ul::after{        animation-play-state: paused;      }      ul{        position:absolute;        list-style: none;        display:flex;        bottom:5px;        left:50%;        margin-left:-72px;        li{          margin:8px;          width:20px;          height: 20px;          border-radius: 50%;          display: grid;          justify-content: center;          align-items: center;          color:white;          background-color: rgba(0, 0, 0, 0.5);          z-index:1;        }                &::after{          content:'';          display: block;          width:20px;          height:20px;          position:absolute;          background-color: red;          border-radius: 50%;          left:8px;          top:8px;          z-index:0;          animation-name:nummove;          animation-duration: 4s;          animation-timing-function: steps(4,end);          animation-iteration-count: infinite;          @keyframes nummove {            to{              transform: translateX(144px);            }          }        }      }      }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215617.JPG" alt="完整轮播图"  />

### CSS实现红心点赞

```html
<body>    <div class='container'>        <i class="fa fa-heart fa-4x" aria-hidden="true" onclick="change()"></i>        <i class="fa fa-heart fa-4x" aria-hidden="true" onclick="change()"></i>    </div>    <script>        function change() {            let nodeClassList = document.getElementsByClassName('container')[0].classList            if (nodeClassList.contains('heart')) {                nodeClassList.remove('heart')            } else {                nodeClassList.add('heart')            }        }    </script></body>
```

```less
*{  margin: 0;  padding: 0;  box-sizing: border-box;}body{  width: 100vw;  height: 100vh;  display: flex;  justify-content: center;  align-items: center;  div.heart i:nth-child(2){    transform: scale(3);    color:#a9294f;    opacity: 0;  }  div.heart i:nth-child(1){    color:#a9294f;    opacity: 1;  }  div.container{    position:relative;    i{      position:absolute;      transition-duration: .5s;      top:50%;      left:50%;      width:60px;      height:60px;      margin-left:-30px;      margin-top:-30px;      color:#557174;    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215621.JPG" alt="点赞效果" style="zoom:50%;" />

### 沿着四边移动的正方形（keyframe)

```html
<body>    <div class='container'>        <div></div>    </div></body>
```

```less
*{  margin: 0;  padding: 0;  box-sizing: border-box;}body{  width: 100vw;  height: 100vh;  display: flex;  justify-content: center;  align-items: center;  div.container{    width:300px;    height:300px;    border:1px solid;    position:relative;    cursor: pointer;    //移动必须用transform,不能用top left 标识位置让其移动    //from(0%)和to(100%)不写的话默认为元素初始样式    @keyframes move {      25%{        transform: translate(248px,0);      }      50%{        transform: translate(248px,248px);      }      75%{        transform: translate(0px,248px);      }    }          @keyframes colorchange{      25%{        background-color: #78c4d4;      }      50%{        background-color: #8f4f4f;      }      75%{        background-color: #f2b4b4;      }    }              &:hover div{      animation-name: move,colorchange; //声明多个帧动画用逗号分隔      animation-duration: 4s,2s;        //分别代表每个帧动画的时间    }    div{      position:absolute;      width:50px;      height:50px;      background-color:cadetblue;      top:0;      left:0;    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215625.JPG" alt="keyframe移动正方形" style="zoom:50%;" />

### 移动端引导背景页(多种动画综合实现)

```html
<body>    <div class='container'>    </div></body>
```

```less
*{  margin: 0;  padding: 0;  box-sizing: border-box;}body{  background-color: #34495e;  .container{    width: 100vw;    height: 100vh;    position: relative;    background-color: #9b59b6;    animation-fill-mode:forwards;    animation-duration: 2s;    animation-name: scaleChange,colorChange,radius;    transform: scale(0);      @keyframes scaleChange {      25%{        transform: scale(0.5);      }      50%{        transform: scale(1) rotate(360deg);      }      75%{        transform: scale(0.5);      }      100%{        transform: scale(1);      }    }      @keyframes radius {      25%{        border-radius: 25%;      }      50%{        border-radius: 0%;      }      75%{        border-radius: 10%;      }      100%{        border-radius: 0%;      }    }      @keyframes opacityChange {      to{        opacity: .8;      }    }      @keyframes colorChange {      25%{        background-color: #e67e22;      }      50%{        background-color: #2980b9;      }      75%{        background-color: #16a085;      }      100%{        background-color: #34495e;      }    }      &::after{      position:absolute;      content:'houdunren';      display: block;      top:50%;      left:50%;      transform: translate(-50%,-50%);      text-transform: uppercase;      letter-spacing: .2em;      font-size: 2em;      color:white;      //设置初始透明度为0，等延迟时间animation-delay过去以后才执行opcaityChange帧动画慢慢显示出来      opacity: 0;        animation-name: opacityChange;      //帧动画持续时间      animation-duration: 2s;      //帧动画延迟指定时间执行      animation-delay: 2s;      //是否保持最后一帧动画显示，而不是元素初始样式，设置为forwards的话则显示      animation-fill-mode: forwards;    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215628.JPG" alt="多种帧动画综合手机端引导背景页" style="zoom:50%;" />

### CSS动画实现心形和无限跳动

* 心形的绘制只需要一个正方形和两个圆组合就可以

```html
<body>    <div class='heart'>    </div></body>
```

```less
*{  margin: 0;  padding: 0;  box-sizing: border-box;}body{  width: 100vw;  height: 100vh;  display: flex;  justify-content: center;  align-items: center;  div.heart{    width:200px;    height:200px;    background-color:#c0392b;    position:relative;    transform: rotate(45deg) scale(0.3);    cursor:pointer;    margin-top:20px;    @keyframes heartBeat {      50%{        transform: rotate(45deg) scale(1);      }    }    &:hover{      animation-name: heartBeat;      animation-duration: 1s;      animation-iteration-count: infinite;    }    &::after{      content:'';      position:absolute;      display: block;      width:200px;      height:200px;      border-radius: 50%;      background-color:#c0392b;      top:-100px;    }    &::before{      content:'';      position:absolute;      display: block;      width:200px;      height:200px;      border-radius: 50%;      background-color: #c0392b;      left:-100px;    }  }}
```

### CSS动画实现弹跳小球

```html
<body>    <div class='ball'></div>    <section></section></body>
```

```less
*{  margin: 0;  padding: 0;  box-sizing: border-box;}body{  width: 100vw;  height: 100vh;  display: flex;  justify-content: center;  align-items: center;  div.ball{    width:50px;    height:50px;    background:radial-gradient(at center,#f1c40f,#e67e22,#e74c3c);    border-radius: 50%;    position:relative;    top:100px;    animation-name:ballmove;    animation-duration: .5s;    animation-iteration-count: infinite;    animation-direction: alternate-reverse;    @keyframes ballmove {      100%{        transform: translateY(-200px);      }    }  }  @keyframes shadowChange {    100%{      width:120px;      height:30px;      opacity: .5;      filter: blur(4px);    }  }  section{      position:absolute;      margin-top:250px;      background-color: #34495e;      width:100px;      height:20px;      z-index:-1;      opacity: 0.8;      border-radius: 50%;      animation-name:shadowChange;      animation-duration: .5s;      filter: blur(2px);      animation-iteration-count: infinite;      animation-direction: alternate-reverse;  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215632.JPG" alt="balljump" style="zoom:50%;" />




### 多动画应用页面布局

```html
<body>    <header>荷西1997</header>    <main>        <div>Millions of developers and companies build, ship, and maintain their software on GitHub—the largest and most advanced development platform in the world.</div>        <div>DevOps is just the start. Top organizations know that transformation also depends on technology, talent, culture, and process. GitHub helps enterprises put them all to work—in one place.</div>    </main>    <footer>I can do all things</footer></body>
```

```less
*{  margin: 0;  padding: 0;  box-sizing: border-box;}body{  color:white;  width: 100vw;  height: 100vh;  display: grid;    grid-template:    'header' 10vh    'main' 1fr    'footer' 10vh;  header{    background-color: #d35400;    display: flex;    justify-content: center;    align-items:center;    font-size: 2em;    transform: translateX(-100%);    //设置header的动画，header第一个出现    animation-name: showHeader;    animation-duration: 1s;    animation-timing-function: cubic-bezier(0.075, 0.82, 0.165, 1);    animation-fill-mode: forwards;    @keyframes showHeader {      100%{        transform: translateX(0);      }    }  }  footer{    background-color: #16a085;    display: flex;    justify-content: center;    align-items:center;    font-size: 1.4em;    text-transform: capitalize;    //设置footer的动画，footer紧挨着header动画之后出现    //由于header动画执行时间为1s，因此这里需要设置延长1s    animation-delay: 0.5s;    animation-name: showFooter;    animation-duration: 1s;    animation-timing-function: cubic-bezier(0.075, 0.82, 0.165, 1);    animation-fill-mode: forwards;    transform:scale(0);    @keyframes showFooter {      100%{        transform: scale(1) rotate(360deg);      }    }  }  main{    display: flex;    flex-direction: column;    align-items: center;    //设置背景    background: url('./素材3.jpg') no-repeat;    background-size: cover;    //设置main动画背景翻转出现    backface-visibility: hidden;    transform:rotateY(180deg);    animation-delay: 1s;    animation-fill-mode: forwards;    animation-duration: 1s;    animation-name: showMain;    @keyframes showMain {      100%{        transform: rotateY(0);      }    }    div{      &:nth-child(1){        margin-top:60px;        margin-bottom:60px;      }      border-radius: 10px;      width:90%;      padding:20px;      font-size: 1.1em;      box-shadow: 0 0 5px #ddd;      opacity: 0;      //设置div慢慢显示      animation-name: showTextDiv;      animation-delay: 1.5s;      animation-fill-mode: forwards;      animation-duration: 3s;      @keyframes showTextDiv {        to{          opacity: 1;        }      }      &:nth-child(1){        background-color: rgba(142, 68, 173,0.4);      }      &:nth-child(2){        background-color: rgba(231, 76, 60,0.4);      }    }  }}
```

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215638.JPG" alt="多动画应用页面布局" style="zoom:50%;" />



### 按钮三点加载效果

```html
<body>    <a href="">提交</a></body>
```

```less
*{  margin:0;  padding:0;  font-size: 62.5%;}body{  display: flex;  justify-content: center;  align-items: center;  background-color: #28527a;  a{    text-decoration: none;    display: block;    color: white;    font-size: 3em;    padding:20px 50px;    border:1px solid;    background-color: #f0a500;    position: relative;    &::after{      content:'';      width:3px;      height:3px;      display:block;      position:absolute;      background-color: white;      top:50%;      left:65%;      animation-name: uploading;      animation-duration: 1s;      animation-iteration-count: infinite;      @keyframes uploading {        33%{          box-shadow: 6px 0px 0px white;        }        66%{          box-shadow: 6px 0px 0px white,12px 0px 0px white;        }      }    }  }}
```

### 后盾人首页仿写



## 变形动画


