```
1.macy.js ======== 网页流布局JS插件，仅4KB大小
```

可以自定义间距、列数，还有个特色就是可以定义不同屏幕分辨率，不同列数，这个应用在响应式[网页设计](https://so.csdn.net/so/search?q=网页设计&spm=1001.2101.3001.7020)是非常方便的。

```
2.Driver.js ======== 页面分步引导组件
```

Driver.js是一个轻量级（约4kb gzip），并且无任何依赖，但功能强大的JavaScript引擎。Driver.js可以提高用户对页面的关注度，，也可以创建强大的分部引导介绍功能，号召性用语组件，焦点移位器等。更重要的是Driver.js支持所有的主流浏览器。系统引导页使用。

```
3.Tippy.js ======== 鼠标悬停提示信息
```

提供多种动画效果和主题效果，并允许用户自定义tooltip主题和使用html代码作为tooltip的模板。

```
4.tesseract.js ======== 面向纯JavaScript的OCR识别引擎
```

Tesseract.js是流行的面向纯Javascript的OCR引擎的。该库支持100多种语言（中文支持），自动文本方向和脚本检测，用于读取段落，单词和字符边界框的简单界面。Tesseract.js可以在浏览器和具有NodeJS服务器上运行。识别文字（需要下载文字库）

```
5.wow.js ======== 纯 JS动画库
```

依赖 animate.css。

```
6.Typed.js ======== 打字效果
```

 

```
7.OWL Carousel.js ======== 走马灯插件
```

 

```
8.Pushbar.js ======== 滑动侧边栏效果
```

 

```
9.jSignatrue.js ======== 手写签名
```

 

```
10.wavesurfer.js ======== 语音文件转曲线图
```

高手区别于普通人的重要一点是，他们善于利用工具，把更多的时间留给了规划和思考。写代码也是同样的道理，工具用好了，你就有更多的时间来规划[架构](https://so.csdn.net/so/search?q=架构&spm=1001.2101.3001.7020)和攻克难点。今天就给大家分享一下当前最流行的 js 工具库，如果觉得有用，就把大拇指点亮一下吧！

**Day.js**

一个极简的处理时间和日期的 JavaScript 库，和 Moment.js 的 [API](https://so.csdn.net/so/search?q=API&spm=1001.2101.3001.7020) 设计保持一样, 但体积仅有2KB。

```coffeescript
npm install dayjs
```

基本用法

```javascript
import dayjs from 'dayjs'
dayjs().format('YYYY-MM-DD HH:mm') // => 2022-01-03 15:06
dayjs('2022-1-3 15:06').toDate() // => Mon Jan 03 2022 15:06:00 GMT+0800 (中国标准时间)
```

**qs**

一个轻量的 url 参数转换的 JavaScript 库

```coffeescript
npm install qs
```

基本用法

```javascript
import s from 'qs'
qs.parse('user=tom&age=22') // => { user: "tom", age: "22" }
qs.stringify({ user: "tom", age: "22" }) // => user=tom&age=22
```

**js-cookie**

一个简单的、轻量的处理 cookies 的 js API

```coffeescript
npm install js-cookie
```

基本用法

```csharp
import Cookies from 'js-cookie'
Cookies.set('name', 'value', { expires: 7 }) // 有效期7天
Cookies.get('name') // => 'value'
```

**flv.js**

bilibili 开源的 html5 flash 视频播放器，使浏览器在不借助 flash 插件的情况下可以播放 flv，目前主流的直播、点播解决方案。

```coffeescript
npm install flv.js
```

基本用法

```javascript
<video autoplay controls width="100%" height="500" id="myVideo"></video>
import flvjs from 'flv.js'
// 页面渲染完成后执行
if (flvjs.isSupported()) {
  var myVideo = document.getElementById('myVideo')
  var flvPlayer = flvjs.createPlayer({
    type: 'flv',
    url: 'http://localhost:8080/test.flv' // 视频 url 地址
  })
  flvPlayer.attachMediaElement(myVideo)
  flvPlayer.load()
  flvPlayer.play()
}
```

**vConsole**

一个轻量、可拓展、针对手机网页的前端开发者调试面板。如果你还苦于在手机上如何调试代码，用它就对了。

```coffeescript
npm install vconsole
```

基本用法

```javascript
import VConsole from 'vconsole'
const vConsole = new VConsole()
console.log('Hello world')
```

最近发现很多小伙只收藏，不点赞，这可不是一个好习惯哦。拒绝白嫖，从你我做起！跟我一起动起来，先点赞！再收藏！

**Animate.css**

一个跨浏览器的 css3 动画库，内置了很多典型的 css3 动画，兼容性好，使用方便。

```coffeescript
npm install animate.css
```

基本用法

```javascript
<h1 class="animate__animated animate__bounce">An animated element</h1>
import 'animate.css'
```

**animejs**

一款功能强大的 Javascript 动画库。可以与CSS3属性、SVG、DOM元素、JS对象一起工作，制作出各种高性能、平滑过渡的动画效果。

```coffeescript
npm install animejs
```

基本用法

```javascript
<div class="ball" style="width: 50px; height: 50px; background: blue"></div>
import anime from 'animejs/lib/anime.es.js'
// 页面渲染完成之后执行
anime({
  targets: '.ball',
  translateX: 250,
  rotate: '1turn',
  backgroundColor: '#F00',
  duration: 800
})
```

**lodash.js**

一个一致性、模块化、高性能的 JavaScript 实用工具库

```coffeescript
npm install lodash
```

基本用法

```javascript
import _ from 'lodash'
_.max([4, 2, 8, 6]) // 返回数组中的最大值 => 8
_.intersection([1, 2, 3], [2, 3, 4]) // 返回多个数组的交集 => [2, 3]
```

**mescroll.js**

一款精致的、在H5端运行的下拉刷新和上拉加载插件，主要用于列表分页、刷新等场景。

```coffeescript
npm install mescroll.js
```

基本用法（vue组件）

```xml
<template>
  <div>
    <mescroll-vue
      ref="mescroll"
      :down="mescrollDown"
      :up="mescrollUp"
      @init="mescrollInit"
    >
      <!--内容...-->
    </mescroll-vue>
  </div>
</template>
<script>
import MescrollVue from 'mescroll.js/mescroll.vue'
export default {
  components: {
    MescrollVue
  },
  data() {
    return {
      mescroll: null, // mescroll实例对象
      mescrollDown: {}, //下拉刷新的配置
      mescrollUp: {
        // 上拉加载的配置
        callback: this.upCallback
      },
      dataList: [] // 列表数据
    }
  },
  methods: {
    // 初始化的回调,可获取到mescroll对象
    mescrollInit(mescroll) {
      this.mescroll = mescroll
    },
    // 上拉回调 page = {num:1, size:10}; num:当前页 ,默认从1开始; size:每页数据条数,默认10
    upCallback(page, mescroll) {
      // 发送请求
      axios
        .get('xxxxxx', {
          params: {
            num: page.num, // 当前页码
            size: page.size // 每页长度
          }
        })
        .then(response => {
          // 请求的列表数据
          let arr = response.data
          // 如果是第一页需手动置空列表
          if (page.num === 1) this.dataList = []
          // 把请求到的数据添加到列表
          this.dataList = this.dataList.concat(arr)
          // 数据渲染成功后,隐藏下拉刷新的状态
          this.$nextTick(() => {
            mescroll.endSuccess(arr.length)
          })
        })
        .catch(e => {
          // 请求失败的回调,隐藏下拉刷新和上拉加载的状态;
          mescroll.endErr()
        })
    }
  }
}
</script>
<style scoped>
.mescroll {
  position: fixed;
  top: 44px;
  bottom: 0;
  height: auto;
}
</style>
```

**Chart.js**

一套基于 HTML5 的简单、干净并且有吸引力的 JavaScript 图表库

```coffeescript
npm install chart.js
```

基本用法

```javascript
<canvas id="myChart" width="400" height="400"></canvas>
import Chart from 'chart.js/auto'
// 页面渲染完成后执行
const ctx = document.getElementById('myChart')
const myChart = new Chart(ctx, {
  type: 'bar',
  data: {
    labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
    datasets: [
      {
        label: '# of Votes',
        data: [12, 19, 3, 5, 2, 3],
        backgroundColor: [
          'rgba(255, 99, 132, 0.2)',
          'rgba(54, 162, 235, 0.2)',
          'rgba(255, 206, 86, 0.2)',
          'rgba(75, 192, 192, 0.2)',
          'rgba(153, 102, 255, 0.2)',
          'rgba(255, 159, 64, 0.2)'
        ],
        borderColor: [
          'rgba(255, 99, 132, 1)',
          'rgba(54, 162, 235, 1)',
          'rgba(255, 206, 86, 1)',
          'rgba(75, 192, 192, 1)',
          'rgba(153, 102, 255, 1)',
          'rgba(255, 159, 64, 1)'
        ],
        borderWidth: 1
      }
    ]
  },
  options: {
    scales: {
      y: {
        beginAtZero: true
      }
    }
  }
})
```

## Three.js

**github:** https://github.com/mrdoob/thr...

**文档：** https://threejs.org/

**Three.js**是出色的JS 3D库，它使用 WebGL 作为主要渲染器，但也支持其他渲染器，例如Canvas 2D，SVG和css3D。 它在GitHub上有58,000个Star，我们可以用它创建非常酷的东西。

## Hammer.js

**github:** https://github.com/hammerjs/h...

**文档：** http://hammerjs.github.io/

Hammer.js是一个 JS 库，具有20,900个GitHub Stars，可为Web应用程序带来多点触摸手势。 它很小，没有任何依赖性，并且可以识别由触摸，鼠标或指针事件产生的手势。 默认情况下，它会添加用于点击，双击，滑动，按下等的识别器，但是您可以定义自己的此类识别器集。



## Leaflet

**github:** https://github.com/Leaflet/Le...

**文档：** https://leafletjs.com/

在创建移动友好的交互式地图时，Leaflet 是一个很棒的 JS 库。它是开源的，在GitHub上有26700个星星，非常轻量，并且拥有大多数开发人员需要的所有特性。

## Ramda

**github:**https://github.com/ramda/ramda

**文档：**https://ramdajs.com/docs/

Ramda 是一个用于函数式编程的很酷的 JS 库，目前在GitHub上有18000个星星。JS 的一个优点是开发人员可以选择函数式编程还是面向对象编程。这两种方法各有利弊，但是如果你喜欢函数式编程，那么一定要看看Ramda。

主要功能是：

- 不变性和无副作用的函数
- 几乎所有的函数都是自动柯里化的
- 参数设置为Ramda函数，便于进行柯里化