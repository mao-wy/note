# day1

李响
15655458016

二个月

1周 node
3周 vue 
2周+ react
微信相关  小程序 app  

框架： 业务逻辑   api封装好 （记住某个api怎么用用来干什么）

## node 是什么

内核：
  内容排版引擎
  js 脚本解释引擎
node: 独立安装的 chrome 内核中的 v8 脚本解释引擎
js 脱离浏览器 运行

es 基础语法
dom  文档对象模型
bom  浏览器对象模型

nodejs中运行的js
es
node 新增api

## nvm   node version manager

管理node版本  
注意：nvm 什么时候用： 使用 一个 框架需要指定版本的node时使用

```js
// 安装 安装包 一直下一步
nvm ls  // 查看 当前系统 可用的node版本
nvm install 版本号  // 安装指定版本的node
nvm use 版本号  // 切换指定版本的node
```

## npm node package manager node 包管理器

```js
// 初始化 项目
npm init [-y/--yes]
// 生成package.json  项目配置文件（包管理文件配置）

{
  "name": "demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {  // 脚本
    "start": "gulp build"
  },
  "author": "",
  "license": "ISC",
  "dependencies": { // 生产环境的依赖 包
    "jquery": "^3.5.1"
  },
  "devDependencies": { //  开发环境依赖包
    
  }

}
// 运行脚本
npm run 脚本名字
// 生产环境 开发环境
dependencies 字段 保存的是  生产 环境所依赖的包
npm install 包名 --save/-S 时 指定 生产环境的包
devDependencies 保存的时开发环境所依赖的包
npm install 包名  --save-dev/-D 时 指定开发环境的包

要求：*****
  装一个包时，一定要加 -S(--save) 或 -D(--save-dev)
注意：
  如果安装一个包 不加 -S 或者 -D,package.json 中不会记录 你装包的信息

npm install // 安装当前项目所有的依赖包（读取package.json中dependencies，devDependencies，去安装对应的版本的包）


npm install 包名[@版本号 --save  --save/dev -g]
    i                    -S         -D     全局安装

npm uninstall 包名[-g]  // 卸载 包

npm info 包  // 查看一个包信息


```

## npm 安装源问题

https://www.npmjs.com/   npm 服务器 可视化页面
npm 官方服务器 是国外的 （npm i安装的源是国外）

+ 使用 nrm 管理npm 源

```js
npm i nrm -g
nrm ls  // 查看 有哪些可用的npm 源
nrm use 源 // 切换npm 源

/* 
  淘宝镜像
  淘宝 做了国内服务器 10分钟 同步一下 npm官方 服务器
*/

```

+ 直接使用 cnpm 命名和npm 一模一样 使用它 源是淘宝的
  https://developer.aliyun.com/mirror/NPM

```js
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

## yarn node 包管理器（官方） yarn.lock  (yarn )

```js
// react 脚手架  yarn
// 初始化项目  生成package.json
npm i yarn -g
yarn init
yarn add 包名 // 安装包到生成环境依赖
yarn add 包名 --dev // 安装到 开发环境的依赖
yarn remove 包名  // 移除包
yarn global add 包名  // 全局安装
yarn // npm install 安装当前项目所有的依赖
```

## nodejs模块化

模块化：
  允许 我们在 一个js中动态 引入 其他的js 文件

+ 系统模块 （nodejs 为我们提供全局api,对应模块中）

```js
const path = require('path') // 引入系统模块 直接使用require引入 即可
// 注意： require直接写 引入模块名
```

+ 第三方模块 （npm下载模块）

```js
npm i xxx模块
// 在js中使用
const 模块 = require('模块名')
// 注意： npm下载模块也是直接写模块名
```

注意：
  当我们require引入一个模块时，直接写模块名时，
  node引入机制：
  1,先去 系统模块中去查询，是不是系统模块
  2，是系统模块，优先引入系统模块
  3，如果不是系统模块，去当前目录下的node_modules目录中去查询同名模块
  4，如果系统模块中没有。node_modules模块也没有，会报错，找不到这个模块

+ 自定义模块

```js
// 接口 一个模块文件 向外提供 别的js 可以引入的 东西
1，一个文件不提供任何接口 那么整个文件 就是一个完整模块 引入后 相当于直接引入 这个js  
  // 注意：不需要定义 一个变量接收返回值
2，一个文件/模块 向外提供多个接口
// 提供接口
a.js
exports.接口名字 = 值
eg:
  exports.a = 20
  exports.b = () => {console.log(111)}
  exports.c = [1,2,3]
// 引入接口
const modulea = require('./a.js') // 一定要加路径 不能直接写模块名
// modulea 是一个对象 对象中有三个属性 分别是 a b c
const { a,b,c } = require('./a.js') // 解构赋值引入也行
3，一个文件 默认只 提供一个接口

module.exports = 值
引入
const a = require('./a.js')

注意：一个文件 如果同时出现exports 和 module.exports 以module.exports为准
```

## node常见的系统模块

node api 设计成异步操作

+ fs
  操作文件
  读文件
  fs.readFile(path[,options],callback)
  fs.readFileSync(path[,options]) 结果 方法返回值
  写文件
  fs.writeFile()
  改文件 追加
  fs.appendFile()
  删文件
  fs.rm()
  fs.unlink()

## url组成

+ 协议名  http/https
+ 主机/源  主机名（域名）+ 端口
+ path 路由   www.baidu.com/a/b/c   /a/b/c path 路径或路由
+ search
  http://www.baidu.com/a/b/c?key=v&k2=v2
  ?key=v&k2=v2 是search
  key=v&k2=v2  query
+ hash 
  url#hash
  hash是 地址 后面的 加 # #xx url的hash值 

# day2

## 全局 变量

__dirname  当前文件所在目录绝对路径

__filename   当前文件所在的 绝对路径
process.cwd() 当前 node  运行目录

## path 模块

```js
Node.js path 模块提供了一些用于处理文件路径的小工具，我们可以通过以下方式引入该模块：
path.join('a','b','c')   // /a/b/c
// 将路径片段 单纯拼接 
path.resolve()  // 生成是绝对路径，解析所有的参数
```

## http模块

nodejs 中用于 发送 请求的模块（get/post请求）

```js
const http = require('http')

http.get(url, (res)=>{
  res.setEncoding('utf8');
  let rawData = ""
  res.on('data', (chunk)=>{
    // 数据过来一次 触发一次
    rawData += chunk
  })
  res.on('end', ()=>{
    // 数据传输完毕后触发
    console.log(rawData)
  })
})
```

## cheerio

以jquery 语法 分析 字符 插件



简书  掘金（app） segmentfault csdn 博客园

## express 

如何使用express启动一个服务器

```js
var a = 'name'

obj = {

[a]:'小明'

}

let obj2 = {
  ...obj
}
let obj2 = Object.assign({}, obj)
```



## express

express 是一个node框架  基于原生nodejs 封装 
https://www.expressjs.com.cn/

nodejs 自己就可以创建一个服务器

express 基本使用

路由： 请求一个地址（要求 参数。。） 返回什么数据

```js
// 创建一个目录
npm init
npm i express -S

// 根目录下创建app.js
const express = require('express')
const app = express()
app.get(path, (req,res)=>{  // 定义一个get路由
  res.send(参数)
  // 参数 可以是 对象 或者字符串
})
app.post(path, (req,res)=>{  // 定义一个post路由
  res.send(参数)
  // 参数 可以是 对象 或者字符串
})
app.listen(9527, ()=> { console.log('start at port 9527') })

```

## nodemon 解决 服务器代码 热加载

```js
npm i nodemon -g

// 启动代码
nodemon 文件路径
```

## 接口测试 软件  api  mvc

post 请求方式 常见传输格式
  multipart/form-data 二进制 数据流  （上传文件）
  application/x-www-form-urlencoded  url编码格式  key=v&key=v  中文会转url编码
  application/json  json格式 传给后台  后端需要配置

## 解析 路由 参数

+ get请求传递参数
  req.query 获取

```js
app.get(path, (req,res)=>{
  // req.query上获取 get请求的参数
  res.send(xx)
})
```

+ post 请求传的参数
  无法直接通过req.body获取的  
  需要处理一下

## express 中间件

渐进式 设计 （框架设计思路）
：
  框架 核心库  功能不多（包含特定 必须要有的功能）
  插件、中间件形式  往核心库 增加新的功能 
  插件、中间件 需要 单独安装的  即插即用 （最大程度节约资源）

中间件： 在 请求 和 响应中间 做一层拦截  （在请求(req)或者响应(res)添加新的 功能）

如何使用
app.use([path], 中间件)

+ 分类
  自定义中间件

```js
// 中间件就是一个函数
const middleware1 = (req, res, next) =>{
  // 在放行之前 可以 往  req上添加一些东西
  next(); // 放行
} 
// 全局中间件
app.use(middleware1) // 自动变成全局中间件
app.use('/', middleware1) // 自动变成全局中间件

// 局部中间件  只拦截 特定 path 中间件
app.use('/home', middleware1)
```

第三方中间件
直接下载使用即可 有特定功能

## body-parser 中间件 解析 post 请求的参数

```js
npm i body-parser -S

body-parser 提供四种解析器
	JSON body parser
	Raw body parser
	Text body parser
	URL-encoded form body parser

const bodyParser = require('body-parser')
// 解析 urlencoded格式的参数
app.use(bodyParser.urlencoded({extended: false}))

// 解析 json格式参数
app.use(bodyParser.json())

```

## restful api 规范  接口定义规范

```js
// 后端返回数据格式 json
{
  code:0, // 自定义业务逻辑编码 不是http状态码
  msg:"消息", //一般用于 前端 弹出消息提醒文本
  data: []/{} // 获取数据时有data
}
// 前端请求方法
获取数据 用 get
提交数据用 post
更新数据 用 put
删除数据 用delete

```

# day3

## 路由中间件

路由中间件创建一个 路由对象
路由对象 相当于 小型的 app
有get方法 有post方法 也有 use方法

```js
routes
  home.js
    const router = require('express').Router()

    router.get()
    router.post()
module.exports = router


// 在app.js中
const homeRouter = require('./router/home')


app.use(homeRouter)
```

好处：定义 路由脱离了 app 全局应用实例
分层定义 路由  提高了 代码的可维护性

## 静态资源的中间件

允许我们 以 当前服务器的 绝对路径 来访问 静态资源（图片 css js 字体图标...）

```js
app.use('/static', express.static(path.join(__dirname,'public')))

```

## 模板引擎

允许我们在模板（html）文件，写 渲染语句 
比如 循环 条件判断 xxxx 
nodejs 
ejs 

```js
npm i ejs -S

app.set("views","./views")
    //告诉服务 视图 文件 放在 views目录中
app.set("view engine","ejs")
  // 告诉应用程序 我们  模板引擎使用的是ejs

路由（渲染页面） 不在 使用 res.send 返回内容
res.render('模板名称',{
  携带参数放这里
})

ejs语法
<%=  %>
输出语法 <%=  %> 
内部
  1，里面是js环境
  2，这里面的变量 只能去找  res.render携带过来的数据
  3，输出 这里面 需要 有值  可以写表达式 不能写语句


<%  %>  在这里面可以写任意的js语句 变量还是只认识 render携带过来

<%- %> 渲染富文本

// 引入公共模块
<%- include("head") -%>

```

## mvc

m model 模型   请求数据数据的函数
v views  视图  （html/css）
c controller 控制器  核心业务代码 （路由）

## multer 中间件  用于上传文件的

```js
npm i multer -S
utils
  upload.js

    const multer  = require('multer')
    const storage = multer.diskStorage({
      destination: function (req, file, cb) {
        cb(null, 'uploads/')
        // 指定 上传的目录的 uploads目录
      },
      filename: function (req, file, cb) {
      
        let fileArr = file.originalname.split('.')
        let extName = fileArr[fileArr.length-1]
        cb(null, file.fieldname + '-' + Date.now()+'.'+extName)
        // 指定文件上传的 文件名
      }
    })
    
    const upload = multer({ storage: storage })

    module.exports = upload


  // 在需要使用的路由中引入
  const upload = require('../utils/upload')

  router.post('/upload',upload.single('img'), (req,res)=>{

  })

```



# nodejs后台项目笔记

基础插件

```
npm i express body-parser mongoose ejs multer -S
express - Express 是一个保持最小规模的灵活的 Node.js Web 应用程序开发框架，为 Web 和移动应用程序提供一组强大的功能。
body-parser - 解析中间件,处理程序之前，在中间件中对传入的请求体进行解析
mongoose - 数据库
ejs - 高效的嵌入式 JavaScript 模板引擎。
multer - 是一个 node.js 中间件，用于处理 multipart/form-data 类型的表单数据，它主要用于上传文件。它是写在 busboy 之上非常高效。
```

创建文件夹

```
文件夹
model	- 数据库创建及连接
	cateModel.js - 创建数据库
	connect.js - 连接数据库
public  - 静态资源 - css  img  等
routes	- 路由
utils	- 工具 
views	- 视图 ejs 

文件
app.js - 
```



connect.js  

```js

var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/dome'); //连接数据库

var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
  // we're connected!
});
```



cateMode.js

```js
// 创建userModel
var mongoose = require('mongoose');
// 创建schema
var Schema = mongoose.Schema;

var blogSchema = new Schema({
  // 数据库表规范
});
// 创建一个 model
var Blog = mongoose.model('Blog', blogSchema);
module.exports = cateModel
```





app.js

```js
const express = require('express')  // 引入express框架
const bodyParser = require('body-parser')	// 引入body-parser解析中间件
const path = require('path')  // 提供了处理和转换文件路径的工具。

const 路由名 = require('路由文件地址')

require('./model/connect') // 运行数据库连接

const app = express()

app.use(bodyParser.urlencoded({extended:false}))	//解析url请求
app.use(bodyParser.json())		// 解析json请求

app.use('static',express.static(path.join(__dirname,'public')))	//连接静态资源

app.set('views', path.join(__dirname,'views'))
app.set('view engine','ejs')  //  解析views里的ejs

app.use(路由名) //挂载

app.listen(3000,()=>{console.log('start at port 3000')})  //设置域名

```



routers   -  文件夹

​			路由示例

​		admin.js

```js
const router - require('express').Router()

router.get('/admin',(req,res)=>{
    res.render('admin')
})

module.exports = router
```

​	cate.js

```js
const router = require('express').Router()
const cateModel = require("../model/cateMOdel")

router.get('/cateList',async (req,res)=>{
    //curr当前页 limit一页多少条
    //当前页
    const curr = req.query.curr?parseInt(req.query.curr):1
})

```



views  - 文件夹

​	admin.ejs

```js
引用layui后台布局
注意问题：
1、侧边导航区域 使用a标签跳转 地址会直接写在地址栏  导致整个页面跳转  iframe失效
解决办法：在a标签内使用--自定义属性 data-href="main"或者_href="/main"
script内  
	$(".link").click(function(){
        $('#frame').attr('src',$(this).attr('_href'))
    })

```

​	catelist.ejs

```js

```

