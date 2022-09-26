# react初识

声明式、组件化

# 安装react-react-app (cra)

全局安装

```sh
$ npm install -g create-react-app
```

如果不想全局安装可以直接使用npx

```sh
$ npx create-react-app you-app 也可以实现相同的效果
```

创建一个项目

```sh
$ create-react-app you-app // 注意命名方式 不要使用驼峰
```

安装过程实际上会安装三个东西

+ react: react  的顶级库
+ react-dom: 因为react有很多运行环境，比如app端的react-native，我们要在web上运行就使用react-dom
+ react-script:包含运行和打包react应用程序的所有脚本及配置

运行项目

```sh
npm start
```

生成项目的目录结构如下

```sh
|-- node_modules                      所有依赖安装的目录
|-- package-lock.json                 锁定安装时的包的版本，保证团队的依赖能保证一致
|-- package.json                      
|-- public                            静态公共目录
|-- src                               开发用的源代码目录
```

常见问题：

- npm安装失败
  - 切换为npm镜像为淘宝镜像
  - 使用yarn，如果本来使用yarn还要失败，还得把yarn的源切换到国内
  - 如果还没有办法解决，请删除node_modules及package-lock.json然后重新执行`npm install命令`
  - 再不能解决就删除node_modules及package-lock.json的同时清除npm缓存`npm cache clean --force`之后再执行`npm install`命令

# 关于React

## React的起源和发展

React 起源于 Facebook 的内部项目，因为该公司对市场上所有 JavaScript MVC 框架，都不满意，就决定自己写一套，用来架设Instagram 的网站。做出来以后，发现这套东西很好用，就在2013年5月开源了。

## React与传统MVC的关系

轻量级的视图层库

React不是一个完整的MVC框架，最多可以认为是MVC中的V（View）,甚至React并不非常认可MVC开发模式；React构建页面UI的库。可以简单地理解为，React将页面分成了各个独立的小块，每个块就是组件，这些组件之间可以组合、嵌套，就成了我们的页面

## React高性能的体现：虚拟DOM

##### React高性能的原理：

在Web开发中我们总需要将变化的数据实时反应到UI上，这时就需要对DOM进行操作。而复杂或频繁的DOM操作通常是性能瓶颈产生的原因（如何进行高性能的复杂DOM操作通常是衡量一个前端开发人员技能的重要指标）。

React为此引入了虚拟DOM（Virtual DOM）的机制：在浏览器端用javascript实现了一套DOM API。基于React进行开发是所有的DOM构造都是通过虚拟DOM进行，每当数据变化时，React都会重新构建整个DOM树，然后React将当前整个DOM树和上次的DOM树进行对比，得到DOM结构的区别，然后仅仅将需要变化的部分进行实际的浏览器DOM更新。而且React等够批量的处理虚拟DOM的刷新，在一个事件循环（Event Loop）内的两次数据变化会被合并，例如你连续的先将节点内容从A-B,B-A，React会认为A变成B，然后又从B变成A  UI不发生任何变化，而如果通过手动控制，这种逻辑通常是极其复杂的。

尽管每一次都需要构造完整的虚拟DOM树，但是因为虚拟DOM是内存数据，性能是极高的，部而对实际DOM进行操作的仅仅是Diff分，因而能达到提高性能的目的。这样，在保证性能的同时，开发者将不再需要关注某个数据的变化如何更新到一个或多个具体的DOM元素，而只需要关心在任意一个数据状态下，整个界面是如何Render的。

##### React Fiber: 

在react 16之后发布的一种react 核心算法，**React Fiber是对核心算法的一次重新实现**(官网说法)。之前用的是diff算法。

在之前React中，更新过程是同步的，这可能会导致性能问题。

当React决定要加载或者更新组件树时，会做很多事，比如调用各个组件的生命周期函数，计算和比对Virtual DOM，最后更新DOM树，这整个过程是同步进行的，也就是说只要一个加载或者更新过程开始，中途不会中断。因为JavaScript单线程的特点，如果组件树很大的时候，每个同步任务耗时太长，就会出现卡顿。

React Fiber的方法其实很简单——分片。把一个耗时长的任务分成很多小片，每一个小片的运行时间很短，虽然总时间依然很长，但是在每个小片执行完之后，都给其他任务一个执行的机会，这样唯一的线程就不会被独占，其他任务依然有运行的机会。



## React的特点和优势

​	1、虚拟DOM

我们以前操作dom的方式是通过document.getElementById()的方式，这样的过程实际上是先去读取html的dom结构，将结构转换成变量，在进行操作

而reactjs定义了一套变量形式的dom模型，一切操作和换算直接在变量中，这样减少了操作真实dom，性能真实相当的高，和主流MVC框架有本质的区别，并不和dom打交道

​	2、组件系统

react最核心的思想是将页面中任何一个区域或者元素都可以看做一个组件component

那什么是组件呢？

组件指的就是同时包含课html、css、js、image元素的聚合体

使用react开发的核心就是将页面拆分成若干个组件，并且react一个组件中同时耦合了css、js、image，这种模式整个颠覆了过去的传统方式

​	3、单项数据流

其实react的核心内容就是数据绑定，所谓数据绑定指的是只要将服务端的数据和前端页面绑定好，开发者只关注实现业务就行了

​	4、JSX 语法

在vue中，我们使用render函数来构建组件的dom结构性能较高，因为省去了查找和编译模板的过程，但是在render中利用createElement创建结构的时候代码可读性较低，较为复杂，此时可以利用jsx语法在render中创建dom，解决这个问题但是前提是需要使用工具来编译jsx



# 编写第一个react应用程序

## 安装cra

```js
npm i create-react-app -g
```

## 启动项目

```js
create-react-app you-react // 注意不要使用驼峰
```

## 启动项目安装的三个包

+ react -- 顶级库

  核心语法包（解析jsx 以及react核心语法 生成虚拟dom 组件语法）

  jsx	xml in js （在js中写组件 包含 html标签）

+ react-dom

  将react-dom 生成的dom树，挂载带index.html上

+ react-script

  webpack的配置文件 （看不见） 包含运行和打包react应用程序的所有脚本及配置

react的入口函数是  src/index.js

react开发需要引入多个依赖文件：react.js、react.dom.js,分别又有开发版本和生产版本，create-react-app里已经帮我们把这些东西都安装好了。把通过CRA创建的工程目录下的src目录清空，然后在里面重新创建一个index.js。写入一下代码

```jsx
// 从react的包当中引入React。只要你要写React.js组件和使用jsx语法就必须引入React
import React from 'react'

// ReactDom 可以帮助我们把React组件渲染到页面上去，没有其它的作用了
import ReactDOM from 'react-dom'

//React里面有一个render方法，功能就是把组件渲染并构造DOM树，然后插入到页面上某个特定的元素

ReactDOM.render(
// 这里是jsx语法
    <h1>hello react</h1>
// 渲染到哪里
    document.getElementById('root')
)
```

# ES6class复习

### 基础用法

#### 类定义

```js
//匿名类
let Example = class {
    constructor(a) {
        this.a = a
    }
}
// 命名类
let Example = class Example {
    constructor(a) {
        this.a = a
    }
}
```

#### 类声明

```js
class Example {
    constructor(a) {
        this.a = a
    }
}
// 注意：不可重复声明
```

#### 注意要点

类定义不会被提升，这意味着，必须在访问前对类进行定义，否则就会报错

类中方法不需要function关键字

方法间不能加分号

#### 类的主体

属性

prototype

ES6中，prototype仍然存在，虽然可以直接自雷中定义方法，但是其实方法还是定义在prototype上的。覆盖方法/初始化时添加方法

```js
Example.prototype={
    //methods
}
```

##### 添加方法

```js
object.assign(Example,protitype,{
    //methods
})
// Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。第一个参数是目标对象，后面的参数都是源对象。
// 注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。实行的是浅拷贝，而不是深拷贝
```

##### 静态属性

class本身的属性，即直接定义在类内部的属性（class.propname）,不需要实例化。es6中规定，class内部只有静态方法，没有静态属性

<font color="red">类的静态属性实例无法调用，只能由类调用</font>

使用场景：当定义一个class的时候，里面有一些变量要参与运算某些方法的运算但是这些变量不能在外部被改变

```js
class Example {
    // 新提案
    static 属性名 = 值; // 内部定义
}
// 目前可行写法
Example.属性名 = 值; // 外部定义

```

##### 公共属性

```js
class Example {}
Example.prototype.a = 2
```

##### 返回对象

```js
class Test {
    constructor(){
        // 默认返回实例对象 this
    }
}
console.log(new Test() instanceof Test); // true
 
class Example {
    constructor(){
        // 指定返回对象
        return new Test();
    }
}
console.log(new Example() instanceof Example); // false
```



##### 定义属性的方法

​	方法1

```js
class Person {
    // 方法1
    name = "小明",
    age = "19
	// 属性挂载到实例上
}
const p = new Person('小明', 20)
console.log(p)
```

![image-20210106222855731](G:\md\第三阶段\react\react个人笔记.assets\image-20210106222855731.png)

​	方法2

```js
class Person {
    constructor (name, age) {
        this.name = name;
        this.age = age
        // this指向实例，即属性挂载到实例上
    }
}
const p = new Person('小明'，11)
// new 实例化的时候后面的参数会传到constructor里
```

![image-20210106223240710](G:\md\第三阶段\react\react个人笔记.assets\image-20210106223240710.png)

注：`constructor()`方法是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法。一个类必须有`constructor()`方法，如果没有显式定义，一个空的`constructor()`方法会被默认添加。constructor()里面的this指向实例

##### 定义类的方法

```js
class Person {
    static sum(a, b) { // 静态方法
        console.log(a+b)
    }
    constructor() {
        this.sum = (a,b) => { // 方法是挂载到实例上的
            console.log(a+b)
        }
    }
    act () {
        // 这里的this指向不确定，方法内部的 this 永远指向的是调用这个方法的对象
        // 这种写法方法是自动挂载到原型上
    }
    act2 = () => {
        // 这种方式相当于定义一个act2属性 ，里面的函数相当于值 赋给属性
        // 这时方法是挂载到实例上的
    }
}
```

##### 类的继承

```js
class Anima {
    constructor (name) {
        this.name = name;
    }
    act(){
        console.log(this.name+"chidongxi")
    }
}
class Cat extends Anima {
    constructor(name,ani){
        super(ani) // 这里super指向父类的constructor
        this.name = name  //this指向实例
    }
    .act2(){
        console.log(this.name+"爱吃鱼")
        console.log(super()) // 这里super指向类本身
    }
}
const p = new Cat('猫','dogwu')
/*
super()
	子类constructor方法中必须有super，且必须出现在this之前
	super自动调用了call() 改变了父类的constructor的this指向，将其指向实例
	调用父类构造函数，只能出现在子类的构造函数
	调用父类方法，super作为对象，在普通方法中，指向父类的原型对象，在静态方法中，指向父类
	向父类传参 在super(这里传参)
*/
```



# 元素与组件

如果代码多了之后，不可能一直在render但方法里写jsx，所以就需要把里面额代码提出了，定义一个变量，就像这样

```jsx
import React from 'react'
import ReactDOM from 'react-dom'
// 定义变量
const app = <h1>hello react</h1>
ReactDOM.render(
	app,
    document.getElementById('root')
)
```

## 函数式组件

由于元素没有办法传递参数，所以我们就需要把之前定义的变量改为一个方法，让这个方法去return一个元素

```JSX
import React from 'react'
import ReactDOM from 'react-dom'

// 注意注意注意： jsx中 需要些js 只需要一个大括号 jsx中的 {} 不管写什么语法 需要有返回值 一般可以写 表达式 想要写 复杂的 分支结构（if else if）需要写在函数内部，且函数 不管走哪个分支，要有返回值
const app = (props) => <h1>hello{props.name}</h1>

ReactDOM.render(
app({
    name: 'react'
}),
    document.getElementById('root')
)
```

这里我们定义的方法实际上也是react定义组件的第一种方式-定义函数式组件，这也是无状态组件。但是这种写法不符合react的jsx风格，更好的方式是使用一下方式进行改造

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

const App = (props) => {
    return <h1>hello{props.name}</h1>
}
ReactDOM.render(
// React组件的调用方式
    <App name="react" />
    document.getElementById('root')
)
```

这样一个完整的函数式组件就定义好了。但是要**注意！注意！注意！**<font color='red'>组件名必须大写</font>，否则报错

特点：1，组件不会被实例化，整体渲染性能得到提升

​	2，组件不能访问this对象

​	3，组件无法访问生命周期方法，无state（后面hooks解决）

​	4，无状态组件只能访问输入的props，无副作用

## class组件

ES6的加入让javaScript直接支持使用class来定义一个类，react的第二种创建组件的方式就是使用类的继承，ES6 class是目前官方推荐的使用方式，它使用了ES6标准语法来构建，看一下代码

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
    render () {
        return (
        // 注意这里得用this.props.name.必须用this.props
            // this指向的是实例
            <h1>hello{this.props.name}</h1>
        )
    }
}
ReactDOM.render(
	<App name='react' /> // 当我们以标签的形式使用组件的时候相当于 new App() 然后调用了里面的render方法 使用一次 就new一次
    document.getElementById('root')
)
```

运行结果和之前一样，因为JS里没有真正的class，这个class只是一个语法糖，但二者的底层运行机制不一样

+ 函数式组件是直接调用

+ es6 class 组件其实就是一个构造器，每次使用组件都相当于在实例化

  例:

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  render () {
    return (
  		<h1>欢迎进入{this.props.name}的世界</h1>
  	)
  }
}

const app = new App({
  name: 'react'
}).render()

ReactDOM.render(
  app,
  document.getElementById('root')
)
// 使用一次 new App 然后调用实例的render方法
```

## 函数式组件和class组件的区别

```js
1、语法上
	函数式组件时一个纯函数，它接受一个props对象返回一个react元素，而类组件需要react.Component并且创建render函数返回react元素，这会需要更多代码
2、状态管理
	因为函数组件时纯函数，所以不能在组件中使用setState(),这也是为什么把函数组价称作无状态组件
	如果需要在组件中使用state，可以选择创建一个类组件或者将state提升到父组件中，然后通过props对象传递到子组件
3、生命周期钩子
	在函数式组件中不能使用生命周期钩子，原因和不能使用state一样，所有的生命周期钩子都来自于继承的React.Component中
	因此想使用生命周期钩子，就需要使用类
    注意： 在react16.8版本中添加了 hooks 使得我们可以在函数组件中使用useState钩子去管理state，使用useEffect钩子去使用生命周期，因此2,3两点不算区别点
4、调用方式
	函数式组件重新渲染将重新调用组件方法返回新的react元素，
    类组件重新渲染将new一个新的组件实例，然后调用render类方法返回react元素，这也说明为什么类组件中this是可变的
5、获取渲染时的值
	react中的props是不可改变的，但是this是可变的，而且是一直可变的，这也是类组件的目的，react自身会随着时间（操作）的推移对this进行修改，以便可以在render函数或生命周期中读取新的版本
    如果组件中在请求重新渲染时(这次的渲染是异步的)，this指向被改变了，那么重新渲染的this.props.xxx中的this是被改变后的的this，这样当回调超时的话this.props会丢失真正的props
https://segmentfault.com/a/1190000020861150
```



## 更老的一种方法

在16以前的版本还支持这样创建组件, 但现在的项目基本上不用

```jsx
React.createClass({
  render () {
    return (
      <div>{this.props.xxx}</div>
  	)
  }
})
```



## 组件的组合、嵌套

将一个组件渲染到某个节点里的时候，会将这个节点里原内容覆盖

组件嵌套的方式就是将子组件写到父组件的模板中去，且react没有Vue中的内容分发机制（slot-插槽），所以我们在一个组件的模板中只能看到父子关系

```jsx
// 从react的包当中引入react 和 react.js 的组件父类 Component
// 还引入了React.js里的一种特殊组件 Fragment
import React, { Component, Fragment } from 'react'
import ReactDOM from 'react-dom'
class Title extends Component {
    render () {
        renturn (
        	<h1>hello</h1>
        )
    }
}
class Content extends Component {
    render () {
        return (
        	<p>react.js是构建UI的库</p>
        )
    }
}
/*
由于每个React组件只能有一个根节点，所以要渲染多个组件的时候，需要最外层包一个容器，如果使用div，会生成多余的一层dom
class App extends Component {
  render () {
	return (
		<div>
			<Title />
			<Content />
		</div>
	)
  }
}
*/
// 如果不想生成多余的一层dom可以使用react提供的Fragment组件在最外层进行包裹
class App extends Component {
    render () {
        return () {
            <Fragment>
              <Title />
              <Content />
            </Fragment>
        }
    }
}
ReactDOM.render(
  <App/>,
  document.getElementById('root')
)
```

<font color="red">注意!: 不管什么类型的组件 内容 必须有一个根标签 组件名首字母必须大写</font>

# JSX原理

要明白JSX原理，需要先明白如何用JavaScript对象来表现一个DOM元素的结构

看下面的DOM结构

```jsx
<div class='app' id='appRoot'>
  <h1 class='title'>hello react</h1>
  <p>
  	react.js是一个帮你构建页面UI库  
  </p>
</div>
```

上面这个HTML所有的信息我们都可以用JavaScript对象来表示

jsx中class必须要写className

```jsx
{
    tag: 'div',
    attr: {className: 'app', id:'appRoot'}
    children: [
        {
            tag: 'h1',
    		attr: {className: 'title'}
            children:['hello react']
        },
        {
            tag: 'p',
            attr: null,
            children: ['react.js 是一个帮你构建页面UI库']
        }
    ]
}
// 当我们在js中写标签时，react会自动调用 React.createElement() 去生成虚拟dom 其实最后编译运行时还是在js运行的js
```

但是用JavaScript写起来太长了，结构看起来有不清晰，用HTML的方式写起来就方便很多了。

于是React.js就把JavaScript的语法扩展了一下，让JavaScript语言能够支持这种直接在JavaScript代码里面编写类似HTML标签结构的语法，这样写起来就方便很多。编译的过程会把类似HTML的JSX结构转换成JavaScript的对象结构。

`React.createElement` 会构建一个 JavaScript 对象来描述你 HTML 结构的信息，包括标签名、属性、还有子元素等, 语法为

```jsx
React.createElement(
  type,
  [props],
  [...children]
)
React.createElememt('div',{
    id: 'box',
    className: 'conta'
	},
    [
    	React.createElement()
	]
                   )
```

所谓的 JSX 其实就是 JavaScript 对象，所以使用 React 和 JSX 的时候一定要经过编译的过程:

> JSX —使用react构造组件，bable进行编译—> JavaScript对象 — `ReactDOM.render()`—>DOM元素 —>插入页面

# 组件中的DOM样式

+ 行内样式（内联）

想给虚拟dom添加行内样式，需要使用表达式传入样式对象的方式来实现：

```jsx
// 样式是一个对象
// 注意这里的两个括号，第一个表我们要在JSX里插入JS了，第二个是对象的括号
<p style={{color:'red', fontSize:'14px'}}>hello react</p>
```

行内样式需要写入一个样式对象，而这个样式对象的位置可以放在render函数里，组件原型上，外链js文件中等

+ 使用class

```jsx
<p className="hello" style = {this.style}>Hello world</p>
```

+ css-in-js

`styled-components`是针对React写的一套css-in-js框架，简单来说就是在js中写css    [npm链接](<https://www.npmjs.com/package/styled-components>)

```jsx
// 简单使用
import styled from 'styled-components'
// 渲染成Title组件（react），渲染成h1标签，且有引号样式
const Title = styled.h1`
	width:200px;
	height:200px;
	color:red;
`
// 使用
<Title>内容</Title>
// 传递props
const Title = tyled.h1`
	width:200px;
	height:200px;
	color:red;
	p{
	  font-size:12px;
	  span{
		xx
	  }
     }
	background: ${props => props.bgc?props.bgc:'pink'}
`
<Tilte bgc='red'>内容</Title>
// 继承
const Button = styled.button`
	color:red;
	font-size:1em;
	margin:1em;
	padding:0.25em 1em;
	border:2px solid #333;
	border-radius:3px
`;
	// 有另一个按钮 具有Button所有的样式
const Qf_btn = styled(Button)`
	background:yellow;
`
```

# 组件的数据挂载方式

## 属性（props）

props是正常的外部传入，组件内部也可以通过一些方式来初始化的设置，属性不能被组价自己更改，但是可以通过父组件主动重新渲染的方式传入新的props

属性是描述性质、特点的，组件自己不能随意更改

总的来说，在使用组件的时候，可以把参数（<font color="red">可以是属性或者方法</font>>）放在标签的属性中，所有的属性都会作为组件props对象的键值。通过箭头函数创建的组件，需要通过函数的参数来接受props



```jsx
import React, { Component, Fragment } from 'react'
import ReactDOM from 'react-dom'

class Title extends Component {
  render () {
    return (
  		<h1>欢迎进入{this.props.name}的世界</h1>
  	)
  }
}

const Content = (props) => {
  return (
    <p>{props.name}是一个构建UI的库</p>
  )
}

class App extends Component {
  render () {
    return (
  		<Fragment>
      	<Title name="React" />
        <Content name="React.js" />
      </Fragment>
  	)
  }
}

ReactDOM.render(
	<App/>,
  document.getElementById('root')
)
```

### props验证 props-types

React其实是为了构建大型应用程序而生, 在一个大型应用中，根本不知道别人使用你写的组件的时候会传入什么样的参数，有可能会造成应用程序运行不了，但是不报错。为了解决这个问题，React提供了一种机制，让写组件的人可以给组件的```props```设定参数检查，需要安装和使用[prop-types](<https://www.npmjs.com/package/prop-types>):

```sh
// 安装
$ npm i prop-types -S

import PrpoTypes from 'prop-types'
// 定义静态属性（外部定义）
MyComponent.propTypes = { // `属性名必须是PropTypes`
  // 必须是 数组
  optionalArray: PropTypes.array,
  // 布尔值
  optionalBool: PropTypes.bool,
  // 函数
  optionalFunc: PropTypes.func,
  // 数字
  optionalNumber: PropTypes.number,
  // 对象
  optionalObject: PropTypes.object,
  // 字符串
  optionalString: PropTypes.string,
  // symbol
  optionalSymbol: PropTypes.symbol,
  // 必须是一个组件.
  optionalElement: PropTypes.element,
  // 必须是 Message这个构造函数的实例
  optionalMessage: PropTypes.instanceOf(Message),
  // 必须是 数组中多个值中的一个
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),
  // 必须 以下数据类型中的某个
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),
  // 必须是数组 且 数组的 元素 必须是 指定类型
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),
  // 必须是对象，且 对象属性的值的类型 必须是 指定类型
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),
  // 必须是对象，且对象必须有定义的这些属性，且属性的值 是指定值类型
  optionalObjectWithShape: PropTypes.shape({
    optionalProperty: PropTypes.string,
    requiredProperty: PropTypes.number.isRequired
  }),
}
//内部定义
class MyComponent extends Component {
	static propTypes = {
	
	}
}
```

### props默认值

定义一个静态属性 defaultProps 在这里定义默认值

```jsx
import React, { Component, Fragment } from 'react'
import ReactDOM from 'react-dom'

class Title extends Component {
  // 使用类创建的组件，直接在这里写static方法，创建defaultProps
  static defaultProps = {
    name: 'React'
  }
  render () {
    return (
  		<h1>欢迎进入{this.props.name}的世界</h1>
  	)
  }
}
// 也可外部定义 类名.defaultProps = {}
const Content = (props) => {
  return (
    <p>{props.name}是一个构建UI的库</p>
  )
}

// 使用箭头函数创建的组件，需要在这个组件上直接写defaultProps属性
Content.defaultProps = {
  name: 'React.js'
}

class App extends Component {
  render () {
    return (
  		<Fragment>
        {/* 由于设置了defaultProps， 不传props也能正常运行，如果传递了就会覆盖defaultProps的值 */}
      	<Title />
        <Content />
      </Fragment>
  	)
  }
}

ReactDOM.render(
	<App/>,
  document.getElementById('root')
)

```

### props的children

使用组件的时候可以嵌套，要在自定义组件使用嵌套，就需要使用props.children

指props中的内容，类似于vue的插槽（slot）

```jsx
import React, { Component } from 'react'
import reactDom from 'react-dom'
class App extends Component {
  render () {
    return (
      <div>
          {console.log(this.props)}
      </div>
      
    )
  }
}
reactDom.render(
  <App name='react' age={12}>
    <h1>这是组价的props.children</h1>
  </App>, // new App() 然后调用render
  // App({
  //   name: 'react'
  // }),
  document.querySelector("#root")
)
```

![image-20210108135752061](G:\md\第三阶段\react\react个人笔记.assets\image-20210108135752061.png)

## 状态（state）

状态就是组件描述某种显示情况的数据，由组件自己设置和更改，使用状态的目的就是为了在不同状态下使组件的显示不同  （就是存储当前组件的内部数据） 

类似vue中的data

### 定义state

+ 直接定义（不推荐）

```jsx
class App extends Component {
  state = {  // 此时state是定义在实例上
    name: 'React',
    isLiked: false
  }
  render () {
    return (
      <div>
        <h1>欢迎来到{this.state.name}的世界</h1>
        <button>
          {
            this.state.isLiked ? '❤️取消' : '🖤收藏'
          }
        </button>
      </div>
  	)
  }
}
```

+ constructor中定义（推荐）

```jsx
class App extends Component {
  constructor() {
    super()
    this.state = {
      name: 'React',
      isLiked: false
    }
  }
  render () {
    return (
  		<div>
        <h1>欢迎来到{this.state.name}的世界</h1>
        <button>
          {
            this.state.isLiked ? '❤️取消' : '🖤收藏'
          }
        </button>
      </div>
  	)
  }
}
```

`this.props`和`this.state`是纯js对象，在vue中，data属性是利用`object.defineProperty`处理过的，更改data的数据会触发数据的`getter`和`setter`，但是React中没有做这样的处理，如果直接更改的话，react是无法得知的，所有需要使用特殊的更改状态的方法`setState`。

### setState



react不是MVVM框架 所以不能直接修改state 应该使用 setState方法

setState方法是react.Component上的

注意：setState调用 传参对象和sate  执行的是合并的操作

（扩：es6合并对象的方法var c = {...a,...b}   var c = object.assign(a,b)）



setState有两个参数

第一个参数可以是对象，也可以是方法，我们把这个参数叫做updater

+ 参数是对象

```jsx
this.setState({
  isLiked: !this.state.isLiked
})
```

+ 参数是方法

```js
this.setState((prevState, props) => {
  return {
    isLiked: !prevState.isLiked
  }
})
# 注意这个方法接受的两个参数，第一个是上一次的state，第二个是props
```

setState又是是同步有时是异步的（可以全当异步对待），所以想要获取最新的state，没有办法获取，就有了第二个参数，这是一个可选的回调函数，回调函数在state改变时触发（）

同步情况：react捕获不到的操作 调用setState修改状态

1、定时器中 修改state  2、js原生事件中  修改state   （因为捕获不到所以react 只能同步修改）

异步情况：react能够捕获到的操作中 setState修改数据 （在程序中异步效率更高，因为不会阻塞进程）

1、react组件的生命周期钩子函数中  2、react合成事件中

总结：不记得什么时候 同步什么时候异步，获取最新的改变后的值  就可以都在setState的第二个参数中获取



### 属性vs状态 props vs state

相似点：都是纯js对象，都会触发render更新，都具有确定性（状态、属性相同，结果相同）

不同点：

1、属性能从父组件获取，状态不能

2、属性可以由父组件修改、状态不能

3、属性能在内部设置默认值、状态也可以

4、属性不可以在组件内部修改，否则立即报错，状态要改使用setState（props单向数据流）

5、属性能设置子组件初始值，状态不可以

6、属性可以修改子组件的值，状态不可以

`state` 的主要作用是用于组件保存、控制、修改自己的可变状态。`state` 在组件内部初始化，可以被组件自身修改，而外部不能访问也不能修改。你可以认为 `state` 是一个局部的、只能被组件自身控制的数据源。`state` 中状态可以通过 `this.setState`方法进行更新，`setState` 会导致组件的重新渲染。

`props` 的主要作用是让使用该组件的父组件可以传入参数来配置该组件。它是外部传进来的配置参数，组件内部无法控制也无法修改。除非外部组件主动传入新的 `props`，否则组件的 `props` 永远保持不变。



<font color="red">注意:</font>

1, 函数式组件没有state       2, state尽量少用  使用UI组件  尽量保证没有state，都是props

组件分类: 有状态组件   无状态组件  （区别有无state）



逻辑组件:  （容器组件  有state  一般是父组件）

Ui组件：（没有state）



受控组件（只有props 没有state 所以外部props的值改变了 ，组件立即改变视图）

非受控组件（容器组件）

半受控组件（）

React组件的数据渲染是否被调用者传递的`props`完全控制，控制则为受控组件，否则非受控组件。



### 状态提升

如果有多个组件共享一个数据，把这个数据放到共同的父组件中来管理



# 渲染数据

+ 条件渲染

```jsx
{
  condition ? '❤️取消' : '🖤收藏'
}
{
    condition && "xxxxx"  // 短路
}
```

+ 列表渲染

```jsx
// 数据
const people = [{
  id: 1,
  name: 'Leo',
  age: 35
}, {
  id: 2,
  name: 'XiaoMing',
  age: 16
}]
// 渲染列表
{
  people.map(person => {
    return (
      <dl key={person.id}>
        <dt>{person.name}</dt>
        <dd>age: {person.age}</dd>
      </dl>
    )
  })
}
```

React的高效依赖于所谓的 Virtual-DOM，尽量不碰 DOM。对于列表元素来说会有一个问题：元素可能会在一个列表中改变位置。要实现这个操作，只需要交换一下 DOM 位置就行了，但是React并不知道其实我们只是改变了元素的位置，所以它会重新渲染后面两个元素（再执行 Virtual-DOM ），这样会大大增加 DOM 操作。但如果给每个元素加上唯一的标识，React 就可以知道这两个元素只是交换了位置，这个标识就是```key```，这个 `key` 必须是每个元素唯一的标识

+ dangerouslySetInnerHTML

对于富文本创建的内容，后台拿到的数据是这样的：

```js
content = "<p>React.js是一个构建UI的库</p>"
```

处于安全的原因，React当中所有表达式的内容会被转义，如果直接输入，标签会被当成文本。这时候就需要使用`dangerouslySetInnerHTML`属性，它允许我们动态设置`innerHTML`

```jsx
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

class App extends Component {
  constructor() {
    super()
    this.state = {
      content : "<p>React.js是一个构建UI的库</p>"
    }
  }
  render () {
    return (
  		<div
        // 注意这里是两个下下划线 __html
        dangerouslySetInnerHTML={{__html: this.state.content}}
      />
  	)
  }
}
ReactDOM.render(
	<App/>,
  document.getElementById('root')
)
```



# 事件处理

## 事件绑定

采用on+事件名的方式来绑定一个事件，注意，这里和原生的事件是有区别的，原生的事件全是小写onclick

React采用的事件是驼峰onClick，React的事件并不是原生事件

## 事件handler的写法

```jsx
//1行内 直接写箭头函数 不推荐

{
  render(){
    return (
      <button onClick = { ()=>{
        this.setState({
          isLike: !this.state.isLike
        })
      } }>按钮</button>
    )
  }
}
// 方法写在外面  行内通过bind 改变this指向

{
  render(){
    return (
      <button onClick = { this.handleClick.bind(this) }>按钮</button>
    )
  }
  handleClick(){
    this.setState(...)
  }
}

// 方法写在外部 在constructor 改变this指向  推荐写法

{
  constructor(){
    super();
    this.handleClick = this.handleClick.bind(this)
  }
  render(){
    return (
      <button onClick = { this.handleClick }>按钮</button>
    )
  }
  handleClick(){
    this.setState(...)
  }
}

// 方法写在外部  通过箭头函数定义  推荐写法
{
  render(){
    return (
      <button onClick = { this.handleClick }>按钮</button>
    )
  }
  handleClick = ()=>{
    this.setState(...)
  }
}
```



+ 直接在render里写行内的箭头函数（不推荐）

+ 在组件内使用箭头函数定义一个方法（推荐）

+ 直接在组件内定义一个非箭头函数的方法，然后在render里直接使用onClick={this.handleClick.bind(this)} (不推荐)
+ 直接在组件内定义一个非箭头函数的方法，然后在contructor里bind（this）（推荐）

### event对象

和普通浏览器一样，事件handler会被自动传入一个event对象，这个对象和普通的浏览器event对象所抱恨的方法和属性都基本一致。不同的是React中的event对象并不是浏览器提供的，而是它自己内部所构建的。它同样句有event.stopPropagation、event.preventDefault 这种常用的方法

### 事件的参数传递

+ 在render里调用方法的地方外面包一层箭头函数
+ 在render里通过this.hendleEvent.bind(this, 参数) 这样的方式来传递
+ 通过event传递
+ 还有的是做一个子组件，在父组件中定义方法，通过props传递到子组件中，然后在子组件通过this.props.method来调用

```jsx
{
  constrcutor(){
    super();
    this.state = {
      inputTxt: ''
    }
  }
  render(){
    return (
      <div>
        <input value = {this.state.inputTxt} onChange={this.handleInput}/>
      </div>
    )
  }
 # 这种推荐
  handleInput = (e)=>{
    const target = e.target;
    this.setState({
      inputTxt: target.value
    })
  }
}
```

<font color="red">状态提升 * 将数据放到父组件中进行管理</font>

## 事件绑定 并传参

行内新增箭头函数作为事件函数，在事件函数函数体中调用方法并传参

```jsx
{
  render(){
    return (
      <div>
        <button onClick = {()=>{ this.handleClick(1) }}>按钮</button>
      </div>
    )
  }
  handleClick = (num)=>{
    console.log(num)
  }
}
```

# 组件生命周期

React中组件也有生命周期，也就是说也有很多钩子函数供我们使用, 组件的生命周期，我们会分为四个阶段，初始化、运行中、销毁、错误处理(16.3之后)

![image-20210119103700897](G:\md\第三阶段\react\react个人笔记.assets\image-20210119103700897.png)

## 初始化

在组件初始化阶段会执行

1、constructor( )    //  当我们<组件>的时候 相当于  New  了一个一个类  自动调用constructor

2、static getDerivedStateFromProps()  //  通弄过props获取派生state  类似于 vue中的计算属性

3、componentWillMount() / UNSAFE_componentWillMount() // 将要被废弃

4、render( )

5、componentDidMount()   // 请求函数 和获取dom的操作 都在componentDidMount完成

## 更新阶段

props或state 的改变可能会引起组件的更新，组件重新渲染的过程会调用一下方法:

1、componentWillReceiveProps() / UNSAFE_componentWillReceiveProps()

2、static getDerivedStateFromProps（）

3、shouldComponentUpdate( ) // 性能优化

4、componentWillUpdate() / UNSAFE_componentWillUpdate() 

5、render（）

6、getSnapshotBeforeUpdate() 

7、componentDidUpdate()   // 可以获取更新的dom

## 卸载阶段

componentWillUnmount()  // 清除定时器  销毁全局事件绑定

## 错误处理

componentDidCatch()

## 各生命周期详解

##### 1.constructor(props)

React组件的构造函数在挂载之前被调用。在实现`React.Component`构造函数时，需要先在添加其他内容前，调用`super(props)`，用来将父组件传来的`props`绑定到这个类中，使用`this.props`将会得到。

官方建议不要在`constructor`引入任何具有副作用和订阅功能的代码，这些应当使用`componentDidMount()`。

`constructor`中应当做些初始化的动作，如：初始化`state`，将事件处理函数绑定到类实例上，但也不要使用`setState()`。如果没有必要初始化state或绑定方法，则不需要构造`constructor`，或者把这个组件换成纯函数写法。

当然也可以利用`props`初始化`state`，在之后修改`state`不会对`props`造成任何修改，但仍然建议大家提升状态到父组件中，或使用`redux`统一进行状态管理。

```jsx
constructor(props) {
  super(props);
  this.state = {
    isLiked: props.isLiked
  };
}
```

##### 2.<font color="red">static getDerivedStateFromProps(nextProps, prevState)</font>

`getDerivedStateFromProps` 是react16.3之后新增，在组件实例化后，和接受新的`props`后被调用。他必须返回一个对象来更新状态，或者返回null表示新的props不需要任何state的更新。

如果是由于父组件的`props`更改，所带来的重新渲染，也会触发此方法。

调用`steState()`不会触发`getDerivedStateFromProps()`。

之前这里都是使用`constructor`+`componentWillRecieveProps`完成相同的功能的

##### 3. componentWillMount() / UNSAFE_componentWillMount()

`componentWillMount()`将在React未来版本(官方说法 17.0)中被弃用。`UNSAFE_componentWillMount()`在组件挂载前被调用，在这个方法中调用`setState()`不会起作用，是由于他在`render()`前被调用。

为了避免副作用和其他的订阅，官方都建议使用`componentDidMount()`代替。这个方法是用于在服务器渲染上的唯一方法。这个方法因为是在渲染之前被调用，也是惟一一个可以直接同步修改state的地方。

##### 4.render()

render()方法是必需的。当他被调用时，他将计算`this.props`和`this.state`，并返回以下一种类型： 

1. React元素。通过jsx创建，既可以是dom元素，也可以是用户自定义的组件。 
2. 字符串或数字。他们将会以文本节点形式渲染到dom中。 
3. Portals。react 16版本中提出的新的解决方案，可以使组件脱离父组件层级直接挂载在DOM树的任何位置。 
4. null，什么也不渲染 
5. 布尔值。也是什么都不渲染。

当返回`null`,`false`,`ReactDOM.findDOMNode(this)`将会返回null，什么都不会渲染。

`render()`方法必须是一个纯函数，他不应该改变`state`，也不能直接和浏览器进行交互，应该将事件放在其他生命周期函数中。 
如果`shouldComponentUpdate()`返回`false`，`render()`不会被调用。

##### <font color="red">5. componentDidMount</font>

`componentDidMount`在组件被装配后立即调用。初始化使得DOM节点应该进行到这里。

**通常在这里进行ajax请求**

如果要初始化第三方的dom库，也在这里进行初始化。只有到这里才能获取到真实的dom.

##### 6.componentWillReceiveProps()/UNSAFE_componentWillReceiveProps(nextProps)

官方建议使用`getDerivedStateFromProps`函数代替`componentWillReceiveProps`。当组件挂载后，接收到新的`props`后会被调用。如果需要更新`state`来响应`props`的更改，则可以进行`this.props`和`nextProps`的比较，并在此方法中使用`this.setState()`。

如果父组件会让这个组件重新渲染，即使`props`没有改变，也会调用这个方法。

React不会在组件初始化props时调用这个方法。调用`this.setState`也不会触发。

##### <font color="red">7.shouldComponentUpdate(nextProps, nextState)</font>

调用`shouldComponentUpdate`使React知道，组件的输出是否受`state`和`props`的影响。默认每个状态的更改都会重新渲染，大多数情况下应该保持这个默认行为。

在渲染新的`props`或`state`前，`shouldComponentUpdate`会被调用。默认为`true`。这个方法不会在初始化时被调用，也不会在`forceUpdate()`时被调用。返回`false`不会阻止子组件在`state`更改时重新渲染。

如果`shouldComponentUpdate()`返回`false`，`componentWillUpdate`,`render`和`componentDidUpdate`不会被调用。

> 官方并不建议在`shouldComponentUpdate()`中进行深度查询或使用`JSON.stringify()`，他效率非常低，并且损伤性能。

```jsx
// shouldComponentUpdate 手动控制刷新
{
  render(){

  }
  shouldComponentUpdate(nextProps,nextState){
    return nextProps.xx !== this.props.xxx
  }
}
// 注意 无法做 深层次比较
```



##### 8.UNSAFE_componentWillUpdate(nextProps, nextState)

在渲染新的`state`或`props`时，`UNSAFE_componentWillUpdate`会被调用，将此作为在更新发生之前进行准备的机会。这个方法不会在初始化时被调用。

*不能在这里使用this.setState()*，也不能做会触发视图更新的操作。如果需要更新`state`或`props`，调用`getDerivedStateFromProps`。

##### 9.getSnapshotBeforeUpdate()

在react `render()`后的输出被渲染到DOM之前被调用。它使您的组件能够在它们被潜在更改之前捕获当前值（如滚动位置）。这个生命周期返回的任何值都将作为参数传递给componentDidUpdate（）。

##### <font color="red">10.componentDidUpdate(prevProps, prevState, snapshot)</font>

在更新发生后立即调用`componentDidUpdate()`。此方法不用于初始渲染。当组件更新时，将此作为一个机会来操作DOM。只要您将当前的props与以前的props进行比较（例如，如果props没有改变，则可能不需要网络请求），这也是做网络请求的好地方。

如果组件实现`getSnapshotBeforeUpdate()`生命周期，则它返回的值将作为第三个“快照”参数传递给`componentDidUpdate()`。否则，这个参数是`undefined`。

##### <font color="red">11.componentWillUnmount()</font>

在组件被卸载并销毁之前立即被调用。在此方法中执行任何必要的清理，例如使定时器无效，取消网络请求或清理在`componentDidMount`中创建的任何监听。

##### 12.componentDidCatch(error, info)

错误边界是React组件，可以在其子组件树中的任何位置捕获JavaScript错误，记录这些错误并显示回退UI，而不是崩溃的组件树。错误边界在渲染期间，生命周期方法以及整个树下的构造函数中捕获错误。

如果类组件定义了此生命周期方法，则它将成错误边界。在它中调用`setState()`可以让你在下面的树中捕获未处理的JavaScript错误，并显示一个后备UI。只能使用错误边界从意外异常中恢复; 不要试图将它们用于控制流程。

错误边界只会捕获树中下面组件中的错误。错误边界本身不能捕获错误。

## PureComponent 

`PureComponnet`里如果接收到的新属性或者是更改后的状态和原属性、原状态相同的话，就不会去重新render了
在里面也可以使用`shouldComponentUpdate`，而且。是否重新渲染以`shouldComponentUpdate`的返回值为最终的决定因素。

```jsx
import React, { PureComponent } from 'react'

class YourComponent extends PureComponent {
  ……
}
```

##  组件性能优化

react 只要父组件 更新了，默认所有的后代组件都会更新

```
// shouldComponentUpdate 手动控制刷新
{
  render(){

  }
  shouldComponentUpdate(nextProps,nextState){
    return nextProps.xx !== this.props.xxx
  }
}
// 注意 无法做 深层次比较

// 使用 PureComponent
// 对props和state做浅层比较
import React,{PureComponent} from 'react'
class xx extends PureComponent{

}
```

## ref