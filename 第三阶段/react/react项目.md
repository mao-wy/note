# 一、项目基本结构

## 创建

```
npx create-react-app my-app
cd my-app

cnpm i react-router-dom redux axios react-redux antd -S

cnpm i @craco/craco bable-plugin-import -D


```

```
react-router-dom --  用于React路由器的DOM绑定。

redux -- Redux 的适用场景：多交互、多数据源

react-redux -- Redux 的作者封装了一个 React 专用的库 React-Redux - 将所有组件分成两大类 - UI 组件（presentational component）和容器组件（container component）。

antd  -- antd 是基于 Ant Design 设计体系的 React UI 组件库，主要用于研发企业级中后台产品。

axios -- Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

@craco/craco  --  环境配置

bable-plugin-import  -- 按需引入


```



## 配置craco

创建craco.config.js

```
const path = require('path')
module.exports = {
  devServer:{
    port: 9527,
    open: true,
    proxy:{
      "/api":{
        target:'http://rap2api.taobao.org/app/mock/270164',
        changeOrigin:true,
        pathRewrite:{   
          '^/api':'/api'
        }
      }
    }
  },
  babel:{
    plugins: [
      ['import', {
        libraryName: 'antd',
        libraryDirectory: 'es',
        style: 'css'
      }, 'antd']
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

那pathRewrite是用来干嘛的呢，这里的作用，相当于是替代‘/api’，如果接口中是没有api的，那就直接置空，就像我截图的一样，如果接口中有api，那就得写成{‘^/api’:‘/api’}，可以理解为一个重定向或者重新赋值的功能。

```

更改package启动项

```
将：
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  改为：
    "scripts": {
    "start": "carco start",
    "build": "carco build",
    "test": "carco test",
    "eject": "react-scripts eject"
  },
```

src -- index.js --App.js

## 项目路由设计

```
dashBoard  -- 仪表盘
文章列表 --  artlist
添加文章 --  artadd
编辑文章 --  
设置
登录
404
没有权限
```



### 建立文件夹

routes  --  存放路由配置

pages  --  存放页面

api  --   存放请求

components  --  存放公共组件

utils  -- 存放工具



### 1、在文件夹routes里定义路由

// 路由单独管理  管理业务相关





```
import __  from '__'
const	adminRoutes = [
{
	path:'/__',
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

### 5、在components创建公共组件layout

​		  （ this.props.children ）

### 6、将二级路由引用在admin

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
```

# 二、mock数据

开发时前后端同时进行  ，前端开发的时候后端接口还没有  此时需要我们自己mock假数据

注意：地址要和后端接口一致

## 本地mock

```
mock.js
安装:npm i mackjs -D
在src下创建mock文件夹，（开发完成后要删除）
```

```
const Mock = require('momckjs')
Mock.mock('url',{})

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

react中引入再undex.js中 import "./mock/index"
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

## 配置http代理 proxy

