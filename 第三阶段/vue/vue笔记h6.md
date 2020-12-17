## 1、Vue

Vue 用于构建用户界面的MVVM  的渐进式框架

## 2、vue特点

+ MVVM

M -> V    VM

数据  改变  视图自动刷新  （通过vm来刷新  vue  new  vue  返回实例）

+ 渐进式

核心库  体积很小  （ 没有把所有的功能都  挂载核心库上 ）

通过插件的形式将插件所携带的功能挂载还Vue上

+ 声明式开发

声明式  （ 高级计算机  想要什么  怎么做 我不管 ）

命令式 （ 将过程中需要运行的每一个指令一步一步告诉高级计算机  运行 ）
## 3、vue雏形
```js
  //创建一个Vue实例  就是vm
  const vm = new Vue({
    el:"#app"  // element缩写  挂载实例  #app元素  内部就可以使用vue代码了
    data:{ //data属性  是  放组件的数据的
      msg: 'hello Vue'
    },
    methods:{ //  存放当前实例的  方法
      act: () {  //  注意  方法必须这样写  es5写法
        return  111
      }
    }
  })
  // 挂载  实例  除了  可以通过 el 属性 和可以在外部挂载
  const vm = new Vue({
    data:{
      msg:"你好a"
    }
  }).$mount(document.querySelector("#app"))
  // vm.$mount(document.querySelector("#app"))

```
## 4、vue中的模板语法 {{}}
ejs中 <%= %>  <%- %>  <% %>  ejs模板渲染语法
```js
  {{  }}  写在  view 视图中的  渲染数据
  // 语法
  1，{{}}
  可以 直接 使用或者调用  实例的属性或者方法  {{}}里面的变量或者 方法调用  会自动去实例上找 ， 如果实例上没有  会报错
  2，{{}} 中式 js环境 且最好式表达式  {{}}  写在视图中  数据渲染的， 如果调用方法，那么这个方法最好有返回值，否则不会渲染内容  （ 不能写if结构 ）
  3，全局方法  vue {{}}  模板 创建一个白名单  白名单中的全局方法  是可以在{{}} 中使用的
  'Infinity,undefined,NaN,isFinite,isNaN,' 
    'parseFloat,parseInt,decodeURI,decodeURIComponent,encodeURI,encodeURIComponent,' 
    'Math,Number,Date,Array,Object,Boolean,String,RegExp,Map,Set,JSON,Intl,' 
    'require'
    特点：1，大部分处理数据  2，都有返回值
```

## 5、vue 学习思路
抛弃 二阶段  操作dom 开发习惯
vue 以数据驱动 （MVVM）

## 6、指令 directive
扩展了 标签（组件） 的属性的功能
```
<组件 v-指令名="值"/>
注意  指令所在  引号中  是一个js环境、且只能写表达式  不要写语句
      变量会自动识别  实例上的属性 或者放法
      全局的  有白名单

v-model  将表单控件的value 与  一个变量  进行双向绑定
      表单控件： 文本框、密码框、单选、多选、下拉、文本域
v-text   将元素的 文本内容 与 一个变量 进行绑定  （不重要）
v-html  渲染富文本数据
v-bind:属性  （属性 是标签的普通属性  包括自定义属性）
          将组件或者标签的普通属性 变成 动态属性  （属性的值 会和 一个实例中的数据进行双向绑定）
    简写：
    :属性
v-on:事件  vue中  定义 元素的事件 事件函数  会与 vue实例上的方法  进行绑定
简写：
  @事件

两种事件函数绑定方式
    <button @click="fn">按钮</button>
    fn是事件函数  第一个参数  就是事件对象
    <button @click="fn()">按钮</button>
    fn()不是事件函数 只不过 按钮点击时 会触发 使用场景 需要自定义传参时

    如果 通过<button @click="fn($event)">按钮</button> 方式绑定  可以在调用时 传入 预定义好的  变量$event   就是事件对象 
```

## 7、vue事件修饰符
修饰 绑定事件的
@事件.修饰符
v-on:事件.修饰符
```js
.stop  //阻止事件冒泡
.prevent //阻止默认事件
.capture  //捕获阶段就提前触发
.self   //当前元素所绑定的事件  只能由自己触发
.once  // 只触发一次
```

## 8、条件渲染
```js
v-if  有条件的渲染一个元素 条件为false时 ，此元素 不在dom树（不渲染）
v-else  必须时 v-if所绑定的下一个兄弟元素  此时  当 v-if 条件为false时 显示，为真时消失（相当于 if..else..结构）

v-else-if  多分支  注意： 必须是连续的兄弟元素  且  if是第一个兄弟 v-else是最后一个  中间是else-if
```
## 9、v-show 控制元素的 显示、消失状态

用法	同 v-if

对比  v-if

相同点：都能改变元素的（显示、消失）切换状态  且值为真显示、false消失

不同点：

​	1、v-show是改变元素的 display切换   v-if 是直接移除元素

​	2、当初始值为false时  v-if所绑定的元素是惰性的（不加载）

使用场景：

​	v-show 有更高的 切换性能

​	v-if	当初始值为false时，且不频繁   使用v-if



## 10、列表渲染  循环

循环渲染

```html
<ul>
    <li v-for="el in arr"></li>  // 普通循环数组
</ul>
<ul>
  <li v-for="(el,index) in arr"></li>  //循环数组加下标
</ul>
<ul>
  <li v-for="el in obj"></li>  // 循环对象
</ul>
<ul>
  <li v-for="(el,key,index) in obj"></li>  // 循环对象  +  key
  el   //属性值
  key  //  属性名
  index  //  下标
</ul>
```

## 11、Vue中的class
```
<div :class="a"></div>  //解析成box类
<div :class="[a,b,'box']"></div>   <div class="box box2 box3"></div>
<div :class="{isActive:true}"></div>
<div :class="{isActive:c}"></div>
<div :class="{isActive:1==2}"></div>
    //div是否有isActive类取决于  对象的key的值是否为真
<div :class="[a,b,'box3',{isActive:c}]"></div>
{
  data:{
    a:"box",
    b:"box2",
    c
  }
}
```

## 12、vue中的style
```html
  <body>
    <div id="app">
      <div :style="{width:100*3+'px',backgroundColor:idRed?''red:'blue'}"></div>
    </div>
  </body>
  
  增加了：style对象的写法
  好处：
    值 是一个js环境 里面可以写 js表达式
```

## 13、MVVM原理
  Vue2.x  mvvm  底层api
  object.definepropery()
![](https://cn.vuejs.org/images/data.png)


mvvm原理：
  当我们new一个Vue实例时，都会传入一个data属性（属性的值是对象），data保存了我们当前实例的属性（MVVM中的M-数据），new的时候，Vue会去遍历data对象中的所有的属性，且通过Object.defineProperty,将data中的每一个属性都转换为  getter/setter ,这样之后，我们data对象中所有的数据，都是可追踪的
    每个组件（实例），都有一个watcher（观察者），监测这个组件（实例），中所有的getter/setter  只要其中一个触发，观察者就  调用render函数  生成虚拟dom，
    会跟上一次储存内存中的虚拟dom进行比较（diff算法），得到代价最小的一种方案，更新真实dom

    （每次渲染真实dom之后，都会将对应的虚拟dom保存在内存中，方便进行下一次比较）

https://www.jianshu.com/p/af0b398602bc

虚拟dom是什么？
  用js对象的结构 去描述一段真实的dom树
  在更新真实dom之前  先去比较dom（diff），尽量减少dom操作

  循环中  v-for
   每个循环的元素一定要加一个key属性 独一无二的key属性
  循环中为什么要加key
    diff算法  默认是逐层比较，但是还是会每层一一比较，如果有了key，会逐层按照相同的key进行比较，加快了diff算法比较的效率（减少了比较次数），加快页面渲染效率

## mvvm中的一些问题
1、一个数据没有data中 初始化定义 而是后期动态添加
vue是监测不到他的变化，所以数据改变后不会自动刷新
要求：使用到的数据一定要在data中初始化
定义不同类型的数据的时候  初始值有一定要求
num：0  /如果代表下面 -1
object  null  {}
arr []
string  ''
2、definProperty  捕获不到 数组的某些操作
  哪些操作
    数组[下标] = 值
    直接改变数组的length
    




