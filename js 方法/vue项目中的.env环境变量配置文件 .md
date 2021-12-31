# [vue项目中的.env环境变量配置文件 ](https://www.cnblogs.com/aidixie/p/15294125.html)

## 开始之前，先说下为什么要设置和读取环境变量

简而言之就是，通过环境变量传参，能让我们在不修改任务代码的情况下执行不同的逻辑。比如在开发环境、测试环境、生产环境的api地址、文件地址等不同，通过环境变量的不同设置不同的api地址、文件地址

 

![img](Untitled.assets/1366381-20210916162641359-1272537236.png)

###  关于.env 文件内容：

　　NODE_ENV 代表是环境 有development （开发环境）、production（生产环境）

　　VUE_APP_FLAG 代表为自定义属性，属性名必须以"VUE_APP_"开头，比如VUE_APP_XXX

 1：env.pro 文件 

```
NODE_ENV = 'production'
VUE_APP_FLAG = 'pro'
```

2：env.test 文件

```
NODE_ENV = 'production'
VUE_APP_FLAG = 'test'
```

3、在package.json中设置环境执行命令

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
"scripts": {
    "serve": "vue-cli-service serve",
    "pro": "vue-cli-service build --mode pro",
    "test": "vue-cli-service build --mode test",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint",
  },
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

4：应用config.js

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
let baseUrl = '';
let pdfUrl = '';
let wsUrl = '';
const env = process.env.NODE_ENV;
const flag = process.env.VUE_APP_FLAG;
console.log('process.env', process.env);
console.log('env', env);
console.log('flag', flag);

if (env == 'production') {　　// 打包的生产环境
  if (flag == 'test') {
    // 测试环境
    baseUrl = 'http://192.168.1.129:8081/center/';
    pdfUrl = 'https://file.runshihua.com/files/c'; // 文件地址
    wsUrl = 'ws://119.136.17.18:8081/';
  } else {
    // 生产环境
    baseUrl = 'http://119.136.17.18:8081/center/';
    pdfUrl = 'https://file.runshihua.com/files/c'; // 文件地址
    wsUrl = 'ws://119.136.17.18:8081/';
  }
} else { // 开发环境
  baseUrl = 'http://119.136.17.18:8081/center/'; // 开发
  pdfUrl = 'https://file.runshihua.com/files/c'; // 文件地址
  wsUrl = 'ws://119.136.17.18:8081/';
}
export {
  baseUrl,
  pdfUrl,
  wsUrl
};
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 4：在axios的请求拦截处映入接口api就完成api随环境配置而改变。