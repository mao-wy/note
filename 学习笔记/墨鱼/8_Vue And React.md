# [#](https://cchroot.github.io/interview/pages/interview notes/你能实现一个没有漏洞的双向绑定吗.html#你能实现一个没有漏洞的双向绑定吗)你能实现一个没有漏洞的双向绑定吗

## [#](https://cchroot.github.io/interview/pages/interview notes/你能实现一个没有漏洞的双向绑定吗.html#简单的双向绑定)简单的双向绑定

以下是利用 Object.defineProperty 实现的一个简单双向绑定：

```html
<p>请输入:</p>
<input type="text" id="input">
<p id="p"></p>
```



```js
let obj = {}
let input = document.getElementById('input')
let p = document.getElementById('p')
// 数据劫持
Object.defineProperty(obj, 'text', {
  configurable: true,
  enumerable: true,
  get() {
    console.log('获取数据了')
  },
  set(newVal) {
    console.log('数据更新了')
    // input.value = newVal
    p.innerHTML = newVal
  }
})
// 输入监听
input.addEventListener('keyup', function(e) {
  obj.text = e.target.value
})
```



利用 proxy 实现：

```js
let input = document.getElementById('input')
let p = document.getElementById('p')
let obj = {
	text: 111
}
const hander = {
	get: function (target, key) {
		console.log(`${key} 被读取`);
		return target[key];
	},
	set: function(target, key, value) {
		console.log(`${key} 被设置为 ${value}`);
	    target[key] = value
	    // 以下改变 input 的内容
		// input.value = newVal
	    p.innerHTML = value
	}
}
const proxy = new Proxy(obj, hander)
console.log(proxy.text)
// 输入监听
input.addEventListener('keyup', function(e) {
  proxy.text = e.target.value
  console.log(obj.text)
})
```



## [#](https://cchroot.github.io/interview/pages/interview notes/你能实现一个没有漏洞的双向绑定吗.html#无漏洞版双向绑定)无漏洞版双向绑定

完整的双向绑定需要**三个核心实现类**：

- Observer : 它的作用是给对象的属性添加 getter 和 setter，用于依赖收集和派发更新
- Dep : 用于收集当前响应式对象的依赖关系,每个响应式对象包括子对象都拥有一个 Dep 实例（里面 subs 是 Watcher 实例数组）,当数据有变更时,会通过 dep.notify() 通知各个 watcher
- Watcher : 观察者对象, 实例分为渲染 watcher (render watcher),计算属性 watcher (computed watcher),侦听器 watcher（user watcher）三种

```html
<p>请输入:</p>
<input type="text" id="input">
<p id="p"></p>
```



```js
const Vue = (function() {
  let uid = 0;
  // 用于储存订阅者并发布消息
  class Dep {
    constructor() {
      // 设置id,用于区分新Watcher和只改变属性值后新产生的Watcher
      this.id = uid++;
      // 储存订阅者的数组
      this.subs = [];
    }
    // 触发target上的Watcher中的addDep方法,参数为dep的实例本身
    depend() {
      Dep.target.addDep(this);
    }
    // 添加订阅者
    addSub(sub) {
      this.subs.push(sub);
    }
    notify() {
      // 通知所有的订阅者(Watcher)，触发订阅者的相应逻辑处理
      this.subs.forEach(sub => sub.update());
    }
  }
  // 为Dep类设置一个静态属性,默认为null,工作时指向当前的Watcher
  Dep.target = null;
  // 监听者,监听对象属性值的变化
  class Observer {
    constructor(value) {
      this.value = value;
      this.walk(value);
    }
    // 遍历属性值并监听
    walk(value) {
      Object.keys(value).forEach(key => this.convert(key, value[key]));
    }
    // 执行监听的具体方法
    convert(key, val) {
      defineReactive(this.value, key, val);
    }
  }

  function defineReactive(obj, key, val) {
    const dep = new Dep();
    // 给当前属性的值添加监听
    let chlidOb = observe(val);
    Object.defineProperty(obj, key, {
      enumerable: true,
      configurable: true,
      get: () => {
        // 如果Dep类存在target属性，将其添加到dep实例的subs数组中
        // target指向一个Watcher实例，每个Watcher都是一个订阅者
        // Watcher实例在实例化过程中，会读取data中的某个属性，从而触发当前get方法
        if (Dep.target) {
          dep.depend();
        }
        return val;
      },
      set: newVal => {
        if (val === newVal) return;
        val = newVal;
        // 对新值进行监听
        chlidOb = observe(newVal);
        // 通知所有订阅者，数值被改变了
        dep.notify();
      },
    });
  }

  function observe(value) {
    // 当值不存在，或者不是复杂数据类型时，不再需要继续深入监听
    if (!value || typeof value !== 'object') {
      return;
    }
    return new Observer(value);
  }

  class Watcher {
    constructor(vm, expOrFn, cb) {
      this.depIds = {}; // hash储存订阅者的id,避免重复的订阅者
      this.vm = vm; // 被订阅的数据一定来自于当前Vue实例
      this.cb = cb; // 当数据更新时想要做的事情
      this.expOrFn = expOrFn; // 被订阅的数据
      this.val = this.get(); // 维护更新之前的数据
    }
    // 对外暴露的接口，用于在订阅的数据被更新时，由订阅者管理员(Dep)调用
    update() {
      this.run();
    }
    addDep(dep) {
      // 如果在depIds的hash中没有当前的id,可以判断是新Watcher,因此可以添加到dep的数组中储存
      // 此判断是避免同id的Watcher被多次储存
      if (!this.depIds.hasOwnProperty(dep.id)) {
        dep.addSub(this);
        this.depIds[dep.id] = dep;
      }
    }
    run() {
      const val = this.get();
      console.log(val);
      if (val !== this.val) {
        this.val = val;
        this.cb.call(this.vm, val);
      }
    }
    get() {
      // 当前订阅者(Watcher)读取被订阅数据的最新更新后的值时，通知订阅者管理员收集当前订阅者
      Dep.target = this;
      const val = this.vm._data[this.expOrFn];
      // 置空，用于下一个Watcher使用
      Dep.target = null;
      console.log(Dep.target, 2);
      return val;
    }
  }

  class Vue {
    constructor(options = {}) {
      // 简化了$options的处理
      this.$options = options;
      // 简化了对data的处理
      let data = (this._data = this.$options.data);
      // 将所有data最外层属性代理到Vue实例上
      Object.keys(data).forEach(key => this._proxy(key));
      // 监听数据
      observe(data);
    }
    // 对外暴露调用订阅者的接口，内部主要在指令中使用订阅者
    $watch(expOrFn, cb) {
      new Watcher(this, expOrFn, cb);
    }
    _proxy(key) {
      Object.defineProperty(this, key, {
        configurable: true,
        enumerable: true,
        get: () => this._data[key],
        set: val => {
          this._data[key] = val;
        },
      });
    }
  }

  return Vue;
})();

let demo = new Vue({
  data: {
    text: '',
  },
});

const p = document.getElementById('p');
const input = document.getElementById('input');

input.addEventListener('keyup', function(e) {
  demo.text = e.target.value;
});

demo.$watch('text', str => p.innerHTML = str);
```



**Watcher 和 Dep 的关系**:

watcher 中实例化了 dep 并向 dep.subs 中添加了订阅者,dep 通过 notify 遍历了 dep.subs 通知每个 watcher 更新。

**依赖收集**

1. initState 时,对 computed 属性初始化时,触发 computed watcher 依赖收集
2. initState 时,对侦听属性初始化时,触发 user watcher 依赖收集
3. render()的过程,触发 render watcher 依赖收集
4. re-render 时,vm.render()再次执行,会移除所有 subs 中的 watcer 的订阅,重新赋值。

**派发更新**

1. 组件中对响应的数据进行了修改,触发 setter 的逻辑
2. 调用 dep.notify()
3. 遍历所有的 subs（Watcher 实例）,调用每一个 watcher 的 update 方法。

**原理**

当创建 Vue 实例时,vue 会遍历 data 选项的属性,利用 Object.defineProperty 为属性添加 getter 和 setter 对数据的读取进行劫持（getter 用来依赖收集,setter 用来派发更新）,并且在内部追踪依赖,在属性被访问和修改时通知变化。

每个组件实例会有相应的 watcher 实例,会在组件渲染的过程中记录依赖的所有数据属性（进行依赖收集,还有 computed watcher,user watcher 实例）,之后依赖项被改动时,setter 方法会通知依赖与此 data 的 watcher 实例重新计算（派发更新）,从而使它关联的组件重新渲染。

一句话总结:

vue.js 采用数据劫持结合发布-订阅模式,通过 Object.defineproperty 来劫持各个属性的 setter,getter,在数据变动时发布消息给订阅者,触发响应的监听回调



# [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#vue源码阅读笔记)vue源码阅读笔记

## [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#数据驱动)数据驱动

Vue.js 一个核心思想是数据驱动。所谓数据驱动，是指视图是由数据驱动生成的，我们对视图的修改，不会直接操作 DOM，而是通过修改数据。

数据驱动还有一部分是数据更新驱动视图变化，这一块内容我们也会在之后的章节分析，这一章我们的目标是弄清楚模板和数据如何渲染成最终的 DOM。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#new-vue-发生了什么)new Vue 发生了什么？

Vue 实际上就是一个用 Function 实现的类，我们只能通过 new Vue 去实例化它。

Vue 只能通过 new 关键字初始化，然后会调用 this._init 方法

```js
vm._self = vm
initLifecycle(vm)
initEvents(vm)
initRender(vm)
callHook(vm, 'beforeCreate')
initInjections(vm) // resolve injections before data/props
initState(vm)
initProvide(vm) // resolve provide after data/props
callHook(vm, 'created')

// ...

if (vm.$options.el) {
  vm.$mount(vm.$options.el)
}
```



Vue 初始化主要就干了几件事情，合并配置，初始化生命周期，初始化事件中心，初始化渲染，初始化 data、props、computed、watcher 等等

其中，在 initState 中会判断 $options 的配置来初始化 props,methods,data,computed,watch 等，并会通过不同的方法挂载在 vm 上，例如： data 就会通过 `proxy(vm,`_data`, key)` ，将 data 对象挂载在 `vm._data.xxx` 上，本质上访问 `this.xxx` （这里的 xxx 指 data 里面的属性）就是访问 `this._data.xxx`，挂载后会通过 `observe(data, true /* asRootData */)` 对 data 进行响应式处理。

在初始化的最后，检测到如果有 el 属性，则调用 vm.$mount 方法挂载 vm，挂载的目标就是把模板渲染成最终的 DOM，那么接下来我们来分析 Vue 的挂载过程。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#vue-实例挂载的实现)Vue 实例挂载的实现

代码首先缓存了原型上的 $mount 方法，再重新定义该方法，我们先来分析这段代码。首先，它对 el 做了限制，Vue 不能挂载在 body、html 这样的根节点上。接下来的是很关键的逻辑 —— 如果没有定义 render 方法，则会把 el 或者 template 字符串转换成 render 方法。这里我们要牢记，在 Vue 2.0 版本中，所有 Vue 的组件的渲染最终都需要 render 方法，无论我们是用单文件 .vue 方式开发组件，还是写了 el 或者 template 属性，最终都会转换成 render 方法，那么这个过程是 Vue 的一个“在线编译”的过程，它是调用 compileToFunctions 方法实现的。最后，调用原先原型上的 $mount 方法挂载。

```js
// public mount method
Vue.prototype.$mount = function (
  el?: string | Element,
  hydrating?: boolean
): Component {
  el = el && inBrowser ? query(el) : undefined
  return mountComponent(this, el, hydrating)
}
```



$mount 方法实际上会去调用 mountComponent 方法，mountComponent 核心就是先实例化一个渲染 Watcher，在它的回调函数中会调用 updateComponent 方法，在此方法中调用 vm._render 方法先生成虚拟 Node，最终调用 vm._update 更新 DOM。

Watcher 在这里起到两个作用，一个是初始化的时候会执行回调函数，另一个是当 vm 实例中的监测的数据发生变化的时候执行回调函数，这块儿我们会在之后的章节中介绍。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#render)render

Vue 的 _render 方法是实例的一个私有方法，它用来把实例渲染成一个虚拟 Node。

我们在平时的开发工作中手写 render 方法的场景比较少，而写的比较多的是 template 模板，在之前的 mounted 方法的实现中，会把 template 编译成 render 方法。

vm._render 最终是通过执行 createElement 方法并返回的是 vnode，它是一个虚拟 Node。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#virtual-dom)Virtual DOM

其实 VNode 是对真实 DOM 的一种抽象描述，它的核心定义无非就几个关键属性，标签名、数据、子节点、键值等，其它属性都是用来扩展 VNode 的灵活性以及实现一些特殊 feature 的。由于 VNode 只是用来映射到真实 DOM 的渲染，不需要包含操作 DOM 的方法，因此它是非常轻量和简单的。

Virtual DOM 除了它的数据结构的定义，映射到真实的 DOM 实际上要经历 VNode 的 create、diff、patch 等过程。那么在 Vue.js 中，VNode 的 create 是通过之前提到的 createElement 方法创建的，我们接下来分析这部分的实现。

这里可以回答一个问题，vue 初始化的时候为什么不能把根元素挂在在 body 或者 html 上？

```js
 if (el === document.body || el === document.documentElement) {
    process.env.NODE_ENV !== 'production' && warn(
      `Do not mount Vue to <html> or <body> - mount to normal elements instead.`
    )
    return this
  }
```



其实是因为，挂在的根元素会给 Virtual DOM 重新生成的 DOM 全部替换掉了，我们不能把整个 html 或者 body 给替换掉，不然就不是 html 了。你可以这样测试：

```text
import app = new Vue({
  el: '#app',
  render(createElement) {
	return createElement('div', {
	  attrs: {
		id: '#app1'
	  }
	}, this.message)
  },
  data() {
	return {
	  message: 'Hellow Vue!'
	}
  }
})
```



### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#createelement)createElement

Vue.js 利用 createElement 方法创建 VNode，createElement 方法实际上是对 _createElement 方法的封装，它允许传入的参数更加灵活，在处理这些参数后，调用真正创建 VNode 的函数 _createElement。

createElement 函数的流程略微有点多，主要有 2 个重点的流程 —— children 的规范化以及 VNode 的创建。children 的规范化将children 变成了一个类型为 VNode 的 Arr，规范化 children 后，接下来会去根据不同情况创建一个 VNode 的实例。

每个 VNode 有 children，children 每个元素也是一个 VNode，这样就形成了一个 VNode Tree，它很好的描述了我们的 DOM Tree。

回到 mountComponent 函数的过程，我们已经知道 vm._render 是如何创建了一个 VNode，接下来就是要把这个 VNode 渲染成一个真实的 DOM 并渲染出来，这个过程是通过 vm._update 完成的，接下来分析一下这个过程。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#update)update

Vue 的 _update 是实例的一个私有方法，它被调用的时机有 2 个，一个是首次渲染，一个是数据更新的时候；_update 方法的作用是把 VNode 渲染成真实的 DOM 。

_update 的核心就是调用 vm.**patch** 方法，这个方法实际上在不同的平台，比如 web 和 weex 上的定义是不一样的，至在 web 平台上，是否是服务端渲染也会对这个方法产生影响。

```js
Vue.prototype.__patch__ = inBrowser ? patch : noop
```



**patch** 方法的定义是调用 createPatchFunction 方法的返回值，createPatchFunction 内部定义了一系列的辅助方法，最终返回了一个 patch 方法，这个方法就赋值给了 vm._update 函数里调用的 vm.**patch**。

例子：

```js
var app = new Vue({
  el: '#app',
  render: function (createElement) {
    return createElement('div', {
      attrs: {
        id: 'app'
      },
    }, this.message)
  },
  data: {
    message: 'Hello Vue!'
  }
})
```



我们在 vm._update 的方法里是这么调用 patch 方法的：

```js
// initial render
vm.$el = vm.__patch__(vm.$el, vnode, hydrating, false /* removeOnly */)
```



结合我们的例子，我们的场景是首次渲染，所以在执行 patch 函数的时候，传入的 vm.$el 对应的是例子中 id 为 app 的 DOM 对象，这个也就是我们在 index.html 模板中写的

， vm.$el 的赋值是在之前 mountComponent 函数做的，vnode 对应的是调用 render 函数的返回值，hydrating 在非服务端渲染情况下为 false，removeOnly 为 false。



确定了这些入参后，我们回到 patch 函数的执行过程，看几个关键步骤。

由于我们传入的 oldVnode 实际上是一个 DOM container，所以 isRealElement 为 true，接下来又通过 emptyNodeAt 方法把 oldVnode 转换成 VNode 对象，然后再调用 createElm 方法。createElm 的作用是通过虚拟节点创建真实的 DOM 并插入到它的父节点中（递归创建）。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#总结)总结

那么至此我们从主线上把模板和数据如何渲染成最终的 DOM 的过程分析完毕了，我们可以通过下图更直观地看到从初始化 Vue 到最终渲染的整个过程。

![new-vue](imgs/Vue And React/new-vue.9f257f78.png)

我们这里只是分析了最简单和最基础的场景，在实际项目中，我们是把页面拆成很多组件的，Vue 另一个核心思想就是组件化。那么下一章我们就来分析 Vue 的组件化过程。

## [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#组件化)组件化

Vue.js 另一个核心思想是组件化。所谓组件化，就是把页面拆分成多个组件 (component)，每个组件依赖的 CSS、JavaScript、模板、图片等资源放在一起开发和维护。组件是资源独立的，组件在系统内部可复用，组件和组件之间可以嵌套。

那么在这一章节，我们将从源码的角度来分析 Vue 的组件内部是如何工作的，只有了解了内部的工作原理，才能让我们使用它的时候更加得心应手。

接下来我们会用 Vue-cli 初始化的代码为例，来分析一下 Vue 组件初始化的一个过程。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#createcomponent)createComponent

我们分析了 createComponent 的实现，了解到它在渲染一个组件的时候的 3 个关键逻辑：构造子类构造函数，安装组件钩子函数和实例化 vnode。createComponent 后返回的是组件 vnode，它也一样走到 vm._update 方法，进而执行了 patch 函数，我们在上一章对 patch 函数做了简单的分析，那么下一节我们会对它做进一步的分析。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#patch)patch

当我们通过 createComponent 创建了组件 VNode，接下来会走到 vm._update，执行 vm.**patch** 去把 VNode 转换成真正的 DOM 节点。这个过程我们在前一章已经分析过了，但是针对一个普通的 VNode 节点，接下来我们来看看组件的 VNode 会有哪些不一样的地方。

- patch 的整体流程： createComponent -> 子组件初始化 -> 子组件 render -> 子组件 patch
- activeInstance 为当前激活的 vm 实例；vm.$vnode 为组件的占位 vnode; vm._vnode 为组件的渲染 vnode
- 嵌套组件的插入顺序是先子后父，对应 vue 的生命周期也是子组件先 mounted

那么到此，一个组件的 VNode 是如何创建、初始化、渲染的过程也就介绍完毕了。在对组件化的实现有一个大概了解后，接下来我们来介绍一下这其中的一些细节。我们知道编写一个组件实际上是编写一个 JavaScript 对象，对象的描述就是各种配置，之前我们提到在 _init 的最初阶段执行的就是 merge options 的逻辑，那么下一节我们从源码角度来分析合并配置的过程。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#合并配置)合并配置

通过之前章节的源码分析我们知道，new Vue 的过程通常有 2 种场景，一种是外部我们的代码主动调用 new Vue(options) 的方式实例化一个 Vue 对象；另一种是我们上一节分析的组件过程中内部通过 new Vue(options) 实例化子组件。

无论哪种场景，都会执行实例的 _init(options) 方法，它首先会执行一个 merge options 的逻辑。不同场景对于 options 的合并逻辑是不一样的，并且传入的 options 值也有非常大的不同。

- 外部调用场景下合并配置是通过 mergeOption，并遵循一定的合并策略
- 组件合并是通过 initInternalComponent，它的合并更快
- 库、框架的设计几乎都是类似的，自身定义了一些默认配置，同时又可以在初始化阶段传入一些定义配置，然后去 merge 默认配置，来达到定制化不同需求的目的

那么至此，Vue 初始化阶段对于 options 的合并过程就介绍完了，我们需要知道对于 options 的合并有 2 种方式，子组件初始化过程通过 initInternalComponent 方式要比外部初始化 Vue 通过 mergeOptions 的过程要快，合并完的结果保留在 vm.$options 中。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#生命周期)生命周期

**beforeCreate & created**

beforeCreate 和 created 函数都是在实例化 Vue 的阶段，在 _init 方法中执行的。

beforeCreate 和 created 的钩子调用是在 initState 的前后，initState 的作用是初始化 props、data、methods、watch、computed 等属性，之后我们会详细分析。那么显然 beforeCreate 的钩子函数中就不能获取到 props、data 中定义的值，也不能调用 methods 中定义的函数。

在这俩个钩子函数执行的时候，并没有渲染 DOM，所以我们也不能够访问 DOM，一般来说，如果组件在加载的时候需要和后端有交互，放在这俩个钩子函数执行都可以，如果是需要访问 props、data 等数据的话，就需要使用 created 钩子函数。之后我们会介绍 vue-router 和 vuex 的时候会发现它们都混合了 beforeCreate 钩子函数。

**beforeMount & mounted**

顾名思义，beforeMount 钩子函数发生在 mount，也就是 DOM 挂载之前，它的调用时机是在 mountComponent 函数中。

在执行 vm._render() 函数渲染 VNode 之前，执行了 beforeMount 钩子函数，在执行完 vm._update() 把 VNode patch 到真实 DOM 后，执行 mounted 钩子。

注意，这里对 mounted 钩子函数执行有一个判断逻辑，vm.$vnode 如果为 null，则表明这不是一次组件的初始化过程，而是我们通过外部 new Vue 初始化过程。那么对于组件，它的 mounted 时机在哪儿呢？

之前我们提到过，组件的 VNode patch 到 DOM 后，会执行 invokeInsertHook 函数，把 insertedVnodeQueue 里保存的钩子函数依次执行一遍，该函数会执行 insert 这个钩子函数，insert 钩子函数中会执行 `callHook(componentInstance, 'mounted')`。

我们可以看到，每个子组件都是在这个钩子函数中执行 mounted 钩子函数，并且我们之前分析过，insertedVnodeQueue 的添加顺序是先子后父，所以对于同步渲染的子组件而言，mounted 钩子函数的执行顺序也是先子后父。

**beforeUpdate & updated**

顾名思义，beforeUpdate 和 updated 的钩子函数执行时机都应该是在数据更新的时候。

beforeUpdate 的执行时机是在渲染 Watcher 的 before 函数中，注意这里有个判断，也就是在组件已经 mounted 之后，才会去调用这个钩子函数。

update 的执行时机是在 flushSchedulerQueue 函数调用的时候，flushSchedulerQueue 函数我们之后会详细介绍，可以先大概了解一下，updatedQueue 是更新了的 wathcer 数组，那么在 callUpdatedHooks 函数中，它对这些数组做遍历，只有满足当前 watcher 为 vm._watcher 以及组件已经 mounted 这两个条件，才会执行 updated 钩子函数。

**beforeDestroy & destroyed**

beforeDestroy 和 destroyed 钩子函数的执行时机在组件销毁的阶段。

beforeDestroy 钩子函数的执行时机是在 $destroy 函数执行最开始的地方，接着执行了一系列的销毁动作，包括从 parent 的 $children 中删掉自身，删除 watcher，当前渲染的 VNode 执行销毁钩子函数等，执行完毕后再调用 destroy 钩子函数。

在 $destroy 的执行过程中，它又会执行 vm.**patch**(vm._vnode, null) 触发它子组件的销毁钩子函数，这样一层层的递归调用，所以 destroy 钩子函数执行顺序是先子后父，和 mounted 过程一样。

**activated & deactivated**

activated 和 deactivated 钩子函数是专门为 keep-alive 组件定制的钩子，我们会在介绍 keep-alive 组件的时候详细介绍。

**总结**

- vue.js 的生命周期函数就是再初始化及数据更新过程各个阶段执行不同的钩子函数
- 在 created 钩子函数中可以访问到数据，在 mounted 钩子函数中可以访问到 DOM，在 destroyed 钩子函数中可以做一些定时器销毁工作

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#组件注册)组件注册

在 Vue.js 中，除了它内置的组件如 keep-alive、component、transition、transition-group 等，其它用户自定义组件在使用前必须注册。Vue.js 提供了 2 种组件的注册方式，全局注册和局部注册。

要注册一个全局组件，可以使用 Vue.component(tagName, options)。那么，Vue.component 函数是在什么时候定义的呢，它的定义过程发生在最开始初始化 Vue 的全局函数的时候。

局部注册：在组件的 Vue 的实例化阶段有一个合并 option 的逻辑，之前我们也分析过，所以就把 components 合并到 vm.$options.components 上，这样我们就可以在 resolveAsset 的时候拿到这个组件的构造函数，并作为 createComponent 的钩子的参数。

注意，局部注册和全局注册不同的是，只有该类型的组件才可以访问局部注册的子组件，而全局注册是扩展到 Vue.options 下，所以在所有组件创建的过程中，都会从全局的 Vue.options.components 扩展到当前组件的 vm.$options.components 下，这就是全局注册的组件能被任意使用的原因。

通常组件库中的基础组件建议全局注册，而业务组件建议局部注册。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#异步组件)异步组件

Vue 的异步组件又 3 种异步组件的实现方式，并且看到高级异步组件的实现是非常巧妙的，它实现了 loading、resolve、reject、timeout 4 种状态。

异步组件实现的本质是 2 次渲染（通常是 2 次），除了 0 delay 的高级异步组件第一次直接渲染成 loading 组件外（总共三次渲染），其它都是第一次渲染生成一个注释节点，当异步获取组件成功后，再通过 forceRender 强制重新渲染，这样就能正确渲染出我们异步加载的组件了。

## [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#深入响应式原理)深入响应式原理

前面 2 章介绍的都是 Vue 怎么实现数据渲染和组件化的，主要讲的是初始化的过程，把原始的数据最终映射到 DOM 中，但并没有涉及到数据变化到 DOM 变化的部分。而 Vue 的数据驱动除了数据渲染 DOM 之外，还有一个很重要的体现就是数据的变更会触发 DOM 的变化。

那么 Vue 是如何在我们对数据修改后自动做这些事情呢，接下来我们将进入一些 Vue 响应式系统的底层的细节。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#响应式对象)响应式对象

Vue.js 实现响应式的核心是利用了 ES5 的 Object.defineProperty，这也是为什么 Vue.js 不能兼容 IE8 及以下浏览器的原因。

**Object.defineProperty**

Object.defineProperty 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

```js
Object.defineProperty(obj, prop, descriptor)
```



obj 是要在其上定义属性的对象；prop 是要定义或修改的属性的名称；descriptor 是将被定义或修改的属性描述符。

比较核心的是 descriptor，它有很多可选键值，具体的可以去参阅它的文档。这里我们最关心的是 get 和 set，get 是一个给属性提供的 getter 方法，当我们访问了该属性的时候会触发 getter 方法；set 是一个给属性提供的 setter 方法，当我们对该属性做修改的时候会触发 setter 方法。

一旦对象拥有了 getter 和 setter，我们可以简单地把这个对象称为响应式对象。那么 Vue.js 把哪些对象变成了响应式对象了呢，接下来我们从源码层面分析。

**initState**

在 Vue 的初始化阶段，_init 方法执行的时候，会执行 initState(vm) 方法，initState 方法主要是对 props、methods、data、computed 和 wathcer 等属性做了初始化操作。这里我们重点分析 props 和 data，对于其它属性的初始化我们之后再详细分析。

props 的初始化主要过程，就是遍历定义的 props 配置。遍历的过程主要做两件事情：一个是调用 defineReactive 方法把每个 prop 对应的值变成响应式，可以通过 vm._props.xxx 访问到定义 props 中对应的属性。对于 defineReactive 方法，我们稍后会介绍；另一个是通过 proxy 把 vm._props.xxx 的访问代理到 vm.xxx 上，我们稍后也会介绍。

data 的初始化主要过程也是做两件事，一个是对定义 data 函数返回对象的遍历，通过 proxy 把每一个值 vm._data.xxx 都代理到 vm.xxx 上；另一个是调用 observe 方法观测整个 data 的变化，把 data 也变成响应式，可以通过 vm._data.xxx 访问到定义 data 返回函数中对应的属性，observe 我们稍后会介绍。

可以看到，无论是 props 或是 data 的初始化都是把它们变成响应式对象，这个过程我们接触到几个函数，接下来我们来详细分析它们。

**proxy**

首先介绍一下代理，代理的作用是把 props 和 data 上的属性代理到 vm 实例上，这也就是为什么比如我们定义了如下 props，却可以通过 vm 实例访问到它。

```js
export function proxy (target: Object, sourceKey: string, key: string) {
  sharedPropertyDefinition.get = function proxyGetter () {
    return this[sourceKey][key]
  }
  sharedPropertyDefinition.set = function proxySetter (val) {
    this[sourceKey][key] = val
  }
  Object.defineProperty(target, key, sharedPropertyDefinition)
}
```



proxy 方法的实现很简单，通过 `Object.defineProperty` 把 `target[sourceKey][key]` 的读写变成了对 `target[key]` 的读写。

**observe 和 Observer**

observe 的功能就是用来监测数据的变化，observe 方法的作用就是给非 VNode 的对象类型数据添加一个 Observer，如果已经添加过则直接返回，否则在满足一定条件下去实例化一个 Observer 对象实例。

Observer 是一个类，它的作用是给对象的属性添加 getter 和 setter，用于依赖收集和派发更新：

bserver 的构造函数逻辑很简单，首先实例化 Dep 对象，这块稍后会介绍，接着通过执行 def 函数把自身实例添加到数据对象 value 的 **ob** 属性上，

def 函数是一个非常简单的 Object.defineProperty 的封装，这就是为什么我在开发中输出 data 上对象类型的数据，会发现该对象多了一个 **ob** 的属性。

回到 Observer 的构造函数，接下来会对 value 做判断，对于数组会调用 observeArray 方法，否则对纯对象调用 walk 方法。可以看到 observeArray 是遍历数组再次调用 observe 方法，而 walk 方法是遍历对象的 key 调用 defineReactive 方法，那么我们来看一下这个方法是做什么的。

**defineReactive**

defineReactive 的功能就是定义一个响应式对象，给对象动态添加 getter 和 setter，defineReactive 函数最开始初始化 Dep 对象的实例，接着拿到 obj 的属性描述符，然后对子对象递归调用 observe 方法，这样就保证了无论 obj 的结构多复杂，它的所有子属性也能变成响应式的对象，这样我们访问或修改 obj 中一个嵌套较深的属性，也能触发 getter 和 setter。最后利用 Object.defineProperty 去给 obj 的属性 key 添加 getter 和 setter。而关于 getter 和 setter 的具体实现，我们会在之后介绍。

**总结**

这一节我们介绍了响应式对象，核心就是利用 Object.defineProperty 给数据添加了 getter 和 setter，目的就是为了在我们访问数据以及写数据的时候能自动执行一些逻辑：getter 做的事情是依赖收集，setter 做的事情是派发更新，那么在接下来的内容我们会重点对这两个过程分析。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#依赖收集)依赖收集

Vue 会把普通对象变成响应式对象，响应式对象 getter 相关的逻辑就是做依赖收集。

之前我们介绍当对数据对象的访问会触发他们的 getter 方法，那么这些对象什么时候被访问呢？

```text
vm._update(vm._render(), hydrating)
```



先执行 vm._render() 方法，因为之前分析过这个方法会生成 渲染 VNode，并且在这个过程中会对 vm 上的数据访问，这个时候就触发了数据对象的 getter。

那么每个对象值的 getter 都持有一个 dep，在触发 getter 的时候会调用 dep.depend() 方法，也就会执行 Dep.target.addDep(this)。

这时候会做一些逻辑判断（保证同一数据不会被添加多次）后执行 dep.addSub(this)，那么就会执行 this.subs.push(sub)，也就是说把当前的 watcher 订阅到这个数据持有的 dep 的 subs 中，这个目的是为后续数据变化时候能通知到哪些 subs 做准备。

所以在 vm._render() 过程中，会触发所有数据的 getter，这样实际上已经完成了一个依赖收集的过程。那么到这里就结束了么，其实并没有，在完成依赖收集后，还有几个逻辑要执行。

考虑到 Vue 是数据驱动的，所以每次数据变化都会重新 render，那么 vm._render() 方法又会再次执行，并再次触发数据的 getters，所以 Watcher 在构造函数中会初始化 2 个 Dep 实例数组，newDeps 表示新添加的 Dep 实例数组，而 deps 表示上一次添加的 Dep 实例数组。

在执行 cleanupDeps 函数的时候，会首先遍历 deps，移除对 dep.subs 数组中 Wathcer 的订阅，然后把 newDepIds 和 depIds 交换，newDeps 和 deps 交换，并把 newDepIds 和 newDeps 清空。

那么为什么需要做 deps 订阅的移除呢，在添加 deps 的订阅过程，已经能通过 id 去重避免重复订阅了。

考虑到一种场景，我们的模板会根据 v-if 去渲染不同子模板 a 和 b，当我们满足某种条件的时候渲染 a 的时候，会访问到 a 中的数据，这时候我们对 a 使用的数据添加了 getter，做了依赖收集，那么当我们去修改 a 的数据的时候，理应通知到这些订阅者。那么如果我们一旦改变了条件渲染了 b 模板，又会对 b 使用的数据添加了 getter，如果我们没有依赖移除的过程，那么这时候我去修改 a 模板的数据，会通知 a 数据的订阅的回调，这显然是有浪费的。

因此 Vue 设计了在每次添加完新的订阅，会移除掉旧的订阅，这样就保证了在我们刚才的场景中，如果渲染 b 模板的时候去修改 a 模板的数据，a 数据订阅回调已经被移除了，所以不会有任何浪费，真的是非常赞叹 Vue 对一些细节上的处理。

总结

- 依赖收集就是订阅数据变化的 watcher 的收集
- 依赖收集的目的是为了当这些响应式数据发生变化，触发它们的 setter 的时候，能知道应该通知哪些订阅者去做响应的逻辑处理。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#派发更新)派发更新

通过上一节分析我们了解了响应式数据依赖收集过程，收集的目的就是为了当我们修改数据的时候，可以对相关的依赖派发更新。

当我们在组件中对响应的数据做了修改，就会触发 setter 的逻辑，最后调用 dep.notify() 方法， 它是 Dep 的一个实例方法。这里的逻辑非常简单，遍历所有的 subs，也就是 Watcher 的实例数组，然后调用每一个 watcher 的 update 方法。

这里对于 Watcher 的不同状态，会执行不同的逻辑，computed 和 sync 等状态的分析我会之后抽一小节详细介绍，在一般组件数据更新的场景，会走到最后一个 queueWatcher(this) 的逻辑。

这里引入了一个队列的概念，这也是 Vue 在做派发更新的时候的一个优化的点，它并不会每次数据改变都触发 watcher 的回调，而是把这些 watcher 先添加到一个队列里，然后在 nextTick 后执行 flushSchedulerQueue。

flushSchedulerQueue 中会对队列进行：队列排序、队列遍历和状态恢复。

对于渲染 watcher 而言，它在执行 this.get() 方法求值的时候，会执行 getter 方法。所以这就是当我们去修改组件相关的响应式数据的时候，会触发组件重新渲染的原因，接着就会重新执行 patch 的过程，但它和首次渲染有所不同。

Vue 数据修改派发更新的过程，实际上就是当数据发生变化的时候，触发 setter 逻辑，把在依赖过程中订阅的的所有观察者，也就是 watcher，都触发它们的 update 过程，这个过程又利用了队列做了进一步优化，在 nextTick 后执行所有 watcher 的 run，最后执行它们的回调函数。nextTick 是 Vue 一个比较核心的实现了，下面我们来重点分析它的实现。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#nexttick)nextTick

Vue.js 提供了 2 种调用 nextTick 的方式，一种是全局 API Vue.nextTick，一种是实例上的方法 vm.$nextTick，无论我们使用哪一种，最后都是调用 next-tick.js 中实现的 nextTick 方法。

next-tick.js 申明了 microTimerFunc 和 macroTimerFunc 2 个变量，它们分别对应的是 micro task 的函数和 macro task 的函数。对于 macro task 的实现，优先检测是否支持原生 setImmediate，这是一个高版本 IE 和 Edge 才支持的特性，不支持的话再去检测是否支持原生的 MessageChannel，如果也不支持的话就会降级为 setTimeout 0；而对于 micro task 的实现，则检测浏览器是否原生支持 Promise，不支持的话直接指向 macro task 的实现。

next-tick.js 申明了 microTimerFunc 和 macroTimerFunc 2 个变量，它们分别对应的是 micro task 的函数和 macro task 的函数。对于 macro task 的实现，优先检测是否支持原生 setImmediate，这是一个高版本 IE 和 Edge 才支持的特性，不支持的话再去检测是否支持原生的 MessageChannel，如果也不支持的话就会降级为 setTimeout 0；而对于 micro task 的实现，则检测浏览器是否原生支持 Promise，不支持的话直接指向 macro task 的实现。

**总结**：

- nextTick 是把要执行的任务推入到一个队列中，在下一个 tick 同步执行
- 数据改变后，触发渲染 watcher 的 update，但是 watchers 的 flush 是在 nextTick 后，所以重新渲染是异步的
- 当 nextTick 不传 cb 参数的时候，提供一个 Promise 化的调用

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#检测变化的注意事项)检测变化的注意事项

**对象添加属性**

对于使用 Object.defineProperty 实现响应式的对象，当我们去给这个对象添加一个新的属性的时候，是不能够触发它的 setter 的。但是添加新属性的场景我们在平时开发中会经常遇到，那么 Vue 为了解决这个问题，定义了一个全局 API Vue.set 方法。

set 方法接收 3个参数，target 可能是数组或者是普通对象，key 代表的是数组的下标或者是对象的键值，val 代表添加的值。首先判断如果 target 是数组且 key 是一个合法的下标，则之前通过 splice 去添加进数组然后返回，这里的 splice 其实已经不仅仅是原生数组的 splice 了，稍后我会详细介绍数组的逻辑。接着又判断 key 已经存在于 target 中，则直接赋值返回，因为这样的变化是可以观测到了。接着再获取到 target.**ob** 并赋值给 ob，之前分析过它是在 Observer 的构造函数执行的时候初始化的，表示 Observer 的一个实例，如果它不存在，则说明 target 不是一个响应式的对象，则直接赋值并返回。最后通过 `defineReactive(ob.value, key, val)` 把新添加的属性变成响应式对象，然后再通过 `ob.dep.notify()` 手动的触发依赖通知。

在 getter 过程中判断了 childOb，并调用了 `childOb.dep.depend()` 收集了依赖，这就是为什么执行 Vue.set 的时候通过 `ob.dep.notify()` 能够通知到 watcher，从而让添加新的属性到对象也可以检测到变化。这里如果 value 是个数组，那么就通过 dependArray 把数组每个元素也去做依赖收集。

**数组**

Vue 也是不能检测到以下变动的数组：

1.当你利用索引直接设置一个项时，例如：`vm.items[indexOfItem] = newValue`

2.当你修改数组的长度时，例如：`vm.items.length = newLength`

对于第一种情况，可以使用：`Vue.set(example1.items, indexOfItem, newValue)`；而对于第二种情况，可以使用 `vm.items.splice(newLength)`。

我们刚才也分析到，对于 Vue.set 的实现，当 target 是数组的时候，也是通过 target.splice(key, 1, val) 来添加的，那么这里的 splice 到底有什么黑魔法，能让添加的对象变成响应式的呢。

其实之前我们也分析过，在通过 observe 方法去观察对象的时候会实例化 Observer，在它的构造函数中是专门对数组做了处理。

vue 中对数组中所有能改变数组自身的方法，如 push、pop 等这些方法进行重写。重写后的方法会先执行它们本身原有的逻辑，并对能增加数组长度的 3 个方法 push、unshift、splice 方法做了判断，获取到插入的值，然后把新添加的值变成一个响应式对象，并且再调用 ob.dep.notify() 手动触发依赖通知，这就很好地解释了之前的示例中调用 vm.items.splice(newLength) 方法可以检测到变化。

**总结**：

- 响应式数据中对于对象新增删除属性以及数组的下标访问修改和添加数据等的变化观测不到
- 通过 Vue.set 以及数组的 API 可以解决这些问题，本质上是它们内部手动去做了依赖更新的派发

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#计算属性-vs-侦听属性)计算属性 VS 侦听属性

**计算属性**

计算属性的初始化是发生在 Vue 实例初始化阶段的 initState 函数中，执行了 if (opts.computed) initComputed(vm, opts.computed)

计算属性本质上就是一个 computed watcher，也了解了它的创建过程和被访问触发 getter 以及依赖更新的过程，其实这是最新的计算属性的实现，之所以这么设计是因为 Vue 想确保不仅仅是计算属性依赖的值发生变化，而是当计算属性最终计算的值发生变化才会触发渲染 watcher 重新渲染，本质上是一种优化。

computed 本质是一个惰性求值的观察者。

computed 内部实现了一个惰性的 watcher,也就是 computed watcher,computed watcher 不会立刻求值,同时持有一个 dep 实例。

其内部通过 this.dirty 属性标记计算属性是否需要重新求值。

当 computed 的依赖状态发生改变时,就会通知这个惰性的 watcher

computed watcher 通过 this.dep.subs.length 判断有没有订阅者

有的话,会重新计算,然后对比新旧值,如果变化了,会重新渲染。 (Vue 想确保不仅仅是计算属性依赖的值发生变化，而是当计算属性最终计算的值发生变化时才会触发渲染 watcher 重新渲染，本质上是一种优化。)

没有的话,仅仅把 this.dirty = true。 (当计算属性依赖于其他数据时，属性并不会立即重新计算，只有之后其他地方需要读取属性的时候，它才会真正计算，即具备 lazy（懒计算）特性。)

**侦听属性**

侦听属性的初始化也是发生在 Vue 的实例初始化阶段的 initState 函数中，在 computed 初始化之后，执行了：

```js
if (opts.watch && opts.watch !== nativeWatch) {
  initWatch(vm, opts.watch)
}
```



watch 的初始化在 data 初始化之后（此时的 data 已经通过 Object.defineProperty 的设置成响应式）

watch 的 key 会在 Watcher 里进行值的读取，也就是立马执行 get 获取 value （从而实现 data 对应的 key 执行 getter 实现对于 watch 的依赖收集），此时如果有 immediate 属性那么立马执行 watch 对应的 回调函数

当 data 对应的 key 发生变化时，触发 user watch 实现 watch 回调函数的执行

**总结**

- 计算属性本质上是 computed watcher
- 而侦听属性本质上是 user watcher，它还支持 deep、sync、immediate 等配置
- 就应用场景而言，计算属性适合用在模板渲染中，某个值是依赖了其它的响应式对象甚至是计算属性计算而来；而侦听属性适用于观测某个值的变化去完成一段复杂的业务逻辑。

同时我们又了解了 watcher 的 4 个 options，通常我们会在创建 user watcher 的时候配置 deep 和 sync，可以根据不同的场景做相应的配置：

- deep watch（深层次监听）
- user watch（用户监听）
- computed watcher（计算属性）
- sync watcher（同步监听）

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#组件更新)组件更新

当数据发生变化的时候，会触发渲染 watcher 的回调函数，进而执行组件的更新过程。

组件的更新还是调用了 vm._update 方法，更新的过程中，会执行 `vm.$el = vm.__patch__(prevVnode, vnode)`，它仍然会调用 patch 函数。

这里执行 patch 的逻辑和首次渲染是不一样的，因为 oldVnode 不为空，并且它和 vnode 都是 VNode 类型，接下来会通过 sameVNode(oldVnode, vnode) 判断它们是否是相同的 VNode 来决定走不同的更新逻辑：

```js
function sameVnode (a, b) {
  return (
    a.key === b.key && (
      (
        a.tag === b.tag &&
        a.isComment === b.isComment &&
        isDef(a.data) === isDef(b.data) &&
        sameInputType(a, b)
      ) || (
        isTrue(a.isAsyncPlaceholder) &&
        a.asyncFactory === b.asyncFactory &&
        isUndef(b.asyncFactory.error)
      )
    )
  )
}
```



sameVnode 的逻辑非常简单，如果两个 vnode 的 key 不相等，则是不同的；否则继续判断对于同步组件，则判断 isComment、data、input 类型等是否相同，对于异步组件，则判断 asyncFactory 是否相同。

所以根据新旧 vnode 是否为 sameVnode，会走到不同的更新逻辑:

**新旧节点不同**

如果新旧 vnode 不同，那么更新的逻辑非常简单，它本质上是要替换已存在的节点，大致分为 3 步:

- 创建新节点
  - 以当前旧节点为参考节点，创建新的节点，并插入到 DOM 中，同过 createElm 函数
- 更新父的占位符节点
  - 到当前 vnode 的父的占位符节点，先执行各个 module 的 destroy 的钩子函数，如果当前占位符是一个可挂载的节点，则执行 module 的 create 钩子函数
- 删除旧节点
  - 把 oldVnode 从当前 DOM 树中删除，如果父节点存在，则执行 removeVnodes 方法

**新旧节点相同**

新旧节点相同，它会调用 patchVNode 方法，patchVnode 的作用就是把新的 vnode patch 到旧的 vnode 上，这里我们只关注关键的核心逻辑，我把它拆成四步骤：

- 执行 prepatch 钩子函数
  - 当更新的 vnode 是一个组件 vnode 的时候，会执行 prepatch 的方法
  - prepatch 方法就是拿到新的 vnode 的组件配置以及组件实例，去执行 updateChildComponent 方法
  - updateChildComponent 的逻辑也非常简单，由于更新了 vnode，那么 vnode 对应的实例 vm 的一系列属性也会发生变化，包括占位符 vm.$vnode 的更新、slot 的更新，listeners 的更新，props 的更新等等
- 执行 update 钩子函数
  - 在执行完新的 vnode 的 prepatch 钩子函数，会执行所有 module 的 update 钩子函数以及用户自定义 update 钩子函数
- 完成 patch 过程
  - 如果 vnode 是个文本节点且新旧文本不相同，则直接替换文本内容。如果不是文本节点，则判断它们的子节点，
- 执行 postpatch 钩子函数
  - 再执行完 patch 过程后，会执行 postpatch 钩子函数，它是组件自定义的钩子函数，有则执行。

**总结**

- 组件更新的过程核心就是新旧 vnode diff，对新旧节点相同以及不同的情况分别做不同的处理。
- 新旧节点不同的更新流程是创建新节点->更新父占位符节点->删除旧节点；
- 新旧节点相同的更新流程是去获取它们的 children，根据不同情况做不同的更新逻辑。
- 最复杂的情况是新旧节点相同且它们都存在子节点，那么会执行 updateChildren 逻辑，这块儿可以借助画图的方式配合理解。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#原理图)原理图

![img](imgs/Vue And React/vue_reactive.c9e2ac37.png)

vue 的响应式原理核心就是去观测数据的变化，当我们这些数据发生变化之后，通知我们对应的观察者来实行相关的逻辑。整个响应式最核心的就是 Dep 的实现，它实际上是连接数据和数据对应观察者的桥梁。

1. 在 vue 的初始化阶段，会对 vue 上定义的数据分别做一些不同的处理
2. 对于 data 和 props 会通过 observe 或 defineReactive 做一系列的操作，把 props 和 data 定义的对象和每一个属性都变成响应式的，同时它们内部会持有一个 Dep 的实例，当我们访问到这些属性的时候，就会触发 Dep 的 depend() 方法来收集依赖。收集的依赖是当前正在进行计算的 watcher (Dep.target)，这些依赖就会做为当前的订阅者，来订阅这些数据的变化，然后当我们去修改这些数据的时候，就会通过调用 Dep.notify() 方法来通知我们这些订阅者去做 updata 的逻辑
3. 对于 computed 属性而言，它实际上内部会创建一个 computed watcher 这样特殊的 watcher 。每个 computed watcher 会持有一个 Dep 实例，当我们去访问 computed 属性的时候，会调用我们 computed watcher 的 evaluate() 计算方法，这个时候就会触发内部 Dep 的 depend 方法去收集依赖，和 data/props 一样会收集当时正在计算的 watcher，把收集到的 watcher 做为 Dep 的 subs（订阅者）收集起来。收集起来的作用就是当我们的计算属性依赖的值发生了变化，就会触发 computed watcher 进行重新计算，重新计算的结果如果变了，就会调用 Dep.notify 通知订阅 computed 变化的这些订阅者来触发它们的更新。
4. 对于 watch 而言，实际上会创建一个 user watcher，user watcher 实际上可以理解为用户自定义的 watcher，它可以观测 data,props 的变化，也可以观测 computed 的变化。当我们的观测的属性发生变化的时候，就会通知 Dep 去遍历我们所有的 user watcher，去调用它们的 updata 方法，当发现新旧值发生变化就会调用 run 方法，来执行用户定义的回调方法。
5. 对于 vue 的渲染和重新渲染就是基于响应式系统的。在 vue 的创建过程中，对于每一个组件而言，都会执行组件的 mount 方法，mount 过程中内部会创建一个唯一的 render watcher， 当执行 render 渲染的时候，也就是去渲染成 vnode 的过程中，会访问 props,data 等定义的数据或 computed 等等。然后 render watcher 就会作为订阅者订阅了那些渲染需要的数据和计算属性的变化。一旦数据发生变化，就会触发 data,props 或 computed 的 notify 方法，然后就会触发 render watcher 的 update 方法，就会执行它们的 run 方法，在执行 run 方法的过程中，实际上最后会调用 updateComponent 方法重新去做一次渲染。

一句话总结:

vue.js 采用数据劫持结合发布-订阅模式,通过 Object.defineproperty 来劫持各个属性的 setter,getter,在数据变动时发布消息给订阅者,触发响应的监听回调

## [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#编译)编译

之前我们分析过模板到真实 DOM 渲染的过程，中间有一个环节是把模板编译成 render 函数，这个过程我们把它称作编译。

虽然我们可以直接为组件编写 render 函数，但是编写 template 模板更加直观，也更符合我们的开发习惯。

Vue.js 提供了 2 个版本，一个是 Runtime + Compiler 的，一个是 Runtime only 的，前者是包含编译代码的，可以把编译过程放在运行时做，后者是不包含编译代码的，需要借助 webpack 的 vue-loader 事先把模板编译成 render函数。

，对编译过程的了解会让我们对 Vue 的指令、内置组件等有更好的理解。不过由于编译的过程是一个相对复杂的过程，我们只要求理解整体的流程、输入和输出即可，对于细节我们不必抠太细。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#编译入口)编译入口

compileToFunctions 方法就是把模板 template 编译生成 render 以及 staticRenderFns。

compileToFunctions 方法接收 3 个参数、编译模板 template，编译配置 options 和 Vue 实例 vm。核心的编译过程就一行代码：

```js
const compiled = compile(template, options)
```



编译的入口我们终于找到了，它主要就是执行了如下几个逻辑：

- 解析模板字符串生成 AST
  - `const ast = parse(template.trim(), options)`
- 优化语法树
  - `optimize(ast, options)`
- 生成代码
  - `const code = generate(ast, options)`

因为 Vue.js 在不同的平台下都会有编译的过程，因此编译过程中的依赖的配置 baseOptions 会有所不同。而编译过程会多次执行，但这同一个平台下每一次的编译过程配置又是相同的，为了不让这些配置在每次编译过程都通过参数传入，Vue.js 利用了函数柯里化的技巧很好的实现了 baseOptions 的参数保留。同样，Vue.js 也是利用函数柯里化技巧把基础的编译过程函数抽出来，通过 createCompilerCreator(baseCompile) 的方式把真正编译的过程和其它逻辑如对编译配置处理、缓存处理等剥离开，这样的设计还是非常巧妙的。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#parse)parse

编译过程首先就是对模板做解析，生成 AST，它是一种抽象语法树，是对源代码的抽象语法结构的树状表现形式。在很多编译技术中，如 babel 编译 ES6 的代码都会先生成 AST。

parse 的目标是把 template 模板字符串转换成 AST 树，它是一种用 JavaScript 对象的形式来描述整个模板。那么整个 parse 的过程是利用正则表达式顺序解析模板，当解析到开始标签、闭合标签、文本的时候都会分别执行对应的回调函数，来达到构造 AST 树的目的。

AST 元素节点总共有 3 种类型，type 为 1 表示是普通元素，为 2 表示是表达式，为 3 表示是纯文本。其实这里我觉得源码写的不够友好，这种是典型的魔术数字，如果转换成用常量表达会更利于源码阅读。

当 AST 树构造完毕，下一步就是 optimize 优化这颗树。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#optimize)optimize

当我们的模板 template 经过 parse 过程后，会输出生成 AST 树，那么接下来我们需要对这颗树做优化，optimize 的逻辑是远简单于 parse 的逻辑，所以理解起来会轻松很多。

为什么要有优化过程，因为我们知道 Vue 是数据驱动，是响应式的，但是我们的模板并不是所有数据都是响应式的，也有很多数据是首次渲染后就永远不会变化的，那么这部分数据生成的 DOM 也不会变化，我们可以在 patch 的过程跳过对他们的比对。

optimize 的过程，就是深度遍历这个 AST 树，去检测它的每一颗子树是不是静态节点，如果是静态节点则它们生成 DOM 永远不需要改变，这对运行时对模板的更新起到极大的优化作用。

我们通过 optimize 我们把整个 AST 树中的每一个 AST 元素节点标记了 static(静态节点) 和 staticRoot(静态根)，它会影响我们接下来执行代码生成的过程。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#codegen)codegen

编译的最后一步就是把优化后的 AST 树转换成可执行的代码，利用 generate 函数生成 render 函数字符串：

```js
const code = generate(ast, options)1
```

## [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#扩展)扩展

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#event)event

- event 在编译阶段生成相关的 data ，对于 DOM 事件在 patch 过程中的创建阶段和更新阶段执行 updateDOMListeners 生成 DOM 事件；对于自定义事件，会在组件初始化阶段通过 initEvents 创建
- 原生 DOM 事件和自定义事件的主要的区别在于添加和删除事件的方式不一样，并且自定义事件的派发是往当前实例上派发，但可以利用在父组件环境定义回调函数来实现父子组件的通讯
- 只有组件节点才可以添加自定义事件，并且添加原生 DOM 事件需要使用 native 修饰符；而普通元素使用 .native 修饰符是没有作用的，也只能添加原生 DOM 事件。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#v-model)v-model

很多同学在理解 Vue 的时候都把 Vue 的数据响应原理理解为双向绑定，但实际上这是不准确的，我们之前提到的数据响应，都是通过数据的改变去驱动 DOM 视图的变化，而双向绑定除了数据驱动 DOM 外， DOM 的变化反过来影响数据，是一个双向关系，在 Vue 中，我们可以通过 v-model 来实现双向绑定。

v-model 是 Vue 双向绑定的真正实现，但本质上就是一种语法糖，但是运行时也做了一些优化。它即可以支持原生表单元素，也可以支持自定义组件。在组件的实现中，我们是可以配置子组件接收的 prop 名称，以及派发的事件名称。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#slot)slot

插槽分为普通插槽和作用域插槽，它们可以解决不同的场景。

普通插槽和作用域插槽的实现，它们有一个很大的差别是数据作用域：

- 普通插槽是在父组件编译和渲染阶段生成 vnodes，所以数据的作用域是父组件实例，子组件渲染的时候直接拿到这些渲染好的 vnodes。
- 而对于作用域插槽，父组件在编译和渲染阶段并不会直接生成 vnodes，而是在父节点 vnode 的 data 中保留一个 scopedSlots 对象，存储着不同名称的插槽以及它们对应的渲染函数，只有在编译和渲染子组件阶段才会执行这个渲染函数生成 vnodes，由于是在子组件环境执行的，所以对应的数据作用域是子组件实例。

简单地说，两种插槽的目的都是让子组件 slot 占位符生成的内容由父组件来决定，但数据的作用域会根据它们 vnodes 渲染时机不同而不同。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#keep-alive)keep-alive

- 组件是一个抽象组件，它的实现通过自定义 render 函数并且利用了插槽
- 会缓存 vnode。缓存渲染的时候不会再执行 created 钩子函数，在 patch 过程也不会执行 mounted 钩子函数，销毁时也不会执行 destroyed 钩子函数，所以不会有一般的组件的生命周期函数
- 的 props 除了 include 和 exclude 还有文档中没有提到的 max，它能控制我们缓存的个数。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#transition)transition

Vue.js 除了实现了强大的数据驱动，组件化的能力，也给我们提供了一整套过渡的解决方案。它内置了

- 条件渲染 (使用 v-if)
- 条件展示 (使用 v-show)
- 动态组件
- 组件根节点

总结起来，Vue 的过渡实现分为以下几个步骤：

1. 自动嗅探目标元素是否应用了 CSS 过渡或动画，如果是，在恰当的时机添加/删除 CSS 类名。
2. 如果过渡组件提供了 JavaScript 钩子函数，这些钩子函数将在恰当的时机被调用。
3. 如果没有找到 JavaScript 钩子并且也没有检测到 CSS 过渡/动画，DOM 操作 (插入/删除) 在下一帧中立即执行。

所以真正执行动画的是我们写的 CSS 或者是 JavaScript 钩子函数，而 Vue 的 `<transition>` 只是帮我们很好地管理了这些 CSS 的添加/删除，以及钩子函数的执行时机。

### [#](https://cchroot.github.io/interview/pages/interview notes/vue源码阅读笔记.html#transition-group)transition-group

只能针对单一元素实现过渡效果。我们做前端开发经常会遇到列表的需求，我们对列表元素进行添加和删除，有时候也希望有过渡效果，Vue.js 提供了 `<transition-group>` 组件，很好地帮助我们实现了列表的过渡效果。

`<transtion-group>` 实现了列表的过渡，以及它会渲染成真实的元素。

当我们去修改列表的数据的时候，如果是添加或者删除数据，则会触发相应元素本身的过渡动画，这点和 `<transition>` 组件实现效果一样，除此之外 `<transtion-group>` 还实现了 move 的过渡效果，让我们的列表过渡动画更加丰富。



# [#](https://cchroot.github.io/interview/pages/interview notes/vueRouter源码阅读笔记.html#vue-router源码阅读笔记)Vue-Router源码阅读笔记

路由的概念相信大部分同学并不陌生，它的作用就是根据不同的路径映射到不同的视图。我们在用 Vue 开发过实际项目的时候都会用到 Vue-Router 这个官方插件来帮我们解决路由的问题。Vue-Router 的能力十分强大，它支持 hash、history、abstract 3 种路由方式，提供了 `<router-link>` 和 `<router-view>` 2 种组件，还提供了简单的路由配置和一系列好用的 API。

### [#](https://cchroot.github.io/interview/pages/interview notes/vueRouter源码阅读笔记.html#路由注册)路由注册

**Vue.use**

Vue 提供了 Vue.use 的全局 API 来注册这些插件

Vue.use 接受一个 plugin 参数，并且维护了一个 _installedPlugins 数组，它存储所有注册过的 plugin；接着又会判断 plugin 有没有定义 install 方法，如果有的话则调用该方法，并且该方法执行的第一个参数是 Vue；最后把 plugin 存储到 installedPlugins 中。

可以看到 Vue 提供的插件注册机制很简单，每个插件都需要实现一个静态的 install 方法，当我们执行 Vue.use 注册插件的时候，就会执行这个 install 方法，并且在这个 install 方法的第一个参数我们可以拿到 Vue 对象，这样的好处就是作为插件的编写方不需要再额外去import Vue 了。

**路由安装**

Vue-Router 的入口文件是 src/index.js，其中定义了 VueRouter 类，也实现了 install 的静态方法：`VueRouter.install = install`，它的定义在 src/install.js 中。

当用户执行 Vue.use(VueRouter) 的时候，实际上就是在执行 install 函数，为了确保 install 逻辑只执行一次，用了 install.installed 变量做已安装的标志位。另外用一个全局的 _Vue 来接收参数 Vue，因为作为 Vue 的插件对 Vue 对象是有依赖的，但又不能去单独去 import Vue，因为那样会增加包体积，所以就通过这种方式拿到 Vue 对象。

**Vue-Router 安装最重要的一步就是利用 Vue.mixin 去把 beforeCreate 和 destroyed 钩子函数注入到每一个组件中。**

```js
export function initMixin (Vue: GlobalAPI) {
  Vue.mixin = function (mixin: Object) {
    this.options = mergeOptions(this.options, mixin)
    return this
  }
}
```



它的实现实际上非常简单，就是把要混入的对象通过 mergeOptions 合并到 Vue 的 options 中，由于每个组件的构造函数都会在 extend 阶段合并 Vue.options 到自身的 options 中，所以也就相当于每个组件都定义了 mixin 定义的选项。

接着给 Vue 原型上定义了 $router 和 $route 2 个属性的 get 方法，这就是为什么我们可以在组件实例上可以访问 `this.$router` 以及 `this.$route`，接着又通过 `Vue.component` 方法定义了全局的 `<router-link>` 和 `<router-view>` 2 个组件，这也是为什么我们在写模板的时候可以使用这两个标签，它们的作用在之后介绍。

最后定义了路由中的钩子函数的合并策略，和普通的钩子函数一样。

**总结**

那么到此为止，我们分析了 Vue-Router 的安装过程，Vue 编写插件的时候通常要提供静态的 install 方法，我们通过 `Vue.use(plugin)` 时候，就是在执行 install 方法。Vue-Router 的 install 方法会给每一个组件注入 beforeCreate 和 destoryed 钩子函数，在 beforeCreate 做一些私有属性定义和路由初始化工作，下一节我们就来分析一下 VueRouter 对象的实现和它的初始化工作。

### [#](https://cchroot.github.io/interview/pages/interview notes/vueRouter源码阅读笔记.html#vuerouter-对象)VueRouter 对象

VueRouter 的实现是一个类，它定义了一些属性和方法，我们先从它的构造函数看，当我们执行 new VueRouter 的时候做了哪些事情。

构造函数定义了一些属性，其中 this.app 表示根 Vue 实例，this.apps 保存持有 `$options.router` 属性的 Vue 实例，this.options 保存传入的路由配置，`this.beforeHooks、 this.resolveHooks、this.afterHooks` 表示一些钩子函数，我们之后会介绍，this.matcher 表示路由匹配器，我们之后会介绍，`this.fallback` 表示在浏览器不支持 `history.pushState` 的情况下，根据传入的 fallback 配置参数，决定是否回退到hash模式，`this.mode` 表示路由创建的模式，`this.history` 表示路由历史的具体的实现实例，它是根据 `this.mode` 的不同实现不同，它有 History 基类，然后不同的 history 实现都是继承 History。

实例化 VueRouter 后会返回它的实例 router，我们在 new Vue 的时候会把 router 作为配置的属性传入，回顾一下上一节我们讲 `beforeCreate` 混入的时候有这么一段代码：

```js
beforeCreate() {
  if (isDef(this.$options.router)) {
    // ...
    this._router = this.$options.router
    this._router.init(this)
    // ...
  }
}  
```



所以组件在执行 beforeCreate 钩子函数的时候，如果传入了 router 实例，都会执行 router.init 方法。init 的逻辑很简单，它传入的参数是 Vue 实例，然后存储到 this.apps 中；只有根 Vue 实例会保存到 this.app 中，并且会拿到当前的 this.history，根据它的不同类型来执行不同逻辑。

**总结**

通过这一节的分析，我们大致对 VueRouter 类有了大致了解，知道了它的一些属性和方法，同时了解到在组件的初始化阶段，执行到 `beforeCreate` 钩子函数的时候会执行 `router.init` 方法，然后又会执行 `history.transitionTo` 方法做路由过渡，进而引出了 matcher 的概念，接下来我们先研究一下 matcher 的相关实现。

### [#](https://cchroot.github.io/interview/pages/interview notes/vueRouter源码阅读笔记.html#matcher)matcher

matcher 是一个对象，它对外暴露了 match 和 addRoutes 方法，通过 createMatcher 方法创建返回。

我们需要了解 Location、Route、RouteRecord 等概念。并通过 matcher 的 match 方法，我们会找到匹配的路径 Route，这个对 Route 的切换，组件的渲染都有非常重要的指导意义。下一节我们会回到 transitionTo 方法，看一看路径的切换都做了哪些事情。

### [#](https://cchroot.github.io/interview/pages/interview notes/vueRouter源码阅读笔记.html#路径切换)路径切换

history.transitionTo 是 Vue-Router 中非常重要的方法，当我们切换路由线路的时候，就会执行到该方法，前一节我们分析了 matcher 的相关实现，知道它是如何找到匹配的新线路，那么匹配到新线路后又做了哪些事情，接下来我们来完整分析一下 transitionTo 的实现。

transitionTo 首先根据目标 location 和当前路径 执行 `this.router.match` 方法去匹配到目标的路径。这里 `this.current` 是 history 维护的当前路径，它的初始值是在 history 的构造函数中初始化的：

```js
this.current = START
```



```js
export const START = createRoute(null, {
  path: '/'
})
```



这样就创建了一个初始的 Route，而 transitionTo 实际上也就是在切换 this.current。

拿到新的路径后，那么接下来就会执行 confirmTransition 方法去做真正的切换，由于这个过程可能有一些异步的操作（如异步组件），所以整个 confirmTransition API 设计成带有成功回调函数和失败回调函数。

通过执行 confirmTransition 方法，拿到 updated、activated、deactivated 3 个 ReouteRecord 数组后，接下来就是路径变换后的一个重要部分，执行一系列的钩子函数。

**导航守卫**

官方的说法叫导航守卫，实际上就是发生在路由路径切换的时候，执行的一系列钩子函数。

我们先从整体上看一下这些钩子函数执行的逻辑，首先构造一个队列 queue，它实际上是一个数组；然后再定义一个迭代器函数 iterator；最后再执行 runQueue 方法来执行这个队列。

按照顺序如下：

1. 在失活的组件里调用离开守卫
2. 调用全局的 beforeEach 守卫
3. 在重用的组件里调用 beforeRouteUpdate 守卫
4. 在激活的路由配置里调用 beforeEnter
5. 解析异步路由组件
6. 在被激活的组件里调用 beforeRouteEnter
7. 调用全局的 beforeResolve 守卫
8. 调用全局的 afterEach 钩子

**url**

当我们点击 `router-link` 的时候，实际上最终会执行 `router.push`，如下：

```js
push (location: RawLocation, onComplete?: Function, onAbort?: Function) {
  this.history.push(location, onComplete, onAbort)
}
```



`this.history.push` 函数，这个函数是子类实现的，不同模式下该函数的实现略有不同。

1. hash模式

即地址栏 URL 中的 # 符号，它的特点在于：hash 虽然出现 URL 中，但不会被包含在 HTTP 请求中，对后端完全没有影响，不需要后台进行配置，因此改变 hash 不会重新加载页面。

1. history模式

利用了 HTML5 History Interface 中新增的 `pushState()` 和 `replaceState()` 方法（需要特定浏览器支持）。history 模式改变了路由地址，因为需要后台配置地址。

**组件**

路由最终的渲染离不开组件，Vue-Router 内置了 `<router-view>` 组件

`<router-view>` 是一个 functional 组件，它的渲染也是依赖 render 函数，那么 `<router-view>` 具体应该渲染什么组件呢，首先获取当前的路径：

```js
const route = parent.$route
```



我们之前分析过，在 src/install.js 中，我们给 Vue 的原型上定义了 $route：

```js
Object.defineProperty(Vue.prototype, '$route', {
  get () { return this._routerRoot._route }
})
```



然后在 VueRouter 的实例执行 router.init 方法的时候，会执行如下逻辑，定义在 src/index.js 中：

```js
history.listen(route => {
  this.apps.forEach((app) => {
    app._route = route
  })
})
```



而 history.listen 方法定义在 src/history/base.js 中：

```js
listen (cb: Function) {
  this.cb = cb
}
```



然后在 updateRoute 的时候执行 this.cb：

```js
updateRoute (route: Route) {
  //. ..
  this.current = route
  this.cb && this.cb(route)
  // ...
}
```



也就是我们执行 transitionTo 方法最后执行 updateRoute 的时候会执行回调，然后会更新 this.apps 保存的组件实例的 _route 值，this.apps 数组保存的实例的特点都是在初始化的时候传入了 router 配置项，一般的场景数组只会保存根 Vue 实例，因为我们是在 new Vue 传入了 router 实例。$route 是定义在 `Vue.prototype` 上。每个组件实例访问 $route 属性，就是访问根实例的 _route，也就是当前的路由线路。

`<router-view>` 是支持嵌套的，回到 render 函数，其中定义了 depth 的概念，它表示 `<router-view>` 嵌套的深度。每个 `<router-view>` 在渲染的时候，执行如下逻辑：

```js
data.routerView = true
// ...
while (parent && parent._routerRoot !== parent) {
  if (parent.$vnode && parent.$vnode.data.routerView) {
    depth++
  }
  if (parent._inactive) {
    inactive = true
  }
  parent = parent.$parent
}

const matched = route.matched[depth]
// ...
const component = cache[name] = matched.components[name]
```



`parent._routerRoot` 表示的是根 Vue 实例，那么这个循环就是从当前的 `<router-view>` 的父节点向上找，一直找到根 Vue 实例，在这个过程，如果碰到了父节点也是 `<router-view>` 的时候，说明 `<router-view>` 有嵌套的情况，depth++。遍历完成后，根据当前线路匹配的路径和 depth 找到对应的 RouteRecord，进而找到该渲染的组件。

那么当我们执行 transitionTo 来更改路由线路后，组件是如何重新渲染的呢？在我们混入的 beforeCreate 钩子函数中有这么一段逻辑：

```js
Vue.mixin({
  beforeCreate () {
    if (isDef(this.$options.router)) {
      Vue.util.defineReactive(this, '_route', this._router.history.current)
    }
    // ...
  }
})
```



由于我们把根 Vue 实例的 _route 属性定义成响应式的，我们在每个 `<router-view>` 执行 render 函数的时候，都会访问 parent.$route，如我们之前分析会访问 `this._routerRoot._route`，触发了它的 getter，相当于 `<router-view>` 对它有依赖，然后再执行完 transitionTo 后，修改 app._route 的时候，又触发了setter，因此会通知 `<router-view>` 的渲染 watcher 更新，重新渲染组件。

**总结**

路径变化是路由中最重要的功能，我们要记住以下内容：路由始终会维护当前的线路，路由切换的时候会把当前线路切换到目标线路，切换过程中会执行一系列的导航守卫钩子函数，会更改 url，同样也会渲染对应的组件，切换完毕后会把目标线路更新替换当前线路，这样就会作为下一次的路径切换的依据。



# [#](https://cchroot.github.io/interview/pages/interview notes/vuex源码阅读笔记.html#vuex源码阅读笔记)Vuex源码阅读笔记

Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的状态 (state)。有些同学可能会问，那我定义一个全局对象，再去上层封装了一些数据存取的接口不也可以么？

Vuex 和单纯的全局对象有以下两点不同：

- Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
- 你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

另外，通过定义和隔离状态管理中的各种概念并强制遵守一定的规则，我们的代码将会变得更结构化且易维护。

### [#](https://cchroot.github.io/interview/pages/interview notes/vuex源码阅读笔记.html#vuex-初始化)Vuex 初始化

当我们在代码中通过 import Vuex from 'vuex' 的时候，实际上引用的是一个对象。

```js
export default {
  Store,
  install,
  version: '__VERSION__',
  mapState,
  mapMutations,
  mapGetters,
  mapActions,
  createNamespacedHelpers
}
```



和 Vue-Router 一样，Vuex 也同样存在一个静态的 install 方法，install 的逻辑很简单，把传入的 _Vue 赋值给 Vue 并执行了 applyMixin(Vue) 方法。

applyMixin 就是这个 export default function，它还兼容了 Vue 1.0 的版本，这里我们只关注 Vue 2.0 以上版本的逻辑，它其实就全局混入了一个 beforeCreate 钩子函数，它的实现非常简单，就是把 options.store 保存在所有组件的 this.$store 中，这个 options.store 就是我们在实例化 Store 对象的实例，稍后我们会介绍，这也是为什么我们在组件中可以通过 this.$store 访问到这个实例。

**Store 实例化**

我们在 import Vuex 之后，会实例化其中的 Store 对象，返回 store 实例并传入 new Vue 的 options 中，也就是我们刚才提到的 options.store 。

我们把 Store 的实例化过程拆成 3 个部分，分别是初始化模块，安装模块和初始化 store._vm 。

**初始化模块**

模块对于 Vuex 的意义：

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象，当应用变得非常复杂时，store 对象就有可能变得相当臃肿。为了解决以上问题，Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter，甚至是嵌套子模块——从上至下进行同样方式的分割。

所以从数据结构上来看，模块的设计就是一个树型结构，store 本身可以理解为一个 root module，它下面的 modules 就是子模块。

对于 root module 的下一层 modules 来说，它们的 parent 就是 root module，那么他们就会被添加的 root module 的 _children 中。每个子模块通过路径找到它的父模块，然后通过父模块的 addChild 方法建立父子关系，递归执行这样的过程，最终就建立一颗完整的模块树。

**安装模块**

初始化模块后，执行安装模块的相关逻辑，它的目标就是对模块中的 state、getters、mutations、actions 做初始化工作，它的入口代码是：

```js
const state = this._modules.root.state
installModule(this, state, [], this._modules.root)
```



installModule 实际上就是完成了模块下的 state、getters、actions、mutations 的初始化工作，并且通过递归遍历的方式，就完成了所有子模块的安装工作。

**初始化 store._vm**

Store 实例化的最后一步，就是执行初始化 store._vm 的逻辑，它的入口代码是：

```js
resetStoreVM(this, state)
```



resetStoreVM 的作用实际上是想建立 getters 和 state 的联系，因为从设计上 getters 的获取就依赖了 state ，并且希望它的依赖能被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。因此这里利用了 Vue 中用 computed 计算属性来实现。

**总结**

我们要把 store 想象成一个数据仓库，为了更方便的管理仓库，我们把一个大的 store 拆成一些 modules，整个 modules 是一个树型结构。

每个 module 又分别定义了 state，getters，mutations、actions，我们也通过递归遍历模块的方式都完成了它们的初始化。

为了 module 具有更高的封装度和复用性，还定义了 namespace 的概念。最后我们还定义了一个内部的 Vue 实例，用来建立 state 到 getters 的联系，并且可以在严格模式下监测 state 的变化是不是来自外部，确保改变 state 的唯一途径就是显式地提交 mutation。

这一节我们已经建立好 store，接下来就是对外提供了一些 API 方便我们对这个 store 做数据存取的操作，下一节我们就来从源码角度来分析 Vuex 提供的一系列 API。

### [#](https://cchroot.github.io/interview/pages/interview notes/vuex源码阅读笔记.html#api)API

Vuex 提供的一些常用 API 包括数据的存取、语法糖、模块的动态更新等。

要理解 Vuex 提供这些 API 都是方便我们在对 store 做各种操作来完成各种能力，尤其是 mapXXX 的设计，让我们在使用 API 的时候更加方便，这也是我们今后在设计一些 JavaScript 库的时候，从 API 设计角度中应该学习的方向。

### [#](https://cchroot.github.io/interview/pages/interview notes/vuex源码阅读笔记.html#插件)插件

Vuex 除了提供的存取能力，还提供了一种插件能力，让我们可以监控 store 的变化过程来做一些事情。

Vuex 的 store 接受 plugins 选项，我们在实例化 Store 的时候可以传入插件，它是一个数组，然后在执行 Store 构造函数的时候，会执行这些插件：

```js
const {
  plugins = [],
  strict = false
} = options
// apply plugins
plugins.forEach(plugin => plugin(this))
```



在我们实际项目中，我们用到的最多的就是 Vuex 内置的 Logger 插件，它能够帮我们追踪 state 变化，然后输出一些格式化日志。当然我们也可以自己去实现 Vuex 的插件，来帮助我们实现一些特定的需求。