# day1

## 对比jquery和vue（react）区别

jquery js库  
命令式开发
  告诉 计算机 每一步的步骤 让计算机去执行
vue js框架
声明式开发
  告诉计算机我要的结果，中间怎么办，自己执行

## vue是什么

是一个js的 mvvm 渐进式 视图 框架
mvvm
  mvc 
  模型  	m model   数据层
  控制器	c controller 核心业务代码
  视图	v view  用户页面 html/css

1、view 传送指令到 Controller

2、Controller 完成业务逻辑后，要求Model改变状态

3、Model 将新的数据发送到  View，用户得到反馈

mvvm
  模型	m 数据
  视图	v 用户页面
 vm(viewmodel vue实例)
当数据改变时，视图自动刷新 （通过vmodel vue的实例控制刷新）

唯一的区别是，它采用双向绑定：view的变动，自动反映在ViewModel

## 学习vue需要注意什么

一切是数据为核心，使用数据来驱动视图刷新（不要想着去操作dom）

## vue雏形

```
<div id="app">
  {{ msg }}  
  <!--
   {{}}
   vue中的模板
   用于渲染 数据
  -->
</div>
script src="./vue.js"
script

  let vm = new Vue({
    el:"#app", //将vue挂在到el对应的元素上，也就是vue控制范围
    data: {
      msg:"你好vue"   // mvvm中 m数据放置的地方
    }
  })

```

## 外部挂载 实例

```
let vm = new Vue({
  data:{
    msg:"hello vue"
  }
})
vm.$mount(document.querySelector("#app")) //外部挂载vue
```

## 模板语法 {{}}

```
{{}}
模板 在视图中渲染数据
注意：
  1，模板中 是js环境（要求放表达式或者值）（不会放语句）
  2，模板中的变量 和 函数 必须是 实例上的属性或者方法
  3，js全局函数 有些可以使用有些不可以
    对于全局widnow下的方法设置白名单，只有在白名单下的方法 模板中才可以使用
       'Infinity,undefined,NaN,isFinite,isNaN,' +
    'parseFloat,parseInt,decodeURI,decodeURIComponent,encodeURI,encodeURIComponent,' +
    'Math,Number,Date,Array,Object,Boolean,String,RegExp,Map,Set,JSON,Intl,' +
    'require'
  4，if结构不能用 ifelse也不能用
    短路
      && 代替if结构
      ||
    ifelse可以使用 三目
    a?'aa':'xxx'
```



## 指令

扩展了 普通html属性的功能（用法同自定义属性)
v-指令名="值"

```
<div v-xxx="值">
```

+ v-model
  将表单控件的value 属性 与 vue中的 数据 进行双向绑定
+ v-text 不需要记
  将元素 的文本内容 与 一个vue的数据进行绑定
+ v-html  不管是v-text还是{{}}都不会渲染富文本数据
  渲染富文本数据
+ v-bind:属性 将一个普通的任意标签的属性（可以是自定义属性）的值，与vue一个数据进行绑定
  简写：
    :属性
+ v-on:事件  将vue元素的事件与vue的方法进行绑定
  简写
    @事件

```
  绑定方式：
    1，
    <button @click="fn">按钮</button>
      new Vue({
        methods:{
          fn(e){
            // 这种绑定方式 fn就是事件函数，第一个参数就是事件对象
          }
        }
      })
    <button @click="fn()">按钮</button>
    // 需要传参的时候
     new Vue({
        methods:{
          fn(){
            // 这种绑定方式 fn是普通调用，不再是事件函数，需要得到事件对象需要手动传入 $event
            // $event 挂载到Vue.prototype上的
          }
        }
      })
    方法中改变数据
      this.数据 = 值
      eg:
        {
          data:{
            isShow:true
          },
          methods:{
            fn(){
              this.isShow = !this.isShow
            }
          }
        }
事件修饰符
  .stop   阻止事件冒泡
  .prevent 阻止默认事件
  .capture(捕获) 捕获阶段就提前触发
  .self 只能自己触发 其后代元素无法触发
  .once
```

总结：
  <元素 v-指令="xxx">
  指令的值需要写在引号中：
    1,也是js环境
    2，里面可以写表达式 不能写语句
    3，全局函数 只能使用白名单中
    相当于模板中的语法



# day2



## 条件渲染  （控制元素 显示、消失）

```
v-if

v-else-if

v-else

控制元素显示消失
有道云笔记

```

## v-show 控制元素的显示消失  v-if比较

```
相同点：都是控制元素的显示、消失 值true 显示 false消失
不同点：值为false时， v-show是改变元素的display属性none v-if直接移除元素
    如果初始值为false 则 v-if绑定的元素 压根初始不加载（惰性）
使用场景：
  1，v-show适用于 频繁切换状态的场景
  2，v-if适用于 初始不加载 切不频繁切换的状态

```

## 列表渲染 循环

```
<ul>
  <li v-for="item in arr">
    {{ item }}
    // 注意 item是循环的每一项 且item的作用域范围是 li内部
  </li>
</ul>
得到下标
<ul>
  <li v-for="(item, index) in arr">
    {{ item }}  {{ index }}
    // 注意 item是循环的每一项 且item的作用域范围是 li内部
  </li>
</ul>
循环json
 <li v-for="item in json">
    {{ item }}
    // 注意 item是循环的每一项 且item的作用域范围是 li内部
  </li>
</ul>

// 得到key
 <li v-for="(item,key) in json">
    {{ item }}
    // 注意 item是循环的每一项 且item的作用域范围是 li内部
  </li>
</ul>

// 得到下标
 <li v-for="(item, key, index) in json">
    {{ item }}
    // 注意 item是循环的每一项 且item的作用域范围是 li内部
  </li>
</ul>

new Vue({
  data: {
    arr: [1, 2, 3, 4],
    json:{
      a:1,
      b:2,
      c:3,
      d:4
    }
  }
})

注意：
  循环的每一项需要加一个独一无二的key（今天不讲，明天将mvvm原理时讲）
```

## :class v-bind:class 

```js
  <div id="app">
    <div :class="a"></div>
    <!-- div class="box" -->
    <div :class="[a,b,'box3']"></div>
    <!-- div class="box box2 box3" -->
    <div :class="{active:isActive}"></div>
    <!-- div class -->
    <div :class="{active:1<2}"></div>
    <!-- div class="active" -->
    <div :class="[a,b,'box3',{active:1==1}]"></div>
    <!-- div class="box box2 box3 active" -->
  </div>
  <script src="./vue.js"></script>
  <script>
    const vm = new Vue({
      el: '#app',
      data: {
        a: 'box',
        b:'box2',
        isActive:false
      }
    })
  </script>

```

## :style vue中的style

```
<div id="app">
    <div :style="a"></div>
    <div :style="{ width:100+200+500+'px',height:100+200+'px',background:'red' }"></div>
  </div>
  <script src="./vue.js"></script>
  <script>
    const vm = new Vue({
      el: '#app',
      data: {
        a: {
          width:'100px',
          height: '200px',
          background: 'red'
        }
      }
    })
  </script>
```

## 表单修饰符

修饰v-model

```
.lazy  v-model 会变成change 事件驱动
.trim 自动去除开头结尾空格
.number 将输入的值自动转换成number类型，过程同parseFloat 注意如果这个值无法被 parseFloat() 解析，则会返回原始的值。

```



# day3

## ref

vue项目开发 不推荐操作dom,但是某些情况下我们需要获取dom
比如：Siwper Echarts 富文本

```
<div  ref="odiv">

<span ref="ospan">

this.$refs 
{
  odiv:div,
  ospan:span
}
获取
  this.$refs['odiv']
  this.$refs.ospan
```

## 侦听器

监测 vue中的数据变化，如果这个数据变化了，有一个回答会触发

```
{
  data: {
    msg: "",
    obj: {
      a: 10,
      b: 20
    }
  },
  watch: {
    // 监听基本数据类型
    msg (newVal, oldVal) {
      // xxxx
      // 基本数据类型监听直接在watch属性中新增同名方法即可
    },
    obj (newVal){ } // 此种方式监听不了对象变化，对象判断的是地址
    //如何监听对象  
    // 1，直接监听对象具体某个属性
    'obj.a' (newVal) {
      //
    }
    // 2，深度监听 可以监听对象任意属性的变化
    obj: {
      handler (newVal) {
        //xxx
      },
      deep: true //深度监听
    }

  }
}

```

## 计算属性 

使用场景：
  当一个数据在使用时，需要 处理一下 再使用，这个时候就应该使用计算属性

```
{
  data: {
    msg: 'xxx'
  }
  computed: {
    msg2 () {
      return this.msg.split('').reverse().join('')
    }
  }
}

注意：
  1，写法是方法，编译后 是实例一个属性，同data中的属性，所以不能和实例上已有属性同名（如data中的数据和methods中的方法)
  2，会基于 依赖进行缓存，当依赖不变时，调用多次，计算属性只会初始计算一次，剩下几次都使用第一个计算 缓存的结果（依赖改变会重新计算）
思考？
  计算属性 可以被手动修改吗？
  如何要修改计算属性
    {
      data:{
        num: 10
      },
      computed:{
        //如何要修改，计算中有getter（获取值） setter （设置值）
        num2: {
          get(){
             //  上面的写法 相当于 只写了get
             // return 什么 num2就是什么 num如何不变 num2 get不会重新计算
            return this.num/2;
          }, 
          set (val) {
            // 当直接修改 num2 会调用set函数(注意，num2不会变，num2永远基于get中的return依赖，只有依赖变了才会重新计算get重新获取新值)
            // 修改依赖
            this.num = val*2;
          }
        }
      }
    }
    使用场景：
      直接使用get 场景更多 
```

## MVVM原理

![mvvm原理](https://cn.vuejs.org/images/data.png)

```
Object.defineProperty(对象,属性,{
  get(){},
  set(){}
})
注意： 这个方法在IE8及以下版本 不支持 （vue兼容IE8及以下浏览器）
用于拦截 对象的 设置 或 获取的操作
获取就会调用 get
设置就会调用 set

let vm = new Vue({
  data:{
    a:10,
    b:20,
    c:30,
    d:{}
    ....
  }
})

编译后
{
  a:10,
  b:20,
  c:30
}


原理：
  当我们 创建一个实例 （new Vue），需要传入data，data值是对象，vue会去遍历data这个对象的所有的属性，用Object.defineProperty()劫持，转换成getter和setter(就是Object.defineProperty参数中的get和set方法)，每一个getter和setter中 放置一个监听者，每一个getter和setter调用时，会触发监听者 监听的事件,监听回调函数就会触发，在这个回调函数中 触发了render函数 ，render触发会 重新生成 虚拟dom和数据未修改前的在内存中存储的上一次的虚拟dom树进行比较（diff算法），得到最优的一种方案来更新真实dom，再将更新后的dom结构用虚拟dom保存，更新内存中存储虚拟dom
  ...



  虚拟dom:
    是用js对象的结构来描述dom树
  为什么要用虚拟dom?虚拟dom在vue mvvm中参与了什么过程？
    数据更新，dom更新，每一次都会在内存中存储一份，当前真实dom对应虚拟dom（包括第一次加载）
    直接操作dom是很耗性能的，所以我们在数据改变后，不会直接修改真实dom树，而是先调用render函数，生成 最新的 虚拟dom结构，和数据未修改前的在内存中存储的上一次的虚拟dom树进行比较（diff算法），得到最优的一种方案来更新真实dom，再将更新后的dom结构用虚拟dom保存，更新内存中存储虚拟dom

    diff算法：
      diff 让dom树，逐层进行比较（一层对一层（1---1   2---2））
      如果每一层 虚拟dom还有独一无二key值，按照，这一层的相同key进行比较（相当于电影院中座位号（几排几号）），所以在做循环 v-for 时，一定要给循环每一项加 独一无二key（提高diff算法比较效率，更快渲染真实dom）

    https://www.jianshu.com/p/af0b398602bc

    eg:
      <div id="box" class="a">
        <p class="op">我是p</p>
        <span>我是span</span>
        我是内容
      </div>

    {
      tag:"div",
      attrs:{
        id:'box',
        className:'a'
      },
      children:[
        {
          tag:'p',
          attrs:{
            className:"op"
          },
          children:[
            '我是p'
          ]
        },
        {
          tag:'span',
          attrs:{},
          children:['我是span']
        },
        '我是内容'
      ]
    }

  

  MVVM中 的bug？
    如果一个数据，在初始化 data中没有定义，而是，后续操作动态添加，vue监听不到这个数据的变化的
  会造成 数据 改变视图不刷新
    new Vue({
      data:{
        a:10
      },
      methods: {
        fn(){
          this.b = 10;
        }
      }
    })

  关于数组的bug 明天讲
  
```

# day4



## MVVM中数组的bug

```
    Object.defineProperty() 
  无法捕获到数组 某些操作
  数组[index] = 值
    
  arr.length = xxx
  不要直接改变length 改用splice()

  如何解决数组相关问题
    this.$set(arr,index,value)
    在set函数的回调中，自动调用render函数






  Vue中全局API
  Vue.xxxx全局API

  实例：
    this.xxx
  现象：
    vue作者 封装了很多api 全局有 实例也有
     比如：
      Vue.set()
      this.$set()

      Vue.nextTick()
      this.$nextTick()
```

## nextTick

```
数据更新后，视图刷新前
  中间过程：生成新虚拟dom，diff比较新旧虚拟dom,更新真实dom
  vue设计成了 异步

  所以带来一个问题》
    改变数据后，立即获取最新的dom，是获取不到，获取是改变前的dom
  vue设置一个观察者
  Vue.nextTick()

  this.$nextTick(()=>{
     // 这个回调会在 真实dom更新完毕后触发
  })

```

## 解决模板中 闪烁的问题

```
给 模板的元素 加上 v-cloak属性
原理：
  当vue编译后，会自动 去除 模板中元素的 v-cloak属性

  [v-cloak]{
    display:none;
  }
  <div v-cloak>
    {{ msg }}
  </div>
```

## 组件  （数据驱动、组件化开发）

网页上的 组成部分 成为组件 （用户去定义的）

```
Vue中的组件如何定义
1，组件也是Vue实例（不是显式new Vue）,所以 实例有的属性和方法组件都有

如何定义组件
  对象？
  const CommonHead = {
    data:,
    methods,
    watch,
    computed
  }
组件挂载（组件在没有挂载前只是一个普通的对象，new Vue） 
  全局挂载   （挂载到 Vue） 在任意一个地方（el:"#app"）控制范围都可以使用
  Vue.component("CommonHead",CommonHead)
  注意：组件名建议使用大驼峰
  局部挂载 （在其他组件中挂载，此时只能在挂载组件的模板中使用这个组件）
   // 定义logo组件
    const Logo = {
      template: `
        <h3> 我是logo组件 </h3>
      `
    }

    // 定义一个 头部组件
    const CommonHead = {
      template: `
        <div>
          <h1>我是公共的头部组件</h1>  
          <logo></logo>
        </div>
      `,
      components: {
        Logo,
        Logo2: {
          template: `
            zzzz
          `
        }
        // 在其他组件的components属性中注册其他子组件
      }
    }
    // 挂载到全局上
    Vue.component('CommonHead', CommonHead)
使用组件
  <组件名></组件名>
  <组件名 />
注意：
  定义组件名建议使用大驼峰
  使用时 改为 下划线
  如组件名 CommonHead
  <common-head></common-head>

1，注意 组件的data属性必须是一个函数 返回一个对象（形成了闭包，形成了自己单独作用域）
  为什么：组件会在 其他组件中多次挂载和使用，如果 data是一个对象，引用类型 值传递的是地址，会造成多个父组件中操作子组件的数据时 互相影响
2，组件 template中必须 有且只有一个根组件（如果使用v-if v-else-if v-else也可以同时写多个保证只显示一个）
3,template外部挂载
  <script id="tpl">
    <div>
      <h1>组件内容</h1>
    </div>
  </script>
  或者
  <template id="tpl">
    <div>
      <h1>组件内容</h1>
    </div>
  </template>

  const CommonHead = {
    template:"#tpl"
  }

```



# day5



## 组件间关系 

组件间 互相嵌套 使用 形成大概以下关系

+ 父子
+ 兄弟
+ 祖先后代

## 组件分类

+ 容器组件（父组件）
+ UI组件 （子组件）
  状态提升（数据）：一个容器组件中 可以使用多个 UI组件 应该将UI组件 需要用到的数据都提升到
  容器组件中管理（只负责展示数据和收集数据不做逻辑处理，逻辑应该在容器组件中处理）

## 组件通信 *****

### 父 --> 子 通信

```
 <template id="Home">
	  <div>
		  Home组件
		  <common-head :title="msg"></common-head>
		  <!--
		  通过自定义标签属性传递 可以传递动态数据 需加 :属性
		  -->
	  </div>
  </template>
  <template id="news">
  	  <div>
  		  news组件
		  <common-head title="我是news组件传过来的数据"></common-head>
  	  </div>
  </template>
  <script src="./vue.js"></script>
  <script>
	  // 头部组件
	 const CommonHead = {
		 // 在子组件中定义props属性接收父组件传递数据 数组
		props:['title'],    // properties
		template:`
			<div>
				我是头部组件
				<h2> {{title}} </h2>
			</div>
		`,
		data () {
			return {
				msg: 'hello'
			}
		}
	 }
    const Home = {
      template: '#Home',
	  components: {
		  CommonHead
	  },
	  data () {
		  return {
			  msg: '我是home组件中data中的数据'
		  }
	  }
    }
	const News = {
	  template: '#news',
	  components: {
		  CommonHead
	  }
	}
	Vue.component('Home',Home)
	Vue.component('news',News)
    const vm = new Vue({
      el: '#app'
    })
  </script>
  
  注意：
	props中接收的数据。最终会挂载到实例上（作为实例属性）
	data中数据 methods中的方法 computed属性，都会编译实例属性或者方法
	所以这几者不能重名 ***

```

### props验证

思考问题？
	子组件 需要接收一个数据，且这个数据必须是数字
	需要对于props做验证

```
props
验证 对于 某个prop 做 以下验证
	类型验证
		String
		Number
		Boolean
		Array
		Object
		Date
		Function
		Symbol
	必传验证
		required 
			true/false(默认false)
	默认值验证
		default
		注意：如果数据类型是 数组或者对象 默认值 必须是 函数 返回这个默认值（引用类型的值传递（传递时地址））

	const componentA = {
		props: {
			propA: String, // 只验证数据类型 直接写类型
			propb: [String,Number], // 只验证类型 类型有多个
			propc: { // 验证类型 和 必传
				type: String,
				required: true
			},
			propd: { // 类型和默认值
				type: String,
				default: '我是默认值'
			},
			prope: { // 如果数据类型是数组或者对象 默认值必须是函数返回默认值
				type: Array,
				default: ()=> [1,2,3,4]
			}
		}
		
	}
```

### 组件通信 --  子-->父

```
子-->通信 是通过自定义事件完成

子组件中  
	this.$emit('自定义事件名',携带参数)
父组件中
	<child @自定义事件名="函数"></child>
```

### 兄弟组件通信

```
实例下
	this.$emit('自定义事件',携带数据)

	this.$on('事件名', ()=>{

	})

谁提交的 就通过 谁 调用$on来监听
事件中心总线 eventBus   创建一个第三方的实例  

const bus = new Vue();

bus.$emit('事件')

bus.$on('事件')


```

以下三种 不常用

### ref组件通信

```
父组件中
<child ref="child"></child>

{
	mounted(){
		this.$refs.child // 子组件的实例 直接使用子组件上的属性和方法
	}
}

```

### $parent $children

```
this.$parent 父组件实例 可以直接使用或者调用父组件的属性和方法
this.$children 所以子组件实例 数组 放多个子组件实例
注意谨慎使用

```

### provide inject

```
父组件

const Parent = {
	provide:{
		a:10,
		b:30
	},
	template:,
	xx
	components:{
		Child
	}
}

子组件

Child 
{
	inject:['a','b'],
	mounted(){
		console.log(this.a);
		console.log(this.b)
	}

}

```



# day6



## 插槽 slot

可以实现  组件 在不同的父组件中 使用 有不同的 html 结构

```
const Child = {
  template: `
    <div>
      <h1>内容</h1>
      <slot/ > 
      slot是 插槽，占位符，最终会被子组件在 使用时的 自定义标签的内容所填充
    </div>
  `
}

父组件
  <child>  组件的自定义标签的内容会自动填充到 slot中
    <div>
      <h2></h2>
      xx
      xxx
    </div>
  </child>
```

### 具名插槽 （插槽起了名字  有多个插槽）

允许我们定义多个插槽，每个插槽起一个名字，在 嵌套内容可以根据名字 一一对应

```
child  
{
  template: `
    <div>
      <slot name="a" />
      <h2>我是子组件</h2>
      <slot name="b"/>
    </div>
  `
}

<child>
  <div slot="a">
    xxx
    x
    x
    xx
  </div>
  <div slot="b">
    xxx
    x
    x
    xx
  </div>
</child>

```

## 混入 mixin 复用

同组件 相同结构对象，将 多个组件  公共的属性或者方法或者...都放到这个对象中，
混入 可以 通过 组件的 mixins属性 灌入到某个组件中

```
定义一个混入
  const mixin  = {
    data () {
      return {
        msg: '我是多个组件公用数据'
      }
    },
    methods: {
      changeMsg () {
        this.msg = xxx
      }
    }
  }


  组件a
  {
    mixins: [mixin]
    组件 就会自动 集成混入中的属性方法。。。。。
  }
注意：
  如何混入中属性或者方法 和 当前组件中的属性或者方法冲突 那么 优先使用自己的

```

## Vue中的过渡（动画）

允许 给元素 显示、消失（v-show v-if或者路由组件 路由切换）加上切换效果

animation-name
        -duration
        -delay
        -timing-function
        -count
        -direction
        -fill-mode  forward backward both
        -play-state 





# day7



## transition组件  只能控制 单组件（标签也属于组件）动画 无法控制列表动画

如果使用 v-if v-else 控制 transtion可以包裹多个组件的（显示的永远只有一个）
那么 给transtion 加一个属性  mode="in-out/ out-in"

## 列表动画 改为 transition-group包裹即可

## 组件生命周期

组件从 创建 到 销毁 整个阶段
不同的阶段  需要 做不同的拦截 （可以实现不同阶段做不同事情）
![组件生命周期图谱](https://cn.vuejs.org/images/lifecycle.png)
生命周期钩子函数：
  在生命周期的不同阶段 自动调用的函数

```
1，挂载阶段
  beforeCreate
    此时 数据还没有完成监听 （操作数据 无响应） 基本不用
  created
    此时 组件的属性和方法已经 可用，且 数据已经监听完毕（所以在这里可以操作数据了）
  beforeMount
    dom挂载之前 （此时当前组件的 dom还未挂载，所以无法获取dom）
  mounted
    dom挂载完毕 此时 已经可以获取当前组件的dom

2，更新 阶段
  beforeUpdate
    数据改变  dom更新之前
  updated
    数据改变  dom已经 更新完成  在这里可以获取 最新的dom
3，销毁 
  beforeDestroy  销毁之前
  destroyed   已经销毁

  销毁时：使用较多的是 beforeDestroy
      在这里  注销 当前组件中在window下 注册的 事件监听 
      取消 当前组件中的定时器
```

## keep-alive 缓存其他组件

当一个组件被keep-alive组件包裹时，这个组件  消失的时候，也不会销毁，会被缓存在 内存中，当组件再一次显示出来时，由于组件没有销毁，生命周期钩子，不会重新调用（组件上数据会缓存）

？ 被缓存的组件 在 重新显示时， 部分数据刷新

```
<keep-alive>
  <组件 v-if="xxx"/>
</keep-alive>

activated(){
  // 被缓存的组件 再一次被 激活时 自动调用（在局部刷新数据函数请求，以及定时器开始和全局事件绑定）
}

deactivated(){
  // 被缓存组件 再一次停用时 触发 在这里可以 注销 全局事件绑定 以及 清除定时器
}

```

## 过滤器 filter

数据在 模板中（html）使用时，可以做一下过滤在输出

```
{{ msg | 过滤器 }}

<div :属性="值|过滤器">
```



# day8

## 自定义指令

<div v-指令名="xxx"  aaa="xxx">


```
vue预定义指令
  v-model v-text v-html v:bind:属性 v-on:事件 ...

  directive

全局指令
Vue.directive('指令名', {

})
局部指令


const Home = {
  directives: {
    指令名: {

    }
  }
}


三常用钩子函数
  inserted  当前指令所在的 组件 插入到 父组件中（父节点），此时可以获取dom啦
  componentUpdated  指令所在组件的 VNode 及其子 VNode 全部更新后调用。
  unbind 解绑 （指令所在的 组件或者标签 移除、 当前组件卸载） 注销全局事件绑定，以及 清除定时器
```

## 前后台分离

## vue中ajax请求

### fetch   ajax

```
fetch
如何使用： "key=v&key2=v"
	fetch(url,{
		headers:{
			"token":localStorage.getItem('token'),
			"content-type":"apllication-xxx-urlencoded"
		
		},
		method:"GET/POST",
		data:{
			
		}
	}).then(function(res){
		return res.json()  //text() arrayBuffer() blob() formData()
	}).then(res=>{
		//res 才是请求的结果 格式化之后（什么格式取决于 第一个 then 返回的处理方法）
	})
	
问？fetch 如何做兼容 
	fetch 兼容性差的原因是啥：
		使用promise 不支持 ie11及以下的
		且，在vue react等脚手架中 配置babel es6转es5，也无法转换promise
		使用babel-polyfill 插件来解决
Vue 如何兼容IE（IE9及以上）， 	使用babel-polyfill 插件来解决

解决fetch 兼容性问题？
	1，使用babel-polyfill
	2,直接使用 git https://github.com/github/fetch
		基于原生的fetch 封装的，当有的浏览器不支持fetch时，转换成普通的xhr对象（内部集成了babel-polyfill）
```


### axios vue+axios react+axios angular+axios 

### get请求

```
1,axios.get('http://xxxx.xxx.com?a=10&b=20').then(res=>{
  //成功回调
}).catch(err=>{
  // 失败回调
})
2，
axios.get(url,{
  params:{ // get请求需要携带的数据 在真实请求时，会自动 放到地址栏后面
    a:10,
    b:20
  },
  headers: { // 在请求头中 加上若干参数...

  }
})
```

### post

```
  axios.post('/user', {  // 这个对象就是请求携带的数据 会自动挂载到请求体中
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

```

### axios方法

```
axios({
  url:"xxx",
  method:"xxx",
  params:{},
  data:{

  },
  headers:{

  }
}).then(res=>{}).catch()

```

## 配置默认值

```
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

### 创建一个 实例 请求  推荐使用的

```
const request = axios.create({
  baseURL:"xxxx",
  timeout: 5000,
  headers:{

  }..

})
好处：
  为什么不直接使用axios而创建一个实例
  好处：可以批量做 默认配置
注意：实例方法 跟直接axios 方法一样
request.get()
      .post()

request({
  url:,
  method,
  data,
  params,
  headers,
})
```


### axios的拦截器

```
// 添加一个axios 请求的拦截
axios.interceptors.request.use(function (config) {
    // 在发送之前可以在这里做某些事情
    // 如果你不rerturn config 请求时发送不出去的
    return config;
  }, function (error) {
    // 请求出错了 走这个拦截
    return Promise.reject(error);
  });

// 添加一个响应拦截器
axios.interceptors.response.use(function (response) {
    // http状态码 为2xx 会走这个响应成功的回调
    return response;
  }, function (error) {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    // Do something with response error
    return Promise.reject(error);
  });

```





# day9

## 路由  https://router.vuejs.org/zh/

实现 spa应用
single page application
优点：
  整个应用只有一个 html文件，所以的 路由对应页面都是由组件来实现
  在切换路由 时  不需要 重新加载 页面 只需要切换组件即可（切换效率较高）
缺点：
  因为代码都在js中，html源码基本看不到内容，seo爬虫爬取的时html源码，不会爬取js中的内容  对于seo不友好

mpa  multiple page application 多页面应用

## 路由基础配置

```
// 定义了  路由组件 （路由组件不要挂载到全局或者局部），需要挂载到路由上
const Home = {
      template: `
        <div>
          <h1> home页 </h1>
        </div>
      `
    }
    const News = {
      template: `
        <div>
          <h1> 新闻页 </h1>
        </div>
      `
    }
    const About = {
      template: `
        <div>
          <h1> 关于我们页 </h1>
        </div>
      `
    }
  

  const routes = [
    {
      path: '/home',
      component: Home
    },
    {
      path: '/news',
      component: News
    },
    {
      path: '/about',
      component: About
    }
  ]
  // 定义路由对象
  const router = new VueRouter({
    routes
  })

  // 最后一步 定义出口
  <router-view />
   
  // 组件用于跳转路由的
  <router-link
    to="/home"
    tag="button"
  >首页</router-link>
  new Vue({
    el: '#app',
    router: router
  })

```

## 处理 根路径

```
const routes = [
  {
    path: '/',
    component: Home
  }, // 不推荐
  {
    path: '/',
    redirect: '/home' // 推荐重定向
  },
  {
    path: '/home',
    component: Home
  }
]
```

## 处理404 路径错误

```
{
  path: '*', // 匹配任意地址
  component: NotFound
}
```

## 动态路由

主要用于 路由跳转 并传参 （如列表进入详情需要传id）

```
路由规则
{
  path: '/detail/:id', // :id 相当于声明一个变量 变量名是id
  component:Detail
}

跳转时
  地址是
    /detail/2
  获取 传递参数

  在目标路由组件中 Detail
    this.$route.params.id 获取
  注意：
    $route对象 是 引入vue-router 自动 往Vue构造函数 的原型上 灌入的 一个对象
    这个对象保存了 当前路由的 基本信息 如 路由地址、路由名字、元信息、参数....
  
  监听动态路由参数的变化
  {
    watch:{
      $route(){
        // 只要参数变化  这个侦听器函数 就会调用，在这里可以重新获取数据
      }
    }
  }
```

## 嵌套路由

```
const News = {
      template: `
        <div class="news">
          <h1> 新闻页 </h1>
          <router-link to="/news/native">国内新闻</router-link>
          <router-link to="/news/abroad">国外新闻</router-link>
          <router-view />
           // 在父路由组件中 定义 子路由的出口
        </div>
      `
    }

const routes = [ // 路由规则
      {
        path: '/news', 
        component: News,
        children: [ // 一级路由中 新增children 在这里定义子路由
          {
            path:"/news",
            redirect: '/news/native'
          },
          {
            path: '/news/native',
            component: NativeNews
          },
          {
            path: '/news/abroad',
            component: AbroadNews
          }
        ]
      },
      {
        path: '/about',
        component: About
      }
    ]

```

## 命名路由

```
{
  path:"/home",
  name:"首页",
  component:Home
}

<router-link
  :to="{
    name: '首页'
  }"
>
```

## 路由传参

```

跳转的时候并传参
<router-link
  :to="{
    name: '首页',
    params: {
      a: 10,
      b: 20
    }
  }"
>
参数获取：
  this.$route.params.xxx
优点：
  隐式传参
缺点：
  刷新后丢失

to为对象 也是可以通过path跳转
<router-link
  :to="{
    path: '/home'
    
  }"
>

to为对象 也是可以通过path跳转，携带params无效
<router-link
  :to="{
    path: '/home',
    params:{
      c:40
    }
  }"
>
注意：
  name和params是固定搭配 
  path和params搭配无法传参的



传参的第三种方式 
query传参
  router-link
    :to="{
      path:"/home",
      query:{
        a:10,
        b:20
      }
    }"
获取参数：
  this.$route.query.xxx
优点：刷新不丢失
缺点：在地址栏上直接显示


总结：
  vue路由跳转传参有哪些方式  面试题
  1，动态路由
  2，name+params传参
  3，path+query
```



# day10

## 编程式导航

+ 声明式导航  

```
  1,to属性值是字符串  直接写path
  <route-link to="/home">首页</router-link>
  2,to属性的还可以是对象  可以跳转传参
  <route-link :to="{
    path:'/home',
    query:{
      xxx:xxx
    }
  }">首页</router-link>
  <route-link :to="{
    name:'首页',
    params:{
      xxx:xxx
    }
  }">首页</router-link>

```

+ 编程式导航
  在逻辑代码中 在js中 控制 路由跳转
  $router 也是 vue路由灌入到  Vue的原型上
  其实是 new VueRouter 返回的那个 路由对象
  $router上挂载了操作了路由的几个方法

- push 相当于 使用router-link to属性来跳转

```
1,字符串 直接写path
this.$router.push('/home')
2,对象
this.$router.push({
  name: '首页',
  params: {
    a:10
  }
})
this.$router.push({
  path: '/home',
  query: {
    a:10
  }
})
```

- replace

```
this.$router.replace() 用法同push
参数和 router-link属性以及 push参数相同 实现也是相同功能

与push区别：
  push会往浏览器 地址栏 中添加一条新的记录
  replace 会替换当前地址
```

- go

```
this.$router.go(n)
  0 刷新
  -1 返回上一步
  1 前进一步
```

## 动态路由 props 参数获取

```
{
  path: '/detail/:id/:id2',
  component: Detail,
  props: true
}

Detail组件中
this.id获取动态路由传的参数
{
  props:['id','id2']
}
```

## 路由模式

```
new VueRouter({
  mode: 'hash/history'
})

hash：会让地址栏上 添加 # 改变地址栏 地址 后的hash值
window.onhashchange 事件判断 地址栏变化，然后再去 遍历 routes配置规则显示对应组件

缺点：地址栏 地址上有 # 丑

history 原理  利用history.pushState()  h5新增api
去掉#
  www.xxx.com/a
  www.xxx.com/b
  www.xxx.com/c
  bug:
    /写在主机后 代表 当前主机这个服务器 服务器软件  监听的根目录，不会解析为 index.html上/a路由
    所以会产生一个 404问题
  解决：
    主机上 服务器 做 重定向处理 只要 你访问了 / 根路径  重定向到 当前vue项目的index.html上
```

## 导航守卫

钩子函数 在  路由 变化时，钩子会自动触发

+ 全局 （守卫 所有路由）

  - 全局前置

  ```
  router.beforeEach((to,from,next)=>{
  
  })
  ```

  - 全局后置

  ```
  router.afterEach((to, from)=>{
  
  })
  ```

+ 路由独享的 路由配置中定义钩子（只会守卫 某个路由地址 /home）

```
{
  path: '/home,
  beforeEnter(to, from, next){

  },
  component:Home
}
```

+ 组件内部 （只针对这个组件）
  - beforeRouterEnter
  - beforeRouterUpdate
  - beforeRouterLeave

## 路由元信息

在定义路由时，增加meta属性，meta属性是对象，可以在路由 $route或者路由守卫to获取meta中的字段



# 脚手架

## 安装

```
npm i -g @vue/cli 

vue --version 查看版本
```

## 启动项目

+ 命令行

```
在需要创建的 目录下
打开 命令行

输入
  vue create 目录名 （注意 不要出现大写字母）

注意：如何选择 sass且解析包 选择是node-sass
经常会出现一个问题：
  node-sass包下载失败
  代码如果出现 sass/scss 代码 会立即报错
怎么解决：
  cnpm i node-sass -D
```

![启动项目选项](G:/第三阶段/vue/h5-2004-three-stages/vue/20201016105346.png)

+ 可视化启动项目

```
命令行中

vue ui 直接可视化操作即可
```

## 项目目录结构

```
根目录下
  node-modules 包文件
  public 静态资源目录
    index.html 当前项目中 唯一的 html
    favicon.ico 标题 图标
  src  源码目录
    assets 放静态资源目录 （代码中的 css js img） 
    components 放公共组件的目录
    router 路由配置文件
    store vuex
    views 放路由组件
    App.vue 全局的根组件
    main.js 入口函数
  .browserslistrc  配置 postCss自动给css加 浏览器前缀的规则
  .eslintrc.js  eslint的配置文件
  babel.config.js babel配置文件
```

## es6模块化

```
前提：
  如果某个文件 没有向外提供接口，那么这个文件就是接口
  直接引入这个文件，就相当于这个文件的代码直接引入

向外提供接口
  1导出多个接口
    a.js
      export const a = 10
      export const b = 20
      export const fn = () => {
        console.log(111)
      }
      相当于 导出一个
        {
          a:10,
          b:20,
          c: ()=>{}
        }
    引入某个接口
    b.js
    import { a as aa, b, c } from './a.js'
    import * as obj from 'xxx'
    as别名 默认 直接引入a相当于在当前文件声明一个变量a，as作用 声明名字改为as 的值

  多出多个接口2
    const a = 10
    const b = 20
    const c = 30

  b.js引入
      import { a as aa, b, c } from './a.js'


只能提供一个接口 导出默认值
  export default 值

引入
  import 变量名  from '路径'
```

## 单文件组件

webpack vue-loader解析.vue结尾的文件，自动解析成vue组件的语法

```
<template>
  <div>
    ddd
  </div>
</template>
<script>
export default {
  data () {

  },
  methods: {}
}
</script>
<style>
</style>
```

## 路由懒加载

```
{
  path: 'xxx',
  component: () => import('@/views/news')
}
// 懒加载 是指 当这个路由匹配到时 才会去 加载 对应news组件
好处：提高首页打开速度
```

## 封装和使用组件

```
在src/components
定义公共组件

在views
  Home.vue
  script
    import CommonHead from '@/components/CommonHead'

    {
      components: {
        CommonHead
      }
    }
```

## 组件中的样式

```
全局css
  reset.css

  assets
    css
      a.css
在入口文件中
  main.js
    import './assets/css/a.css'

组件内部的css
  .vue 单文件组件 
  style定义 当前组件的css
  lang 使用css预处理器
  scoped 让style的样式 只针对当前标签有效
  /deep/ 穿透scoped的限制 选择器 可以选中 后台组件中的 元素
  <style lang="scss" scoped>
  </style>
```

### main.js 入口函数

```
当前项目
  唯一 
  index.html vue代码需要挂载index.html才能才浏览器起效果
  main.js 入口函数
  自动 在index.html上引入
  按需引入  资源挂载

  在main.js中 引入的资源 影响是全局

  在脚手架环境中：
    在js文件中  不止可以引入 js文件 还可以引入其他文件
    如css 图片
    原因 webpack有专门的解析器
```

## development 发展 开发

## eslint配置

```
.eslintrc.js中
  值有三种
   '0'  warn警告
   '1'  off 关闭
   '2' error直接报错
   https://www.jianshu.com/p/a09a5a222a76
```

## 如何修改 项目配置

当前项目是基于webpack进行构建，webpack配置文件默认是隐藏的
如何去自定义一些配置
比如：修改端口、修改eslint 的格式化时间 

```
根目录下
vue.config.js
  module.exports = {
    
  }
```


## 备注：不管是修改什么配置 （eslint，还是babel,项目配置），改完之后记得重启

```
在根目录下创建 
  vue.config.js
const path  = require('path')
module.exports = {
  devServer: {
    port: 9527,
    open: true,
    proxy: {

    }
  },
  lintOnSave: false,   //关闭 lint
  chainWebpack:(config)=>{  // 配置路径别名
    config.resolve.alias
    .set('@', path.join(__dirname, 'src'))
    .set('components', path.join(__dirname, 'src/components'))
  }
}
```

## vue渐进式框架  设计插件的机制

```
使用插件：
  Vue.use(插件名)
使用插件后，会自动往 Vue构造函数 原型上 增加一些新的方法或者属性

扩展了 Vue构造函数的功能（新的方法、一般名字开头会加$）
在任意一个组件中
  就可以通过
    this.xxx使用新增的功能
```

## vuex 管理状态  （数据）

基于vue的 集中式状态管理工具（插件）
vue插件

### 基础

```
  npm i vuex -S
  src
    sotre
      index.js
      import Vue from 'vue'
      import Vuex from 'vuex'

      Vue.use(Vuex)

      const store = new Vuex.store({
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


  main.js
  import store from './store'

  new Vue({
    store
  })


  在组件中使用：
    this.$store.state.xxx
  在组件中提交mutation
    this.$store.commit('addNum', 2)

```

### actions 异步修改状态

vuex中修改state 只能是mutations
action 不是直接修改状态
而是提交 mutation mutation去修改state
什么时候用？
  多个组件公用统一状态，且这个状态需要 发送 ajax请求，将请求 代码
  写在action，将数据返回后，在action 提交一个mutation 赋值
特点： vuex规定 mutation不要包含异步请求，action可以包含

```
const store = new Vuex.Store({
  state: {
    num: 10
  },
  actions: {
    /* addNumAsync (context, n) {
      console.log(n)
      setTimeout(() => {
        context.commit('addNum', 5)
      }, 2000)
    } */
    addNumAsync ({commit}, n) {
      console.log(n)
      setTimeout(() => {
        commit('addNum', 5)
      }, 2000)
    }
  },
  mutations: {
    addNum (state, num) {
      state.num += num
    },
    reduceNum (state, num) {
      state.num -= num
    }
  }
})

组件中如何 触发action

this.$store.dispatch('action'[, params])
```

### vuex提供助手函数 简化 vuex操作

mapState  简化 组件中 获取state步骤
mapMutations 简化组件中 提交mutation的操作
mapActions 简化组件中 触发action的操作

```
import { mapState, mapMutations, mapActions } from 'vuex


{
  methods: {
    ...mapMutations(['addNum']),
    ...mapActions(['addNumAsync'])
    // 会在methods定义 的和mutations或者action同名的方法 用于提交mutations或actions
  },
  computed: {
    ...mapState(['num', 'num2']) 
    //就在组件中 新增了 2个计算属性 num num2他们依赖是 store中的state中的同名数据
  }
}

使用状态
  {{ num }}
提交mutation
  this.addNum(参数)
提交action
  this.addNumAsync(参数)

```

### getters

相当于 vuex中的 计算属性
使用场景：某个 状态 需要 依赖另一个状态 存在，当这个状态 改变时，getter会重新计算

```
store = {
  state: {
    num: 10
  },
  getters: {
    numDouble (state) {
      return state.num*2
    }
  }
}


在组件中如何获取
  1,this.$store.getters.numDouble
  2,通过助手函数
    import { mapGetters } from 'vuex'

    computed: {  // 直接通过this.numDouble使用
      ...mapGetters(['numDouble'])
    }
注意：
  不要直接修改getter的值 vuex也没有提供 修改getter的方法
  应该去修改依赖 state中的数据
```

### module

默认store是单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

```
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

如何在组件中使用
  获取
    this.$store.state.模块.num
    computed:{
      ...mapState({
        num: (state)=>{
          return state.模块.num
        }
      })
    }

如何提交mutations 或者action
    1,this.$store.commit('addNum')
      this.$store.dispatch('addNumAsync')
注意：
  mutations和actions默认提交 没有模块化，会导致 
  所以的模块中 同名mutation或者action同时触发
？如何解决这个问题
    你可以通过添加 namespaced: true 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名

const moduleA = {
  namespaced: true,
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA
  }
})

提交
  this.$store.commit('a/addNum')
如果助手函数

  methods: {
    ...mapMutations(['a/addNum'])
  }
  可以通过
    this['a/addNum'](params)
  methods: {
    ...mapMutations('a', ['addNum'])
  }
    this.addNum() 提交a模块中的addNum
```


## chrome  vue-dev-tools

## 常见 ui 组件库

移动端
  mint-ui  饿了么  不维护
  vant-ui  有赞 
  nut-ui 
pc
  element-ui 饿了么
  ivewi
  ant-design-vue

```
element-ui的使用
1，全部引入
  npm i element-ui -S
  在main.js中
  import ElementUI from 'element-ui';
  import 'element-ui/lib/theme-chalk/index.css';

  Vue.use(ElementUI)
2，按需引入
  1，安装babel插件
  npm install babel-plugin-component -D
  2，babel配置
  在
    babel.config.js
      中
    module.exports = {
      presets: [
        '@vue/cli-plugin-babel/preset'
      ],
      plugins: [
        [
          "component",
          {
            "libraryName": "element-ui",
            "styleLibraryName": "theme-chalk"
          }
        ]
      ]
    }

```

## 接口地址 https://www.it120.cc/