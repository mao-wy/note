# window.onresize在vue项目中的运用

```
<template>
  <el-container>
    <el-main>
      <div class="content">

        <router-view/>

      </div>
    </el-main>
  </el-container>
</template>
<script>

export default {
  name: "index",
  mounted() {
    let _this = this;
    _this.setPageH()
    window.onresize = () => {
      return (() => {
       _this.setPageH()
      })()
    }
  },
  methods: {
    setPageH() {
      const h = $(window).height()
      $('.el-container').css({'min-height': h})
    }
  }
}
</script>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

监听界面大小发生变化

常在做官网时首页设置某div的高度

在界面变化缩放时重新设置echarts的尺寸

注意：

1.window.onresize事件一般放在created或者mounted生命周期中，这样界面改变是能触发。

2.window.onresize中的this指向的是window，不是指向vue，如果需要调用methods中的函数，需要在window.onresize事件的前面把指向vue的this赋值给其他字符，比如"_this"。

3.window.onresize是全局事件，在其他页面改变界面时也会执行，若仅仅是该页面需要，那么需要在页面销毁时清除事件。

```
//注销window.onresize事件
destroyed(){
  window.onresize = null;
}
```

 