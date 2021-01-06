# react初识

声明式、组件化

# 安装react-react-app (cra)

全局安装

```sh
$ npm install -g create-react-app
```

如果不想全局安装可以直接使用npx

```sh
$ npx create-react-app you-app 也可以实现相同的效果
```

创建一个项目

```sh
$ create-react-app you-app // 注意命名方式
```

安装过程实际上会安装三个东西

+ react: react  的顶级库
+ react-dom: 因为react有很多运行环境，比如app端的react-native，我们要在web上运行就使用react-dom
+ react-script:包含运行和打包react应用程序的所有脚本及配置

运行项目

```sh
npm start
```

生成项目的目录结构如下

```sh
|-- node_modules                      所有依赖安装的目录
|-- package-lock.json                 锁定安装时的包的版本，保证团队的依赖能保证一致
|-- package.json                      
|-- public                            静态公共目录
|-- src                               开发用的源代码目录
```

常见问题：

- npm安装失败
  - 切换为npm镜像为淘宝镜像
  - 使用yarn，如果本来使用yarn还要失败，还得把yarn的源切换到国内
  - 如果还没有办法解决，请删除node_modules及package-lock.json然后重新执行`npm install命令`
  - 再不能解决就删除node_modules及package-lock.json的同时清除npm缓存`npm cache clean --force`之后再执行`npm install`命令

# 关于React

## React的起源和发展

React 起源于 Facebook 的内部项目，因为该公司对市场上所有 JavaScript MVC 框架，都不满意，就决定自己写一套，用来架设Instagram 的网站。做出来以后，发现这套东西很好用，就在2013年5月开源了。

## React与传统MVC的关系

轻量级的视图层库

React不是一个完整的MVC框架，最多可以认为是MVC中的V（View）,甚至React并不非常认可MVC开发模式；React构建页面UI的库。可以简单地理解为，React将页面分成了各个独立的小块，每个块就是组件，这些组件之间可以组合、嵌套，就成了我们的页面

## React高性能的体现：虚拟DOM

##### React高性能的原理：

在Web开发中我们总需要将变化的数据实时反应到UI上，这时就需要对DOM进行操作。而复杂或频繁的DOM操作通常是性能瓶颈产生的原因（如何进行高性能的复杂DOM操作通常是衡量一个前端开发人员技能的重要指标）。

React为此引入了虚拟DOM（Virtual DOM）的机制：在浏览器端用javascript实现了一套DOM API。基于React进行开发是所有的DOM构造都是通过虚拟DOM进行，每当数据变化时，React都会重新构建整个DOM树，然后React将当前整个DOM树和上次的DOM树进行对比，得到DOM结构的区别，然后仅仅将需要变化的部分进行实际的浏览器DOM更新。而且React等够批量的处理虚拟DOM的刷新，在一个事件循环（Event Loop）内的两次数据变化会被合并，例如你连续的先将节点内容从A-B,B-A，React会认为A变成B，然后又从B变成A  UI不发生任何变化，而如果通过手动控制，这种逻辑通常是极其复杂的。

尽管每一次都需要构造完整的虚拟DOM树，但是因为虚拟DOM是内存数据，性能是极高的，部而对实际DOM进行操作的仅仅是Diff分，因而能达到提高性能的目的。这样，在保证性能的同时，开发者将不再需要关注某个数据的变化如何更新到一个或多个具体的DOM元素，而只需要关心在任意一个数据状态下，整个界面是如何Render的。

##### React Fiber: 

在react 16之后发布的一种react 核心算法，**React Fiber是对核心算法的一次重新实现**(官网说法)。之前用的是diff算法。

在之前React中，更新过程是同步的，这可能会导致性能问题。

当React决定要加载或者更新组件树时，会做很多事，比如调用各个组件的生命周期函数，计算和比对Virtual DOM，最后更新DOM树，这整个过程是同步进行的，也就是说只要一个加载或者更新过程开始，中途不会中断。因为JavaScript单线程的特点，如果组件树很大的时候，每个同步任务耗时太长，就会出现卡顿。

React Fiber的方法其实很简单——分片。把一个耗时长的任务分成很多小片，每一个小片的运行时间很短，虽然总时间依然很长，但是在每个小片执行完之后，都给其他任务一个执行的机会，这样唯一的线程就不会被独占，其他任务依然有运行的机会。



## React的特点和优势

​	1、虚拟DOM

我们以前操作dom的方式是通过document.getElementById()的方式，这样的过程实际上是先去读取html的dom结构，将结构转换成变量，在进行操作

而reactjs定义了一套变量形式的dom模型，一切操作和换算直接在变量中，这样减少了操作真实dom，性能真实相当的高，和主流MVC框架有本质的区别，并不和dom打交道

​	2、组件系统

react最核心的思想是将页面中任何一个区域或者元素都可以看做一个组件component

那什么是组件呢？

组件指的就是同时包含课html、css、js、image元素的聚合体

使用react开发的核心就是将页面拆分成若干个组件，并且react一个组件中同时耦合了css、js、image，这种模式整个颠覆了过去的传统方式

​	3、单项数据流

其实react的核心内容就是数据绑定，所谓数据绑定指的是只要将服务端的数据和前端页面绑定好，开发者只关注实现业务就行了

​	4、JSX 语法

在vue中，我们使用render函数来构建组件的dom结构性能较高，因为省去了查找和编译模板的过程，但是在render中利用createElement创建结构的时候代码可读性较低，较为复杂，此时可以利用jsx语法在render中创建dom，解决这个问题但是前提是需要使用工具来编译jsx