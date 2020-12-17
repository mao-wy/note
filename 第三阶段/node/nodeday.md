# day1



##  nodejs是什么

```
二阶段：
	浏览器内核：  内容排版引擎  脚本解释引擎(js)  chrome webkit  脚本解释引擎  v8
	定义：运行在浏览器中的脚本语言  错误的**  js运行在浏览器（浏览器内核中的js解释引擎）
nodejs:基于chrome v8引擎 的 运行环境 （独立安装的chrome v8引擎）
	为js 提供了 一个独立的运行环境
js运行在网页：
	es
	dom bom（运行环境给他的 浏览器）
	操作数据库  读取硬件 读取本地文件  不可以（所有的浏览器端的东西都是不安全的）
nodejs运行js
	es基础语法
	操作数据库
	读取本地文件....
```

## node特点

事件驱动  并发

非阻塞的IO （input output） 异步（高并发） python

## npm nodejs 包管理器  node package manager

```
npm init 初始化  package.json(项目配置文件)
npm install 包名[@版本][-g] --save-dev --save
					[-D]    [-S]
npm uninstall 包名[-g]


npm install 
package.json
	
```

### 如何切换npm 源

```
nrm  
	nrm ls
	nrm use
```

### yarn

```
包管理器
安装

	npm i yarn -g
yarn init  package.json
yarn add 包名  默认是生产环境依赖  npm i 包名 -S
yarn add 包名 --dev 开发环境依赖
yarn global add 包名 全局安装

yarn remove [package] 移除一个包

安装项目全局依赖
	npm install npm i
	yarn 
	yarn add
```

## 模块化

```
模块分类：
	node 自己模块
	第三方
			require("模块名")
	自定义
		哪怕路径是当前目录 也不能直接写 模块名./
	注意：
		当我们引入一个模块 直接写模块名时，nodejs先从 系统模块中 查找，如果不是，查找node_modules,如果有则返回node_modules里面模块，如果没有报错
	
commonjs:
	1,一个文件就是一个模块
	2，暴露接口
		a.js
		exports.a=10;
		exports.b=20;
		module.exports
		b.js
		const json = require("./a")  {a:10,b:20}
	
		
```

### node就是运行环境 repl

```
在命令行运行  
	node 文件名
```

## node常用模块 系统模块

```
fs模块
	文件夹操作 目录操作 读 写 重命名 删 （crud）
	
	
目录
	读
fs.readdir
fs.readdirSync
	创建
fs.mkdir
fs.mkdirSync
	重命名
fs.rename()
fs.renameSync
	删除
fs.rmdir
	无法删除  目录中有内容的文件夹
fs.rmdirSync

文件：file
	curd
	读文件  
fs.readFile
fs.readFileSync
	创建
fs.writeFile()
fs.writeFileSync
	更新
fs.appendFile()
fs.appendFileSync()
	删除
fs.unlink(path,cb);
	
buffer 二进制的数据流
```

### nodejs全局变量

```
console.log(__dirname);
//返回的是当前文件所在的 目录的绝对路径
console.log(__filename);
//返回的是当前文件的完成路径  绝对路径

console.log(process.cwd());
// nodejs 运行目录
```

### path模块

```
path.join("/a","/b","/c")
单纯拼接   结果/a/b/c
path.resolve("/a","/b","./c","d")
解析每一个路径 拼接为一个绝对路径
结果：
	C:/b/c/d
```

url模块

```
https://www.baidu.com/a/b/c?us=xiaoming&ps=12345#123
协议  	主机
		域名、端口    path
				pathname search 
parse
format
```

### 案例

```
简易爬虫：
	1，爬取目标网站内容
		http://sc.chinaz.com/tupian/rentixiezhen.html
		在通过后台 发送一个get请求
	2，解析内容 得到 想要的数据
		第三方库  cheerio
		
		
```

```
const http = require("http")
const cheerio = require("cheerio")
http.get('http://sc.chinaz.com/tupian/rentixiezhen.html', (res) => {
    let arr = [];
  let str = "";
  res.on('data', (chunk) => { 
    str+=chunk.toString();
   });
  res.on('end', () => {
  
    // 分析数据
    let $ = cheerio.load(str);
    $("img").each(function(i,el){
        arr.push( $(el).attr("src2") );
    });
    console.log(arr);
  });
}).on('error', (e) => {
  console.error(`出现错误: ${e.message}`);
});
```



### 技术博客

- csdn
- 简书
- segmentfault 思否
- 掘金

# day2

## express   （koa、egg.js）

### 项目开发模式

```
mvc 模式  开发模式 （思想、并不是具体的代码）
m:model  数据 （封装好的函数，将数据库数据拿过来，处理一下）
v: view  视图 （前端写好的html/css）
c:controller 控制器 （主要核心逻辑代码（处理前端请求 处理视图和model之间的关系））

这种模式下：前端 只写静态页（变态后端、偶尔也要写模板）

混合开发

前后端 分离
  请求的是前端路由，路由返回某个 组件（html） ，在这个页面中发送ajax请求给后端，某个数据接口
  后端提供数据
  此种模式下好处：
    mvc不好
    1，mvc 下前后台代码杂糅一起，开发时，比较混乱，也容易扯皮（前端一定吃亏）
    2，开发效率不高
  前后台分离好处：
    1，代码 分离的 代码结构清晰 开发同时进行的效率快，且不会扯皮
        接口文档（一般是后端提供的，有某些公司，后台比较low 就用一些软件，如：postman）
        后台 你发送什么地址，传什么参数，我返回给什么内容（具体字段）
     
      接口 设计 需要遵循一定标准吗？
        restful
      规范：
        规定了 不同 模式下的请求方法 
        以及后端返回给前端的数据格式
        1，不同 模式的 请求方法 （get、post） put delete 
          get 用于获取数据
          post 用于提交数据
          put 用于更新数据
          delete 用于删除数据
        2，后台返回非前端的 数据格式
        {
          code:200,// 自己定义的标准 0成功了 -1 xx错误 -2xxx错误  也可以直接返回http状态码
          msg:"消息", // 登录成功
          data:{
            list:[],
            total:200,
            page:2
          }
        }
```

## express

nodejs的框架 基于nodejs原生的代码做了一层封装
nodejs框架： koa2 sail.js egg.js

### express 如何启动一个服务器

nodejs  不止是js运行环境，本身就是一个服务器（apache）

```
npm init
npm install express -S

根目录下新建 一个js app.js

  const express = require("express")
  const app = express()


  app.listen(端口,callback)


express 原生路由

  app.get(path,callback)

  app.get("/home",(req,res)=>{
    //返回给前端数据
    res.send({
      code:xx,
      msg:xx,
      data:xx
    }) //可以是字符串、也可以是json或者数据
  })

  app.post("/home",(req,res)=>{
    //返回给前端数据
    res.send({
      code:xx,
      msg:xx,
      data:xx
    }) //可以是字符串、也可以是json或者数据
  })

  ?问题？
    服务端代码只要修改 都要重新启动？怎么解决
    nodemon 解决问题

    npm i nodemon -g
    在启动 项目是  不再使用 node app.js
    而是 nodemon app.js
    每一次都在 控制器 nodemon app.js 也比较麻烦容器单词写错
    可以将经常使用的命令 放在 package.json 的scripts字段

    {
      "name": "express1",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
        "start":"nodemon app.js"
      },
      "author": "",
      "license": "ISC",
      "dependencies": {
        "express": "^4.17.1"
      }
    }
    如何运行 scripts中的命令
    npm run 名字

    使用云 笔记  有道云 印象笔记 

    看技术博客
      csdn 博客园

      segmentfault 
      简书

      掘金  （质量比较好 vue学完）

    写的post接口 如何请求？

    <form>
      action 定义提交地址
      method 提交方式
      enctype 提交数据类型
        默认是："application/xxx-xxx-urlencoded" 
    如何表单提交文件
      input type="file"
    此时 form 
      enctype="multipart/form-data"
      二进制的数据流

```

### postman 用于 做接口测试

### 中文网 http://www.expressjs.com.cn/

### express 路由解析 get 请求的参数

解析get请求的参数

```
app.get("/home",(req,res)=>{
  req.query 就是get请求的参数 是json格式
})

```

### express中间件

功能 劫持（拦截），路由请求，可以做一些其他事情
语法：

  ```
    epxress中间件语法 就是一个函数
    三个参数
      req,
      res,
      next

    全局中间件
    局部中间件  
    取决于你如何使用这个中间件

    全局中间件
      app.use("/",中间件)
      简写
      app.use(中间件)
    局部
      app.use(路径,中间件)
        只有你访问的地址 是 中间件拦截的地址 才起作用
      局部中间件 如何定义多个

      app.use(路径,fn,fn,fn,fn....)
  ```

### post 请求参数的解析

```
npm i body-parser

const bodyParser = require("body-parser")

app.use(bodyParser.urlencoded({extended:false}))
app.use(bodyParser.json())
```

### 静态资源中间件  需要下载 express 内部就有

```
app.use(express.static(path))
path:监听哪个目录 这个目录就是根目录
 源/资源路径 访问

```

### 案例 注册接口

# day3

### express 路由中间件

```
express 本身就有路由
app服务实例上
app.get()
app.post()

如果用原生路由不好的地方？
  原生路由 需要 依赖 实例而存在，导致 我们所有的路由都需要些在 app.js上，会导致代码难以维护

路由中间件

let router = express.Router()

router.get()
router.post()
router.use()

相当于一个小型 的app（服务）

在开发过程中 一般 将不同页面的路由 单独 拿到外部的文件夹中
每个页面的 路由 单独创建一个模块来处理，然后在 app.js中使用
```

### express 模板引擎

我们使用 get请求 访问 /home   home html页面 在页面加载
                      /news   news html页面 浏览器上加载

返回html同时， html中的数据 （从数据库取数据）
拿到数据 还需要渲染

模板引擎 主要功能在于 渲染页面（取代了传统了 拼接字符串）

常见的nodejs 模板引擎有哪些
  ejs   温和的 非破坏式 （内部写法跟html是一样的只是赋予了html 一些渲染的功能，如循环，条件判断，引入公共模板....）
  pug    侵入式 （破坏式） 破坏了html 原有的写法

  jade

  模板引擎  渲染页面的（html写循环，判断...）
使用步骤：
  npm i ejs 

  在app.js中

  app.set("views","./views")
    //告诉服务 视图 文件 放在 views目录中
  app.set("view engine","ejs")

  views
    index.ejs  里面代码 就是html代码

  如何使用
    home.js home路由中

    router.get("/home",(req,res)=>{
      res.render("index")
    })

其他功能？  使用模板引擎 是为了 方便 渲染页面
  res.render(模板,{
    //携带数据
    arr:[1,2,3,4],
    title:"我是标题"
  })
ejs语法：
  <%%>
  在内部 可以写任意的js 代码，注意 将js 用 <%%>包裹起来就可以

  ejs <%=%> 默认不会解析  html字符串

  <%- 变量 %>  会解析 字符串 会html

  引入公共的模块
  <%- include("head") -%>


### 在服务器环境下 路径

```
  应该使用绝对路径 
  如果使用相对路径 弄清楚 路径规则（可能有问题）

  / 放在最前面 绝对路径 代表 服务器监听的根目录
  在我们当前代码中 / 自动进入 public

  

```

### 安装mongodb

```
  点击安装
    注意 mongodb compass 千万不要勾选

  如何启动mongodb
    1，假设你安装在 D:mongo
      1，直接进入这个目录，进入 bin目录 在这个目录下 打开命令行，运行mongod
      2，想任意位置 启动mongodb
          需要添加环境变量
            计算机->属性->高级系统设置->环境变量->系统变量->path
             环境变量语法 ;分割的多个环境变量
              末尾加一个 ; D:mongo\bin\添加进入 就可以了
        此时 win+r cmd 打开 默认 c盘 报错  
          此时 数据库 自动放在 c盘 下 data目录中的 db目录
          但是我们没有这个目录，需要手动创建来解决

```

回顾

```
中间件

  app.use("路径",中间件)

  const  middleware = (req,res,next)=>{
    next()
  }

  app.use("/",middleware) 可以省略 app.use(middleware)

  app.use("/home",fn,fn,fn)

  常用中间件
    body-parser  解析 post请求的参数

    const bodyParser = require("body-parser")

    app.use(bodyParser.urlencoded({extended:false}))
    app.use(bodyParser.json())
  
    app.use(express.static("./public"))  静态资源

    路由中间件

      const express = require("express");
      const router = express.Router();

      router.get()
      router.post()
      router.use()

    模板引擎 ejs

    npm i ejs 

    app.set("views","./views")

    app.set("view engine","ejs")

    res.render("名字",{
      title:xxx
    })


  ejs:
    <% %> 在里面可以写 任意js代码

    <%= %> 这是赋值
    <%- %> 解析为html
    <%- include("header") -%> 引入公共模板

    / 定位静态资源根目录
```

### multer  上传文件的 中间件

```
  <form action="" method="post" enctype="multipart/formdata">
    <input/>
  </form>

  

```

### ajax上传文件

```
formdata （容器中放的数据格式是 multipart/formdata）的格式
容器：放表单的数据

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>

<body>
  <!-- <form action="/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="img">
    <input type="submit"/>
  </form> -->
  <input type="file" id="file">
  <hr>
  <img src="" alt="">
  <button id="btn">上传图片</button>
  <script src="/lib/jquery.js"></script>
  <script>
    // 创建一个 formdata容器（用于ajax 上传 multipart/formdata格式的数据）
    let formdata = new FormData();

    $("#file").change(function () {
      //  事件源 下 files属性
      console.log(this.files[0]);
      formdata.append("img", this.files[0])
   /*    let reader = new FileReader();
      reader.readAsDataURL(this.files[0]);
      // 读取文件 是异步的
      reader.onload = function () {
        console.log(this.result);
        $("img").attr("src", this.result)
      } */
    })
    $("#btn").click(function () {
    
      let file = formdata.get("img");
      if (!file) {
        alert("请选择要上传的文件");
        return;
      }
      // 发送ajax
      $.ajax("/upload", {
        method: "POST",
        dataType: "json",
        processData: false, //必须 缓存关了
        contentType: false, //必须 关闭 设置 数据格式（以 数据本身格式为准 xxxxx urlencoded）
        data: formdata,
        success: (res) => {
          $("img").attr("src",res.data)
        },
        error:(err)=>{
          console.log(err);
        }
      })
    })
  </script>
</body>

</html>
```

### mongodb

```
非关系型数据  存储数据格式 就是json  mongodb redis

关系型数据：mysql oracle

对比
        关系型                               非关系型

        数据库（database） （数据仓库）       数据库（database）
        表     (table)                        collection 集合  [{},{},{}]
        行     row                            document 文档 json
        列 字段  col                           key
```

### mongodb 原生sql 语句

```
show dbs  查看有哪些数据库
use 数据库 切换到这个数据库
show collections 查看当前数据库下 有哪些 集合 （前提 你得先切换到目标数据库）
一下 所有的操作 都需要切换到 目标数据库才能执行

db.createCollection("名字")  创建一个集合
db.集合名.drop()
以上命令 都不重要（这些操作 我们一般会在可视化软件中操作）

下面 增删改查 才是重点

```

### robot 3T mongodb可视化

## 查看文档

最简单查看文档的方法就是`find()`，会检索集合中所有的文档结果

```js
db.集合名.find()  
```

要以格式化的方式显示结果，可以使用`pretty()`方法。

```js
db.集合名.find().pretty()
```

### 1.固值寻找

寻找age集合里面所有含有属性值为wscats的文档结果，相当于`where name = 'wscats'`

```js
db.age.find({name:"wscats"})
```

### 2.范值寻找

操作	语法	示例	等效语句
相等	{:}	`db.age.find({"name":"wscats"}).pretty()`	where name = 'wscats'
小于	{:{$lt:}}	`db.age.find({"likes":{$lt:50}}).pretty()`	where likes < 50
小于等于	{:{$lte:}}	`db.age.find({"likes":{$lte:50}}).pretty()`	where likes <= 50
大于	{:{$gt:}}	`db.age.find({"likes":{$gt:50}}).pretty()`	where likes > 50
大于等于	{:{$gte:}}	`db.age.find({"likes":{$gte:50}}).pretty()`	where likes >= 50
不等于	{:{$ne:}}	`db.age.find({"likes":{$ne:50}}).pretty()`	where likes != 50

### 3.AND和OR寻找

#### AND

在find()方法中，如果通过使用`，`将它们分开传递多个键，则mongodb将其视为**AND**条件。 以下是AND的基本语法

寻找`_id`为1并且`name`为wscats的所有结果集

```js
db.age.find(
{
   $and: [
      {"_id": 1}, {"name": "wscats"}
   ]
}
)
```

#### OR

在要根据OR条件查询文档，需要使用`$or`关键字。以下是OR条件的基本语法

寻找`name`为corrine或者`name`为wscats的所有结果集

```js
db.age.find(
{
   $or: [
      {"name": "corrine"}, {“name“: "wscats"}
   ]
}
)
```

#### AND和OR等结合

相当于语句`where title = "wscats" OR ( title = "corrine" AND _id < 5)`

```js
db.age.find({
$or: [{
 "title": "wscats"
}, {
 $and: [{
   "title": "corrine"
 }, {
   "_id": {
     $lte: 5
   }
 }]
}]
})
```

## 插入文档

文档的数据结构和JSON基本一样。
所有存储在集合中的数据都是BSON格式。
BSON是一种类json的一种二进制形式的存储格式,简称**Binary JSON**。

要将数据插入到mongodb集合中，需要使用mongodb的`insert()`或`save()`方法。

```js
db.集合名.insert(document)
```

比如我们可以插入以下数据

```js
db.wscats.insert({

title: 'MongoDB Tutorials', 
description: 'node_tutorials',
by: 'Oaoafly',
url: 'https://github.com/Wscats/node-tutorial',
tags: ['wscat','MongoDB', 'database', 'NoSQL','node'],
num: 100,
})
```

也可以支持插入多个，注意传入的是数组形式

```js
db.wscats.insert([{
_id: 100,
title: ‘Hello’
},{
_id: 101,
title: ‘World’
}])
```

在插入的文档中，如果不指定_id参数，那么mongodb会为此文档分配一个唯一的ObjectId
要插入文档，也可以使用`db.post.save(document)`。如果不在文档中指定_id，那么`save()`方法将与`insert()`方法一样自动分配ID的值。如果指定_id，则将以save()方法的形式替换包含**_id**的文档的全部数据。

```js
db.wscats.save({
_id: 111,
title: 'Oaoafly Wscats', 
})
```

## 更新文档

### 1.update()方法

寻找第一条title为wscats的值，并且更新值title为corrine和age为12

```js
db.age.update({
'title': 'wscats'
}, {
$set: {
 'title': 'corrine',
 'age': 12
}
})
```

默认情况下，mongodb只会更新一个文档。要更新多个文档，需要将参数`multi`设置为true，还可以配合find方法里面的各种复杂条件判断来筛选结果，然后更新多个文档

寻找所有title为wscats的值，并且更新值title为corrine和age为12

```js
db.age.update({
'title': 'wscats'
}, {
$set: {
 'title': 'corrine',
 'age': 12
}
}, {
multi: true
})
```

### 2.save()方法

将`_id`主键为3的文档，覆盖新的值，注意`_id`为必传

```
db.age.save({
'_id':3,
'title': 'wscats'
})
```

## 删除文档

删除主键`_id`为3的文档，默认是删除多条

```js
db.age.remove({
'_id':3
})
```

建议在执行`remove()`函数前先执行`find()`命令来判断执行的条件是否正确

如果你只想删除第一条找到的记录可以设置**justOne**为1，如下所示

```js
db.age.remove({...},1)
```

全部删除

```js
db.age.remove({})
```

### Limit与Skip方法 (结合find方法使用)

#### Limit

如果你需要在mongodb中读取指定数量的数据记录，可以使用mongodb的Limit方法，`limit()`方法接受一个数字参数，该参数指定从mongodb中读取的记录条数。

```js
db.age.find().limit(数量)
```

#### Skip

我们除了可以使用`limit()`方法来读取指定数量的数据外，还可以使用`skip()`方法来跳过指定数量的数据，skip方法同样接受一个数字参数作为跳过的记录条数。

```js
db.age.find().limit(数量).skip(数量)
//skip()方法默认值为0
current: 当前页
pageSize:一个多少条

10
1  0 9
2  10 19
3  20 29

db.age.find().limit(pageSize).skip((current-1)*pageSize)
```

所以我们在实现分页的时候就可以用limit来限制每页多少条数据(一般固定一个值)，用skip来决定显示第几页(一个有规律变动的值)

### 排序

在mongodb中使用使用`sort()`方法对数据进行排序，`sort()`方法可以通过参数指定排序的字段，并使用1和-1来指定排序的方式，其中1为升序排列，而-1是用于降序排列。

1	升序排列
-1	降序排列

```js
db.集合名.find().sort({键值(属性值):1})
```

把`age`集合表重新根据`_id`主键进行降序排列

```js
db.age.find().sort({
"_id": -1
})
```

# day4

## 利用mongodb（nodejs） 插件 连接 mongodb  了解 不推荐

```
 内部写的是mongodb 原生的sql语法
 为什么不推荐：mongodb是非关系型数据库，对于数据不做验证，在实际开发过程中是可取的
 
 // Requires official Node.js MongoDB Driver 3.0.0+
    var mongodb = require("mongodb");

    var client = mongodb.MongoClient;
    var url = "mongodb://localhost:27017/";

    client.connect(url,{useUnifiedTopology:true}, function (err, client) {
        if(err){
            client.close();
        }
        var db = client.db("test");
        var collection = db.collection("stu");

        var query = {
            age:{
                $gt:20
            }
        };

        var cursor = collection.find(query);

        cursor.forEach(
            function(doc) {
                console.log(doc);
            }, 
            function(err) {
                client.close();
            }
        );

        // Created with Studio 3T, the IDE for MongoDB - https://studio3t.com/

    });


```

## 利用 mongoose 连接mongodb 重点

```
 nodejs库 封装好很多的方法 用于连接或者操作数据库
 对于mongodb 数据库中的数据做了类型的验证  ***

 typejs  超集

 http://www.mongoosejs.net/docs/
 中文文档

连接：
	



 两个 重要概念

 Schema  计划  映射 了 一个 collection 里面所有的字段，对于字段做了 类型验证
    const mongoose = require("mongoose")
    const { Schema } = mongoose;
    定义个schema
    const cateSchema = new Schema({
        cateName:{
            type:String,
            required:true
        },
        cateIcon:String,
        pid:{
            type:Number,
            required:true
        },
        showHome:{
            type:Number,
            default:1
        }
    });
 model    collection的实例（collection），定义时需要传入 他的 Schema
    下 提供了很多的方法 用于操作 sql语句
    定义model
    const cateModel = mongoose.model("qf_cate",cateSchema)
    命名最好使用下划线命名法

model下的方法

```

### 启动后台管理项目

任意一个项目 都最少有两个平台甚至更多
layui 适用于 传统开发（mvc）模式下 后台管理项目

## express生成器 自动生成MVC项目结构