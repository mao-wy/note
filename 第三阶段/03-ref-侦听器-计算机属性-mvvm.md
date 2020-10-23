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