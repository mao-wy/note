```
const scrollBehavior = function (to, from, savedPosition) {
  if (savedPosition) {
    // savedPosition is only available for popstate navigations.
    // savedPosition只对popstate导航可用。 
    return savedPosition
  } else {
    const position = {}

    // scroll to anchor by returning the selector
	//     通过返回选择器滚动到锚点
    if (to.hash) {
      position.selector = to.hash

      // specify offset of the element
      // 指定元素的偏移量
      if (to.hash === '#anchor2') {
        position.offset = { y: 100 }
      }

      // bypass #1number check
      // 绕过 #1number 检查
      if (/^#\d/.test(to.hash) || document.querySelector(to.hash)) {
        return position
      }

      // if the returned position is falsy or an empty object,
      // 如果返回的位置是假的或空对象， 
      // will retain current scroll position.
      // 将保留当前滚动位置。
      return false
    }
    
    
	// 这个用不到
    return new Promise(resolve => {
      // check if any matched route config has meta that requires scrolling to top
      // 检查是否有任何匹配的路由配置需要滚动到顶部 
      if (to.matched.some(m => m.meta.scrollToTop)) {
        // coords will be used if no selector is provided,
        // 如果没有提供选择器，将使用坐标， 
        // or if the selector didn't match any element.
        // 或者选择器不匹配任何元素。
        position.x = 0
        position.y = 0
      }
    })
  }
}
```

