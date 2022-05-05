# 第一章：什么是JavaScript

​	1995年，JavaScript问世。用途：代替Perl等服务器端语言处理输入验证

## 1.1简短的历史回顾

​	1995 年，网景公司一位名叫 Brendan Eich 的工程师，开始为即将发布的 Netscape Navigator 2 开发一个叫 Mocha（后来改名为 LiveScript）的脚本语言。当时的计划是在客户端和服务器端都使用它，它在服务器端叫 LiveWire。

​	为了赶上发布时间，网景与 Sun 公司结为开发联盟，共同完成 LiveScript 的开发。就在 Netscape Navigator 2 正式发布前，网景把 LiveScript 改名为 JavaScript，以便搭上媒体当时热烈炒作 Java 的顺风车。

​	1996年8月 微软重磅进入web浏览器领域，代表JavaScript作为一门语言迈进了一大步

​	微软的JavaScript实现意味着出现两个版本的JavaScript：Netscape Navigator 中的 JavaScript，以及 IE 中的 JScript。

​	1997年，JavaScript1.1作为提案被提交给欧洲计算机制造商协会(Ecma),花费数月打造出ECMA-262，也就是ECMAScript（发音为" ek-ma-script "）这个新的脚本语言标准

​	1998年，国际标准化组织（ISO）和国际电工委员会（IEC）也将 ECMAScript 采纳为标准（ISO/ IEC-16262）。自此以后，各家浏览器均以 ECMAScript 作为自己 JavaScript 实现的依据，虽然具体实现各有不同

## 1.2  JavaScript 实现

​	虽然JavaScript和ECMAScript基本上是同义词，但JavaScript远远不限于ECMA-262所定义的那样。完整的JavaScript实现包含一下几个部分（见图1-1）：

​	1.核心（ECMAScript）

​	2.文档对象模型（DOM）

​	3.浏览器对象模型（BOM）

![](https://raw.githubusercontent.com/sady-19/MdImage/main/img/202203291501746.png)

### 1.2.1 ECMAScript

​	ECMAScript，即ECMA-262定义的语言，并不局限于web浏览器，这门语言没有输入和输出之类的方法，ECMA-262将这门语言作为一个基准来定义，以便在它之上再构建更稳健的脚本语言。web浏览器只是ECMAScript实现可能存在的一种宿主环境（host environment）宿主环境提供ECMAScript的基准实现和环境自身交互必需的扩展（如：DOM）。其他宿主环境还有服务器端JavaScript平台Node.js和即将被淘汰的Adobe Flash
​	如果不涉及浏览器的话，ECMA-262到底定义了什么？在基本层面，它描述这门语言的如下部分：
​				 语法     类型     语句     关键字     保留字     操作符     全局对象
​	ECMAScript 只是对这个规范描述的所有方面的一门语言的称呼。JavaScript实现了ECMAScript，而Adobe ActionScript同样也实现了ECMAScript

#####  1.ECMAScript 版本

​		ES 2019 （ES10）这次修订增加了 Array.prototype.flat()/flatMap()、String.prototype.trimStart()/trimEnd()、Object.fromEntries()方法，以及 Symbol.prototype.description 属性，明确定义了 Function.prototype.toString()的返回值并固定了 Array.prototype.sort()的顺序。另外，这次修订解决了与 JSON 字符串兼容的问题，并定义了 catch 子句的可选绑定。

​		ES 2018 （ES9）这次修订包括异步迭代、剩余和扩展属性、一组新的正则表达式特性、Promise finally()，以及模板字面量修订。

​		ES 2017 （ES8）这一版主要增加了异步函数（async/await）、SharedArrayBuffer 及 Atomics API，以及Object.values()/Object.entries()/Object. getOwnPropertyDescriptors()和字符串填充方法，另外明确支持对象字面量最后的逗号。

​		ES 2016（ES7，2016年6月）。这次修订只包含少量语法层面的增强，如 Array.prototype.includes 和指数操作符。

​		ECMAScript 2015（ES6，2015年6月）ES6 正式支持了类、模块、迭代器、生成器、箭头、函数、期约、反射、代理和众多新的数据类型。

​		ECMAScript 5.1（2011年6月）
​		ECMAScript 5.0（ES5，2009年12月）
​		ECMAScript 1.0（1997年）

##### 2.ECMAScript符合性是什么意思。

ECMA-262 阐述了什么是 ECMAScript 符合性。要成为 ECMAScript 实现，必须满足下列条件：

​		 支持 ECMA-262 中描述的所有“类型、值、对象、属性、函数，以及程序语法与语义”；
​		 支持 Unicode 字符标准。此外，符合性实现还可以满足下列要求。
​		 增加 ECMA-262 中未提及的“额外的类型、值、对象、属性和函数”。ECMA-262 所说的这些额外内容主要指规范中未给出的新对象或对象的新属性。
​		 支持 ECMA-262 中没有定义的“程序和正则表达式语法”（意思是允许修改和扩展内置的正则表达式特性）。
以上条件为实现开发者基于 ECMAScript 开发语言提供了极大的权限和灵活度，也是其广受欢迎的原因之一。

##### 3.浏览器对ECMAScript的支持

![image-20220329153512524](https://raw.githubusercontent.com/sady-19/MdImage/main/img/202203291535561.png)



### 1.2.2 DOM

​	**文档对象模型**（DOM，Document Object Model）是一个应用编程接口（API），用于在HTML中使用用扩展的XML。DOM将整个页面抽象为一组分层节点。Html或XML页面的每个组成部分都是一种节点，包含不同的数据。

​	**DOM**通过创建表示文档树，让开发者可以随心所欲的控制网页的内容和结构。使用DOM API，可以轻松的删除、添加、替换、修改节点0

##### 1.为什么DOM是必须的

​		如果无法控制网景和微软各行其是，那么web就会发生分裂，导致人们面向浏览器开发网页为了保持web跨平台的本性，万维网（W3C）开始了制定DOM标准的进程

##### 2.DOM级别

​		1998 年 10 月，DOM Level 1 成为 W3C 的推荐标准。这个规范由两个模块组成：DOM Core 和 DOMHTML

​		DOM Level 2 新增了以下模块，以支持新的接口。

​			 DOM 视图：描述追踪文档不同视图（如应用 CSS 样式前后的文档）的接口。
​			 DOM 事件：描述事件及事件处理的接口。
​			 DOM 样式：描述处理元素 CSS 样式的接口。
​			 DOM 遍历和范围：描述遍历和操作 DOM 树的接口

​		DOM Level 3 进一步扩展了 DOM，增加了以统一的方式加载和保存文档的方法（包含在一个叫 DOM 

​	Load and Save 的新模块中），还有验证文档的方法（DOM Validation）。

​		目前，W3C 不再按照 Level 来维护 DOM 了，而是作为 DOM Living Standard 来维护，其快照称为

​	DOM4。DOM4     新增的内容包括替代 Mutation Events 的 Mutation Observers。

##### 3.其他 DOM 

​	除了 DOM Core 和 DOM HTML 接口，有些其他语言也发布了自己的 DOM 标准。下面列出的语言

​	是基于 XML 的，每一种都增加了该语言独有的 DOM 方法和接口：

​		 可伸缩矢量图（SVG，Scalable Vector Graphics） 

​		 数学标记语言（MathML，Mathematical Markup Language） 

​		 同步多媒体集成语言（SMIL，Synchronized Multimedia Integration Language）

​	此外，还有一些语言开发了自己的 DOM 实现，比如 Mozilla 的 XML 用户界面语言（XUL，XML User 

Interface Language）。不过，只有前面列表中的语言是 W3C 推荐标准。

##### 4.Web浏览器对DOM的支持情况

![image-20220329160108504](https://raw.githubusercontent.com/sady-19/MdImage/main/img/202203291601535.png)

### 1.2.3 BOM

​	IE3和Netscape Navigator 3 提供了**浏览器对象模型**（**BOM**） **API**，用于支持访问好操作浏览器的窗口。使用BOM，开发者可以操作浏览器显示页面之外的部分。而BOM 它是位移没有相关标准的JavaScript实现。HTML5的出现改变了这个局面，这个的HTML以正式的形式涵盖了尽可能多的BOM特性。

​	总体；来说，BOM只要针对浏览器窗口和子窗口（frame），不过人们同长会把任何特定于浏览器的扩展都归在BOM的范畴，比如：

- [ ] 弹出新浏览器窗口的能力
- [ ] 移动、缩放和关闭浏览器窗口的能力
- [ ] navigator对象，提供关于浏览器的详尽信息
- [ ] location对象，提供浏览器加载页面的详尽信息
- [ ] screen对象，提供关于用户屏幕分辨率的详尽信息
- [ ] performance对象，提供浏览器内存占用、导航行为和时间统计的详尽信息
- [ ] 对cookie的支持
- [ ] 其他自定义对象，如XMLHttpRequest和IE的ActiveXObject

## 1.3 JavaScript 版本

## 1.4 小结

​	JavaScript 是一门用来与网页交互的脚本语言，包含一下三个组成部分。

 -  [ ] ECMAScript ： 由ECMA-262定义并提供核心功能
 -  [ ] 文档对象模型（DOM）：提供与网页交互的方法和接口
 -  [ ] 浏览器对象模型（BOM) : 提供与浏览器交互的方法和接口

​	JavaScript 的这三个部分得到了五大 Web 浏览器（IE、Firefox、Chrome、Safari 和 Opera）不同程度

的支持。所有浏览器基本上对 ES5（ECMAScript 5）提供了完善的支持，而对 ES6（ECMAScript 6）和

ES7（ECMAScript 7）的支持度也在不断提升。这些浏览器对 DOM 的支持各不相同，但对 Level 3 的支

持日益趋于规范。HTML5 中收录的 BOM 会因浏览器而异，不过开发者仍然可以假定存在很大一部分

公共特性

# 第二章：HTML 中的JavaScript

## 2.1 \<script> 元素

​		将JavaScript插入HTML的主要方法是使用\<script>元素，这个元素是由网景公司创造出来的

  - [ ] async : 可选。表示立即开始下载脚本，但不能阻止其他页面动作. 只对外部脚本文件有效
  - [ ] charset：可选。使用src属性指定的代码字符集
  - [ ] crossorigin： 可选。配置相关请求的CORS（跨资源共享）设置。默认不使用CORS
  - [ ] defer：可选。表示脚本可以延迟到文档完全被解析和显示后再执行。只对外部脚本文件有效
  - [ ] src：可选。表示包含要执行的代码的外部文件。
  - [ ] type：可选。代替language，表示代码块中脚本语言的内容类型（也称MIME类型）。惯例，这个值始终都是’text/javaScript‘。如果这个值是module，则代码会被当成ES6模块，而且只有这个时候代码中才能出现import和export关键字。

​		包含在\<script>内的代码会被从上到下解释。

### 2.1.1 标签位置

​		过去，所有\<script>元素都被放在页面的\<head>标签内，这就意味着必须把所有JavaScript代码都下载、解析和解释完成后，才能开始渲染页面（页面在浏览器解析到\<body>的起始标签时开始渲染）。对于有很多JavaScript的页面，这回导致页面渲染的明显延时，在此期间浏览器创楼完全空白，为了解决这个问题，现代Web应用程序同城讲所有的JavaScript引用放在\<body>元素的页面内容后面。

### 2.1.2 推迟执行脚本

​		**defer**这个属性表示脚本在执行的时候不会改变页面结构（只对外部脚本有效），也就是说，这个脚本会被延迟到整个页面都解析完毕后再运行。相当于立即下载，但延时执行。但推迟的脚本不一定会按顺序执行或者在DOMContentLoaded事件之前执行，因此最好只包含一个这样的脚本

#### 2.1.3 异步执行脚本3

​		**async**与defer类似，都只适用于外部脚本，都是立即下载，不同的是，标记为async的脚本并不保证能按照它们出现的次序执行，给脚本添加async属性的目的是告诉浏览器，不必等脚本下载和执行完后再加载页面，同样也不必等到该异步脚本下载和执行后再加载其他脚本，因此异步脚本不应该在加载期间修改DOM

### 2.1.4 动态加载脚本

​		因为 JavaScript 可使用 DOM API，所以通过向 DOM 中动态添加script 元素即可

```js
let script = document.createElement('script');
script.src = 'giberish.js';
document.head.appendChild(script);
```

 在把HTMLElement 元素添加到DOM 且执行到这段代码之前不会发送请求。默认情况下，这种方式创建的\<script> 是以异步方式加载的，相当于添加了async 不过不是所有浏览器都支持 async 属性，因此，如果要动态脚本的加载行为可明确将其设置为同步加载

```js
let script = document.createrElement('script')
script.sc = 'XXX.js'
script.async = false
document.head.appendChild(script)
```

这种方式获取的资源对浏览器预加载是不可见的。这回严重影响它们在资源获取队列中的优先性。根据应用程序的工作方式以及怎么使用，这种方式会严重影响性能，要让预加载器知道这些请求文件的存在可以在头部显式声明它们:

```js
<link rel='preload' href='XXX.js'>
```

### 2.1.5 XHTML中的变化

​		可扩展超文本标记语言（XHTML，Extensible HyperText Markup Language）在XHTML中使用JavaScript必须指定type属性且值为 text/javascript 。

### 2.1.6 废弃的语法

1995 年 Netscape 2 发布以来，所有浏览器都将 JavaScript 作为默认的编程语言。type 属性使用一个 MIME 类型字符串来标识<script>的内容，但 MIME 类型并没有跨浏览器标准化。即使浏览器默认使用 JavaScript，在某些情况下某个无效或无法识别的 MIME 类型也可能导致浏览器跳过（不执行）相关代码。因此，除非你使用 XHTML 或<script>标签要求或包含非 JavaScript 代码，最佳做法是不指定 type 属性。

​		在最初采用 script 元素时，它标志着开始走向与传统 HTML 解析不同的流程。对这个元素需要应用特殊的解析规则，而这在不支持 JavaScript 的浏览器（特别是 Mosaic）中会导致问题。不支持的浏览器会把<script>元素的内容输出到页面上，从而破坏页面的外观。

​		Netscape 联合 Mosaic 拿出了一个解决方案，对不支持 JavaScript 的浏览器隐藏嵌入的 JavaScript 代码。最终方案是把脚本代码包含在一个 HTML 注释中，像这样：

```js
<script>  <! --
 function sayHi(){ 
	 console.log("Hi!"); 
 } 
//-- ></script>
```

使用这种格式，Mosaic 等浏览器就可以忽略<script>标签中的内容，而支持 JavaScript 的浏览则必须识别这种模式，将其中的内容作为 JavaScript 来解析。

​		虽然这种格式仍然可以被所有浏览器识别和解析，但已经不再必要，而且不应该再使用了。在XHTML 模式下，这种格式也会导致脚本被忽略，因为代码处于有效的 XML 注释当中。

## 2.2 行内代码与外部代码

​		虽然HTML文件中可以直接嵌入JavaScript 代码，但是通常最佳实践是尽可能极爱那个JavaScript 代码放在外部文件中，推荐使用外部文件理由如下。

- 可维护。
- 缓存。浏览器会根据特定的设置缓存所有尾部链接JavaScript 文件，如果两个页面都用到同一个文件，则该文件只需下载一次，意味着页面加载更快。
- 适应未来。

## 2.3 文档模式

​		IE5.5 发明了文档模式，即可以使用doctype切换文档模式，最初文档模式有两种：

- 混杂模式（quirks mode）。让IE想IE5一样（支持一些非标准的特性）
- 标准模式（standards mode）。让IE具有兼容标准的行为。

随着浏览器的普遍实现，又出现了第三种文档模式：

- 准标准模式。（almost standards mode）。这种模式下的浏览器支持很多标准的特性，但是没有标准规定的那么严格

​		混杂模式在所有浏览器中都可以省略文档开头的doctype 声明作为开关。这种约定并不合理，因为混杂模式在不同的浏览器中的差异非常大，不使用黑科技基本上就没有浏览器一致性可言

​		标准模式通过一下几种文档类型声明开启

```html
< !-- HTML 4.01 Strict -- > 
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" 
"http://www.w3.org/TR/html4/strict.dtd"> 
    
< !-- XHTML 1.0 Strict -- > 
<!DOCTYPE html PUBLIC 
"-//W3C//DTD XHTML 1.0 Strict//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"> 
    
< !-- HTML5 -- > 
<!DOCTYPE html>
```

​		准标准模式通过过渡性文档类型（Transitional）和框架集文档类型（Frameset）来触发：

```
< !-- HTML 4.01 Transitional -- > 
<!DOCTYPE HTML PUBLIC 
"-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd"> 

< !-- HTML 4.01 Frameset -- > 
<!DOCTYPE HTML PUBLIC 
"-//W3C//DTD HTML 4.01 Frameset//EN" 
"http://www.w3.org/TR/html4/frameset.dtd"> 

< !-- XHTML 1.0 Transitional -- > 
<!DOCTYPE html PUBLIC 
"-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 

< !-- XHTML 1.0 Frameset -- > 
<!DOCTYPE html PUBLIC 
"-//W3C//DTD XHTML 1.0 Frameset//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd"> 
```

## 2.4 \<noscript> 元素

​		针对早起浏览器不支持 Javascript 的问题，需要一个页面优雅降级的处理方案

## 2.5 小结

​		JavaScript 是通过\<script> 元素插入到HTML页面中的。这个元素可用于把JavaScript 代码嵌入到HTML页面中，跟其他标记混合在一起，也可以用于引入保存在外部文件中的 JavaScript

​		重点总结：

- 要包含外部 JavaScript 文件，必须讲src属性设置为要包含文件的 URL。文件可以跟网页在同一台服务器上，也可以位于完全不同的域。
- 所有\<script> 元素会依照它们在网页中出现的次序被解释。在不使用defer和async属性的情况下，包含在\<script> 元素中的代码必须严格按次序解释。
- 对不推迟执行的脚本，浏览器必须解释完位于\<script> 元素中的代码，然后才能继续渲染页面的剩余部分，为此，通常把\<script> 元素放到页面末尾，介于主内容之后及\</body>标签之前。
- 可以使用defer 属性把脚本推迟到文档渲染完毕之后再执行。推迟的脚本原则上按照它们被列出的次序执行。
- 可以使用 async 属性表示脚本不需要等待其他脚本，同时也不阻塞文档渲染，即异步加载。异步脚本不能保证按照它们在页面中出现的次序执行。
- 通过\<noscript>元素，可以指定浏览器不支持脚本时渲染的内容。如果浏览器支持并启用脚本，则\<noscript> 元素中的任何内容都不会被渲染。



# 第三章：语言基础

## 3.1 语法

​	ECMAScript 的语法很大程度上借鉴了C语言和其他类C语言

### 3.1.1 区分大小写

​		ECMAScript 中一切都区分大小写。

### 3.1.2 标识符

​		所谓标识符，就是变量、函数、属性或参数的名称。标识符可以又一个或多个下列字符组成：

- 第一个字符必须是一个字母、下划线（_）或美元符号（$）
- 剩下的其他字符可以是字母、下划线、美元符号或数字

​		标识符中的字符可以是ASCII（Extended AscII）中的字母，也可以是Unicode的字母字符（但不推荐）

​		按照惯例，ECMAScript标识符使用驼峰大小写形式（非强制），即第一个单词的首字母小写，后面每个单词的首字母大写，如firstSscond myCar

> 关键字、保留字、true、false、和null不能作为标识符

### 3.1.3 注释

​		单行注释：

```js
// 单行注释
```

​		块注释：

```js
/* 这是多行
注释*/
```

### 3.1.4 严格模式

​		ECMAScript 5 增加了严格模式的概念。严格模式是一种不同的JavaScript 解析和执行模型，ECMAScript 3 的一些不规范写法在这种模式下会被处理，对于不安全的活动将抛出错误，要对整个脚本启用严格模式在，在脚本开头加上：	

```js
"use strict"
```

​		这是一个预处理指令。任何支持 JavaScript 引擎看到它都会切换到严格模式

​		也可以单独指定一个函数在严格模式下执行，只要把这个预处理指令放到函数体开头即可：

```js
function doSomething() {
    "use strict"
    // 函数体
}
```

​		所有现代浏览器都支持严格模式

### 3.1.5 语句

​		ECMAScript 中的语句以分号结尾。省略分号意味着由解析器确定语句在哪里结尾，

​		即使语句末尾的分号不是必须的，也应该加上。加分号有助于防止省略造成的问题，比如可以避免输入内容不完整。此外，加分号也便于开发者通过删除空行来压缩代码（如果没哟结尾的分号，只删除空行，则会导致语法错误）。加分号也有助于在某些情况下提升性能，以为解析器会尝试在合适的位置补上分号以纠正语法错误。

​		多条语句可以合并到一个C语言风格的代码块中。代码块由一个左花括号（{）标识开始，一个右花括号（}）标识结束

​		if 之类的控制语句只在执行多条语句时要求必须有代码块。

## 3.3 关键字与保留字

​		ECMA-262 描述了一组保留的关键字，这些关键字有特殊用途，比如表示控制语句的开始和结束，或者执行特定的操作。按照规定，保留的关键字不能用作标识符或属性名。ECMA-262 第 6 版规定的所有关键字如下：

```js
break		do		 	in		 			typeof 
case		else	 	instanceof		 	var 
catch		export  	new					void 
class 		extends		return				while 
const		finally		super				with 
continue	for			switch				yield 
debugger	function	this 
default		if			throw 
delete		import		try
```

​		规范中也描述了一组**未来的保留字**，同样不能作标识符或属性名。

​		以下是 ECMA-262 第 6 版为将来保留的所有词汇。

​		始终保留: 

```js
enum 
```

​		严格模式下保留: 

​			

```js
implements	package		public 
interface	protected	static 
let			private 
```

​		模块代码中保留: 

```js
await 
```

​		这些词汇不能用作标识符，但现在还可以用作对象的属性名。一般来说，最好还是不要使用关键字和保留字作为标识符和属性名，以确保兼容过去和未来的 ECMAScript 版本。

## 3.3 变量

​		ECMAScript 变量是松散类型的，意思是变量可以用于保存任何类型的数据。每个变量只不过是一个用于保存任意值的命名占位符。有 3 个关键字可以声明变量：var、const 和 let。其中，var 在ECMAScript 的所有版本中都可以使用，而 const 和 let 只能在 ECMAScript 6 及更晚的版本中使用。

### 3.3.1 var 关键字

#### 		1. var声明作用域

​		使用var 操作符定义的变量会成为包含它的函数的局部变量。比如，使用var在一个函数内部定义一个变量，就意味着该变量将在函数退出是被销毁：

```js
function test () {
    var message = 'hi'	// 局部变量
}
test();
console.log(messsage) // 出错
```

​		函数内定义变量是省略var 操作符，可以创建一个全局变量

#### 		2. var 声明提升

​		var 声明的变量会自动提升到函数作用域顶部

### 3.3.2 let 声明

​		let 的作用和var差不多，区别：let声明的是块作用域，而var声明的范围是函数作用域。

​		let 不允许同一个块作用域中出现冗余

```js
var name;
var name;

let age;
let age;	// 报错
```

#### 		1. 暂时性死区

​		let 与 var 的另一个重要的区别，就是let 声明的变量不会在作用域中被提升

#### 		2. 全局声明

​		使用let 在全局作用域中声明的变量不会成为window对象的属性（var 声明的变量则会）

​		不过，let 声明仍然是在全局作用域中发生的，相应变量会在页面的生命周期内续存。因此，为了避免SyntaError，必须确保页面不会重复声明同一个变量。

#### 		3. 条件声明

​		因为 let 的作用域是块，所以不可能检查前面是否已经使用 let 声明过同名变量，同时也就不可能在没有声明的情况下声明它

```js
 let name = 'Nicholas'; 
 let age = 36; 
</script> 
<script> 
 // 假设脚本不确定页面中是否已经声明了同名变量
 // 那它可以假设还没有声明过
 if (typeof name === 'undefined') { 
 let name; 
 } 
 // name 被限制在 if {} 块的作用域内
 // 因此这个赋值形同全局赋值
 name = 'Matt'; 
 try { 
 	console.log(age); // 如果 age 没有声明过，则会报错
 } 
 catch(error) { 
 	let age;
 }
     // age 被限制在 catch {}块的作用域内
 // 因此这个赋值形同全局赋值
 age = 26; 
</script> 
为此，对于 let 这个新的 ES6 声明关键字，不能依赖条件声明模式。
```

#### 		4. for循环中的let声明

​		在let 出现之前，for 循环定义的迭代变量会渗透到循环体外部

```js
for (var i = 0; i < 5; ++i) {
   	// 循环逻辑
}
console.log(i)	// 5
```

​				改成使用let之后，这个问题就消失了，应为迭代变量的作用域仅限于for循环块内部

​		在使用var 的时候，最常见的问题就是对借贷变量的奇特声明和修改：

```js
for (var i = 0; i < 5;++i) {
    setTimeout(() => console.log(i), 0)
}
// 输出 5 5 5 5 5
```

​		之所以是这样，是因为在退出循环时，迭代变量保存的是导致循环退出的值： 5.在之后执行超时逻辑时，所有的i 都是同一个变量，

​		而使用let 声明迭代变量是，JavaScript引擎在后台会为每一个迭代循环声明一个新的迭代变量。每个setTimeout 引用的都是不同的变量实例，

​		这种每次迭代声明一个独立变量实例的行为适用于所有风格的for循环，包括 for-in和 for-of 循环

### 3.3.3 const 声明

​		const 的行为与let 基本相同，唯一的重要区别是用它声明变量时必须同时初始化变量，且尝试修改const 声明的变量会导致运行时错误。

​		const 声明的限制只适用于它指向的变量的引用。换句话说，如果const 变量引用的是一个对象，那么修改这对象内部的属性并不违反 const 的限制。

​		JavaScript 引擎会为for 循环中的let 声明分别创建独立的变量实例，虽然 const 变量跟let 变量相似，但不能用const 来声明迭代变量（因为迭代变量会自增），
