## BFC

```txt
BFC(Block formatting context)直译为“块级格式化上下文”。它是一个独立的渲染区域，只有Block-level box（块）参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。
```



## BFC的布局规则

```txt
一、内部的Box会在垂直方向，一个接一个地放置。
二、Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠（按照最大margin值设置）
三、每个元素的margin box的左边， 与包含块border box的左边相接触
四、BFC的区域不会与float box重叠。
五、BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。
六、计算BFC的高度时，浮动元素也参与计算
```



## 哪些元素或属性能触发BFC

```
根元素(html)
float属性不为none
position为absolute或fixed
display为inline-block, table-cell, table-caption, flex, inline-flex
overflow不为visible
```



## BFC的应用

```txt
1、自适应两栏布局
2、清除内部浮动
3、防止margin上下重叠
```

