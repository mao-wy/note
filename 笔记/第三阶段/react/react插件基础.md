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

## 

## react-redux    [链接](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)

连接 redux和react组件

为了方便使用，Redux 的作者封装了一个 React 专用的库 [React-Redux](https://github.com/reactjs/react-redux)，本文主要介绍它。

React-Redux 将所有组件分成两大类：UI 组件（presentational component）和容器组件（container component）。

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

## 

## axios基础