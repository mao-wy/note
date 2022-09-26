# [详解globalThis](https://blog.fundebug.com/2020/02/17/detail_about_globalthis/)

**摘要：** globalThis 将不同环境下的全局 this 统一



- 原文：[什么是 globalThis，为什么要学会使用它？](https://segmentfault.com/a/1190000021472711)
- 译者：前端小智

**[Fundebug](https://www.fundebug.com/)经授权转载，版权归原作者所有。**

JS 语言越来越多被用于各种环境中。除了最常见的浏览器之外，它还可以在服务器、智能手机甚至机器人硬件上运行。

每个环境都有其自己的对象模型，并提供了不同的语法来访问全局对象。 例如，在 Web 浏览器中，可以通过`window`，`self`或`frames`访问全局对象。 但是，在 **Node.js** 中，这些属性不存在，而必须使用`global`。 在**Web Worker**中，只有`self`可用。

这些不同的全局对象引用方法让 JS 实现跨平台变得非常困难。幸运的是，有一个[提案正在制定中](https://github.com/tc39/proposal-global)，旨在通过引入一个名为`globalThis`的标准属性来解决这个问题，该属性将在所有环境中可用。

在本文中，我们首先研究一下 JS 环境中的全局对象，然后了解`globalThis`如何提供统一的机制来访问它。

## window

`window`属性用于在浏览器环境中引用当前文档的全局对象。在代码的顶层，使用`var`关键字声明的变量将成为`window`的属性，并且可以从代码中的任何位置进行访问:

```
var a = [10, 20];

console.log(window.a); // → [10, 20]
console.log(a === window.a); // → true
```

通常，在使用`window` 的属性时不需要直接引用它，因为引用是隐式的。但是，当存在与全局变量同名的局部变量时，使用`window`是惟一的选择：

```
var a = 10;

(function() {
    var a = 20;
    console.log(a); // → 20
    console.log(window.a); // → 10
})();
```

如上所看到的，`window` 对于引用全局对象非常有用，无论代码在哪个作用域内运行。注意，`window`实际上引用了自身:`window.window`，所以，`window.window === window`。

除了标准的 JS 属性和方法外，`window`对象还包含几个额外的属性和方法，这些属性和方法允许我们控制 web 浏览器窗口和文档本身。

## self

**Web Workers API**没有`Window`对象，因为它没有浏览上下文。相反，它提供`WorkerGlobalScope`接口，其中包含类似`window`携带的数据。

要访问**Web Workers**中的全局对象，我们使用`self`，它是`window`对象的同义词。与`window`类似，`self`是对全局对象的引用：

```
// a web worker
console.log(self); // => DedicatedWorkerGlobalScope {...}

var a = 10;

console.log(self.a); // → 10
console.log(a === self.a); // → true
```

在浏览器环境中，此代码打印的`window`对象而不是`DedicatedWorkerGlobalScope`。 由于`self`的值会根据使用环境的不同而变化，所以有时它比`window`更可取。 `self`在**Web worker**上下文中引用`WorkerGlobalScope.self`时，在浏览器上下文中引用`window.self`。

不要将`self`属性与声明局部变量(用于维护对上下文的引用)的通用 JS 模式相混淆，这一点很重要。例如

```
const obj = {
    myProperty: 10,
    myMethod: function() {
        console.log(this === obj); // => true

        // 将 this 值存储在变量中，以便在嵌套函数中使用
        const self = this;

        const helperFunction = (function() {
            console.log(self === obj); // => true (self 指的是外部的 this 值)
            console.log(this === obj); // => false (this 指的是全局对象。在严格模式下，它的值为undefined)
        })();
    }
};

obj.myMethod();
```

## frames

在浏览器环境中访问全局对象的另一种方法是使用`frames`属性，其工作方式类似于`self`和`window`:

```
// browser environment
console.log(frames); // => Window {...}
```

此只读属性通常用于获取当前窗口的子帧列表。例如，我们可以使用`window.frames[0]`或`frames[0]`来访问第一帧。

## global

在**Node.js**中，可以使用`global`关键字访问全局对象：

```
// node 环境
console.log(global); // => Object [global] {...}
```

`window`、`self`或`frames` 在 Node 环境中不起作用。请记住**Node.js**中的顶级作用域不是全局作用域。在浏览器中，`var abc = 123`将创建一个全局变量。 但是，在 Node.js 中，变量将是模块本身的局部变量。

## this

在浏览器中，可以在程序的顶层使用`this`关键字来引用全局对象：

```
this.foo = 123;
console.log(this.foo === window.foo); // => true
```

在非严格模式下运行的内部函数或箭头函数中的 `this` 也引用了全局对象。 但是，在严格模式下运行的函数不是这种情况，在这种模式下，`this`值为`undefined`：

```
(function() {
    console.log(this); // => Window {...}
})();

(() => {
    console.log(this); // => Window {...}
})();

(function() {
    "use strict";
    console.log(this); // => undefined
})();
```

Node 模块中，`this`在顶层不引用全局对象。相反，它具有与`module.exports`相同的值。在函数(Node 环境)内部，`this`值是根据调用函数的方式确定的。在 JS 模块中，this 在顶层是`undefined`的。

## globalThis 的引入

`globalThis`旨在通过定义一个标准的全局属性来整合日益分散的访问全局对象的方法。该提案目前处于第四阶段，这意味着它已经准备好被纳入**ES2020**标准。所有流行的浏览器，包括 Chrome 71+、Firefox 65+和 Safari 12.1+，都已经支持这项功能。你也可以在**Node.js 12+**中使用它。

```
// 浏览器环境
console.log(globalThis); // => Window {...}

// node.js 环境
console.log(globalThis); // => Object [global] {...}

// web worker 环境
console.log(globalThis); // => DedicatedWorkerGlobalScope {...}
```

通过使用`globalThis`，你的代码将在 window 和非 window 上下文中工作，而无需编写额外的检查或测试。在大多数环境中，`globalThis`直接引用该环境的全局对象。但是，在浏览器中，内部使用代理来考虑`iframe`和跨 window 安全性。 实际上，它并不会改变我们编写代码的方式。

另一方面，如果你确定你的代码将在什么环境中使用，那么可以使用现有的方法来引用环境的全局对象，这样就不必为`globalThis`包含一个`polyfill`了。

## 创建一个 `globalThis` polyfill

在引入`globalThis`之前，跨不同环境访问全局对象的常用方法是使用以下模式

```
function getGlobalObject() {
    return Function("return this")();
}

if (typeof getGlobalObject().Promise.allSettled !== "function") {
    // the Promise.allSettled() method is not available in this environment
}
```

此代码的问题在于，在使用[内容安全策略（CSP）](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)的网站中不能使用`Function`构造函数和`eval`。 由于 CSP，Chrome 的扩展程序系统也不允许此类代码运行。

引用全局对象的另一种模式如下：

```
function getGlobalObject() {
    if (typeof globalThis !== "undefined") {
        return globalThis;
    }
    if (typeof self !== "undefined") {
        return self;
    }
    if (typeof window !== "undefined") {
        return window;
    }
    if (typeof global !== "undefined") {
        return global;
    }
    throw new Error("cannot find the global object");
}

if (typeof getGlobalObject().Promise.allSettled !== "function") {
    // the Promise.allSettled() method is not available in this environment
}
```

此模式通常在网络上使用。 但这也有一些缺陷，使其在某些情况下不可靠。 幸运的是，Chrome 开发者工具（Chrome DevTools）的 Mathias Bynens 提出了一种不受这些缺点影响的创造性模式：

```
(function() {
    if (typeof globalThis === "object") return;
    Object.defineProperty(Object.prototype, "__magic__", {
        get: function() {
            return this;
        },
        configurable: true // This makes it possible to `delete` the getter later.
    });
    __magic__.globalThis = __magic__; // lolwat
    delete Object.prototype.__magic__;
})();

// Your code can use `globalThis` now.
console.log(globalThis);
```

与其他方法相比，这种 polyfill 是更可靠的解决方案，但仍然不够完美。 如 Mathias 所述，修改`Object`，`Object.defineProperty`或`Object.prototype .__ defineGetter__`可能会破坏 polyfill。

## 总结

编写可在多种环境下工作的可移植 JS 代码是很困难的。每个宿主环境都有稍微不同的对象模型。因此，要访问全局对象，需要在不同的 JS 环境中使用不同的语法。

随着`globalThis`属性的引入，访问全局对象将变得更加简单，并且不再需要检测代码运行的环境。

乍一看，`globalThis` 的实现很容易，但是在实践中，却是很复杂的。所有现有的实现方案都是不完美的，如果不小心的话可能会引入 bug。