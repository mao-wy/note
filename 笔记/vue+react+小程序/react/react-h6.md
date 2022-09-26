## 安装 cra

```js
npm i create-react-app -g

// 启动项目
create-react-app you-project // 注意不要使用驼峰

```

生成 入口函数 是 src/index.js

## 启动项目安装的三个包

- react  
  核心语法包 （解析 jsx 以及 react 核心语法 生成虚拟 dom 组件语法）
  jsx xml in js (在 js 中写组件 包含 html 标签)
- react-dom
  将 react 生成 dom 树，挂载到 index.html 上
- react-scripts
  webpack 的 配置文件（看不见）

## jsx 语法

是指 可以直接 在 js 中 写 xml（组件、自定义标签），react 库 会自动编译成虚拟 dom 对象
注意注意注意：
jsx 中 需要些 js 只需要一个大括号
jsx 中的 {} 不管写什么语法 需要有返回值
一般可以写 表达式 想要写 复杂的 分支结构（if else if）需要写在函数内部，且函数 不管走哪个分支，要有返回值

## 组件

- 函数式组件

```js
const App = (props) => {
  return <div>{props.title}</div>;
};

//使用时

reactDom.render(<App title="值" />, document.querySelector("#root"));
```

注意：
不管是什么类型的组件 内容 必须有一个根标签
组件名首字母必须大写

## es6class

## class 组件

```js
import React, { Component } from "react";

class App extends Component {
  render() {
    return <div>我是class组件</div>;
  }
}
// 使用时
<App />; // 使用一次 new App 然后调用实例的render方法
```

## jsx

xml in js 可以在 js 中写标签 这种语法教 jsx react 特有

jsx 中 class 必须要写 className

```
<div id="box" class="container">
  <p class="p">你是一个p</p>
  <span>span</span>
  文本内容
</div>

{
  tag: 'div',
  attrs: {
    id: 'box',
    className: 'container'
  },
  children: [
    {
      tag: 'p',
      attrs: {
        className:'p'
      },
      children: [
        '你是一个p'
      ]
    },
    {
      tag: 'span',
      attrs:{},
      children: ['span']
    },
    '文本内容'
  ]
}

当我们在js中 写 标签时 ，react 会自动调用
React.createElement()
去生成虚拟dom
其实 最后编译 运行时 还是在js运行的js
```

## 组件中的样式

- 内联
  要求 style 属性时一个 对象

```js
<div style={{ width: 200 + 300, height: 400, backgroundColor: "red" }} />
```

- 引入外部的样式

```js
import "./style.css";
// cra启动的项目 默认配置的时 scss
```

- style in js 万物皆组件
  styled-components

```js
// 简单使用
import styled from 'styled-components'
// 渲染成 Title 组件（react），渲染成h1标签，且有引号中的样式
const Title = styled.h1`
  width: 200px;
  height: 200px;
  color:red;
`
// 使用
<Title>内容</Title>
// 传递props
const Title = styled.h1`
  width: 200px;
  height: 200px;
  color:red;
  p{
    font-size: 12px;
    span{
      xx
    }
  }
  background: ${props => props.bgc?props.bgc:'pink'};
`
<Title bgc="red">内容</Title>

// 继承

const Button = styled.button`
  color: palevioletred;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;
// 我有另一个按钮 具有 Button 所有的样式
const Qf_Btn = styled(Button)`
  background:blue;
`
```

## 组件的数据挂载 --外部数据 props

- props 验证
  prop-types

类 的静态属性 实例无法调用 只能 由 类调用

```js
class X {
  // 内部定义
  static 属性名=值
}
X.属性 = 值

// 安装
npm install prop-types -S

import PropTypes from 'prop-types'
// 定义静态属性
MyComponent.propTypes = {
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
```

- props 默认值
  定义一个静态属性 defaultProps 在这里定义默认值

```js
class Xxx extends Component {
  static defaultProps = {
    a: 10,
  };
}
```

- props 中的内容 children 类似于 插槽（vue 中的 slot）

```js
this.props.children;
```

## 组件数据挂载 -- state 存储当前组件的内部数据

state 定义方式

- 直接定义

```js
class XX extends Component {
  state = {
    a: 10,
    b: 20,
  };
  render() {
    return <div>{this.state.a}</div>;
  }
}
```

- constructor 中定义

```js
class XX extends Component {
  constructor() {
    super();
    this.state = {
      a: 10,
    };
  }
  render() {
    return <div>{this.state.a}</div>;
  }
}
```

- react 不是 mvvm 框架 所以不能直接修改 state 应该使用 setState 方法

```js
constructor(){
  super();
  this.state = {
    a: 5
  }
}
this.setState({
  a: 10
})

// 注意 setState 调用 传参对象和state 执行的是合并操作
this.setState((preState, props)=>{
  return {
    a: 20
  }
})


// setState不管是以上哪种方式调用 省略第二个参数，参数是回调函数。回调函数在 state改变时触发
// 注意： setState 方式 调用 修改state 有时候是同步的，有时候是异步的
// 同步情况： react捕获不到的操作中 调用setState修改状态
// 1，定时器中 修改state 2，js原生的事件中 修改state
// 异步情况： react 能够捕获到的操作中 setState修改数据
// 1，react组件的生命周期钩子函数中 2，react合成事件中

总结： 不记得什么时候 同步什么时候异步， 获取最新的 改变后的值 就可以 都在 setState的 第二个参数中获取
```

注意：
1，函数式 组件 没有 state
2，state 尽量少用 UI 组件 尽量保证 没有 state，都是 props

组件分类：
有状态组件
无状态组件

逻辑组件 （容器组件 有 state 父组件）
UI 组件 （没有 state）

受控组件（只有 props 没有 state 所以外部 props 的值变了，组件立即改变视图）
非受控组件 （容器组件）
半受控组件 （有 props 也有 state）
对比 props 和 state
1，尽量少用 state 多用 props
2，尽量 状态 提升
3，props 不能直接修改 否则立即报错

## react 中的数据渲染

- 提供了 一个 空 容器组件 Fragment

```js
import React,{Fragment} from 'react'
{
  render(){
    return(
      <Fragment>
        的味道无多无
      </Fragment>
    )
  }
}

{
  render(){
    return(
      <>
        的味道无多无
      </>
    )
  }
}
//Fragment做组件 根标签 不会渲染
```

- 条件渲染

```js
{
  constructor(){
    super();
    this.state = {
      isLike: false
    }
  }
  render(){
    return (
      <div>
        {
          this.state.isLike
          ?
          <button>你真漂亮</button>
          :
          <span>你是好人</span>
        }

        {
          this.state.isLike && <button>你好漂亮</button>
        }
      </div>
    )
  }
}
```

- 列表渲染

```js
{
  this.state.arr.map((el, index) => {
    return <li key={index}> {el} </li>;
  });
}
```

- 渲染富文本

```js
<div dangerouslySetInnerHTML={{ __html: this.state.content }} />
```

## react 事件绑定

on 事件名 事件名首字母大写
这是 react 合成事件 并不是 原生事件

```js
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

## 处理表单处理问题

事件函数的 第一个 参数 默认是 事件对象

e.target 获取事件源
e.stopPropagation() 阻止冒泡
e.preventDefault() 阻止默认事件

```js
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

  handleInput = (e)=>{
    const target = e.target;
    this.setState({
      inputTxt: target.value
    })
  }
}
```

## 状态提升 **\*** 将数据 放到 父组件中进行管理

## 事件 绑定 并传参

行内新增 箭头函数 作为事件函数，在事件函数的函数体中 调用方法 并传参

```js
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

## 组件生命周期

- 挂载阶段
  constructor

static getDerivedStateFromProps()
// 通过 props 获取派生 state 类似于 vue 中的计算属性

render

componentDidMount 请求函数 和获取 dom 的操作 都在 componentDidMount 完成

- 更新阶段
  static getDerivedStateFromProps()
  shouldComponentUpdate() // 性能优化

render()

componentDidUpdate() // 可以获取更新的 dom

- 卸载阶段
  componentWillUnmount() // 清除定时器 销毁全局 事件绑定

## 组件性能优化

react 只要父组件 更新了，默认所有的后代组件都会更新

```js
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

## ref 获取 dom（组件）

1，写法 1 最新写法

```js
import React, { Component, createRef } from "react";

class XX extends Component {
  constructor() {
    super();
    this.btn = createRef();
    // 实例创建一个属性 btn 属性的值 是 空的放置dom容器
  }
  render() {
    return (
      <div>
        <button ref={this.btn}>按钮</button>
      </div>
    );
  }
  componentDidMount() {
    console.log(this.btn.current); // 就是button这个dom
  }
}
```

2,另一种写法

```js
import React, { Component } from "react";

class XX extends Component {
  render() {
    return (
      <div>
        <button
          ref={(btn) => {
            this.btn = btn;
          }}
        >
          按钮
        </button>
      </div>
    );
  }
  componentDidMount() {
    console.log(this.btn); // 就是button这个dom
  }
}
```

## 高阶组件 HOC hign order component

```js
//面试题
/* 
fn(3)(4)   返回12
function fn(a){
  return function(b){
    return a*b
  }
}
//  函数柯里化   外层叫高阶函数
*/
```

其实 高阶 组件 就是一个高阶函数，返回一个组件，且参数接收的也是一个组件，这个参数组件在内部返回组件中使用了
使用场景： 修饰普通的组件 可以 给普通组件上 在原有的 功能基础上加一些其他的功能（结构、props。。。）

```js
import React, { Component } from "react";

const WithCopy = (Decorator) => {
  return class Copy extends Component {
    constructor() {
      super();
      this.state = {
        a: 10,
        b: 20,
      };
    }
    render() {
      return (
        <>
          <Decorator {...this.props} {...this.state} />

          <h3>&copy;版权所有</h3>
        </>
      );
    }
  };
};

export default WithCopy;

//使用时

class A extends xx {}
export default WithCopy(A);
```

## react 组件通信

context 组件通信 类似于 vue 中 event Bus

```js
import {  createContext } from 'react'

// 创建context对象
const context = createContext();

const { Provider, Consumer } = context;
// Provider是提供数据的组件 要求 提供的数据 只能由 后代组件获取

export {
  Provider,
  Consumer
}

// 入口函数 index.js
reactDom.render(
  <Provider value={{a:10,b:20}}>
    <App/>
  </Provider>
)

// 在任意一个组件中 使用Consumer回去Provider提供的value
// Consumer的内容 是一个函数 return jsx 函数参数就是 value

{
  render(){
    return (
      <div>
        <Consumer>
          {
            (value)=>{
              return (
                <div>
                  {value.a}
                  {value.b}
                </div>
              )
            }
          }
        </Consumer>
      </div>
    )
  }
}
```

## redux 集中式 管理 状态 （redux 和 react 是解耦的）

![](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg)
[](https://www.redux.org.cn/)
三大原则：
单一数据源

state 是只读的

reducer 必须是纯函数 （没有任何副作用函数，调用传参，必须返回，且不能破坏结构）

action 对象
{
type: '干什么'
}

```js
// 安装
npm i redux -S

// src/store
// index.js
import { createStore } from 'redux'
import reducer from './reducer'

const store = createStore(reducer)

export default store

// reducer.js
const defaultState = {
  num: 10
}
// reducer是纯函数
const reducer = (state=defaultState, action)=>{
  // state是只读的
  const newState = JSON.parse(JSON.stringify(state))
  switch (action.type){
    case 'xxx':
        xxx
        break;
    default:
      break;
  }
  return newState;
}
// 组件中使用
store.getState()  // 获取state
store.subscribe(()=>{
  // 获取最新的state
  store.getState()// 得到最新的
})

// 提交action
// action 是一个对象 必须有一个属性type 干什么
store.dispatch({
  type: 'addNum',
  value: 2
})

```

## actionCreators 统一 管理 action

```js
// store/actionCreators

// addNum
const addNum = (value) => {
  return {
    type: "addNum",
    value,
  };
};
const reduceNum = (value) => {
  return {
    type: "reduceNum",
    value,
  };
};

export { addNum, reduceNum };

// 组件中
import { addNum, reduceNum } from "./store/actionCreators";

store.dispatch(addNum(5));

store.dispatch(reduceNum(10));
```

## 利用常量 保存 action 的 types

```js
// store/actionTypes.js

export const ADDNUM = "ADDNUM";
export const REDUCENUM = "REDUCENUM";

// actionCreators、reducer
import { ADDNUM, REDUCENUM } from "./actionTypes";
// 利用这些常量作为action 的type属性的值
// reduce比较时 比较常量
```

## 安装 chrome-redux-devtools

```js
// 创建store
import { createStore } from "redux";
import reducer from "./reducer";
const store = createStore(
  reducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);

export default store;
```

## redux 中的异步操作

问题：createStore 最多只能有两个参数
redux 异步插件
redux-thunk
redux-saga (generator)

```js
// 安装
npm i redux-thunk -S

// store
// 创建store
import { createStore, applyMiddleware } from 'redux'
import reducer from './reducer'
import thunk from 'redux-thunk'
const store = createStore(
  reducer,
  applyMiddleware(thunk)
)

export default store
/*
使用redux-thunk插件后，  actionCreator 原来是函数返回action（对象）
允许 actionCreator 是一个函数 返回 一个函数

store.dispatch(actionCreator) 内层函数 会自动 传入dispatch方法
*/


//actionCreators

const addNumAsync = (value)=>{
  return (dispatch)=>{
    setTimeout(()=>{
      dispatch({
        type: 'addNum',
        value
      })
    },2000)
  }
}

// 组件中
store.dispatch(addNumAsync(300))

// 使用插件同时 使用 redux-dev-tools
// 创建store
import { createStore, applyMiddleware, compose  } from 'redux'
import reducer from './reducer'
import thunk from 'redux-thunk'
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const store = createStore(
  reducer,
  composeEnhancers(applyMiddleware(thunk))
)

export default store
```

## combineReducers 拆分多个 reducer（不同 reducer 负责不同模块的 state）

```js
// msg
reducer;
actionTypes;
actionCreators;

rootReducer;
import { combineReducers } from "redux";
import msgReducer from "./msg/reducer";

const rootReducer = combineReducers({
  msg: msgReducer,
});

export default rootReducer;

// createStore第一个参数改为rootReducer即可 其他不变
```

## react-redux 连接 react 组件与 redux

```js
npm i react-redux -S
// index.js 入口函数中
import store from './store'
import { Provider } from 'react-redux's

reactDom.render(
  <Provider store={{store}}>
    <App />
  </Provider>
)
// 组件中
import { connect } from 'react-redux'

class A extends Component{

}
const mapStateToProps = (state)=>{
  return {
    num: state.msg.num,
    todoLists: state.todo.todos
  }
}

const mapDispatchToProps = (dispatch)=>{
  return {
    addNum: (value)=>{
      dispatch(addNum(value))
    }
  }
}
export default connect(mapStateToProps, mapDispatchToProps)(A)
```

## react 中的 hooks

函数式组件中的 一些 缺陷
缺点：
1，没有内部的 state
2，函数式组件没有生命周期

```js
// useState 解决 函数式组件没有state问题
const [num,setNum] = useState(初始值)
// num 是一个值 setNum是改变这个值的方法
// 注意：直接定义变量，修改变量视图不会刷新
// 问题：useState中改变数据的方法 传入的参数 必须是 覆盖式（必须是新值、针对引用类型）


// useEffect 解决函数式组件没有生命周期的问题
useEffect(()=>{
  // 在 组件 视图 加载完毕触发，在数据更新视图更新完成后也会触发
  // 相当于 componentDidMount componentDidUpdate
})
// useContext 解决函数式组件 获取context提供的数据问题
const value = useContext(context对象)
// value 就是context对象 Provider提供的数据

// useRef
const btn = useRef(null)

<button ref={btn}>按钮</button>

```

## react 路由

万物皆组件（分散 各个组件中）
对比 vue:vue 路由是配置写法（单独在一个配置文件中定义路由）
react-router 核心包
react-router-dom 渲染成 dom（用于 bs 应用）
react-router-native （app 开发 react-native）
B/S C/S

```js
// 安装
npm i react-router-dom -S
// 核心容器组件  BrowserRouter HashRouter 用于包裹根组件
// BrowserRouter包裹相当于vue中的history模式 HashRouter就是hash模式
import { HashRouter } from  'react-router-dom'

// index.js
<HashRouter>
<App/>
</HashRouter>

// Route组件  定义路由  即是路由定义 也是 路由组件的出口
import { Route } from 'react-router-dom'
// app
{

  render(){
    return (
      <div>
        <Switch>
            <Route path="/" exact component={Home}/>
            <Route path="/news" component={News}/>
            <Route path="/about" component={About}/>
          </Switch>
      </div>
    )
  }
}
/*
问题： path匹配规则包含（开头）即符合
  比如
    /home/a/b
    匹配
    /
    /home
    /home/a
    /home/a/b
  Route 默认是贪婪模式 当一个Route匹配成功后，会继续匹配下面的Route


  Switch组件 包裹 Route组件 会 变成懒惰模式，匹配成功一个Route之后会立即 停止匹配

  path匹配规则包含（开头）即符合  Route加 exact属性，会变成精准匹配（即地址栏的地址 必须和 path一模一样才能匹配）
*/

// Link和NavLink组件 用于跳转路由 声明式
import { Link,NavLink } from 'react-router-dom'
<Link to="/home">首页</Link>

<NavLink to="/home">首页</NavLink>
// NavLink 当前路由 匹配 组件 会自动加上 active类
// 也可以通过  activeClassName自定义active类
// 或者通过activeStyle自定义高亮 样式

// Redirect组件  重定向
<Redirect />
// 属性：
  to // 重定向地址
  from // 地址是 什么时候 重定向
  exact // 精准匹配 针对from

 //  解决 /根路径重定向到home页的问题
  <Redirect to="/home" from="/" exact/>
// 解决404问题
  <Route path="/404" component={NotFound}/>

  <Redirect to="/404"/> //一般写在最后
```

## react 路由组件 路由会往路由组件中灌入 三个对象（props）

```js
// 路由组件中的props中的三个对象
history; // 对象保存的是 操作路由方法
go(n); // 0刷新 -1 回退一步 1前进一步
goBack(); // 回退一步
goForward(); // 前进一步
push(); // 跳转路由（添加新的历史记录）
replace(); // 跳转路由 （替换当前路由）
location; // 保存了地址栏相关参数 （用于路由跳转传参获取参数）

match; // 获取路由跳转传参
```

## react 中路由跳转并传参

- 动态路由

```js
// 路由定义
<Route path="/路径/:id" />;
// 组件中获取
this.props.match.params.id;
// 优点：刷新后不丢失  缺点：地址栏上显示
```

- state 传参

```js
// 跳转时
<Link
  to={{
    pathname: "/news",
    state: {
      a: 10,
      b: 20,
    },
  }}
></Link>; // NavLink一样
// 动态路由传参
this.props.history.push({
  pathname: "/news",
  state: {
    a: 10,
    b: 20,
  },
});
// 获取
this.props.location.state.a;
/* 
优点：隐式传参
缺点：刷新后丢失
*/
```

- search 传参

```js
// 跳转
<Link to="/news?a=10&b=20"></Link>;
this.props.history.push("/news?a=10&b=20");
// 获取
this.props.location.search;
// 值?a=10&b=20
// 缺点：需要手动解析  且 在地址栏显示
```

- query 传参

```js
<Link
  to={{
    pathname: "/news",
    query: {
      a: 10,
      b: 20,
    },
  }}
></Link>;
this.props.history.push({
  pathname: "/news",
  query: {
    a: 10,
    b: 20,
  },
});

// 获取
this.props.location.query.xxx;
/* 
优点：隐式传参
缺点：刷新后丢失 （query属性都没了）
*/
```
## 非路由组件 操作 路由
路由相关三大对象
history、location、match 都是在路由组件的 props中
react-router提供了一个高阶组件  用于 非路由组件获取三大对象
withRouter
```js
import { withRouter } from 'react-router-dom'

export default withRouter(普通组件)
```
## 路由鉴权
- 方式1 利用Redirect组件 鉴权
```js
import React, { Component } from 'react'
import {Redirect} from 'react-router-dom'
export default class News extends Component {
  render() {
   const token = localStorage.getItem('access_token')
    return (
      <div>
        {
          !token && <Redirect to="/login"/>
        } 
        新闻页
      </div>
    )
  }
  
}

```
- 方式2 利用Route组件的render属性鉴权
```jsx
<Route path="xxx" render={ ()=>{
  const token = localStorage.getItem('access_token');
  if(token){
    // 登录了
    return <News/>
  }else{
    return <Redirect to={{
      pathname:"/login",
      state: {
        from: '/new'
      }
    }}/>
  }
} }/>
```
## craco
```js
npm i @craco/craco -D
// 根目录下创建
craco.config.js
module.exports = {
  devServer: {
    port: 9527,
    proxy:{},
    open: true
  },
  babel: {
    plugins: []
  },
  webpack: {
    alias: { // 路径别名

    }
  }
}

// 修改 package.json

"scripts": {
-   "start": "react-scripts start",
+   "start": "craco start",
-   "build": "react-scripts build",
+   "build": "craco build"
-   "test": "react-scripts test",
+   "test": "craco test"
}
```

## es6 装饰器

```js
@testable
class MyTestableClass {
  // ...
}

function testable(target) {
  target.isTestable = true;
}

```
## 开启es6装饰器模式
```js
// npm i @babel/plugin-proposal-decorators -D
//  craco.config.js
module.exports = {
  babel: {
    plugins: [
      ["@babel/plugin-proposal-decorators", { legacy: true }]
    ]
  }
}

// 未使用装饰器前  使用高阶组件

class App extends Component{
  xx
  dwdw
}


export default withRouter(App)

// 开启装饰器模式后

@withRouter
class App extends Component{
  
}

export default App


// 使用react-redux connect连接组件 和redux

const mapStateToProps = {

}
@connect(mapStateToProps)
class X extends Component {

}
export default X
```

## react ui组件库
antd
antd-mobile
```js
// 下载
npm i antd -S

index.js // 入口函数中
import 'antd/dist/antd.css'

// 使用组件
import { Button } from 'antd'

{
  render(){
    return (
      <div>
        <Button>按钮</Button>
      </div>
    )
  }
}
```