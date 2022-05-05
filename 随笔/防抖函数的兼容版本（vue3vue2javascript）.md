// vue 防抖，兼容vue3,vue2和普通js

// delay: 延迟时间（毫秒）

1.封装 debounce.js

```js
export default class Debounce {

　　constructor(delay){
　　this.delay = delay  ? delay : 500;

　　this.timeOut = null;

　　}

　　debounceEnd(){

　　　　return new Promise((resolve,reject)=>{

　　　　　　if(this.timeout){

　　　　　　　　clearTimeout(this.timeout)

　　　　　　}

　　　　　　this.timeout = setTimeout(()=>{

　　　　　　　　resolve('success');

　　　　　　　　},this.delay)

　　　　})

　　}

}
```

注：不需要把调用的函数作为参数传进去，仅仅通过使用promise来让我们的代码同步化，达到防抖的效果。

在vue3的setup中调用。通过new Debounce并传入一个延迟时间。获得一个实例，这个实例返回一个promise的成功回调，

如果当前页面有多个不同的操作需要防抖，可以通过new多个实例来分别处理。

在vue3中引入debounce.js

```js
setup(){

　　const de = new Debounce(500);

　　async function handleInit(){

　　　　await de.debounceEnd()

　　　　//模拟发送请求

　　　　setTimeout(()=>{

　　　　　　.....

　　　　　　},100)

　　　　}

}
```

在vue2中

```
created(){

　　　this.de = new Debounce(500)

},

methods:{

	async handleClick(name){

　　　　　　await this.de.debounceEnd();

　　　　　　//500毫秒之后打印

	}

}
```

在原生js上

```
let div = document.querySelector('#div');

const de = new Debounce(1000);

div.addEventListener('click',async()=>{s

　　await de.debounceEnd();

}）
```

