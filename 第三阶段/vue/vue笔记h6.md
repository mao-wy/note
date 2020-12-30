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
+ 父-->子 通信
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

## props 验证
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
  
```

  