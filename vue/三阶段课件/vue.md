### bem命名规范

常用于class命名规范

```
 b 块
 e 元素
 m 状态
 .head__button
 .head-top__input

 .head-top__input--active

 .head__input
 .head__input--active

https://www.jianshu.com/p/feb0fd5c6e0b


```

周报：
合肥-h5讲师李响-12月-第二周周报

### 前端发展史

- 纯粹页面展示 （html/css） web1.0刚出现
- 2.0 网页需求 越来越复杂，要求动画效果 交互 jquery、插件
- 互联网+ 前端需求 4g普及（手机硬件提升） 混合开发 多端（安卓、ios、小程序、h5、pc端cs软件）

### vue 是什么？

是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

### vue特点

- mvvm
  - 双向数据绑定 mv vm 无需关注 dom操作（无需关注视图），只需要关注数据即可
- 组件化
  - 组件 页面上的 公共小单元（如 网页头部 商品卡片）

### vue雏形

```
        script src="vue.js"

        let vm = new Vue({
            el:"#box",
            data:{  //用于存放vue的状态
                msg:123
            }
        }) 
        vue mvvm原理
        利用es5 defineProperty()
        拦截 数据的设置和获取，动态的改变视图
```

### 模板的语法

```
 {{}} 模板  插值  展示 vue 状态（可以是一个数据也可以是一个函数返回数据）

 {{}} 可以写 简单的js表达式 （加减乘除 三目 短路）
注意：
    1，不能写语句 
        var a=10;
        a+b
    2，能不能流程控制 不能写流程控制 只能写三目 或者短路
    3,模板中 js全局变量有些可以使用有些不可以使用（vue有一个白名单）
    'Infinity,undefined,NaN,isFinite,isNaN,' +
    'parseFloat,parseInt,decodeURI,decodeURIComponent,encodeURI,encodeURIComponent,' +
    'Math,Number,Date,Array,Object,Boolean,String,RegExp,Map,Set,JSON,Intl,' +
    'require' 
```

### 外部挂载vue实例

```
 let vm = new Vue({
     data:{
        msg:123
     }
 })
 vm.$mount(document.querySelector("#box"));
```

### vue指令

vue 扩展了 标签属性的功能，赋予它一些特殊的功能

```
语法：
    v-指令名字
使用方法：同标签的属性
v-model   将表单的值 与 data中的状态 进行 双向绑定
v-text    将元素的内容 与data状态进行双向绑定
v-html    将内容解析成 html （慎重使用，一般后台返回富文本内容时使用）
v-bind:属性  扩展了是任意属性（可以包括自定义属性，也可以是标签默认的属性）
            干了什么事：
                将属性的值 变成动态的值，和 vue的data中的state进行绑定
    简写：    :属性
v-on:事件
    将元素的事件 与 methods中的方法进行绑定（方法变成事件函数）
    事件调用时  全局变量 $event 保存了事件对象
    简写： @事件
注意：
    vue
        v-on:事件="这里不适合写过多的业务逻辑代码"
        {{ 也不适合写过多的业务逻辑代码 }}
原因：
    只能使用一些简单的表达式，不能写语句，不能使用 逻辑控制语句，很多js全局变量也不能使用（白名单里面的才能用）
    视图 中一般只简单的展示数据，调用命令，不适合 写过多的业务逻辑代码，否则会造成，代码混乱（高耦合）
```

### 如何打通 v-on:事件="这里不适合写过多的业务逻辑代码" 与原生js的隔阂（所有的js语法逻辑控制语句，所有js全局变量也不能使用）

```
 <button @click="fn">点击</button>

 new Vue({
     data:{},
     methods:{
         fn(){
             //在这里可以写任意的js代码 都可以用  
         }
     }
 })

function fn(){}
 box.onclick = fn;
 box.onclick = function(){
     fn();  $event
 }
```

### 事件修饰符

```
    修饰符是由点开头的指令后缀来表示的。赋予了事件一些特殊的功能 addEventListener("事件名",函数,true/false)
    .stop  阻止事件冒泡
    .prevent  阻止默认事件
    .capture    在捕获阶段触发
    .self   只能通过元素本身触发，后代元素无法触发
    .once  只触发一次
    
```

### 条件渲染

```
  一切以数据为核心 不要去操作dom
  v-if
  v-else-if
  v-else

  v-show

  v-if和v-show的区别以及使用场景？
    1，当元素消失时不出来时。v-show 是display:none v-if直接dom移除节点
  都有啥使用场景：
    场景1： 代码块 代码很多，默认是不显示的，后面有条件的显示   v-if
        首次加载 惰性的


    v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。
```

### 列表循环

```
    <li v-for="item in arr">
        {{item}}
    </li>

    拿下标
    <li v-for="(item,index) in arr">
        {{item}}
    </li>

    json
    <li v-for="value in json">
        {{value}}
    </li>
    json拿key
    <li v-for="(value,key) in json">
        {{value}}
    </li>
    json拿key和index
    <li v-for="(value,key,index) in json">
        {{value}}
    </li>
    注意：*********
        循环的每一项加 独一无二的key值
        eg:
            <li v-for="(item,index) in arr" :key="index">
                {{item}}
            </li>

        预习虚拟dom 
        https://www.jianshu.com/p/af0b398602bc
```

### ref获取dom

```
不推荐操作dom 
如果需要获取dom
    01，原生方法
        document.query。。。。。
    02,ref $refs
    <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <meta http-equiv="X-UA-Compatible" content="ie=edge">
            <title>Document</title>
            <script src="vue.js"></script>
        </head>
        <body>
            <div id="box">
                <button @click="fn">按钮</button>
                <div class="box" ref="box">div</div>
                <p ref="op">p标签</p>
            </div>
            <script>
                new Vue({
                    el:"#box",
                    data:{
                    
                    },
                    methods:{
                        fn(){
                        // console.log(document.querySelector(".box"));
                        console.log(this.$refs);
                        console.log(this.$refs['box']);
                        console.log(this.$refs.box);
                        console.log(this.$refs.op);
                        }
                    }
                })
            </script>
        </body>
        </html>
```

### vue中的class style

```
    动态的属性
    :class *****
        div :class="a"  -->解析  div class='box'
        div :class="[a,b]" -->解析 div class="box box2"
        div :class="{box3:c}"  --> div class="box3"
        div :class="{box3:d}"  --> div class=""
        new Vue({
            data:{
                a:"box",
                b:"box2",
                c:true,
                d:false
            }
        })
    :style ***
        div :style="a"

        div :style="{width:wh}"

    new Vue({
            data:{
                a:{
                    width:"100px",
                    height:"100px",
                    background:"red
                },
                wh:"100px"
            }
        })
```

###  侦听器

```
 观察者：检测一个数据(状态)的变化，只要变化了，就会触发某个函数
 watch属性

 实例增加watch属性
 new Vue({
	 el,
	 data:{
		 msg:123,
		 json:{
			 a:10,
			 b:1
		 },
		 watch:{
			 msg(newVal,oldVal){
				 
			 },
			 "json.a"(newVal,oldVal){ //一定加上字符串

			 }，
			 json:{
				 handler(newVal,oldVal){

				 },
				 deep:true  //深度监听
			 }
		 }
	 }
 })
```

### 计算属性

```
计算属性

使用场景：
	需要一个状态 依赖另一个状态，（另一个状态变化，当前状态也要同步变化，此时需要使用计算属性
	new Vue({
	 el,
	 data:{
		 msg:'hello world',
	 },
	 computed:{
		 msg2(){
			 return this.msg.split("").reverse().join("")
		 }
	 }
 })

 注意：
	1，计算属性的写法 是函数 返回一个值 使用时，和data中state一样
	2，不能和 data中的属性或者 methods中的方法同名
	3，计算属性是基于它们的响应式依赖进行缓存的  依赖如果没有改变 计算属性的函数不会重新调用，会使用上一次的缓存的结果


 思考？
	计算属性应该修改吗（这个值应该直接改变吗）

	1，不建议直接修改 计算属性，还应该修改依赖（从数据源头触发）
	2，如果要修改计算属性应该怎么办  还是不建议修改计算属性
	计算属性有两个拦截器：
		一个是 getter 获取值
		一个是setter 设置值
	前面的写法 只用到了 getter ，没有用到setter
	computed:{
		msg2:{
			get(){
				return this.msg*2
			},
			set(newVal){
				//newVal是 你直接设置 msg2的值 不会真的改变msg2 set拦截了
				this.msg = newVal/2;
			}
		}
	}
	总结：最好不要直接修改计算属性的值，如果要修改，修改 计算属性的依赖的值
```

### vue组件 component

```
	组件化：
		组件可以理解为 网页上 独立的一个部分 （如网页头部）
	vue:
		组件也是Vue实例 （实例的属性 组件都有）
	如何定义一个组件
	let CommonHeader = {
		template:""
		data(){
			return{

			}
		},
		methods:{

		}
	}
	如何区分全局组件和局部组件？ 在Vue上注册的就是全局组件 在某个组件或者实例中注册的就是局部组件
	全局组件  
		在任意的组件和实例上使用
		Vue.component("组件名",{
			template:"",
			data(){
				return {

				}
			},
			methods:{

			}
		})
	
	局部组件
		只能在注册的组件或者实例上使用

	注意：
		1，组件中的data 必须是 一个函数 返回一个对象
		2，每个组件有自己的作用域，组件内部的数据，和方法只能在组件内部使用（如向跨组件，需要组件间通信）
		3，组件的命名 可以有两种 1大驼峰 2下划线命名法
			eg:
				CommonHead   common-head
			使用时二者都一样
				<common-head></common-head>
		4，局部组件 在哪个组件的components中注册，就只能在这个组件的template中使用
		5，组件的template 有且只能有一个根标签（元素）
	
```

### 组件间的关系

由于组件嵌套使用 组件间的关系可以分为以下几种： 父子 兄弟 没有关系的

容器组件 一般是 路由组件 当前 路由对应的 页面（组件） 傻瓜组件 一个页面中可以抽离很多独立公共的部分（傻瓜组件，如网页的头部，轮播） 值负责 展示数据 和 收集数据，不做业务逻辑的处理

不同关系的 组件 之间的通信

### 父向子传参： props传参的

```
	1，简单用法
		在子组件中 定义props属性 值是一个数组 数组中 每个元素，就是 从父组件接收的 数据
		子组件
		let  CommonHead={
            template:`
               <div>    
                    <h1>我是 {{title}} 的标题</h1>
                    <h2>我是 {{ subTitle }} </h2>  
               </div>
            `,
            props:['title',"subTitle"]
        }
		在父组件中 使用 子组件 的自定义标签中 通过 属性的形式 传入 父组件的数据
		父组件
		let Home = {
            template:`
                <div>
                    <h4>我是home组件</h4>
                    <common-head :title="myTitle" :subTitle=" '我是home组件副标题' "></common-head>
                </div>
            `,
            data(){
                return {
                  myTitle:"首页组件"
                }
            },
            components: {
                CommonHead
            }
        }

	2，props验证
		上面的props用法，没有校验 props数据类型，以及必须填写，默认值 等，会造成 程序 存在 不稳定性 （代码不健壮）

		验证主要验证三个类型：
			数据类型 
				String
				Number
				Boolean
				Array
				Object
				Date
				Function
				Symbol
			必须填写
				required:true/false 如果为false 就别写了
			默认值：
				default
		注意：props 不再是数组 而是对象

		props:{
			a:String,
			b:{
				type:String,
				required:true
			},
			c:{
				type:Number,
				default:0
			}
		}
		注意：
			1，当 一个数组 类型为 数据的时候 传数据时，不能直接静态传一个数据（传的是字符串）
			2，数据类型为 Array，或者Object 默认值 必须是一个 函数 返回一个默认值
			3，props 能在子组件中 直接改变值吗？  不能（单向数据流）
```

### 子向父 事件进行通信

```
 通过 事件 弹射 形式通信

 在子组件中 通过
	名字 Child组件
	this.$emit("事件名",携带的数据)
 父组件 Parent中使用
	<child @事件名="fn"></child>

	{
		methods:{
			fn(data){
				data就是子组件传过来的数据
			}
		}
	}
```

### 兄弟组件传参 （任意组件） 事件中心总线

```
原理：利用一个第三方的 组件 进行弹射事件，且 进行接收事件
let  bus = new Vue();
弹射：
bus.$emit("事件名字",携带的参数)
监听事件：
bus.$on(事件名,回调函数)
```

备注：以上三种方式 通信 是开发中常用的通信，一下三种是装逼，或者面试用的

### ref属性 进行通信 耦合（高内聚低耦合）

```
	父组件中

	<child-component ref="child"></child-component>

	父组件中：
		this.$refs['child'] 找到了子组件这个实例
```

### parentparentchildren

```
	在子组件中 this.$parent 可以直接得到父组件

	在父组件中 this.$children[下标] 直接得到子组件
```

### provide inject

```
在父组件中
 let Home={
	 provide:{
		 demo:值
	 }
 }

在子组件中

let Child={
	inject:['demo']
}
```

### 插槽

```
01简单用法：
    定义 组件时
    在组件 template 属性中 加 <slot></slot>  占位 （vue系统组件）

    在使用组件时，组件（自定义标签）的内容 会自动灌入到slot的位置

02，具名插槽
    插槽可以命名，在 使用组件时 组件中的内容，可以有针对的灌入不同的命名插槽中

     let CommonHead = {
            template:`
                <div class="child">
                    <slot></slot>
                    <h3>我是子组件</h3>    
                    <header>
                        <slot name="header"></slot>
                    </header>
                    <section>
                        <slot name="main"></slot>
                    </section>
                    <footer>
                        <slot name="footer"></slot>
                    </footer>
                </div>
            `
        }
    let Home={
            template:`
                <div>
                    <h2>首页</h2>    
                    <common-head>
                        <h3>我是没有名字的内容</h3>
                        <h4 slot="header">我是头部</h4>
                        <h5 slot="main"> 我是主体内容 </h5>
                        <h6 slot="footer">
                            <i> 我是底部 </i>
                        </h6>
                    </common-head>
                </div>
            `,
            components:{
                CommonHead
            }

        }
    注意：
        在使用组件  内容中，如果没有加 slot属性对应命名插槽，会默认 灌入到没有名字的slot中
```

### transition 组件 用于定义 过渡（动画）

```
<transition name="fade">
    <div v-show="isShow">
        dwdw 
    </div>
</transition>

.fade-enter{

}
.fade-enter-active{

}
.fade-leave-to{

}
.fade-leave-active{

}

css3中动画？
    01，定义动画  @keyframes 名字{}
    02，目标元素 使用动画
    animation:名字 总时间 延迟时间 效果 次数 顺序（normal,reverse alternate alternate-reverse）
    次数 (infinite) （forward backward both） 

    animation-fill-mode:
        forward 
        backward
        both

    使用css3 动画 只需要 
    .v-enter-active
    .v-leave-active
    在这两个类中指定 动画属性即可

注意：
    transition 组件 只能控制 单个元素动画
    一下几种属于单个元素
        eg:
            <transition>
                <div>
                    <p></p>
                    <p></p>
                    <p></p>
                    <p></p>
                </div>
            </transition>

             <transition>
                <common-header>
                    <p></p>
                    <p></p>
                    <p></p>
                </common-header>
            </transition>

            <transition>
                <div v-if="isShow"></div>
                <p v-else></p>
            </transition>
            注意： 通过v-if v-else 的动画，需要定义mode属性
                in-out：新元素先进行过渡，完成之后当前元素过渡离开。
                out-in：当前元素先进行过渡，完成之后新元素过渡进入。 常用

            <transition>  这是多个元素的动画  transition 控制不了
                <div v-show="isShow"></div>
                <p v-show="isShow"></p>
            </transition>

        transition-group 用于控制多个元素的动画 （动画列表）
```

### mixin 混入

```
当多个组件 有相同的属性 和相同的方法 或者 相同的其他的属性如（侦听器。。。。。），我们就可以定义一个公共的混入
let mixin1 = {
    data(){
        return {

        }
    },
    methods:{},
    watch:{},
    components:{

    },
    computed:{

    }
    ....
}
注意：混入 的结构 一定要和 组件的结构 一致

注入混入
在需要 注入的 组件中 设置 mixins

let Home={
    template:
    data,
    methods,
    mixins:[mixin1]
}
```

### vue自定义指令

```
全局指令
    Vue.directive('指令名字',{
        bind(el,binding){
            //el 是绑定元素(dom对象)
            //binding 是对象 name 是指令名字 value 使用指令传的参数
            // 只触发一次
        },
        inserted(el,binding){
             //el 是绑定元素(dom对象)
            //binding 是对象 name 是指令名字 value 使用指令传的参数
            // 当前元素 插入到 父节点中 触发
        }
    })

    let Home = {
        template:{},
        data(){
            return{}
        },
        directives:{
            "组件名":{
                bind(el,binding){
                    //el 是绑定元素(dom对象)
                    //binding 是对象 name 是指令名字 value 使用指令传的参数
                    // 只触发一次
                },
                inserted(el,binding){
                    //el 是绑定元素(dom对象)
                    //binding 是对象 name 是指令名字 value 使用指令传的参数
                    // 当前元素 插入到 父节点中 触发
                }
            }
        }
    }

    注意：
        1，如何使用 指令 是当做 自定义属性来用
            在需要 使用的dom节点上 v-指令名="传值"
        2，全局指令可以在任意实例上使用，局部的只能在当前实例上使用
```

### 过滤器

```
 全局过滤器
    Vue.filter("过滤器名字",(v)=>{
        return "$"+v
    })

 局部过滤器

 let home={
     filters:{
         "过滤器名字":(v)=>{
             return v+xxx;
         }
     }
 }
 使用：
    不传参
    {{ 状态 | 过滤器名字 }}
    传参
    {{ 状态 | 过滤器名字(参数) }}
```

### 组件生命周期

![组件生命周期](https://cn.vuejs.org/images/lifecycle.png)

```
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

### keep-alive vue提供的系统组件 包裹需要缓存的组件，在这个组件销毁后（不会将组件真的销毁，而是缓存起来）

### vue中请求ajax

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

### axios ajax库 （vue+axios react+axios angular+axios）

```
axios 是一个 封装好的 第三方ajax请求的库
    特点：支持restfulapi 
        支持promise 解决回到地狱
        支持ajax请求拦截
vue-resource 停止维护了 ，官网不推荐使用
get请求：
    axios.get('url?params1=v&params2=值2').then().catch()
    axios.get(url,{
        params:{
            key1:v1,
            key2:v2
        }
    })

post请求：
    axios.post(url,{
        key1:v1,
        key2:v2
    }).then().catch()
注意：axios 中 默认 是以 json的形式发送数据 不是以 
application/x-www-form-urlencoded  "名字=值&名字2=值2"
json 
    {
        key:v
    }
    所以 你在和后端交互时，后端如果没有配置 接收post json格式，会导致后端接收不到数据
    如何解决？
    1，设置axios 
        request-headers content-type默认值 "application/json"
             content-type:"application/x-www-form-urlencoded"
    2,使用qs 库
        npm i qs -S
        import qs from 'qs'
        qs.stringify({
            a:10,
            b:20
        })
        结果： "a=10&b=20"

        qs.parse("a=10&b=20");
        结果：
            {
                a:10,
                b:20
            }

 直接使用
    axios({
    method: 'post',
    url: '/user/12345',
    data: {
        firstName: 'Fred',
        lastName: 'Flintstone'
    }
    }).then().catch()
    

创建一个实例来使用

  const http = axios.create({
            baseURL: 'http://localhost:3000',
            timeout: 8000,
            headers: {}
        });
    使用时，同axios
    http.get()
    http.post()

axios拦截器： 用于做全局的axios封装（结合实例来使用）

axios.interceptors.request.use(function (config) {
    // 可以拦截请求，在请求发送之前做啥都行
    config.data = qs.stringify(data)
    return config;
  }, function (error) {
    // 当请求错误之后会在这里拦截
    return Promise.reject(error);
  });

// Add a 相应拦截 interceptor
axios.interceptors.response.use(function (response) {
    // response 是服务端相应的数据 成功返回会在这里拦截
    return response;
  }, function (error) {
    // 服务器返回出错，会在这里进行拦截
  
    return Promise.reject(error);
  });


https://www.jianshu.com/p/7387a28a7e6f 实战中  vue 封装axios
```

### vue路由 ***

```
https://router.vuejs.org/zh/
单页面应用：single page application  
（整个项目只有一个html）
优点：
    页面性能较高 
    页面间切换速度很快

缺点：
    不利于seo （搜索引擎优化）
    爬虫 爬取页面关键字
    单页面应用一般都是基于webpack进行构建的，所以资源，都是在js中，爬虫不认识js

vue路由：
    一个地址 对应 一个组件

  
vuecli脚手架
```

