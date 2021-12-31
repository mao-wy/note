在vue开发中，因为引用的父组件和子组件都使用了window.onresize以至于一个window.onresize失效。
找了下解决方案，可以采用下面的方式写就可以了。

```
window.onresize = () => {this.measure()}
window.addEventListener('resize',() => this.measure1(), false)
window.addEventListener('resize',() => this.measure2(), false)
```

销毁可以采用下面的方式

```
beforeDestroy () {
  window.removeEventListener('resize', this.measure1(), false)
}
beforeDestroy () {
  window.onresize = null
}
```



