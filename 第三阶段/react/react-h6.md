安装cra （create-react-app）

```js
  npm i create-react-app -g

  // 启动项目
  create-react-app you-project  // 注意不要使用驼峰
```
生成入口函数是 src/index.js

## 启动项目安装的三个包
+ react
  核心语法包 （解析 jsx 以及react核心语法生成虚拟dom组件语法） jsx 就是 xml in js（在js中写组件 包含html标签）
+ react-dom 将react生成dom树，挂载到index.html上
+ react-script  webpack的配置文件 （看不见 隐藏的）

## jsx语法
  是指可以在js中写 xml （组件、自定义标签）， react库会自动编译成虚拟dom对象 

  注意 注意：
    jsx中需要谢js只需要一个大括号  jsx中的{ } 不管写什么语法都需要有返回值  一般可以写表达式  相要写复杂的繁殖结构（if else if） 需要写在函数内部  且函数不管走哪个分支，都要有返回值

## 组件
+ 函数式组件
```js
  const App = (props) => { // 命名首字母要大写
    return (
      <div>
        { props.title }
      </div>
    )
  }

  // 使用时
  reactDOM.render(
    <App title+"值"/>,
    document.querySelector("#root")
  )
  # 注意：
    不管是什么类型的组件 内容必须有一个根标签 组件名首字母必须大写
```