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
    data:{ //data属性  是  放组件的数据的＆
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
  2，{{}} 中式 js环境 且最好是表达式  {{}}  写在视图中  数据渲染的， 如果调用方法，那么这个方法最好有返回值，否则不会渲染内容  （ 不能写if结构 ）
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

## 7、vue事件修饰符和表单修饰符
修饰 绑定事件的
@事件.修饰符
v-on:事件.修饰符
```js
事件修饰符
.stop  //阻止事件冒泡
.prevent //阻止默认事件
.capture  //捕获阶段就提前触发
.self   //当前元素所绑定的事件  只能由自己触发
.once  // 只触发一次
表单修饰符
.lazy   v-model 会变成change事件驱动
.trim   自动去除开头结尾空格
.number 将输入的值自动换成number类型，过程同parseFloat  注意如果这个值无法被parseFloat() 解析  则会返回原始的值
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

## 14、虚拟dom是什么？

  用js对象的结构 去描述一段真实的dom树
  在更新真实dom之前  先去比较dom（diff），尽量减少dom操作

  循环中  v-for
   每个循环的元素一定要加一个key属性 独一无二的key属性
  循环中为什么要加key
    diff算法  默认是逐层比较，但是还是会每层一一比较，如果有了key，会逐层按照相同的key进行比较，加快了diff算法比较的效率（减少了比较次数），加快页面渲染效率

## 15、mvvm中的一些问题
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
    直接改变数组的lengt

​    

## 16、nextTick 解决数据更新，dom更新异步的问题
当我们数据改变后，视图刷新（调用render生成虚拟dom -- 比较 -- 渲染dom）
vue将这个过程设计成了异步，
问题：
    我们在改变数据后，想立即获取最新的dom。获取不了
vue提供一个回调函数  去监听 每一次的数据改变后的渲染的操作 在渲染完成后，回调触发
想要获取最新的dom，应该在改变数据后，调用这个数据
```js
  this.$nextTick(()=>{
    // 在这里获取最新的dom
  })
```

## 17、vue解决闪烁的问题
闪烁问题？
  代码执行顺序从上往下，在vue还没有加载出来时，视图中写 {{}} 模板，
  对于html之前就是字符串，
  等待vue加载完成之后，{{}}会解析成vue模板，数据会编译，会出现闪烁过程
```html
  <style>
    [v-cloak]{
      display:none;
    }
  </style>
  <div v-cloak>
    {{ msg }}
  </div>

  原理：
    vue在加载完毕后，会自动移除页面上 有v-cloak指定的元素的v-cloak这个属性
```
## 18、侦听器

监测vue实例上的数据变化 （监听data中的数据的）
功能：当这个数据变化时，可以监听到（有一个函数会自动运行）
```js
  new Vue({
    data:{
      msg:'a',
      obj:{
        a:10,
        b:20
      }
    },
    watch:{
      // 侦听基础数据类型   直接写同名的方法即可
      msg(newVal,oldVal){  // newval 改变后的新值
      //  当msg改变这个方法会调用
      },
      //  侦听对象
      //  直接侦听对象的某个属性
      'pbj.a'(newVal,oldVal){

      },
      // 侦听对象 深度监听
      obj：{
        handler(newVal,oldVal){},
        deep:true
      }
    }
  })

watch的高级用法
  1，handler（）：当页面刚进入时，自动绑定watch事件，不需要进行触发
  2，immediate属性：布尔值
      immediate：true：首次加载就监听数据变化
      immediate：false：只有发生改变才监听
  3, deep:true；是开启深层次的监听，即所有属性都加上监听器，如果其中一个发生改变了就执行handler函数。
  watch: {// 页面加载时，就自动触发此事件
    nameValue:{
      handler(new){
        this.checkName(value);
        this.tip = "正在验证......";
      },
      immediate: true 
    }
  }
```

## 19、计算属性  computed
使用场景：
    当我们使用一个数据时，不是原样输出，而是需要处理一下再使用，此时可以会用计算属性
    或者，我们希望新增一个属性，这个属性需要基于其他已有的数据，进行处理

```js
{
  data:{
    a:10,
    b:20
  }
  computed:{
    c(){
      return this.a+this.b
    }
  }
}

/*
新增computed属性，在computed属性中，增加 方法（编译出来的是同名的属性，和data中的数据是一样挂载到实例上的）
注意：
  1、不能和data中或者methods中已经存在的属性或者方法同名，
  2、基于依赖进行缓存，在多次使用时，如果依赖没有变，不会重新计算（和methods中方法最大的不同）


*/
```

## 20、methods、watch和computed的区别
```js
1、methods
  不存在缓存，执行一次运行一次，执行n次，运行n次
2、computed：通过属性计算而得来的属性
  使用场景：当页面中的某些数据依赖其他数据进行变动的时候，可以使用计算属性
    1,computed内部的函数在调用时不加()。(可加可不加)

    2,computed是依赖vm中data的属性变化而变化的，当data中的属性发生改变的时候，当前函数才会执行，data中的属性没有改变的时候，当前函数不会执行。

    3,computed中的函数必须用return返回。

    4,在computed中不要对data中的属性进行赋值操作。如果对data中的属性进行赋值操作了，就是data中的属性发生改变，从而触发computed中的函数，形成死循环了。

    5,当computed中的函数所依赖的属性没有发生改变，那么调用当前函数的时候会从缓存中读取。

    6,每个计算属性都包含两个set、get属性

3、watch：属性监听   
  使用场景：数据变化时执行异步或开销较大的操作，可以随时修改状态的变化  当一条数据的更改影响到多条数据的时候---------搜索框
    watch：类似于监听机制 + 事件机制。

    1,watch中的函数名称必须要和data中的属性名一致，因为watch是依赖data中的属性，当data中的属性发生改变的时候，watch中的函数就会执行。

    2,watch中的函数有两个参数，前者是newVal，后者是oldVal。

    3,watch中的函数是不需要调用的。

    4,watch只会监听数据的值是否发生改变，而不会去监听数据的地址是否发生改变。也就是说，watch想要监听引用类型数据的变化，需要进行深度监听。“obj.name”(){}------如果obj的属性太多，这种方法的效率很低，obj:{handler(newVal){},deep:true}------用handler+deep的方式进行深度监听。

    5,特殊情况下，watch无法监听到数组的变化，特殊情况就是说更改数组中的数据时，数组已经更改，但是视图没有更新。更改数组必须要用splice()或者s e t 。 t h i s . a r r . s p l i c e ( 0 , 1 , 100 ) − − − − − 修 改 a r r 中 第 0 项 开 始 的 1 个 数 据 为 100 ， t h i s . set。this.arr.splice(0,1,100)-----修改arr中第0项开始的1个数据为100，this.set。this.arr.splice(0,1,100)−−−−−修改arr中第0项开始的1个数据为100，this.set(this.arr,0,100)-----修改arr第0项值为100。

    6、immediate:true 页面首次加载的时候做一次监听。
```
 ### 计算属性 能被修改么？
    计算属性不能直接修改，应该修改依赖，修改依赖后，计算属性会自动重新计算
    如果想修改结算属性，修改依赖应该用set(){}修改
```js
  data:{
    num:10
  },
  computed:{  
    num2:{ // 变成对象 get是获取num2的拦截  return的值就是获取的值
      get () {
        return 
      },
      set ( val ) {
        set是设置 这个计算属性时的拦截 set参数就是你设置的值（methods中改变的）， 应该在set中 改变依赖的值 
      }
    }
  }
```
## 21、组件
  组件化
  组件是广义概念：
    代表页面上独立的组成部分
      （div/span/xxx  属于html自带组件）
  Vue中的组件
  注意：
    1，组件  也是Vue的实例（显示 new Vue），所以 实例有的属性和方法组件都有
    2，每个组件都有自己的作用域，内部的数据方法 只能在自己内部使用
    3，不管是全局组件还是局部  都需要在Vue控范围内使用
      new Vue实例，将Vue语法挂载到 页面上的某个内容

+ 全局的组件  在任意一个地方或者其他组件中都可以使用的
```js
  Vue.component(组件名,options)
  // eg
  const CommonHead = {
    // 这是当前组件的结构  写在template属性中
    template:``,
    data () { // 这是当前组件中的数据  需要是  函数返回对象
      return {

      }
    }，
    methods:{},
    watch:{}
  }

  // 注册全局组件
  Vue.component("CommonHead", CommonHead)
```
+ 局部组件
```js
// 在其他组件的 components 属性中定义
{
  components:{
    CommonTitle:{
      template:``,
      data () {},
      xx, 
      xx,
    }
  }
}
// 注意 ： 只能在  注册组件的 template使用
```

注意：
  1， 组件 template 只能有一个 根元素
  2， 组件名 命名
      大驼峰 或者 -  连接符
      CommonHead -->使用时 <common-head></common-head>
      'common-head' -->使用时 <common-head></common-head>
  3， 组件的data属性 必须是函数 返回对象
    组件中的data数据写成函数，数据以函数返回值形式定义，这样每复用一次组件，就会返回一份新的data，类似于给每个组件实例创建一个私有的数据空间，让各个组价实例维护各自的数据。而单纯的写成对象形式，就使得所有组件实例公用了一份data，就造成一个变了全都会变得结果

## 22、组件  template 另外两种定义方式
```js
  <script id="commonHead" type="text/html">
    <div>
      <h1>我是头部组件</h1>
      <button>按钮</button>
    </div>
  </script>
  // 或者
  <template id="commonHead">
    <div>
      <h1>我是头部组件</h1>
      <button>按钮</button>
    </div>
  </template>

  const CommonHead = {
      template:"#commonHead",
  }
```

## 23、组件间的关系
  父子关系
  兄弟

## 24、组件间通信
###  1、父-->子 通信
  通过子组件的props属性来接受
```js
  const Child = {
    template:`
      <div>
        子组件
        {{ title }}

        {{ title }}
      </div>
    `,
    // 新增props属性 数组  数组中写带接受的外部属性的key
    // 带接受的所有props会编译到实例的属性上 
    // 注意：props  不要和实例中已有的属性重名
    props:['title', 'title2']
  }
  // 传数据 通过自定义组件标签的属性传递 如果需要传递父组件中动态的数据需要加  :属性
    <child title="我是静态的值" :title2="msg"/>
```

### 2、props 验证
```js
  const Child = {
    props:{
      // 只验证类型
      a:Number,
        // 只验证类型  类型 是数组中的一个即可
        b:[String,Number],
        c:{ // 验证类型 必须 传
          type: String,
          required: true
        },
        d: { // 验证类型 有一个默认值
          type:String,
          default: '我是默认值'
        },
        e:{ // 如果是数组 或者 对象 默认值 必须是 一个函数 返回这个默认值
          type:Array,
          default: ()=> [1,2,3]
    }
  }

//可选类型有  String Number Boolean Object Array Symbol Function
```
注意：
  1， props 不能和 实例中已经存在的属性重名（data中的数据 methods中方法 计算属性...）
  2,props不能直接修改（在子组件中）
  数据是可追踪的
  组件分类
    容器组件  （ 页面 路由组件 ）
      所有的数据处理 或者 逻辑处理 在容器组件中完成
    UI 组件
      接受数据  渲染数据， 收集数据给容器组件
    状态提升

  + 组件通信 ---子 --> 父
    自定义事件 形式 通信
```js
  // 子组件中 触发一个自定义事件  同时可以携带数据
  this.$emit('自定义事件的名字'[,携带的数据])
  // 监听事件 子组件的标签 监听
  <child @自定义事件名="fn"/>

  Parent
  {
    methods: {
      fn (params) {
        // params是 自定义事件触发携带的参数
      }
    }
  }
```

### 3、兄弟组件之间通信

$emit 方法 是在Vue原型上 所有的组件 和 显示的实例（new Vue）都能调用这个方法
哪个实例$emit的自定义事件 就应该由哪个实例 来监听
监听这个自定义事件 除了 在组件标签上 @自定义事件来监听，还可以通过

实例.$on('事件',)()=>{})
原理： 事件中心总线  eventHub  eventBus
  创建一个第三方实例
  在 a组件中 引入这个实例 实例.$emit()触发自定义事件
  在 b组件中 通过这个实例 实例.$on() 监听这个事件
```js
  // 创建 第三方实例  事件中心总线
  const bus = new Vue（）
  // a组件中
  bus.$emit('自定义事件名',[,params])
  // b组件中
  bus.$on('自定义事件名',回调函数)
```
以下三种不常用
### 4、ref 方便在vue中获取dom （获取组件的实例）
```js
  const child = {
    template：`
      <div>
        <h3 ref="title">子组件</h3>
        <button ref="btn">按钮</button>
      </div>
    `
  }
  // 在子组件中  通过this.$refs.xxx获取对应的dom
  // ref绑定在组件中

  // 父组件
  <Child ref="child"/>
  // 父组件中 获取子组件的实例 直接获取子组件的属性或者调用子组件的方法
  this.refs.child.属性
  this.refs.child.方法()
```
### 5、实例的 $parent 和 $children属性
```
  实例的$parent属性 返回的是当前组件的父组件实例
  实例 $children 返回是数组 保存了当前组件中 所有的子组件的实例
```
### 6、provide inject 通信

## 25、组件的生命周期

一个组件 从开始注册到最后销毁的中间的整个过程
每个过程：vue提供不同的钩子函数（在不同阶段 自动调用的函数）
![vue实例生命周期图谱](https://cn.vuejs.org/images/lifecycle.png)

```html
组件、实例生命周期 是指一个组件从创建到销毁的整个过程，vue提供了 不同时期的生命周期的钩子函数，来监控不同的生命周期过程，在这些钩子函数中，可以做不同的操作
beforeCreate 实例初始化之前
created 实例已经初始化  在这里可以使用data和methods的方法了，一般在这个生命周期内，调用获取数据的ajax函数
beforeMount 视图渲染之前
mounted 视图已经渲染，在这里获取获取渲染后dom，可以做一些 特效（绑定一些全局的事件..）
beforeUpdate 数据更新 视图重新渲染之前
updated 数据更新 视图 渲染之后 在这里可以获取 更新后最新的dom
activated  组件激活
deactivated 组件 停止使用
beforeDestroy  实例销毁之前  在这里可以清楚定时器 销毁 全局事件如滚动
destroyed 实例销毁之后
errorCaptured  捕获错误钩子

注意：
activated
deactivated
当一个组件 通过 keepalive组件缓存 之后，所有的生命周期都不会重新调用，原因组件被缓存了没有真正的销毁。
activated 组件再一次使用时，会调用 让缓存的组件 局部刷新（调用局部刷新 数据的 请求函数）
deactivated 组件 停止使用时会调用 （清除定时器 销毁全局的事件）
```



## 26、生命周期使用场景
```js
  created
    // 实例已经创建完成，实例上数据和方法 也初始化完成，且实例上的数据 已经被劫持变成响应式数据，但是还没有渲染到页面上，在这里除了不能获取dom操作之外，初始化数据的操作在这里都可以执行
    // 可做操作： 从服务器获取一些初始化的数据，或通过ajax向服务器发送一些数据
  mounted
    // 组件的视图已经渲染完成，一切组件在初始化的时候需要获取dom的操作都要在这里完成，比如使用swiper/富文本/echarts/高德百度地图
  updated
    // 当数据改变想要获取更新后的dom
  beforeDestory
    // 组件销毁时 清除组件中定义的定时器 以及 销毁一些全局事件绑定
```
## 27、keep-alive
  vue提供了一个组件keep-alive  用它包裹组件  组件在消失时不会销毁会一直缓存在内存中

```
  <keep-alive>
    <child v-if="isShow" />
  </keep-alive>

  包裹组件后，这个组件不会销毁，会缓存在内存中，所以当组件，再一次显示时，所有的生命周期不会触发（更新触发），常用于组件缓存（列表页进详情，回到列表时列表状态缓存）

  两个生命周期钩子会触发
  activated   激活时触发
  deactivated   停用时触发
```
## 28、插槽
  slot 占位符，这个符号代表了当前组件内部不同的结构在使用组件时，通过组件的内容传入
```js
  const Child = {
    template: `
      <div>
        <h1>组件</h1>
        <slot/>
      </div>
    `
  }
  <child>
    XXXX
    XX
    XXXXX
    xX
  </child>
  // child组件便签内容会自动灌入slot所在的位置中
```
## 29、具名插槽
每个插槽slot可以定义一个name属性，这样在使用这个组件时，就可以传入多个不同的结构块  对应不同的slot

```js
  const Child = {
    template: `
      <div>
        <slot name="a" />
        <h1>组件</h1>
        <slot name="b" />
      </div>
    `
  }

  // 使用时
  <child>
    <div name="a">
      这里的内容会自动灌入到name=a 的slot中
    </div>
    <div>
      这里的内容会自动灌入name=b的slot中
    </div>
  </child>
```

## 30、切换动效
元素在使用v-if控制显示、消失状态
transition 组件控制元素显示、消失的动效
```html
  <transition name="fade">
    <div class="box" v-if="isShow"></div>
  </transition> 
入场
  .v-enter  入场一瞬间 初始状态
  .v-enter-to  入场完成 最终状态（一般不用），元素定义的样式就是最终状态
  .v-enter-active 入场的过程

.v-enter{
  定义初始状态的css样式  状态1 元素自己定义样式是状态2
}
.v-enter-active{
  使用过渡
  transition：all 1s
}
出场
  .v-leave 出场瞬间  初始状态 一般就是元素自己的样式
  .v-leave-to  出场完成 最终状态 定义元素最终的样式
  .v-leave-active 出场的过程在这里定义

.v-leavee-to{
  这里定义出场最终样式 （和元素自己的样式做切换中间使用过渡）
}
.v-leave-active{
  使用过渡
  transition：all 1s
}
# 注意：
  transition 是用于控制单组件的动画（只能包裹一个根组件），或者使用v-if else... 包裹多个组件（同时只会显示一个组件） 
  如果控制多个动画包裹多个组件（列表动画） 使用transition-group组件包裹即可

  使用 transition 通过条件渲染包裹多个元素时
  默认：出场和入场动画是同时进行（不推荐）
  此时需要给transition加一个属性 mode 值 in-out （先入场再出场 不推荐） out-in（先出场再入场  推荐）
```

## 31、动画
  先定义动画
  @keyframes 动画名{
    from{}
    to{}
  }
  @keyframes 动画名{
    0%{}
    25%{}
    50%{}
    75%{}
    100%{}
  }
  再使用动画
  animation-name
            duration
            delay
            timing-function
            count  （infinite）
            direction normal reverse alternate alternate-reverse
            fill-mode forward backward both 动画充填模式
            play-state running paused
  animation：name duration [delay ....]

结合动画：
  v-enter-active  入场
  v-leave-active  出场

## 32、vue中的混入minin
可以讲多个组件中公共的属性方法计算属性 XXX 单独定义在一个混入中  这个混入可以插入任意一个组件中
如何定义混入
```js
  // 定义混入 是一个对象结构和组件对象一样
  const mixin = {
    data () {
      return {
        msg: "mixin"
      }
    },
    methods: {},
    computed:{}
  }
  // 将混入 灌入组件中

  const Home = {
    template: ``,
    mixin: [mixin]
  }
  // home组件中就可以使用mixin这个复用中的数据、方法...
  /*
  注意：
      如果组件中的属性或者方法有和混入冲突的
      以组件自己为准
  */
```

## 33、自定义指令
### 1、全局指令
```js
Vue.directive('指令名', {
  bind(el,binding){
    // 指令 指令第一次绑定到元素时调用 此时绑定元素已经变成真实dom，只不过还未挂载到文档
  },
  inserted(){
    // 是指 指令所绑定的dom，插入到父节点时，触发  此时dom还不一定插入到文档中
  },
  update(){
    // 数据更新 当前组件的vnode 更新完毕
  },
  componentUpdated(){
    // 数据更新 当前组件的vnode以及后代组件vnode都更新完毕触发
  },
  unbind(){
    // 指令和元素解绑时触发
  }
})
// bind  update unbind
```
### 2、局部指令
```js
  // 组件
  const Home = {
    directives:{
      指令名: {

      }
    }
  }
```
如何使用指令  <组件 v-指令名="值">

## 34、过滤器
当我们在模板中{{}}使用一个数据时，需要过滤一下再使用就可以定义一个过滤器
eg：
  后端返回的金额 是数据 在模板中渲染 需要再前面加一个货币符号

### 1、怎么使用过滤器
// 基础使用
{{ 变量 | 过滤器名 }}
// 使用时 传参
{{ 变量 | 过滤器名(参数) }}
// 过滤器 串联
{{ 变量 | 过滤器1() | 过滤器2() | 过滤器3 }}

### 2、定义过滤器
全局的
```js
  Vue.filter(名字，(v[,...params])=>{
    // v就是过滤的那个变量 return什么  最终就显示什么
    return XXX
  })
```
局部的
```js
  const Home = {
    filter: {
      currency(){}
    }
  }
```
## 35、fetch 不重要看看就行
```js
fetch(url,{
  method: 'post',
  headers:{

  },
  body:{
    // post 请求携带数据
  }
}).then(res=>{
  return res.json(); //解析json格式 res.json()
  res.text() res.blob()
}).then(res=>{
  // res 解析后的数据
})
```

## 36、axios ajax请求库
vue+axios react+axios angular+axios
```js
  axios.get(url,{
    params:{}, // get 请求传的参数 参数也可以在url上一query格式传
    headers:{},
  }).then(res).catch(err)

  // post的第二个参数 就是传的数据
  axios.post(url,{
    a:10,
    b:20
  })
  // axios方法
  axios({
    url:'xxx',
    method:'get/post',
    params:{}, // get请求传的参数
    data:{}, //post 请求传的参数
    headers:{}
  })

  // 创建一个实例
  const instance = axios.crete({
    baseURL:'xxx', //  定义基础url 会自动加在 所有实例发的请求url前面去
    timeout: 8000, // 请求超时时间
    headers:{}
  })
  // 好处： 创建实例时，统一做一些初始配置 如统一url 超时时间  请求头xxx
  // axios 有什么方法 实例就有什么方法
  instance.get  instance.post  instance

  // axios的默认配置
  axios.defaults.baseURL = 'https://api.example.com';
  axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
  axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
  /*
  注意：
  统一创建实例 统一配置，或者通过axios defaults配置 二选一
  */

 // 拦截器
 // 添加一个请求的拦截
 axios.interceptor.request.use(function (config) {
   // 在发送请求之前，可以做一些事情
   /*
    config.headers.token="xxx"
    config.method // get/post
    config.data 拿到post请求传的数据
   */
  return config; // 如果不return config 请求就发送不出去
 }), function (error) {
    // 请求出错的拦截
    return Promise.reject(error);
  });

// 添加响应拦截
axios.interceptors.response.use(function (response) {
    // Any status code that lie within the range of 2xx cause this function to trigger
    // response是响应的数据 
    return response;
  }, function (error) {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    // Do something with response error
    return Promise.reject(error);
  });
```

## 37、vue路由
VueRouter 是Vue的一个路由插件
让vue事件 单页面应用 SPA（single page application）整个应用程序只有一个html文件
优点：加载速度 切换的速度 快 （单页面，只渲染一次，切换页面不重新加载）
缺点：不利于seo（搜索引擎优化）（nuxt）

## 38、vue路由基础
```
  vueRouter是基于vue的前端路由库（设计成了 Vue的插件）让前端实现SPA单页面应用
  spa优点： 切换速度快 （加载速度快）
  缺点：前端渲染（nuxt 解决seo问题）seo 搜索引擎优化 爬虫 不利于seo

  <script src="vue.js">
  后面引入vue-router
  <script src="vue-router.js">

  定义 路由组件
    const Home = {
      template:`
        <div>
          <h1>home页</h1>
        </div>
      `
    }
    const News = {
      template:`
        <div>
          <h1>新闻页</h1>
        </div>
      `
    }
    const routes = { // 路由规则
      {
        path: '/home',
        component: Home
      },
      {
        path: '/news',
        component: News
      }
    }
    // 创建路由对象
    const router = new VueRouter({
      routes
    })

    const vm = new Vue({
      el:"XXX",
      router
    })

    在模板是上需要新增出口(占位)
    <router-view />
    在路由匹配到某个组件时，组件在router-view位置渲染

    router-link 组件做路由调转
    <router-link to="/home" tag="button"></router-link>
    to定义调转path
    tag定义渲染的标签

    在匹配的touter-link中会自动加入 router-link-active高亮类 

    解决 /路由组件默认显示某个组件问题

    1、新增路由规则
    如：
    {
      path: '/',
      component: Home
    }
    问题？ 如果有home router-link 不会高亮
    2、重定向 推荐
    {
      path: '/'
      redirect: '/home' // 当地址为 /  会自动变成/home
    }
```

## 39、动态路由
```
当vue-router引入时，如果是直接script src引入的话，会自动往构造函数以及原型中添加一个属性 route保存了当前路由的基础信息 所有组件就可以直接通过this.route获取当前路由基础信息

定义：
  routes = [
    {
      path: "/detail/:id", // 定义一个动态路由 定义了一个变量是id
      component: Detail
    }
  ]
  // 跳转
  router-link
          to="/detail/2"  // 此时 2 是动态路由定义的变量id的值
  获取：
      this.$route.params.定义变量名  获取动态路由传的参数

  // path
  {
    path: "/a/:b/c/:d"
  }
  跳转路径： /a/b/c/d
            /a//2/3/e 路径是错误的
  结果：
    this.$toute.params
          {
            b:"b",
            d:"d"
          }  
```

## 40、动态路由 监听 
监听路由参数 （动态路由 传的动态的值）的变化
```
  watch:{
    "$route"(to, from){ // to去哪  from从哪来
      console.log('变化了')
      console.log(to, from)
    }
  }
```
## 41、解决404
```
{
  path: "*", *匹配任意路由 尽量将404页面 路由规则放到最后
  component: NotFound
}
```

## 42、嵌套路由
```
  一级路由中 在路由组件中还可以显示 （根据路由的变化 父级组件）显示不同子级组件

  const routes = [
    {
      path: '/news',
      component: News,
      children: [ // 在父级路由规则中新增 children，在属性中定义二级路由
        {
          path: "/news/native", // 建议二级路由path 携带一级路由的path作为前缀
          component: native
        },
        {
          path:"/news/abroad",
          component: Abroad
        }
      ]
    }
  ]
  
  // 在一级路由中  对应的 组件中 新增router-view 作为二级路由 出口（占位）
  const News = {
    template: `
      <div>
        <h1>news组件</h1>
        <router-view />
      </div>
    `
  }
  <router-view />
```

## 43、命名路由
给每个路由规则新增name属性，相当于给每个路由起个名字
```
  routes = {
    path:'/home',
    name: '首页', // 给这个路由 新增name属性
    component: xxx
  }
```

## 44、命名视图 （不重要）看文档

## 45、编程式导航
```
  声明式导航利用组件跳转路由（router-link）
  编程式导航 js内部通过方法来控制路由跳转 当引入vue-router后 构造函数还灌入一个对象 $router  $router其实就是路由对象实例 new VueRouter()

  this.$router.push()  // 跳转路由的 参数同  router-link的同属性 栈 
    // 跳转路由  并往历史记录中添加一条新的记录
  this.$router.replace()  // 跳转路由 参数通router-link的to属性
    // 跳转路由  不添加新纪录 而是覆盖当前的记录
  this.$router.go(n)
                  0  // 刷新
                  -1 // 回退一步
                  1  // 前进一步
```

## 46、别名
```
  '重定向'的意思是，当用户访问/a 时， URl将会被替换成/b,然后匹配路由为/b ，

  /a 的别名是/b,意味着，当用户访问/b 时URl会保持为/b,但是路由匹配则为 /a ，就像用户访问 /a 一样

  上面对应的路由配置为：
    const router = new VueRouter({
      routes: [
        {
          path: '/a',
          component: A,
          alias: '/b'
        }
      ]
    })

```

## 47、路由组件参数
+ 动态路由传参
```
  {
    path:'/news/:id'
    componemt: News
  }
  // 跳转时
  <router-link to="/news/2">
  // 获取
  this.$route.params.id
  优点：
    刷新后 数据不丢失
```
+ params传参
不管是router-link的to属性 还是push方法的参数，他两是一样的
前提：
  1，路由必须加name属性
  2，参数（router-link to的参数 push参数）必须是对象
可写的值是
  1，直接写字符串， 这种情况，必须写path router-link to="/news"  this.$router.push("/news)
  2,参数可以是对象 router-link :to="{name:'home'}" 可以以名字跳转
```
  params传参
  1，声明式导航
  <router-link :to="{name:'news',params:{a:10,b:20}}">
  2,编程式导航
  this.$router.push({name:"/news,params:{a:1,b:2}"})

  获取：
  this.$route.params.xxx

  注意：
    params传参，必须结合命名路由一起使用 跳转时，必须要通过name跳转，否则传不过去
    params传参的优点：隐式传参  地址栏看不见
    缺点：刷新后数据丢失
```
+ query传参 
```
  router-link
          :to="{path:'/news',query:{a:1,b:2}}"

  this.$router.push({
    path: '/news',
    query:{
      a:1,
      b:2
    }
  })
  获取
    this.$route.query.xxx
  注意：
    优点：刷新后 数据不丢失
    缺点：参数放在地址栏上
    query最好结合path  尽量不结合name
```

## 48、路由模式

路由模式：
  默认是hash模式 ，通过url hash值得变化 来控制路由的跳转
  缺点：
    丑  url#/a   #/b
  优点：
    不会改变 html 文件的路径指向

  history模式
    不会往url上加 # 
    /a   /b   /c
    优点：好看
    缺点：改变html文件的指向关系

hash路由原理：面试时问一下
  改变url后面的hash值，通过window的hashchange事件来监听 hash变化，根据定义路由规则，来显示对应的组件
history 模式原理
  history对象 新增两个方法 pushState（） replaceState（）这两个方法来改变url地址，在这个方法中匹配路由规则

## 49、导航守卫
导航守卫、导航钩子函数
监听甚至 拦截 路由变化 
场景： 例如登录鉴权等

全局守卫 （拦截所有的路由）
全局前置守卫

```jsx
router.beforeEach((to,from.next)=>{
  // to  目标路由
  // from 从哪来路由
  // next  拦截器 不调用next() 路由无法进入，next参数同router-link to属性的值，重定向地址
  next()
})
```

全局后置守卫

```jsx
router.afterEach((to, from)=>{

})
```

路由独享的（定义在路由规则中 只拦截这个路由）

+ 路由独享
```
{
  path: "/home"
  component:Home,
  beforeEnter(to,from,next){
    // 只会拦截/home这个路由
  }
}
```
+ 组件内部  ( 只针对这个组件 )
```
const Foo = {
  template: `...`
  beforeRouterEnter (to,from,next) {
    // 在渲染改组件的对应路由被 config 前调用
    // 不！能！ 获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没有被创建
  },
  beforeRouterUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用是调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id,在/foo/1和/foo/2之间跳转的时候，
    // 由于会渲染同样的foo组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例   `this`
  },
  beforeRouterLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例  `this`
  }
}
```
## 50、滚动行为
```js
const router = new VueRouter({
  mode:"hash"
  routes:[],
  scrillBehavior (to, from, savedPosition) {
    if(savedPosition){
      return savedPosition
    }else{
      return {
        x:0,
        y:0
      }
    }
    // return 期望滚动到哪个位置
    /*
      return 的位置
      应该是一个对象
      {
        x:0,
        y:0
      }
      savedPosition参数 保存每一次离开这个路由时的滚动条位置
      对象{
        x:xxx,
        y:xxx
      }
    */
  }
})
```

## 51、vueCli  vue脚手架
基于webpack 构建一个脚手架
安装
```js
  npm install -g @vue/cli
  #OR
  yarn global add @vue/cli

  vue ---version  // 验证是否安装成功
```

启动项目
命令行启动项目
```js
vue create 目录名  // 注意 目录不能出现大写字母
```

可视化网页 启动

## 52、项目目录结构

```js
publuc  // 服务器静态资源的 根目录 这里的静态资源可以通过绝对路径访问
src		// 源码目录
	assets  // 放置静态资源的目录
    		// assets中的静态资源  webpack会处理（优化），路径写相对路径 public写绝对路径
    components  // 放置公共的子组件
    router	// 路由配置
    	index.js
	views	// 放路由组的目录
    App.vue	// 根组件
	main.js // webpack的入口文件
babel.config.js	// babel的配置文件  es6转es5 插件
package.json	// 项目配置文件
```



## 53、webpack运行过程

webpack搭建很多环境 以及创建服务器入口文件 main.js会自动挂载到index.js  服务器自动打开index.html，main.js中引入的资源会自动挂到index.html

webpack可以将任意类型资源当做模块依赖，只要在main.js中引入即可，在main.js引入了，就相当于在index.html引入了



## 54、ES6模块化

webpack搭建了es6开发环境webpack有loader解析器（不同的loader，用于解析不同的文件在js中的支持）有了这些loader 就可以在js中引入 任意类型的资源（如 js css img 字体图标，以及vue的单文件组件） webpack构建的环境中 可以使用 es6模块化或者 nodejs模块化（引入任意资源）

```js
1、如果一个文件 没有导出任何接口（如js未导出接口，或者其他类型的文件如css）
这种文件可以直接引入
引入语法
	import '路径'  （require）
2, 一个文件 导出多个接口（js）
  方法1
    导出
    a.js
      export const a = 10;    
      export let b = {
        a: 10,
        b: 30
      } 
      export const fn = () => {
        console.log(111)
      }
      或者
      const a = 10
      const b = 20
      const c = () => {
        console.log(111)
      }

      export {
        a,
        b,
        c
      }
    引入接口
    1,按需引入
    import { a, b } from './a.js'
    2,全部引入 
    import * as 别名 from '路径'  
  3， 一个文件 只导出一个接口（只能）
  export default 值
  
  引入
  import 变量名 from '路径'

  #注意：
    模块化  每一个模块的 文件 都有一个单独的作用域
```



## 55、Vue单文件组件

以.vue结尾文件webpack会使用vue-loader将vue结尾文件，自动解析成一个vue组件对象

```js
<template>
    
</template>
<script>
export default {
  data () {return {}},
  methods: {},
  computed: {},
  watch: {},
  components: {},
  filters: {}
}
</script>
<style lang="scss" scoped>
/* 
lang指定css预处理器
scoped 作用域 当前 样式 只针对 当前组件的template中的 组件有效不会影响其他组件
/deep/ 深度选择器  穿透 scoped的限制
在使用 UI组件库 如果需要 自定义 ui组件样式时，需要 加 /deep/
*/
</style>
```



## 56、路由懒加载

```js
// es6 引入模块 import 必须是 顶部引入 预加载
// import()

{
    path:'/home',
    component: () => import('路径')
}
```



## 57、自定义脚手架配置

根目录下 新增vue.config.js 配置好之后 重启代码

```js
const path = require('path')
module.exports = {
  devServer: { // 配置服务器
    port: 9527,
    open: true
  },
  lintOnSave: false, // 配置 eslint 格式化
  chainWebpack: config => { // 配置路径别名
    config.resolve.alias
      .set('@', path.join(__dirname, 'src'))
      .set('assets', path.join(__dirname, 'src/assets'))
      .set('components', path.join(__dirname, 'src/components'))
      .set('views', path.join(__dirname, 'src/views'))
      .set('utils', path.join(__dirname, 'src/utils'))
      .set('api', path.join(__dirname, 'src/api'))
      .set('mixins', path.join(__dirname, 'src/mixins'))
  }
}
```



## 58、VUEX

VueX是一个专为Vue.js应用程序开发的状态管理模式。它采用集中式储存管理应用的所有组件的状态，并以响应的规则保证状态以一种<font color="red">可预测的方式</font>发生变化。



程序中的公共的数据一定是可预测  可追踪的（容器组件  UI组件  状态提升）



VueX和vue-router一样是vue的插件 使用插件Vue.(插件)  // 往Vue(构造函数上) 增加一下些属性或方法...

```js
// 下载
npm i vuex -S
// src新建store
// store
	index.js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    num: 10
  },
  mutations: {
    addNum (state, n) {
      state.num += n
    },
    reduceNum (state, n) {
      state.num -= n
    }
  }
})
export default store
// main.js中
import store from './store'

new Vue({
  store
})
// 在组件中 获取 state
this.$store.state.xx
// 组件中提交mutation
this.$store.commit('mutation名字'[,传递的参数])
```



##  59、vuex中的actions

action 是方法 不能直接 修改 vuex中的state 而是 提交提个mutation 来修改 action内部可以包含异步操作 使用场景： 一个 公共数据 数据来源 需要 请求一个接口（发送ajax） 应该将 请求在action中请求，得到数据后，触发一个mutation 赋值 给state

```js
// 定义actions
{
  state: {
    num: 10
  },
  mutations: {
    setNum (state, n) {
      state.num = n
    }
  },
  actions: {
    getNum (context) { // context是 store这个对象
      fetchNum().then(res => { // 封装的ajax请求函数
        if (res.data.code === 200) {
          context.commit('setNum', res.data.data)
        }
      })  
    }
  }
}
```



## 60、vuex提供的助手函数

mapState方便组件获取state

mapMutations 方便组件提交 mutations

mapActions 方便组件触发action

```js
import { mapState, mapMutations, mapActions } from 'vuex'
{
    methods:{
        ...mapMutations(['addNum', 'reduceNum']),
        ...action(['addNumAsync'])
    },
    computed:{
        ...mapState(['num']) // 新增一个计算属性num ，依赖 store中state的num
    }
}
this.num // 获取 store中的state中的num
this.addNum(参数) // 提交 addNum这个mutation
this.addNumAsync(参数) // 触发 addNumAsync 这个action
```



## 61、 getters

相当于 vuex中的计算属性

```js
{
  state:{
    num: 10
  },
  getters:{
    doubleNum (state) {
      return state.num*2
    }
  }
}

// 组件中获取
// 1直接获取
this.$store.getters.doubleNum
// 2 助手函数获取
import { mapGetters } from 'vuex'

{
  computed:{
    ...mapGetters(['doubleNum'])
  }
}
```



## 62、 vuex 模块化 在vuex state上 增加一些模块

```js
const modulea = {
  state:() =>{
    num: 10
  },
  mutations: {
    addNum (state, n) {
      state.num += n
    }
  }
}
const moduleb = {
  state:() =>{
    num: 10
  },
  mutations: {
    addNum (state, n) {
      state.num += n
    }
  }
}

const store = new Vuex.Store({
  modules: {
    modulea,
    moduleb
  }
})
// 组件中获取  state
// 直接获取
this.$store.state.modulea.num
// 助手函数获取
computed:{
  ...mapState({
    num: state => state.modulea.num,
    numb: state => state.moduleb.num
  })
}

//  问题 模块化之后 只是将state 分了模块 mutation、actions并没有
this.$store.commit('mutation')
// 会将所有的模块中同名mutation都触发
```

##  63、模块命名空间

增加命名空间后 mutation action 就有了作用域

```js
const modulea = {
  namespaced: true, // 增加命名空间
  state: {
    num: 10
  },
  mutations: {
    addNum (state, n) {
      state.num += n
    }
  },
  actons
}
// 组件中调用mutation
this.$store.commit('modulea/addNum'[,参数])
// 助手函数
{
  ...mapMutations(['modulea/addNum'])
}
this['modulea/addNum']([参数])
{
  ...mapMutations('modulea', ['addNum'])
                    // 模块名  // 这个模块下的哪个mutation
}
this.addNum([params])
```



附加： 多次复用mutations中的方法时、防止方法名写错 -解决方案

```js
在store/  中 新增types.js
export default const ADDNUM = 'addNum'

在store/  index中引入
import { ADDNUM } from './types'
使用时注意：将方法名变为变量

[ADDNUM]
```

