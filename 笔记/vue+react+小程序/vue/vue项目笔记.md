## 移动端布局原理

rem原理

```
移动端常见布局方案
  1，百分比布局
  2，动态改变 视口缩放比
  3，rem 布局
  4，flex
开发中：flex(布局)+rem（尺寸）
rem原理：
  rem是css3新增的一个单位  代表 根元素（html）字体大小
  适配原理：
    页面首页的元素单位都使用rem，如果我动态改变html字体大小
    ？页面上 所以的元素的 尺寸都会变化

    获取屏幕宽度设置html字体大小 
      设置为 屏幕宽度的1/10
    如果设置尺寸变化 以上设置html字体大小需要重新运行
  
  单位怎么写？
    移动端设计图宽度 是 750px
      750px  1rem 75px
    width: 500 / 75
  设计图的宽度的 1/10 rem单位尺寸 一般是75像素
  如果设计图上量的尺寸是  500
    width: 500/75 rem;


  postCss 
    自动给 浏览器  会css加 浏览器前缀解决 css3兼容
    px2rem
  字体大小：

    pxtorem
    作用：将项目中css的px转成rem单位，免去计算烦恼
    安装：yarn add postcss-pxtorem
    配置：package.json内，在postcss内添加：

      "postcss": {
        "plugins": {
          "autoprefixer": {},
          "postcss-pxtorem": {
            "rootValue": 75, // 设计稿宽度的1/10,（JSON文件中不加注释，此行注释及下行注释均删除）
            "propList":["*"] // 需要做转化处理的属性，如`hight`、`width`、`margin`等，`*`表示全部
        }
        }
      },
  注意：
    如果 希望 某些 单位不自动转换成 rem 可以 使用PX
```

## vant组件按需引入

```
cnpm i babel-plugin-import -D

修改 babel.config.js

```

## 如何引入外部的字体图标

iconfont下载图标

```
assets
  fonts
    iconfont.css
    xxxx

main.js中
  import './assets/fonts/iconfont.css'

组件中使用
  1，
    <i class="iconfont icon-xxx"></i>
  2,使用vant icon组件
    <van-icon name="mine" class-prefix="icon" class="iconfont"/>
    class-prefix定义类前缀 就是 <i class="iconfont icon-xxx"></i>第二个类
```

## 接口base 地址

https://api.it120.cc/conner

### vue.config.js中配置反向代理

```
{
  devServer: {
    port: 3000,
    open: true,
    proxy: {
      '/conner': { // 请求 接口 以 /conner开头，自动匹配
        target: 'https://api.it120.cc', // 目标地址
        ws: false,
        changeOrigin: true, // 开启代理：在本地会创建一个虚拟服务端，然后发送请求的数据，并同时接收请求的数据，这样服务端和服务端进行数据的交互就不会有跨域问题
        pathRewrite: { // 路径重写
          '^/conner': '/conner'
        } // 这里重写路径 （默认 /conner会消失变成target） 路径重写的值 会放在 target后面
      }
    }
    // /conner/a   https://api.it120.cc/conner/a
  }
}
```

## 关于接口函数封装

```
1, 适用于小型项目
  main.js
    import axios from 'axios'

    Vue.prototype.$http = axios

  在组件中
    直接  this.$http.get()请求
2，
  好处：
    统一维护 统一管理 复用
  src
    utils
      封装request

    api
      单独封装请求函数

```

## axios post请求的参数 传递的格式模式是 

```
headers: {
  "content-type": "application/json"
}
某些接口 post参数解析 不支持json格式


怎么解决这个问题
  "key=v&k=v"

  {
    k: v
  }


  qs 
    parse() 解析query 成json
    stringify() 将json 解析成query
npm i qs -S

qs.stringify(params)
  
```