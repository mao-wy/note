Vue 2 相关：

#### 请简要描述 Vue 2 的响应式原理是什么？

Vue 2 的响应式原理是通过 Object.defineProperty() 实现的。当一个 Vue 实例被创建时，Vue 会对 data 对象中的每个属性都通过 Object.defineProperty() 进行处理，将它们转换为 getter/setter，并在 getter 中收集依赖，将依赖的 Watcher 对象添加到一个订阅列表中，当数据发生变化时，setter 会触发订阅列表中所有 Watcher 对象的更新方法。这个过程是在 Vue 的内部实现的，开发者只需要关注数据的使用即可。

#### Vue 2 中的 computed 和 watch 的区别是什么？它们的使用场景是什么？

computed 和 watch 都是 Vue 2 中的响应式API。computed 计算属性根据它所依赖的属性的值自动计算出一个新值，并且计算结果会被缓存，只有当依赖的属性发生变化时才会重新计算。这使得计算属性非常适合处理多个相关属性之间的计算，可以减少重复计算的次数，提高性能。watch 监听一个表达式或一个函数，当表达式的值或函数的返回值发生改变时执行相应的回调函数。watch 可以监听任何数据的变化，并且不仅可以监听数据的变化，还可以监听到其他事件的变化，比如路由变化、用户行为等。因此，watch 更适合处理一个数据变化需要引起其他副作用的情况，比如请求数据、操作 DOM 等。

#### Vue 2 中的指令有哪？请列举并简要说明它们的作用。

Vue 2 中的指令包括 v-if、v-show、v-for、v-bind、v-on、v-model、v-text、v-html 等。其中 v-if 和 v-show 都是控制元素的显示和隐藏的指令，不同之处在于 v-if 是根据表达式的值来判断是否渲染元素，如果表达式的值为 false，元素将不会被渲染到 DOM 中，而 v-show 则是根据表达式的值来控制元素的 display 样式，如果表达式的值为 false，元素将隐藏，但是元素的 DOM 元素仍然存在。



请简要描述 Vue 2 中的路由原理是什么？

### Vue 3 相关：

1. 请简要描述 Vue 3 中的响应式原理是什么？它与 Vue 2 响应式的区别是什么？

2. Vue 3 中的 Composition API 是什么？它的使用场景是什么？

3. Vue 3 中的生命周期函数有哪些？它们分别在什么时候被调用？

4. Vue 3 中的指令有哪些？请列举并简要说明它们的作用。

5. Vue 3 中的路由原理是什么？Vue 3 中推荐使用哪个路由库？

   

#### Vue 2 和 Vue 3 有哪些区别？

Vue 3 相对于 Vue 2，有以下几个重大的改进：

- 更小更快：Vue 3 的打包体积更小，运行时的性能也更快。
- Composition API：Vue 3 引入了 Composition API，它可以让开发者更灵活地组织组件代码，将代码按照功能进行组织，而不是按照生命周期函数进行组织。
- Teleport：Vue 3 中新增了 Teleport 组件，可以将子组件渲染到任意 DOM 节点中，而不仅仅是父组件的模板中。
- 其他改进：Vue 3 中还有其他改进，比如增强了 TypeScript 支持、改进了响应式系统等。



#### Vue 2 的双向绑定原理是什么 响应式原理？

Vue 2 的响应式原理是通过 Object.defineProperty() 实现的双向绑定。当一个 Vue 实例被创建时，Vue 会对 data 对象中的每个属性都通过 Object.defineProperty() 进行处理，将它们转换为 getter/setter，并在 getter 中收集依赖，将依赖的 Watcher 对象添加到一个订阅列表中，当数据发生变化时，setter 会触发订阅列表中所有 Watcher 对象的更新方法。这个过程是在 Vue 的内部实现的，开发者只需要关注数据的使用即可。

具体实现时，Vue 2 会在编译模板时，对模板中使用到的数据进行依赖分析，并在运行时创建一个 Watcher 对象来监视这些数据。Watcher 对象会将自身添加到数据对象的订阅列表中，当数据发生变化时，Watcher 对象会触发对应的更新方法，从而更新视图。

在具体实现时，Vue 2 使用了 Object.defineProperty() 方法对数据对象的属性进行劫持，然后在属性的 getter 和 setter 方法中添加依赖收集和派发更新的逻辑，从而实现数据的响应式更新。当数据发生改变时，setter 方法会自动通知依赖于该属性的所有 Watcher 对象进行更新。

总之，Vue 2 的响应式原理是通过 Object.defineProperty() 方法实现的，通过将数据对象转换为响应式对象，并在数据的 getter 和 setter 方法中进行依赖收集和派发更新，实现了双向绑定的功能。

#### Vue 2 中的 computed 和 watch 有什么区别？

Vue 2 中的 computed 和 watch 都可以用来监听数据的变化，但是它们的应用场景有所不同。computed 适用于需要根据数据的变化来动态计算出新值的情况，computed 的值会被缓存，只有当依赖的数据发生改变时，才会重新计算。而 watch 适用于需要在数据发生变化时执行一些异步或开销较大的操作的情况，watch 接收一个回调函数和一个可选的选项对象，可以在回调函数中执行一些操作。

#### Vue 2 中的指令有哪些？

Vue 2 中的指令包括 v-if、v-show、v-for、v-bind、v-on、v-model、v-text、v-html 等。指令是 Vue 中常用的一种特殊属性，它们使用 v- 前缀来标识，并且可以通过指令参数、修饰符等来实现不同的功能。比如 v-if 指令用来根据表达式的值来控制元素的显示或隐藏，v-for 指令用来循环渲染一个数据数组等。

#### Vue 2 中的组件如何传递数据和向父组件发送消息？

Vue 2 中的组件可以使用 props 来接收父组件传递过来的数据，父组件在使用子组件时可以通过 props 来传递数据。子组件中通过 props 属性接收数据，并可以在组件内使用这些数据。子组件可以通过 $emit 方法来向父组件发送消息，$emit 方法接收一个事件名和一个可选的数据参数。父组件可以通过 v-on 指令来监听子组件的事件，v-on 指令需要传递一个事件名和一个回调函数，回调函数可以接收子组件传递过来的数据。

#### Vue 3 中的 Composition API 如何使用？

Vue 3 中的 Composition API 可以通过导入 `vue` 模块中的 `defineComponent` 函数来使用。在组件中使用 Composition API，需要先定义一个 setup 函数，在 setup 函数中可以使用 `reactive`、`computed`、`watch` 等函数来创建响应式数据、计算属性和监听数据变化等。在 setup 函数中还可以使用 `ref` 函数来创建一个包装基本类型值的响应式对象，使用 `toRefs` 函数将一个 reactive 对象的属性转换成响应式的 ref 对象。

#### Vue 3 中的 Teleport 组件有什么作用？

Vue 3 中的 Teleport 组件可以将子组件的内容渲染到任意 DOM 节点中，而不仅仅是父组件的模板中。这个功能在实现模态框、下拉菜单等组件时非常有用，可以让组件在 DOM 结构上更加灵活。

#### Vue 3 中的响应式系统有哪些改进？

Vue 3 中的响应式系统相对于 Vue 2，有以下改进：

- Proxy 代替 Object.defineProperty()：Vue 3 中使用了 Proxy 对象来实现数据劫持，相对于 Object.defineProperty()，Proxy 对象的性能更好，也支持更多的特性。
- 更好的类型推导：Vue 3 的响应式系统对 TypeScript 的支持更加友好，可以更好地推导出数据的类型信息。
- 可以监听 Map 和 Set 对象：Vue 3 的响应式系统可以监听 Map 和 Set 对象的变化。
- 更好的响应式更新：Vue 3 在响应式更新时，会根据数据变化的位置和范围，智能地进行更新，从而提高更新的性能。

### Vue 3 中的异步组件如何实现？

在 Vue 3 中，异步组件可以通过 import() 函数来实现。import() 函数返回一个 Promise 对象，当 Promise 对象 resolve 时，会得到一个组件对象，可以在组件中使用这个对象来渲染组件。在组件定义中，可以使用 `defineAsyncComponent` 函数来定义一个异步组件，该函数接收一个返回 Promise 对象的函数作为参数，该函数会在组件需要渲染时被调用，返回一个 Promise 对象。

#### Vue 3 中的模板和渲染函数如何选择？

在 Vue 3 中，可以选择使用模板语法或者渲染函数来编写组件。使用模板语法可以更容易理解，编写起来也更简单；使用渲染函数则更加灵活，可以进行更精细的控制。在选择模板和渲染函数时，可以根据具体的场景和需求进行选择，通常情况下，简单的组件可以使用模板语法，而复杂的组件可以使用渲染函数。

#### Vue 3 中的 provide 和 inject 如何使用？

Vue 3 中的 provide 和 inject 可以用于父子组件之间的数据传递。在父组件中使用 provide 函数，该函数接收一个对象作为参数，该对象中包含需要传递给子组件的数据。在子组件中使用 inject 函数，该函数接收一个数组或一个对象作为参数，数组或对象中包含需要从父组件中接收的数据。使用 provide 和 inject 可以实现跨层级的数据传递，但是需要注意避免在子组件中修改从父组件中接收到的数据，以避免出现意料之外的行为。

#### Vue 3 中的 Vite 是什么？

Vite 是一种基于 ES Modules 的前端构建工具，它可以在开发环境下实现即时编译和热更新，从而提高开发效率。在 Vue 3 中，可以使用 Vite 作为开发工具，通过 Vite 可以实现更快的启动和热更新速度，同时也可以在生产环境下使用 Rollup 来构建项目，从而减小项目的体积。

#### Vue 3 中的 Tree Shaking 如何实现？

在 Vue 3 中，可以通过使用 ES Modules 的 import 和 export 语法来实现 Tree Shaking。在编写代码时，需要将每个组件作为一个单独的模块来编写，并在组件中使用 ES Modules 的 export 语法将组件导出。在使用组件时，通过 ES Modules 的 import 语法来引入组件，Webpack 等构建工具可以通过静态分析来判断哪些组件是没有被使用的，从而进行 Tree Shaking。

#### Vue 3 中的 Fragments 如何使用？

在 Vue 3 中，可以使用 Fragment 来渲染多个根节点。Fragment 是一个占位符，它不会在 DOM 中生成任何节点，可以用来包裹多个根节点，从而实现多个根节点的渲染。在模板中，可以使用 `<template>` 标签或 `v-fragment` 指令来表示 Fragment。

#### Vue 3 中的 Suspense 组件有什么作用？

在 Vue 3 中，Suspense 组件可以用来实现异步组件的加载和错误处理。在使用异步组件时，可以使用 Suspense 组件来包裹异步组件，在组件加载时显示一个 loading 组件，在组件加载失败时显示一个错误组件。当异步组件加载完成后，可以通过 Suspense 组件的 `default`插槽来渲染异步组件的内容。使用 Suspense 组件可以简化异步组件的处理逻辑，提高开发效率。

#### Vue 3 中的 Composition API 和 Options API 有什么区别？

Vue 3 中的 Composition API 和 Options API 都是用来编写组件的 API，但是它们的思想和使用方式有所不同。Options API 是 Vue 2 中默认的编写组件的方式，它将组件的各个选项分别定义在不同的属性中，从而实现组件的编写。Composition API 则是 Vue 3 中引入的新的 API，它是一种基于函数的 API，可以将相关的代码组织在一起，从而提高代码的可读性和复用性。

#### Vue 3 中的响应式原理是什么？

Vue 3 中的响应式原理是通过 Proxy 对象来实现的。在组件中使用 reactive 函数可以将一个普通的对象转换为响应式对象，当响应式对象中的属性发生变化时，Vue 3 可以自动更新视图。在实现响应式原理时，Vue 3 使用了 Proxy 对象来代理响应式对象的访问，从而实现了对属性的拦截和监听。

#### Vue 3 中的 Teleport 组件有什么作用？

在 Vue 3 中，Teleport 组件可以用来实现将组件的内容渲染到指定的 DOM 节点中，从而实现组件的复用和灵活性。在使用 Teleport 组件时，可以通过 `to` 属性指定需要渲染到的 DOM 节点，从而实现组件内容的位置灵活性。Teleport 组件可以用于实现如模态框、弹出菜单等常见组件的编写。

#### Vue 3 中的 defineComponent 函数有什么作用？

在 Vue 3 中，defineComponent 函数可以用来定义组件。与 Vue 2 中的 Vue.extend 不同，defineComponent 函数是一种基于函数的 API，可以将组件的选项封装在一个函数中，从而提高代码的可读性和复用性。在定义组件时，可以使用 setup 函数来编写组件的逻辑，使用 props 属性来定义组件的属性，从而实现组件的功能。

#### Vue 3 中的动画是如何实现的？

在 Vue 3 中，动画是通过使用 `<transition>` 和 `<transition-group>` 组件来实现的。在使用 `<transition>` 组件时，可以通过 `name` 属性指定动画名称，通过 `enter`、`leave`、`enter-active-class`、`leave-active-class` 等属性来定义动画的具体效果。在使用 `<transition-group>` 组件时，可以通过 `name` 属性指定动画名称，通过 `enter-class`、`leave-class`、`move-class` 等属性来定义动画的具体效果。通过使用 `<transition>` 和 `<transition-group>` 组件，Vue 3 可以方便地实现各种动画效果。

#### Vue 3 中的 Vite 是什么？

Vite 是一个基于原生 ES 模块的前端构建工具，可以用来构建 Vue 3 应用程序。与传统的打包工具不同，Vite 不需要预先将所有模块打包成一个文件，而是根据需要动态地加载模块，从而提高开发效率。在使用 Vite 构建 Vue 3 应用程序时，可以使用最新的 ECMAScript 标准，使用原生的 ES 模块化语法来编写代码，从而实现更加简洁、可读性更好的代码。

#### Vue 3 中的 Composition API 有哪些优点？

Vue 3 中的 Composition API 相比于 Vue 2 中的 Options API，有以下优点：

- 可以将相关的代码组织在一起，从而提高代码的可读性和复用性。
- 可以使用函数来编写组件的逻辑，从而避免了 this 的指向问题。
- 可以将组件的选项封装在一个函数中，从而提高代码的模块化程度。
- 可以通过 ref 和 reactive 函数来实现对数据的响应式处理，从而避免了 Vue 2 中需要使用 Vue.set 和 Vue.delete 来手动更新数据的麻烦。
- 可以通过 provide 和 inject 函数来实现组件之间的数据传递，从而避免了 props 需要一层一层传递的问题。

#### Vue 3 中的 Teleport 和 Portal 有什么区别？

Vue 3 中的 Teleport 组件和 Portal 有相似的功能，都可以实现将组件的内容渲染到指定的 DOM 节点中。但是两者的实现方式有所不同，Teleport 是 Vue 3 中新增的组件，使用起来更加简单，而 Portal 则是基于 Vue 2 中的插槽来实现的。

在具体使用时，Teleport 组件可以通过 `to` 属性指定需要渲染到的 DOM 节点，从而实现组件内容的位置灵活性；而 Portal 则需要使用插槽来渲染目标节点，使用起来相对较为繁琐。

#### Vue 3 中的响应式数据是如何实现的？

Vue 3 中的响应式数据是通过 Proxy 对象来实现的。在组件中使用 reactive 函数可以将一个普通的对象转换为响应式对象，当响应式对象中的属性发生变化时，Vue 3 可以自动更新视图。在实现响应式原理时，Vue 3 使用了 Proxy 对象来代理响应式对象的访问，从而实现了对属性的拦截和监听。

与 Vue 2 中使用的 Object.defineProperty 相比，Vue 3 中的 Proxy 对象有以下优点：

- 可以监听到数组的变化，而 Object.defineProperty 只能监听数组的部分操作。
- Proxy 可以直接代理整个对象，而 Object.defineProperty 只能对对象的属性进行代理。
- Proxy 可以自定义属性的拦截器，比 Object.defineProperty 更加灵活。

1. Vue 3 中的 Suspense 组件是什么？

Vue 3 中的 Suspense 组件是一种用于优化组件异步加载的方式。在 Vue 3 中，当组件的异步加载还未完成时，可以使用 Suspense 组件来展示一个占位符，从而避免页面的空白。当异步加载完成时，Vue 3 可以自动渲染出组件的内容。

使用 Suspense 组件时，可以通过 `fallback` 属性来设置组件还未加载完成时的占位符。在具体使用时，需要将异步加载的组件包裹在 `Suspense` 组件中，并将 `fallback` 属性设置为需要展示的占位符。

#### Vue 3 中的 provide 和 inject 函数是什么？

Vue 3 中的 provide 和 inject 函数是一种用于在组件之间传递数据的方式。在具体使用时，可以在父组件中使用 `provide` 函数提供数据，然后在子组件中使用 `inject` 函数来接收数据。

与 Vue 2 中的 props 不同，provide 和 inject 函数不受层级限制，可以在任何深度的组件之间进行数据传递。同时，由于 provide 和 inject 函数是基于 Vue 3 中的响应式数据实现的，因此数据的变化可以自动更新视图。

#### Vue 3 中的 Fragment 是什么？

Vue 3 中的 Fragment 是一种可以在组件中渲染多个元素而不需要添加额外 DOM 元素的方式。在 Vue 3 中，可以使用 `<template>` 标签来实现 Fragment 的效果，也可以直接使用空标签 `<>` 或者 `</>` 来实现。

使用 Fragment 的好处在于可以减少页面中额外的 DOM 元素，从而提高页面的渲染性能和代码的可读性。此外，Vue 3 中的 Fragment 也可以配合 v-for 指令和条件渲染指令一起使用，实现更加灵活的组件渲染方式。

#### Vue 3 中的 Template Refs 是什么？

Vue 3 中的 Template Refs 是一种用于在组件中引用 DOM 元素的方式。与 Vue 2 中的 `$refs` 不同，Vue 3 中的 Template Refs 可以直接在模板中使用，并且不需要使用 `$nextTick` 来等待组件渲染完成。

在具体使用时，可以使用 `ref` 属性来声明一个 Template Refs，然后在组件中使用 `setup` 函数来访问该 DOM 元素。Vue 3 中的 Template Refs 也可以与 reactive函数和 watch 函数一起使用，实现更加灵活的组件交互效果。

#### Vue 3 中的 Teleport 组件是什么？

Vue 3 中的 Teleport 组件是一种用于将组件渲染到指定位置的方式。在具体使用时，可以使用 Teleport 组件将需要渲染的内容插入到指定的 DOM 元素中，从而实现更加灵活的组件布局。

使用 Teleport 组件时，可以通过 `to` 属性来指定需要渲染的目标 DOM 元素，从而将组件的内容插入到指定位置。在具体使用时，也可以将 Teleport 组件配合 Transition 组件一起使用，实现更加流畅的动画效果。

#### Vue 3 中的 Tree-Shaking 是什么？

Vue 3 中的 Tree-Shaking 是一种用于优化代码的方式。在具体实现时，Vue 3 使用了静态分析的技术，来检测代码中不需要使用的模块和组件，并将其从最终的构建代码中删除，从而减少构建文件的大小。

Vue 3 中的 Tree-Shaking 可以在打包时自动生效，也可以通过手动配置构建工具来进行优化。同时，Vue 3 中的 Tree-Shaking 还可以与 Code Splitting 技术一起使用，实现更加灵活的代码分割和优化效果。





1. 闭包 
   闭包是指有权访问另一个函数作用域中变量的函数。在JavaScript中，函数即是对象，因此可以在一个函数内定义另一个函数。闭包可以用来创建私有变量，以及实现一些高级的编程模式，比如函数式编程和模块化。

   ```js
   function makeCounter() {
     let count = 0;
     function counter() {
       count++;
       console.log(count);
     }
     return counter;
   }
   let counter = makeCounter();
   counter(); // 1
   counter(); // 2
   counter(); // 3
   
   ```

   

2. 原型链 
   JavaScript中的每个对象都有一个指向它原型的链，这个链就是原型链。原型链的作用是实现对象之间的继承。当访问一个对象的属性时，如果对象本身没有这个属性，JavaScript就会沿着原型链向上查找，直到找到这个属性或者到达原型链的顶部（即Object.prototype）。

   原型链是一种用于实现继承的机制，每个对象都有一个指向其原型的链接，如果在对象上访问一个属性或方法，而该对象没有定义该属性或方法，JavaScript 引擎会在原型链上递归查找，直到找到该属性或方法或者原型链的顶端为止。

   可以使用对象的 __proto__ 属性或者 Object.getPrototypeOf() 方法访问对象的原型。

   ```js
   let obj = { foo: 1 };
   let protoObj = { bar: 2 };
   Object.setPrototypeOf(obj, protoObj);
   console.log(obj.bar); // 2
   
   let obj2 = { baz: 3 };
   Object.setPrototypeOf(obj2, obj);
   console.log(obj2.bar); // 2
   
   ```

   

3. 数据类型 
   在JavaScript中，数据类型分为原始类型和对象类型。原始类型包括Undefined、Null、Boolean、Number和String和 symbol（ES6 新增），对象类型包括Object、Array、Function等。其中前 6 种数据类型为基本数据类型，object 为引用数据类型。可以使用 `typeof` 运算符判断变量的数据类型，但需要注意的是，`typeof null` 的返回值是 `'object'`，这是一个历史遗留问题。引用类型有对象、数组、函数等。其中原始类型存储在栈内存中，引用类型存储在堆内存中，变量存储的是对象在堆内存中的地址。

   ```
   let a;
   console.log(typeof a); // 'undefined'
   
   a = null;
   console.log(typeof a); // 'object' (历史遗留问题)
   
   a = true;
   console.log(typeof a); // 'boolean'
   
   a = 123;
   console.log(typeof a); // 'number'
   
   a = 'hello';
   console.log(typeof a); // 'string'
   
   a = Symbol('foo');
   console.log(typeof a); // 'symbol'
   
   a = {};
   console.log(typeof a); // 'object'
   
   ```

   

4. 冒泡排序 冒泡排序是一种简单的排序算法，它重复地遍历要排序的数组，比较相邻的两个元素，如果它们的顺序错误就交换它们，直到数组已经排序完成。

   ```js
   function bubbleSort(arr) {
     let len = arr.length;
     for (let i = 0; i < len - 1; i++) {
       for (let j = 0; j < len - 1 - i; j++) {
         if (arr[j] > arr[j + 1]) {
           [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
         }
       }
     }
     return arr;
   }
   
   // 测试代码
   const arr = [5, 3, 8, 4, 1, 9];
   console.log(bubbleSort(arr)); // [1, 3, 4, 5, 8, 9]
   ```

   

5. 递归算法 递归是指在函数中调用函数本身的一种技巧。递归算法常用于解决一些需要重复执行同一任务的问题，比如树的遍历、阶乘计算等。
   ```js
   function factorial(n) {
     if (n === 0) {
       return 1; // 基本情况：0! = 1
     } else {
       return n * factorial(n - 1); // 递归情况：n! = n * (n - 1)!
     }
   }
   
   // 测试代码
   console.log(factorial(5)); // 120
   // 尾递归
   function factorial(n, result = 1) {
     if (n === 0) {
       return result;
     } else {
       return factorial(n - 1, n * result);
     }
   }
   
   ```

   

6. 数组排序 

   JavaScript中可以使用sort()方法对数组进行排序。sort()方法会按照Unicode编码对数组元素进行排序，如果需要按照其他标准进行排序，可以传入一个比较函数作为参数。

7. 浅拷贝深拷贝 

   浅拷贝是指只复制对象或数组的引用，而不是复制对象或数组本身。深拷贝是指创建一个新的对象或数组，并将原对象或数组的所有属性或元素递归地复制到新对象或数组中。在JavaScript中，可以使用Object.assign()方法或者使用JSON.parse(JSON.stringify())方法进行浅拷贝或深拷贝。

8. this 的指向 在JavaScript中，this关键字指向当前执行代码的上下文对象。如果当前代码是在函数中执行，this就指向该函数所属的对象。如果当前代码是在全局作用域中执行，this就指向全局对象。

9. 继承 JavaScript中的继承可以通过原型链实现。子类通过继承父类的原型对象来继承父类的属性和方法。ES6引入了class语法，可以更方便地实现继承。

10. H5/C3/ES6 H5指HTML5，是HTML的最新版本，提供了更多的新特性和API，比如Canvas、Web Worker等。CSS3是CSS的最新版本，增加了很多新的特性和选择器，比如border-radius、box-shadow等。ES6是JavaScript的最新版本，引入了很多新的语法和特性，比如箭头函数、模板字符串、解构赋值等。

11. 跨域解决 跨域是指浏览器限制了页面从一个源加载另一个源的资源。跨域解决可以通过修改服务器响应头来实现，比如设置Access-Control-Allow-Origin。

12. 性能优化 性能优化是指提高程序运行效率和响应速度的一系列技术。在前端开发中，可以通过减少HTTP请求、使用CDN、压缩文件、使用缓存等方法来优化性能。

13. Less/Sass用法和编译 Less和Sass是两种CSS预处理器，它们提供了更强大的CSS编写方式和一些便利的功能，比如变量、嵌套、函数等。在使用Less或Sass时，需要将它们编译成普通的CSS文件才能在浏览器中使用。

14. 兼容性解决 在开发过程中，需要考虑不同浏览器对于CSS和JavaScript的支持情况。可以使用CSS Reset、Modernizr等工具来解决兼容性问题。

15. rem原理、移动端适配 rem是一种相对于根元素（即html元素）字体大小的单位，可以根据不同设备的屏幕大小动态调整字体大小。在移动端适配中，可以使用rem单位来实现自适应布局，根据屏幕大小动态设置根元素的字体大小，从而实现页面元素的缩放。

16. 常见的状态码 HTTP状态码是指当客户端向服务器发送请求时，服务器返回的HTTP响应中包含的状态码。常见的状态码包括200（请求成功）、404（请求的资源不存在）、500（服务器内部错误）等。

17. git冲突怎么解决 当多个开发者在同一个文件上进行修改并提交时，可能会导致冲突。解决冲突可以使用git的合并功能，通过手动解决冲突并提交修改来解决。

18. 遍历数组/对象、for in遍历对象时会不会访问原型、判断是不是原型的方法 可以使用for循环、forEach、map等方法遍历数组。遍历对象时，可以使用for-in循环来遍历对象的所有属性，包括继承的属性。可以使用hasOwnProperty()方法来判断属性是否是对象本身的属性，而不是继承的属性。

    怎么遍历数组/对象？

    JavaScript 中遍历数组和对象的常用方法有：

    for 循环：可以通过数组的 length 属性或对象的 Object.keys() 方法获取数组或对象的长度或键值，然后循环遍历数组或对象，使用索引或键值获取每个元素或属性值。
    forEach 方法：数组的 forEach() 方法可以遍历数组的每个元素，不需要显式地指定索引，可以使用回调函数处理每个元素。
    for...in 循环：可以遍历对象的每个可枚举属性，包括原型链上的属性，但不推荐用来遍历数组。
    Object.keys 方法：返回一个由对象的所有可枚举属性组成的数组，不包括原型链上的属性。
    Object.values 方法：返回一个由对象的所有可枚举属性值组成的数组，不包括原型链上的属性。
    Object.entries 方法：返回一个由对象的所有可枚举属性键值对组成的二维数组，不包括原型链上的属性。
    下面是遍历数组和对象的示例代码：

    ```js
    // 遍历数组
    let arr = [1, 2, 3];
    for (let i = 0; i < arr.length; i++) {
      console.log(arr[i]);
    }
    
    arr.forEach((item) => {
      console.log(item);
    });
    
    // 遍历对象
    let obj = { name: 'Alice', age: 20 };
    for (let key in obj) {
      console.log(`${key}: ${obj[key]}`);
    }
    
    Object.keys(obj).forEach((key) => {
      console.log(`${key}: ${obj[key]}`);
    });
    
    Object.values(obj).forEach((value) => {
      console.log(value);
    });
    
    Object.entries(obj).forEach(([key, value]) => {
      console.log(`${key}: ${value}`);
    });
    
    ```

    

    **for...in 遍历对象时会不会访问原型？**

    `for...in` 循环会遍历对象的所有可枚举属性，包括原型链上的属性。如果要遍历对象自身的属性，可以使用 `Object.keys()` 或 `Object.getOwnPropertyNames()` 方法获取对象的所有属性名，然后遍历属性名数组。
    下面是示例代码：

    ```js
    let obj = { name: 'Alice', age: 20 };
    Object.prototype.gender = 'female';
    
    // 使用 for...in 遍历对象
    for (let key in obj) {
      console.log(`${key}: ${obj[key]}`);
    }
    // 输出：name: Alice, age: 20, gender: female
    
    // 使用 Object.keys() 获取对象自身的属性名，遍历属性名数组
    Object.keys(obj).forEach((key) => {
      console.log(`${key}: ${obj[key]}`);
    });
    // 输出：name: Alice, age: 20
    
    ```

    **判断是不是原型的方法？**

    可以使用 `Object.getPrototypeOf()` 方法获取对象的原型，然后与期望的原型进行比较，判断对象是否为期望原型的实例。如果期望原型是某个构造函数的原

19. 事件循环机制
     JavaScript是单线程的，但是它支持异步编程。异步编程的实现依靠事件循环机制。当JavaScript执行异步任务时，会将任务放入任务队列中，等待主线程空闲时再执行。事件循环机制就是负责管理任务队列和主线程的调度过程，保证异步任务能够按照正确的顺序执行。
    事件循环是 JavaScript 引擎中的一种机制，它负责协调和管理 JavaScript 运行时的所有任务。JavaScript 是单线程执行的，但可以通过事件循环来模拟并发执行。当 JavaScript 引擎启动时，会创建一个主线程和一个调用栈，调用栈用于记录当前执行的函数，主线程用于执行代码和处理事件。事件循环则用于管理异步任务的执行。

    事件循环由以下几个部分组成：

    - 调用栈：用于记录当前正在执行的函数，遵循后进先出的原则；
    - 任务队列：用于存储异步任务的回调函数，分为宏任务队列和微任务队列；
    - 微任务：在任务队列中优先级较高的任务，例如 Promise、process.nextTick；
    - 宏任务：在任务队列中优先级较低的任务，例如 setTimeout、setInterval、setImmediate、requestAnimationFrame、I/O 操作等；
    - Event Loop：用于监听调用栈和任务队列的变化，当调用栈为空时，从任务队列中取出任务进行执行。

    事件循环的运行过程可以用以下伪代码描述：
    ```js
    while (true) {
      if (call stack is empty) {
        if (microtask queue is not empty) {
          execute all microtasks in the queue
        } else if (macrotask queue is not empty) {
          execute the oldest macrotask in the queue
        } else {
          exit the loop
        }
      }
    }
    
    ```

    

20. 输入URL到页面展示的详细过程 
    输入URL后，浏览器会先将URL解析成协议、主机、端口、路径等信息，然后发起HTTP请求。服务器接收到请求后，会根据路径等信息返回对应的资源文件。浏览器接收到响应后，会进行页面解析、布局、渲染等过程，最终将页面展示在浏览器窗口中。

21. 怎么打断点 在调试JavaScript代码时，可以使用断点来暂停程序的执行，以便查看程序状态和调试代码。在Chrome浏览器中，可以在Sources面板中选择要打断点的代码行，然后点击行号左侧的区域添加断点。

22. HTTP缓存 HTTP缓存是指在浏览器缓存中存储资源文件，以便下次访问时可以直接从缓存中获取，而不需要重新请求服务器。可以通过设置响应头中的Cache-Control和Expires字段来控制缓存的行为。

23. XSS攻击和CSRF攻击的区别 XSS攻击是指攻击者通过在网页中注入恶意脚本，以获取用户信息或者攻击用户设备的一种攻击方式。CSRF攻击是指攻击者通过伪造请求，以冒充合法用户的身份执行一些操作的一种攻击方式。两种攻击方式的本质区别在于，XSS攻击是通过注入脚本实现的，而CSRF攻击是通过伪造请求实现的。

24. for in遍历 for-in循环是用于遍历对象属性的一种循环方式，语法为：for(variable in object) {...}。在循环中，变量variable代表对象的属性名，object代表要遍历的对象。需要注意的是，for-in循环会遍历对象的所有属性，包括继承的属性。可以使用hasOwnProperty()方法来判断属性是否是对象本身的属性，而不是继承的属性。

25. 生命周期 在Vue框架中，生命周期指的是组件在创建、更新、销毁等过程中所经历的各个阶段。在Vue中，组件的生命周期可以分为8个阶段，分别是：

- beforeCreate：`beforeCreate`：在组件实例被创建之初，初始化数据之前被调用，此时组件的`data`、`methods`等属性还未被初始化。

- `created`：在组件实例被创建之后，初始化数据之后被调用，此时组件的`data`、`methods`等属性已经被初始化，但还未挂载到DOM上。

- `beforeMount`：在组件挂载到DOM之前被调用，此时组件的模板已经编译完成，但还未被渲染到页面上。

- `mounted`：在组件挂载到DOM之后被调用，此时组件已经被渲染到页面上，可以对DOM进行操作。

- `beforeUpdate`：在组件更新之前被调用，此时组件的数据已经发生变化，但DOM还未被重新渲染。

- `updated`：在组件更新之后被调用，此时组件的数据已经发生变化，DOM也已经重新渲染完成。

- `beforeDestroy`：在组件销毁之前被调用，此时组件还可以进行操作。

- `destroyed`：在组件销毁之后被调用，此时组件已经被完全销毁，无法再进行操作。

  在组件生命周期的各个阶段中，可以通过相应的钩子函数来实现一些特定的操作，比如在`mounted`钩子函数中进行DOM操作，在`beforeDestroy`钩子函数中清除定时器等等。了解组件的生命周期可以帮助我们更好地管理组件的状态和数据，提高代码的可维护性和可读性。

26. 传值的方式 在Vue框架中，可以通过props和$emit来实现父子组件之间的传值。父组件可以通过props属性将数据传递给子组件，而子组件则可以通过$emit方法向父组件发送事件并传递数据。此外，还可以通过Vuex来实现组件之间的数据共享，或者通过provide和inject属性实现跨层级组件之间的传值。
    在Vue中，我们可以通过多种方式来实现组件之间的数据传递，主要有以下几种方式：

    - `props`：父组件可以通过`props`向子组件传递数据，子组件通过`props`接收数据，可以通过设置`props`的类型、默认值等来对数据进行限制和验证。
    - `emit`：子组件可以通过`emit`触发一个事件，并向父组件传递数据，父组件通过监听这个事件来接收数据。
    - `provide/inject`：父组件可以通过`provide`向子组件注入一个依赖，子组件可以通过`inject`来访问这个依赖，这种方式适合在多层级嵌套的组件之间共享数据。
    - `$parent/$children`：通过访问组件的`$parent`属性和`$children`属性来访问父组件和子组件的实例，从而可以直接访问它们的属性和方法来实现数据的传递和通信。
    - `$refs`：通过`ref`来获取组件的实例，并将其存储在父组件中，从而可以直接访问子组件的属性和方法来实现数据的传递和通信。
    - `vuex`：`vuex`是一个专为Vue.js设计的状态管理库，可以集中管理Vue应用中的所有组件的状态，从而实现组件之间的数据共享和通信。
    - `localStorage/sessionStorage`：可以使用`localStorage`和`sessionStorage`等浏览器提供的API来实现数据的存储和传递，但这种方式只适用于一些简单的应用场景。

    不同的传值方式适用于不同的场景和需求，我们可以根据实际情况选择合适的方式来实现组件之间的数据传递和通信。

27. 计算属性和监听属性的区别 计算属性和监听属性都是Vue中用于处理数据的方式，计算属性和监听属性都可以用于在数据发生变化时执行某些逻辑，但二者的实现方式不同。

    计算属性是基于依赖缓存的。当计算属性依赖的数据发生变化时，计算属性会重新计算，并将计算结果缓存起来。当依赖的数据未发生变化时，计算属性会直接从缓存中获取结果，而不会重新计算。

    监听属性则是在数据变化时执行一些指定的逻辑，无需缓存计算结果。监听属性可以通过watch选项实现，也可以通过$watch方法动态添加监听器。

    计算属性和监听属性都有各自的优缺点，在使用时需要根据实际情况进行选择。如果需要根据已有的数据计算出新的数据，而且这个计算过程比较复杂或者计算量比较大，那么就应该使用计算属性。如果需要监听某个数据的变化，并在数据变化时执行一些操作，那么就应该使用监听属性。

28. Vuex 
    Vuex是一个专为Vue.js应用程序开发的状态管理模式。它集中管理应用程序的状态，并提供了一些工具和规则来保证状态的可维护性。Vuex主要包含了以下几个核心概念：

- state：单一状态树，用于存储应用程序的所有状态。存储应用中的所有状态，是唯一的数据源，所有组件共享这些数据。

- mutation：用于修改状态的函数，只能进行同步操作。用于修改State中的状态，是Vuex中唯一允许修改State的方式，且只能通过提交Mutation来修改State。

- action：用于触发mutation并进行异步操作。用于处理异步操作和复杂的业务逻辑，可以提交Mutation来修改State。

- getter：类似于计算属性，用于从state中派生出一些状态。类似于Vue的计算属性，用于从State中派生出一些状态，以供组件使用。

- Modules：用于将大型的State拆分成小的模块，方便管理和维护。

  通过Vuex，我们可以更方便地管理应用的状态，减少组件之间的耦合度，避免了数据在组件之间的传递和维护上的问题。但是，Vuex的使用也有一些注意事项：

  1. Vuex应该只用于管理应用中的共享状态，不应该用于管理组件的私有状态。
  2. 修改State的唯一方式是提交Mutation，Mutation是同步的操作，如果需要异步操作或复杂的业务逻辑，应该使用Action。
  3. Getter和Action都可以接收State作为参数，并可以根据State计算出新的值或执行某些操作，但是Getter只能读取State，而Action可以修改State。
  4. 使用Vuex时需要注意模块化管理，将大型的State拆分成小的模块，避免出现代码耦合和维护上的问题。
  
  Vuex的主要特点包括：
  
  - 集中式存储：状态集中存储在一个store对象中，方便管理和修改。
  - 明确的状态流：通过mutations和actions明确状态的修改流程。
  - 单向数据流：保证了应用状态的一致性和可维护性。
  - 可以通过插件机制扩展功能。

29. 数据双向绑定原理 Vue框架的数据双向绑定是基于ES5的Object.defineProperty()方法和观察者模式实现的。当Vue实例化时，会将data对象中的每个属性都转换为getter和setter方法。当数据发生变化时，会触发setter方法更新视图。而当视图发生变化时，会通过观察者模式通知相关的数据更新。

    具体而言，Vue框架将模板解析为AST（抽象语法树），并将其转换为渲染函数，然后使用render函数生成虚拟DOM。当数据发生变化时，Vue会重新生成虚拟DOM，并通过比较新旧虚拟DOM来确定需要更新的DOM节点。这样，数据和视图就可以实现双向绑定。
    Vue中的数据双向绑定是通过MVVM模式实现的。MVVM是Model-View-ViewModel的缩写，是一种UI设计模式，用于将UI层和业务逻辑层分离，从而实现UI和数据的解耦。在Vue中，数据双向绑定的原理可以简单地概括为：

    1. Vue通过劫持数据的setter和getter方法来实现数据的监听和响应式更新。
    2. 在模板中使用指令和表达式来绑定数据和视图，当数据发生变化时，Vue会自动更新视图。
    3. 在用户与视图交互时，Vue通过事件绑定和双向绑定指令来将视图中的变化同步到数据中，从而实现数据的双向绑定。

    通过数据双向绑定，我们可以更方便地处理UI和数据之间的关系，减少了编写DOM操作的代码量，提高了代码的可维护性和可读性。

30. #### v-show和v-if的区别 
    
    v-show和v-if都可以用于根据条件控制DOM元素的显示和隐藏，但它们的实现方式不同。
    v-show是通过CSS样式的display属性来控制DOM元素的显示和隐藏。当条件为true时，元素的display属性会被设置为block，否则会被设置为none。这样，元素在DOM中仍然存在，只是不会显示出来。
    v-if则是根据条件动态添加或移除DOM元素。当条件为true时，元素会被添加到DOM中，否则会被移除。这样，元素在DOM中存在与否取决于条件是否满足。
    因此，v-show适合用于频繁切换显示状态的元素，而v-if则适合用于一次性渲染的元素。
31. keep-alive keep-alive是Vue框架中的一个高阶组件，可以缓存组件的状态或避免重新渲染。当组件被包裹在keep-alive标签中时，该组件的状态会被缓存，而不是被销毁。当组件再次被访问时，它会从缓存中获取状态，而不是重新渲染,避免了重复创建和销毁组件的开销。这样，可以提高组件的性能和用户体验。
32. nextTick 
    nextTick是Vue框架中的一个异步方法，用于在DOM更新之后执行一些操作。当数据发生变化时，Vue会异步执行DOM更新操作，而nextTick则可以在DOM更新之后执行一些回调函数。这样，可以保证回调函数在DOM更新完成之后执行，避免一些由于DOM更新导致的问题。例如，在某些情况下，使用nextTick可以确保获取到正确的DOM元素尺寸。
33. 输入URL到页面展示的详细过程 
    在浏览器中输入URL并按下回车键后，浏览器会进行如下操作：

- DNS解析：浏览器会向DNS服务器发送一个DNS解析请求，获取目标服务器的IP地址。
- 建立TCP连接：浏览器会向目标服务器发送一个TCP连接请求，并建立TCP连接。
- 发送HTTP请求：浏览器会向目标服务器发送一个HTTP请求，并等待服务器响应。
- 服务器响应：目标服务器会收到HTTP请求并返回HTTP响应，包括HTTP状态码、响应头和响应体。
- 浏览器解析HTML
- 响应体：浏览器会解析HTML响应体，并将其转换为DOM树。
  - 解析CSS：浏览器会解析CSS样式文件，并将其转换为CSSOM树。
  - 构建渲染树：浏览器会将DOM树和CSSOM树合并，构建渲染树。
  - 布局和绘制：浏览器会根据渲染树进行布局和绘制，生成页面的最终视图。

34. 怎么打断点 在调试JavaScript代码时，打断点是一种非常有用的调试技术。在浏览器中打断点的方法如下：

- 在代码行上单击鼠标左键，将会在该代码行上打一个断点。

- 在开发者工具的Sources面板中，找到要打断点的文件，然后在该文件的代码行上单击鼠标左键。

- 在开发者工具的调试面板中，选择要打断点的代码行，然后单击鼠标左键。

- 在代码中插入一个debugger语句，这样浏览器会自动在该语句上打一个断点。

  打断点后，当代码执行到该断点时，代码会自动暂停，并在开发者工具中显示当前的代码执行状态。此时，可以通过调试面板中的各种调试工具来查看和修改变量值、单步执行代码、查看函数调用堆栈等操作，从而帮助我们排查代码问题。

35. HTTP缓存 HTTP缓存是一种在客户端和服务器之间缓存资源的技术，可以减少网络请求次数，提高页面加载速度和用户体验。HTTP缓存主要分为两种类型：强缓存和协商缓存。

    强缓存指的是通过设置HTTP响应头中的Cache-Control和Expires字段来控制缓存的有效期和缓存机制。当浏览器发起请求时，如果命中了强缓存，则直接从缓存中获取资源，并不会向服务器发送请求。否则，浏览器会向服务器发起请求，并根据响应头中的缓存控制字段来确定是否需要更新缓存。

    协商缓存指的是通过设置HTTP响应头中的Last-Modified和ETag字段来控制缓存的更新机制。当浏览器发起请求时，服务器会根据请求头中的If-Modified-Since和If-None-Match字段来判断资源是否有更新。如果资源没有更新，则服务器会返回304状态码，告诉浏览器直接使用缓存中的资源。否则，服务器会返回200状态码，并返回新的资源内容。

36. XSS攻击和CSRF攻击的区别 XSS攻击和CSRF攻击都是Web应用程序中的常见安全问题

37. XSS攻击指的是利用Web应用程序对用户的输入不加过滤或过滤不严，导致恶意脚本被注入到网页中，从而实现攻击者的恶意目的。XSS攻击分为存储型和反射型两种类型。存储型XSS攻击指的是攻击者将恶意脚本存储到Web应用程序中的数据库中，并在其他用户访问相应页面时注入到页面中。反射型XSS攻击指的是攻击者将恶意脚本作为参数注入到URL中，当用户点击该URL时，恶意脚本会被执行，从而实现攻击者的目的。

    CSRF攻击指的是攻击者利用用户的身份，发送伪造的请求来执行恶意操作。CSRF攻击需要利用到用户的身份验证信息，通常是通过伪造包含恶意操作的URL或表单来实现攻击。当用户点击链接或提交表单时，由于用户已经登录了Web应用程序，攻击者就可以利用用户的身份执行恶意操作。

38. for...in遍历 for...in语句是JavaScript中一种用来遍历对象属性的方法。它的语法如下：

    ```
    cssCopy codefor (variable in object) {
      // code to be executed
    }
    ```

    

    其中，`variable`是一个变量名，用来存储对象属性的名称；`object`是要遍历的对象。在循环中，`variable`会依次取到对象的每一个属性名，然后可以使用`object[variable]`来访问相应的属性值。

    需要注意的是，for...in语句不仅会遍历对象自身的属性，还会遍历对象原型链上的属性。因此，在使用for...in语句遍历对象属性时，通常需要使用`object.hasOwnProperty(variable)`方法来判断属性是否为对象自身的属性，以避免遍历到原型链上的属性。

    另外，for...in语句只适用于遍历对象属性，不适用于遍历数组元素。如果需要遍历数组元素，可以使用for循环或forEach方法来实现。