

# 一、项目基本结构

## 创建

```
 npm i create-creat-app -g
 create-creat-app 目录名  // 注意不要用驼峰式
// npx create-react-app my-app
cd my-app


cnpm i react-router-dom redux axios react-redux antd -S   // dayjs

cnpm i @craco/craco @babel/plugin-proposal-decorators -D


```

```jsx
#-S
react-router-dom --  用于React路由器的DOM绑定。

dayjs -- 用于处理时间和日期的 JavaScript 库

redux -- Redux 的适用场景：多交互、多数据源

react-redux -- Redux 的作者封装了一个 React 专用的库 React-Redux - 将所有组件分成两大类 - UI 组件（presentational component）和容器组件（container component）。

antd  -- antd 是基于 Ant Design 设计体系的 React UI 组件库，主要用于研发企业级中后台产品。

axios -- Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

#-D
@craco/craco  --  react配置是隐藏的利用craco配置环境

// bable-plugin-import  -- 按需引入

@babel/plugin-proposal-decorators -- es6装饰器模式（可用于装饰类-高阶组件）


```

## 配置craco

创建craco.config.js

```
const path = require('path')
module.exports = {
  devServer:{
    port: 9527,
    open: true,
    proxy: {
      '/api':{
        target: 'http://rap2api.taobao.org/app/mock/275069',
        changeOrigin: true,
        pathRewrite:{
          '^/api': '/api'
        }
      }
    }
  },
  babel: {
    plugins: [
      ["@babel/plugin-proposal-decorators", { legacy: true }]
    ]
  },
  webpack: {
    alias: {
      '@': path.join(__dirname, 'src'),
      'pages': path.join(__dirname, 'src/pages'),
      'utils': path.join(__dirname, 'src/utils'),
      'components': path.join(__dirname, 'src/components'),
      'api':  path.join(__dirname, 'src/api'),
      'store':  path.join(__dirname, 'src/store')
    }
  }
}

那pathRewrite是用来干嘛的呢，这里的作用，相当于是替代‘/api’，如果接口中是没有api的，那就直接置空，就像我截图的一样，如果接口中有api，那就得写成{‘^/api’:‘/api’}，可以理解为一个重定向或者重新赋值的功能。

```

更改package启动项

```
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



### 建立文件夹

routes  --  存放路由配置

pages  --  存放页面

store -- 仓库

api  --   存放请求

components  --  存放公共组件

utils  -- 存放工具



src -- index.js

```jsx
import React from 'react'
import { render } from 'react-dom'
import App from './App'

render(
	<App />,
	document.querySelector('#root')
)

```



 --App.js

## 项目路由设计

```
首页    --  Admin
仪表盘	  -- DashBoard
文章列表 --  ArtLists
添加文章 --  ArtAdd
编辑文章 --  ArtEdit
设置    --  Settings
登录	  --  Login
消息列表 --  MsgLists
404	   --  NotFound
没有权限
```

### 1、在文件夹routes里定义路由

// 路由单独管理  管理业务相关

```
import __  from '__'
const	adminRoutes = [
{
	path:'/__',
	name: '',
	component: __
}
]
```

### 2、在src下index.js的文件中引入app

​		import { HashRouter as Router } from 'react-router-dom'

​		用Router包裹<app/>

### 3、在app.js中引用路由

​		 --   admin  --  login  --   NotFound

### 4、应用框架antd  下载Icon图标 

​		 npm install --save @ant-design/icons

​			路由中增加icon属性

### 5、在components创建公共组件layout

​		  （ this.props.children ）

### 6、将二级路由引用在src--admin

```

return(
	<div>
		<Layout>
			<Switch>
			{
				adminRoutes.map(route=>{
					return (
						<Route
							key={route.path}
							path={ route.path }
							component={ route.component }
						/>
					)
				})
			}
			<Redirect to="" from="" exact/>
			<Redirect to="/404"/>
			</Switch>
		</Layout>
	</div>
)
```

### 7、并不是所有路由都需要在侧边栏显示  给routes里面的组件添加一个字段

```
const adminRoutes = [
	{
		meta:{
			inNav:true/false
	}
}
]
```

### 8、过滤侧边栏

```
import adminRoutes from 'routes'
import { withRouter } from 'react-router-dom' // 非路由组件使用路由

@withRouter
class MyLayout extends Component {
render () {
	const navrouter = adminRoutes.filter(nav=>nav.meta.isNav)  // 过滤
	return (
		<Menu
               theme="dark"
                mode="inline"
                onClick = {   // 点击跳转  
                  ( { key } )=>{ 
                    this.props.history.push(key)
                  }
                }
                 defaultSelectedKeys={['1']}>
                {
                  navRoutes.map(route => {
                    return(
                      <Menu.Item key={ route.path } icon={<route.ico />}>
                        { route.name }
                      </Menu.Item>
                    )
                  })
                }
                
              </Menu>
	)
}
}
```

# 二、mock数据

开发时前后端同时进行  ，前端开发的时候后端接口还没有  此时需要我们自己mock假数据

注意：地址要和后端接口一致

## 本地mock

​	拦截ajax请求 ajax其实并没有真的发出去

```
mock.js
安装:npm i mackjs -D
在src下创建mock文件夹，（开发完成后要删除）
```

```
const Mock = require('momckjs')
Mock.mock('url',{})  //url  拦截的请求地址

修饰字段量
"n|5":值  如果这个n是数组  数组中有5条
    如果是 字符串 随机产生多少个字符串（值）
    如果是数字 值 5
"n|5-100" 如果是数组 数组中 5-100条
如果是 字符串 随机产生5-100个字符串（值）
如果是数字： 5-100

"id|+1" 自增1

定义了一个 预设变量 自动生成不同的随机数 用于做值
mockjs介绍  https://www.jianshu.com/p/d812ce349265

react中引入再index.js中 import "./mock/index"
vue中引入再main.js
```

**Mock.Random 提供的完整方法（占位符）如下**

| 类型          | 方法                                                         |
| ------------- | ------------------------------------------------------------ |
| Basic         | boolean, natural, integer, float, character, string, range, date, time, datetime, now |
| Image         | image, dataImage                                             |
| Color         | color                                                        |
| Text          | paragraph, sentence, word, title, cparagraph, csentence, cword, ctitle |
| Name          | first, last, name, cfirst, clast, cname                      |
| Web           | url, domain, email, ip, tld                                  |
| Address       | area, region                                                 |
| Helper        | capitalize, upper, lower, pick, shuffle                      |
| Miscellaneous | guid, id                                                     |



## 在线mock网站

​			 rap2.taobao.org              easymock

![image-20210113203147245](G:\md\第三阶段\react\react项目.assets\image-20210113203147245.png)



## 配置http代理 proxy

```js
const path = require('path')
module.exports = {
  devServer:{
    port: 8888,
    open: true,
    proxy:{
      "/api":{
        target:'http://rap2api.taobao.org/app/mock/275401',
        changeOrigin:true,
        pathRewrite:{   
          '^/api':'/api'
        }
      }
		}
  },
    babel: {
      plugins: [
        ["@babel/plugin-proposal-decorators", { legacy: true }]
      ]
    },
  webpack: {
   alias:{
     "@": path.join(__dirname,'src'),
     "routes": path.join(__dirname,'src/routes'),
     "components": path.join(__dirname,'src/components'),
     "pages": path.join(__dirname,'src/pages'),
     "utils": path.join(__dirname,'src/utils'),
     "api": path.join(__dirname,'src/api')
    }
  }
}
```

# 三、页面代码

## 文章列表页

```jsx
将请求的参数，和请求回来的数据 先在组件中的constructor(){}中定义this.state
请求回来的数据使用this.setState({}) 改变this.state 中的变量

注意antd里面的icon要加标签
```



## 添加文章

```js
使用From表单组件
注意：非表单控件不要加name

使用富文本编辑器

```



![8d4e8aa9dae375b13aa5efd766788ac](G:\md\第三阶段\react\react项目.assets\8d4e8aa9dae375b13aa5efd766788ac-1610717481109.png)

![8d4e8aa9dae375b13aa5efd766788ac](G:\md\第三阶段\react\50b0e3b0ddcfc34e1e857ab6e7aab13.png)

因为这不是表单空间

### 常见的富文本编辑器

```
ueditor  百度  老
wangeditor
Tinymce
quill-editor
使用步骤：
1、渲染富文本（提供容器dom）
2、输入内容，获取 输入的带格式的html内容
3、渲染默认内容
```

## 编辑文章

```
在文章列表页点击编辑时  跳转到编辑页同时传入需要编辑的id  利用id获取需要编辑的文章数据  填到表单中
改变后  发送请求 提交改变后的数据

注意在将请求的数据渲染在富文本的时候  利用antd的from表单组件，from表单组件Api initialValues 表单默认值，只有初始化以及重置时生效
使用条件渲染，当art文章详情数据请求回来之后再渲染富文本(注意将渲染富文本的函数 在请求函数后面的回调中)  （this.state.art && <from></from>）

注意：堆叠顺序问题
富文本  z-index  很高  容易覆盖其余的dom 

将改变的数据请求提交到后端时 要将文章的id传过去
```



## 删除文章

```
点击删除传id 调用api请求
antd 中 modal
```

## 仪表盘

```
数据可视化
	Highcharts
	echarts
	antV G2
绘图前需要装备一个有高度的容器

echarts Gallery  echarts的社区
```

## 登录UI实现以及登录鉴权

```jsx
在Mylaout
在app.js里判断是否登录
<Switch>
<Route path='/admin' render-{(routeProps)=>{
	{/*
		判断是否登录，登录了可以访问，否则不能访问
	*/}
	const token = localStorage.getItem('token');
	if(token){
		return <admin {...routeProps}>
	}else{
		return <Redirect to="/login">
	}
}}
	
	/>
</Switch>

利用redux处理登录页
from表单组件写登录页面
mock登录数据
api请求dologin
创建store
// 创建store
import { createStore, applyMiddleware, compose  } from 'redux'
import rootReducer from './rootReducer'
import thunk from 'redux-thunk'
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const store = createStore(
  rootReducer,
  composeEnhancers(applyMiddleware(thunk))
)

export default store

# rootReducer.js
import { combineReducers } from 'redux'
import msgReducer from './msg/reducer'
const rootReducer = combineReducers({
  msg: msgReducer
})

export default rootReducer
```

```
无论使用vuex还是redux管理公共数据的时候  当这个公共的数据是需要请求ajax的时候， 这个请求我们应该写在redux或者vuex里面  vuex在action  redux在actionCreators里面创建
```

