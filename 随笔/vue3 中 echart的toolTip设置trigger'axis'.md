# vue3 中 echart的toolTip设置trigger'axis' 无效问题

项目中使用折线图 x轴坐标数据是请求后渲染 为tooltip的trigger设置为axis 无效问题

解决办法 vue3中 [响应式 API：进阶](https://cn.vuejs.org/api/reactivity-advanced.html)

### shallowRef()

[`ref()`](https://cn.vuejs.org/api/reactivity-core.html#ref) 的浅层作用形式。

- **类型**

  ts

  ```
  function shallowRef<T>(value: T): ShallowRef<T>
  
  interface ShallowRef<T> {
    value: T
  }
  ```

- **详细信息**

  和 `ref()` 不同，浅层 ref 的内部值将会原样存储和暴露，并且不会被深层递归地转为响应式。只有对 `.value` 的访问是响应式的。

  `shallowRef()` 常常用于对大型数据结构的性能优化或是与外部的状态管理系统集成。

- **示例**

  js

  ```
  const state = shallowRef({ count: 1 })
  
  // 不会触发更改
  state.value.count = 2
  
  // 会触发更改
  state.value = { count: 2 }
  ```

代码实现：

```
setup(props) {
	let optionData =  ref(props.option)
		mayEchart = shallowRef()
		echartInit = ():any => {
			nextTick(() => {
				if(!(myEchart.value != null && myEchart.value != "" && myEchart.value != undefined) ) {
					let chartDom = document.getElementById('echartMain')
					myEchart.value = echart.init(chartDom)
				}
				let option = {
					...
					toolTip: {
						trigger: 'axis'
					}
					...
				}
				option && myEchart.value.setOPtion(option)
			})
		}
		watch(option.value, (newVal) => {
			echartInit()
		},{deep:true})
		onMounted(() => {
			echartInit()
		})
		onUnmounted(()=>{
			myEchart.value && myEchart.value.dispose()	// 销毁
		})
}
```

