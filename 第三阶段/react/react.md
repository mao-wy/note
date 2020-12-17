

##  组件 组件化开发

注意： 1，组件名必须大写 2，组件jsx中 必须有一个根元素

### 函数式组件

```
const App = (props) =>{
  return (
    <div>
      你好函数式组件
    </div>
  )
}

使用
<App title="a" num="2" num2={2}/>
```

### 类组件

```
import React, { Component } from 'react'

export default class App extends Component {
  render() {
    return (
      <div>
        类组件
        { this.props.title }
      </div>
    )
  }
}


使用时
<App title="hello react" />  //传递props
注意：
  当我们 <App/> 使用类组件时，react会自动 new App类，调用render函数
  生成dom片段
  props在 实例上 this.props.xxx获取
```

## jsx中 想要使用 js 就放到 {}

```
jsx  js中写标签
jsx中的 {}是js环境
```

## 复习

```
react  核心包 组件语法 jsx语法解析成dom(虚拟dom fiber)
react-dom  全局就用了一次 
react-scripts

函数式组件
  const App = (props)=>{
    return (
      <div>
        { props.a }
      </div>
    )
  }

  使用：
    <App title="x" a="c"/>
  
  class xxx extends Component{
    constructor(){
      super();
    }
    render(){
      return (
        <div>
          { this.props.xxx }
        </div>
      )
    }
  }
```

## 组件数据挂载----props 外部的数据

单向数据流 props 不能在子组件中修改

### props验证以及默认值处理

#### 验证

prop-types 包

```
安装
cnpm i prop-types -S

import PropTypes from 'prop-types'


class XXX extendx Component{
  static propTypes = {
    a: PropTypes.array, // 数组类型
    b: PropTypes.bool, // bool
    c: PropTypes.func,
    d: PropTypes.number,
    e: PropTypes.object,
    f: PropTypes.string,
    g: PropTypes.symbol,
    // 必须是react 组件 （<div /> <TodoList></TodoList>）
    h: PropTypes.element,
    // 要求 是 Message构造函数或者类的实例
    i: PropTypes.instanceOf(Message),
    // 枚举  传的值 必须是 以下值中的某一个
    j: PropTypes.oneOf(['News', 'Photos']),
    // 传的值的类型 是以下类型中的某一个
    k: PropTypes.oneOfType([
      PropTypes.string,
      PropTypes.number,
      PropTypes.instanceOf(Message)
    ]),
  
    // 是传的值是数组 且数组的中的值 必须是 定义的类型
    l: PropTypes.arrayOf(PropTypes.number),
  
    // 传的值必须是对象 且对象的值 必须是 定义的类型
    m: PropTypes.objectOf(PropTypes.number),
    // 传的值 必须是对象，且定义了 对象的结构 什么属性 属性值是什么类型
    n: PropTypes.shape({
      optionalProperty: PropTypes.string,
      requiredProperty: PropTypes.number.isRequired
    }),
    // 必传
    o: PropTypes.isRequired
  }
}
```

#### 组件props默认值

```
class X extends Component{
  static defaultProps = {
    a: '默认值'
  }
  render(){

  }
}
```

#### props.children 组件 内容

```
<Todolist>
  <div>
    <h1>x<h1>
    dwdew
    xx
    xxx
  </div>
</Todolist>

可以在 Todolist组件中
通过this.props.children 获取todoList标签内容（虚拟dom）
```

## jsx注释

```
{/*注释内容*/}
```

## 内部状态 --- state 相当于vue中的 data

```
class xxx extends Component{
  constructor(){
    super();
    // 定义组件内部的状态
    this.state = {
      title: 'xxx'
    }
  }
  componentDidMount(){
    // 相当于vue中的mounted
    setTimeout(()=>{
      this.setState({
        title:'数据改变了'
      })
    },2000)
    // 注意 不要直接修改state，不是mvvm框架。调用 setState方法修改数据render才会更新
  }
  render(){
    return (
      <div>
        {this.state.title}
      </div>
    )
  }
}

setState的 写法
1，
this.setState({
  title:'xxx'
})

2,setState第二种写法
this.setState((prevState, props)=>{
  // preState 改变前的state props当前组件的props
  return {
    title: '数据改变了'
  }
})

setState省略的第二个参数
this.setState({
  title: 'sss'
},()=>{
  // 这是回调函数 我们setState改变数据 有时候是异步的 有时候是同步的 
})

setState 写在 react可以捕获到的动作中 就是异步的 react捕获不到的就是同步的
（
  react合成事件 以及 组件生命周期中 react可以捕获到
  原生事件 中   捕获不到 定时器中 全局事件绑定
）
```

## 比较 state和props

```
1,state是组件内部的数据  props组件外部
2,不管是state改变还是props改变 就是 重新调用当前组件的render方法 导致页面刷新
3，UI组件 推荐使用props，容器组件 使用state
```

## 单向数据流、状态提升

一个父组件（通常是容器组件、路由组件）中使用了多个子（包括后代组件）组件，应该讲所有子组件的状态 放到 父组件中管理（props传到子组件中 props不能直接改变 ） 好处：状态（数据），是单一的 可控的（改变是可预料的）

## 容器组件 UI组件 受控组件 非受控组件（半受控组件）

容器组件 父组件 （大部分都是路由组件） UI组件 子组件（只负责渲染、和处理数据，不做逻辑处理） 受控组件 这个组件的所有的样式，都受外部的控制（没有state，只有props） 非受控组件 容器组件 半受控组件 有props也有state的

## 组件中的样式

jsx xml in js js中可以写标签 同vue组件相比 差了组件的样式

```
<student>
  <name>小农</name>
  <age>18</age>
  <gender>1</gender>
</student>
{
  name:"小明",
  age: 18,
  gender: 1
}

1，内联样式
  style属性的值 是对象

        <h1 style={ {color:'red',fontSize:'20px'} }>今日事今日毕</h1>
        <div style={ {
          width: 100+200+500/100,
          height:200,
          background:'red'
        } }></div>
2，引入外部的样式  cra启动项目 默认配置了scss支持
  import '..style.scss'
  问题：
    任意组件中引入的css都会影响全局
  怎么解决？
    每一个组件 根元素 都加上独一无二的 类或者id
    所以选择器 都写 子后代选择器
3，css in js 将css 变成react组件
  styled-components
  cnpm i styled-components

  基础
    
```

复习：

```
props

  <组件名 title="xxx"/>

  函数式组件：
  const App = (props)=>{
    return (
      <div>
        { props.title }
      </div>
    )
  }


  class X extends Component{
    render(){
      (
        <div>
          { this.props.title }
        </div>
      )
    }
  }

  验证
    prop-types
    npm i props-types -S
    import PropTypes from 'prop-types'
  class X extends Component{
    static propTypes = {
      a: PropTypes.string
    }
    static defaultProps = {
      a: '我是默认值'
    }
    render(){
      (
        <div>
          { this.props.title }
        </div>
      )
    }
  }


  this.props.children

  <组件>
    <div>
      ciejf
      dewfde
    </div>
  </组件>

  state 内部数据  状态


  class X extends Component{
    /*
    state = {
      title:'xxxx'
    }
    */
    constructor(){
      super();
      this.state = {
        xxx
        xxx
      }
    }
    render(){
      return (
        <div>
          { this.state.xxx }
          { /*注释*/ }
        </div>
      )
    }
  }

  如何设置状态
  this.state.xx = 值  值会修改 但是视图不刷新（mvvm）

  this.setState({
    title:'xxx'
  })
  this.setState((preState, props)=>{
    return {
      title:props.xxxx
    }
  })

  setState 第二个参数 回调函数
  setState({},()=>{
    // setState修改数据 有时候是异步的有时候是同步的
    // react可以捕获到的（react合成事件、生命周期钩子） 动作 中操作 是异步的 捕获不到（原生事件绑定、定时器）同步的
  })

  组件中的样式

  内联
  <div style={ { width: 100, height:200, fontSize:"20px"} }>
  引入外部样式
  注意：没有像vue一样的scoped作用域，选择器需要注意

  css in js
  styled-components

  import styled from 'styled-components'

  // 渲染一个 Title组件，渲染成h2标签且具有以下样式
  const Title = styled.h2`
    width:100px;
    height:200px;
    font-size:20px;
    background: ${ (props)=> props.color };
    a{
      color:blue;
    }

  `

  const Title2 = styled(Title)`
    
  `

  <Title color="red">
    <a>xxxx</a>
  </Title>
```

## 条件渲染

```
{
  constructor(){
    super();
    this.state = {
      isShow: false
    }
  }
  render(){
    return (

      <div>
        {
          this.state.isShow
          ?
          '真的'
          :
          '假的'
        }

        {
          this.state.isShow
          ?
          <div />
          :
          <button>xxx</button>
        }
      </div>
    )
  }
}
```

## 列表渲染

```
{
  constructor(){
    super();
    this.state = {
      arr:[1,2,3,4]
    }
  }
  render(){
    return (
      <div>
        {
          this.state.arr.map((el, index)=>{
            return (
              <div key={index}> {/*需要加独一无二key*/}
                  {el}
              </div>
            )
          })
        }
      </div>
    )
  }
}
```

## 渲染富文本

```
{
  constructor(){
    super();
    this.state = {
      content:`
        <div>
          <h1 style="color:red">我是标题</h1>
          <button>按钮</button>
        </div>
      `
    }
  }
  render(){
    return (
      <div>
        <div dangerouslySetInnerHTML={ {__html:this.state.content} }></div>
      </div>
    )
  }
}
```

## Fragment 占位容器标签（页面渲染时，消失）

```
import React,{Component, Fragment}
{
  render(){
    return (
      <Fragment>
          dwhdjwo
          xxx
      </Fragment>
    )
  }
}
```

## React组件 中 事件绑定

- 直接在render里写行内的箭头函数(不推荐)
- 在组件内使用箭头函数定义一个方法(推荐) act = ()=>{}
- 直接在组件内定义一个非箭头函数的方法，然后在render里直接使用`onClick={this.handleClick.bind(this)}`(不推荐) act(){}
- 直接在组件内定义一个非箭头函数的方法，然后在constructor里bind(this)(推荐) act(){}
- （个人补充:React元素的事件处理和DOM元素类似。但是有一点语法上的不同：
- React事件绑定属性的命名采用驼峰式写法，而不是小写。
- 如果采用JSX的语法你需要传入一个函数做为事件处理函数，而不是一个字符串（DOM元素的写法））

### 合成事件函数中获取事件对象以及事件源

```
第一个参数就是事件源（没有兼容问题）
<button onClick={ this.handleClick }></button>

{
  handleClick = (e)=>{
    // e.stopPropagation()
    // e.preventDefault()
    // e.target 事件源
  }
}
```

## 处理用户表单输入

```
{
  constructor(){
    super();
    this.state = {
      inputTxt: ''
    }
  }
  render(){
    return (
      <div>
        <input value={this.state.inputTxt} onChange={this.handleInput}/>
      </div>
    )
  }
  handleInput = (e)=>{
    this.setState({
      inputTxt: e.target.value
    })
  }
}
```

## 生命周期

三个阶段

- 挂载

```
constructor(){
  super();
  this.state = {
    // 这里是初始的state的定义
  }
}
static getDerivedStateFromProps(props,state){

  return { 
    /*
    在这里 可以派生一些state 返回对象中的属性会自动 
    合并到当前实例的state属性中
    一般有点类似于vue中的计算属性的用法
    某个state 基于 其他的props或者state存在
    */
  }
}
  // 类似于vue中的计算属性的功能（根据props来获取派生的state）
render()
  // 生成虚拟dom 比较(fiber算法)（生成真实dom）
componentDidMount(){
// 渲染完毕 相当于vue中的 mounted 在这里获取真实dom以及 调用ajax请求函数
}
  



componentWillMount() 在后面版本 马上要废弃，谨慎使用
UNSAFE_componentWillMount() 使用时加+UNSAFE_


ref获取dom
  1， createRef
  {
    constrcutor(){
      super();
      this.btn = createRef();
    }
    render(){
      return (
        <div>
          <button  ref={ this.btn }>按钮</button>
        </div>
      )
    }
    componentDidMount(){
      console.log(this.btn.current) // 就是button这个dom对象
    }
  }
  2,
  {
   
    render(){
      return (
        <div>
          <button  ref={ btn=>{this.btn = btn} }>按钮</button>
        </div>
      )
    }
    componentDidMount(){
      console.log(this.btn) // 就是button这个dom对象
    }
  }
```

- 更新 注意： react 父组件更新是，去更新父组价中所以的子组件（不管子组件中的数据（props,state）有没有更新）

```
static getDerivedStateFromProps()
shouldComponentUpdate(nextProps){
  return nextProps.todo !== this.props.todo
  /*
    此钩子会拦截 当前组件的刷新
    return true 永远刷新
    return false 永远不刷新
    不写return 不不会刷新
    nextProps最新的props
    this.props 刷新前的props
    注意：如果某个props是对象的话，只能进行浅层比较
    */
}
// 性能优化 手动控制组件的刷新
render()
// 渲染
componentDidUpdate()

注意:

下述方法即将过时，在新代码中应该避免使用它们：

UNSAFE_componentWillUpdate()
UNSAFE_componentWillReceiveProps()
```

- 卸载

```
componentWillUnmount()
```

## PureComponent 解决性能优化的问题

功能根shouldComponentUpdate比较像 只能做浅层比较

## 高阶组件 HOC hign-order-component

高阶组件其实 是一个函数 这个函数 返回一个组件 且这个函数接收一个参数 是组件，参数的组件可以在返回组件中使用 功能：修饰普通的组件 给其他组件 加一个功能

```
fn(2)(5)
10

function fn(a){
  return function(b){
    return a*b
  }
}
// 函数 柯里化  外层称为高阶函数


import React, {Component} from 'react'

const WithCopy = (Comp)=>{  // comp被修饰的其他的普通组件
  return class Copy extends Component{
    render(){
      console.log(this.props,'111')
      return (
        <div>
          <Comp {...this.props}/>
          <h4>我是版权信息</h4>
        </div>
      )
    }
  }
}


export default WithCopy
注意：
  高阶组件其实是一个函数 返回一个组件，且函数参数接收一个组件（被修饰组件）
使用：
  在修饰组件中
  import Hoc from xxx

  export default Hoc(普通组件)
问题？
  被高阶组件修饰的组件，在使用时，其实已经不再是修饰组件了，而是高阶组件这个函数返回的那个组件
  问题父组件传递的props 其实是 给了 高阶组件的 返回组件，而不是修饰组件
解决
  <Comp {...this.props}/>
```

## 组件通信 context 上下文

类似于vue中 event bus

```
contexta.js
  import {  createContext } from 'react'

 // 创建 context对象 有两个属性Provider和Consumer 属性值都是组件
 const contexta = createContext()


在src/index.js
import contexta from 'xxx'
reactDom.render(
  // Provider value属性挂载属性可以在任意后代组件中通过Consumer拿到
  <contexta.Provider value={{
      a:10,
      b:20,
      c:33
    }}>
    <App />
  </contexta.Provider>
  ,
  document.querySelector('#root')
)

// 其他组件中使用

Todolist
import contexta from 'xxx'
  {
    render(){
      return (
        <contexta.Consumer>
          {
            ({a,b,c})=>{ //可以拿到Provider的value
              return (
                <div></div>
              )
            }
          }
        </contexta.Consumer>
      )
    }
  } 
```

## hooks 钩子函数

解决 函数式组件 某些痛点 1,函数式组件 无法 设置内部的state 2,函数式组件 没有生命周期 注意：钩子函数 只在函数式组件中使用

```
useState解决了 函数式组件没有 内部状态问题
注意：
  设置 值得时候 不要值传递
  例如：
    const [arr,setArr] = useState([])
    const changeArr = ()=>{
      let newArr = [...arr];
      xxx
      xxx
      setArr(newVal)
    }

    const [obj,setObj] = useState({})

    const changeObj = ()=>{
      let newObj = {...obj};
      xxx
      xxx
      setObj(newObj)
    }

    

useEffect解决了 函数式组件没有生命周期的问题


import React, { useState,useEffect } from 'react'

const Todolist = (props)=>{
 
  const [num, setNum] = useState(1)
  const [arr,setArr] = useState([1])
  const addArr = ()=>{
    setArr([...arr,arr.length+1])
  }

  useEffect(()=>{
    console.log("我触发啦")
    /* 
    这个回调会在初次渲染完成（dom挂载完毕后），触发一次
    当数据更新，视图刷新，刷新完成后，也会触发
    相当于：
      componentDidMount
      componentDidUpdate
    */
  })
  return (
    <div>
      <button onClick = { ()=>{
        setNum(num+1)
      } }>+</button>
      <h1>{ num }</h1>
      <button onClick={
        ()=>{
          setNum(num-1)
        }
      }>-</button>

      <hr/>
      <button onClick={ addArr }>增加一个</button>
      <ul>
        {
          arr.map((el,index)=>{
            return (
              <li key={index}>
                {el}
              </li>
            )
          })
        }
      </ul>
    </div>
  )
}
export default Todolist




useContext
解决函数式组件获取 context 提供的数据的问题

import { useContext } from 'react'

const value = useContext(myContext)
注意：
  useContext是一个context对象
  返回值是 context对象Provider上挂载的公共的数据以及方法


useRef  在函数式组件中获取dom



import React, { useEffect, useRef } from 'react'

const Todolist = ()=>{
 
  const inputTxt = useRef(null)
  useEffect(()=>{
    console.log(inputTxt)
    inputTxt.current.focus();
  })
  return (
    <div>
      todolist
      <input ref={ inputTxt }  className="form-control"/>
    </div>
  )
}

export default Todolist
```

## redux基础

![redux图解](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg)

## Redux基础：

Redux 的适用场景：多交互、多数据源

从组件角度看，如果你的应用有以下场景，可以考虑使用 Redux。

> - 某个组件的状态，需要共享
> - 某个状态需要在任何地方都可以拿到
> - 一个组件需要改变全局状态
> - 一个组件需要改变另一个组件的状态

<font color='red'>Action</font>是把数据从应用（译者注：这里之所以不叫 view 是因为这些数据有可能是服务器响应，用户输入或其它非 view 的数据 ）传到 store 的有效载荷。它是 store 数据的**唯一**来源。一般来说你会通过 [`store.dispatch()`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 将 action 传到 store。

<font color='red'>**Reducers**</font> 指定了应用状态的变化如何响应 [actions](https://www.redux.org.cn/docs/basics/Actions.html) 并发送到 store 的，记住 actions 只是描述了*有事情发生了*这一事实，并没有描述应用如何更新 state。

<font color='red'>**Store**</font> 就是把Action和Reducers联系到一起的对象。Store 有以下职责：

- 维持应用的 state；
- 提供 [`getState()`](https://www.redux.org.cn/docs/api/Store.html#getState) 方法获取 state；
- 提供 [`dispatch(action)`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 方法更新 state；
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 注册监听器;
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 返回的函数注销监听器。

再次强调一下 **Redux 应用只有一个单一的 store**。当需要拆分数据处理逻辑时，你应该使用 [reducer 组合](https://www.redux.org.cn/docs/basics/Reducers.html#splitting-reducers) 而不是创建多个 store。

三大原则： 单一数据源 仓库是唯一
state是只读的 不要直接修改state （dispatch action）使用reducer来修改 reducer必须是纯函数 没有任何副作用

```
npm i redux -S
store
  index.js
  import { createStore } from 'redux'
  import reducer from './reducer'
  const store = createStore(
    reducer
  )

  reducer
    import { ADD_NUM, REDUCE_NUM } from './actionTypes'
    const defaultValue = {
      num: 10,
    };

    // reducer是一个纯函数
    const reducer = (state = defaultValue, action) => {
      /*  console.log(state)
      console.log(action) */
      // 深克隆
      let newState = JSON.parse(JSON.stringify(state));
      switch (action.type) {
        case ADD_NUM:
          newState.num += action.value;
          break;
        case REDUCE_NUM:
          newState.num -= action.value;
          break;
        default:
          break;
      }
      return newState;
    };

    export default reducer;

  actionCreators
    import { ADD_NUM, REDUCE_NUM } from './actionTypes'
    // 增加num
    const addNum = (value)=>{
      return {
        type: ADD_NUM,
        value
      }
    }
    // 减少num
    const reduceNum = (value)=>{
      return {
        type:REDUCE_NUM,
        value
      }
    }

    export {
      addNum,
      reduceNum
    }

  组件中
  store.getState() 获取 state
  store.subscribe(()=>{
    // 监听仓库的变化、在这里重新获取state
    this.setState({
      ...store.getState()
    })
  })

  store.dispatch(action)
```

### redux高级 -- actionCreators中添加异步

redux-saga (es6 genetator) redux-thunk redux-promise

```
npm i redux-thunk -S

store 
  index.js
  import { createStore, applyMiddleware } from 'redux'
  import reducer from './reducer'
  import thunk from 'redux-thunk'
  // 创建store

  const store = createStore(
    reducer,
    applyMiddleware(thunk)
  )
  export default store

当使用redux thunk后
允许我们actionCreator 返回一个函数

const addNumAasync = (value)=>{
  return (dispatch)=>{
    fetchItems().then(res=>{
      disptach(addNum(res.data.data))
    })
  }
}


组件中：
  store.dispatch(addNumAasync())

使用 redux插件的同时，并使用redux_dev_tools

import { createStore, applyMiddleware, compose  } from 'redux'
import reducer from './reducer'
import thunk from 'redux-thunk'

// 这是一个更大的容器
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
 

// 创建store
const store = createStore(
  reducer,
  composeEnhancers( applyMiddleware(thunk))
)
export default store



import {combineReducers} from 'redux'
import todoReducer from './todoList'
import itemReducer from './item'
const rootReducer = combineReducers({
  todoReducer,
  itemReducer
})
```

## react-redux 连接 redux和react组件

```
npm i react-redux -S

Provider  
src
  index.js
    import { Provider } from 'react-redux'

    import store from './store'

    reactDom.render(
      <Provider store={ store }>
        <App/>
      </Provider>
    )


connect
  在任意组件中

  import { connect } from 'react-redux'

  class AA extends xx{

  }

  const mapStateToProps = (state)=>{
    return {
      num: state.todoList.num
    }
  }

  const mapDispatchToProps = (dispatch)=>{
    return {
      addNum: (n)=>{
        dispatch(addNum(n))
      }
    }
  }

  export default connect(mapStateToProps, mapDispatchToProps)(AA)npm i react-redux -S

Provider  
connect
```

## react-router 路由

万物皆组件 （路由）

```
npm i react-router-dom -S


HashRouter
BrowserRouter
用于包裹根组件  包裹根组件后 才可以使用路由

Route 组件 用于定义路由
  path 定义 路由的 path路径
  component 定义当这个path 匹配成功后显示的组件（路由组件）
  ？ path 匹配规则是模糊匹配 （包含关系）
    路径如果是/ 那 /home /news /任何 都可以匹配到/因为有包含关系
  exact（bool） 精准匹配 path匹配规则有包含关系 变 变成必须要 相等


Switch 组件用于包裹 Route组件（父组件）
    Route默认是贪婪模式（匹配成功一个后，会继续向下匹配）
      Switch包裹后，会变成懒惰模式，匹配成功一个后就不在向下匹配

Redirect 组件  重定向组件
  to 重定向到哪个path
  from 当path为 xxx时重定向 （注意：from值得这个path默认是模糊匹配）
  exact 精准匹配

      <Switch>
        <Route
            path="/home"
            component={Home}
            exact
          />
          <Route
            path="/news"
            component={News}
          />
          <Route
            path="/about"
            component={About}
          />
          <Route
            path="/404"
            component = { NotFound }
          />
          <Redirect to="/home" from="/" exact/>
          {/* 404放到最后处理 */}
          <Redirect to="/404"/>
        </Switch>
Link  跳转路由
  to属性定义跳转地址
NavLink 跳转路由 多了高亮的类
  to定义跳转地址
  activeClassName 自定义高亮类名
  activeStyle 内联定义高亮样式
```

## react路由传参

```
声明式导航
  Link
    to=""
    replace={true/false}
  NavLink
    to=""
    replace={true/false}

    to可以是字符串
      也可以是对象
编程式导航
  参数同 to属性
```

### 参数传递

- 动态路由

```
<Route
  path="/news/:newsId"
/>
<Link to="/news/2">
在组件中 获取
  this.props.match.params.newId
```

- state传参

```
<Link to={
  {
    pathname:"/news",
    state:{
      a:10
    }
  }
}>

获取
  this.props.localtion.state.a
优点：
  隐式传参
缺点：
  刷新丢失
```

- search传参

```
to="/news?a=10&b=20"

this.props.location.search中获取 
注意：获取的值是?a=10&b=20 需要手动解析
```

## 注意：只有路由组件的props中才有 history/location/match对象

## 编程式导航

路由组件中

```
  this.props.history
    push  相当于 Link to属性 普通跳转 添加新的历史记录 
          参数同to 可以是字符串 或者是对象
    replace
      相当于Link to属性 同时 Link replace属性为true
        跳转路由，但是覆盖当前路由记录
    go(n)
      0 刷新
      1 前进一步
      -1 后退一步
```

## 如何在非路由组件中 操作路由（获取路由参数）

```
引入withRouter高阶组件 修饰 非路由组件即可
import { withRouter } from 'react-router-dom'

class App xxx {

}

export default withRouter(App)
```

## render

```
<Route
  path="/xxx"
  render={
    ()=>{
      return (
        <div>
          xxx
        </div>
      )
    }
  }
/>

Route除了可以用component属性 来渲染路由组件，
还可以用render 属性来渲染
好处：
  可以做路由拦截 如登录鉴权
缺点：
  此时这个组件不再是路由组件，组件中props操作路由三大对象丢失
  解决：
        <Route
            path="/news"
            render={(routeProps)=>{
            //routeProps是 路由三大对象，通过扩展运算符传递到 组件中
            //  判断是否登录
              const token = localStorage.getItem('token')
              if(token){
                return <News {...routeProps}/>
              }else{
                return <Redirect to={
                  {
                    pathname:'/login',
                    state:{
                      from: '/news'
                    }
                  }
                }/>
              }
            }}
          />
```