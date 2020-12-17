## 安装

```
npx create-react-app my-app
cd my-app
npm start
```

## React中的Static对象

​	static对象允许定义静态方法，这些静方法额可以在组件类上调用
​	首先明确两个定义：
​	静态属性：通过构造函数直接访问到的属性
​	实例属性：通过new出来的实例，访问到的属性。

```
function Person(name, age){
    this.name = name
    this.age = age
}
Person.info = 'aaaa'
const p1 = new Person('王多多', 18)
console.log(p1.info)//undefined
console.log(Person.info)//aaaa
```

由于info属性事直接挂载给了构造函数，所以式静态属性

```
class Animal{
    constructor(name, age){
        this.name = name;
        this.age = age;
    }
    static info = "eee"
}
const a1 = new Animal('大黄','3')
console.log(a1.info);//undefined
console.log(Animal.info)//eee
```

在class内部，通过static修饰的属性，就是静态属性，因此info是静态属性

### react类组件中定义static方法的作用

在java语言中也存在static方法，它属于类本身，跟实例对象没有关系，可以通过类名直接调用react类组件中的static方法也是通过类名调用

为什么使用静态方法： 类内部的方法都会被子类继承，但是使用静态方法定义的不会被子类继承，也不会初始化到实例对象中



React事件处理

React元素的事件处理和DOM元素类似。但是有一点语法上的不同