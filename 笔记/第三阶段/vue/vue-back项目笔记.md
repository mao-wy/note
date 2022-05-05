创建vue项目

```
vue create 项目名称
```

配置

```
const path = require('path')
module.exports = {
  devServer: {
    port: 3000,
    open: true,
    proxy: {
      '/galaxy97': { // 请求 接口 以 /galaxy97开头，自动匹配
        target: 'https://api.it120.cc', // 目标地址
        ws: false,
        changeOrigin: true, // 开启代理：在本地会创建一个虚拟服务端，然后发送请求的数据，并同时接收请求的数据，这样服务端和服务端进行数据的交互就不会有跨域问题
        pathRewrite: { // 路径重写
          '^/galaxy97': '/galaxy97'
        } // 这里重写路径
      }
    }
    // /conner/a   https://api.it120.cc/conner/a
  },
  lintOnSave: false,
  chainWebpack: (config) => {
    config.resolve.alias
      .set('@', path.join(__dirname, 'src'))
      .set('components', path.join(__dirname, 'src/components'))
      .set('views', path.join(__dirname, 'src/views'))
      .set('api', path.join(__dirname, 'src/api'))
      .set('utils', path.join(__dirname, 'src/utils'))
      .set('mixins', path.join(__dirname, 'src/mixins'))
  }
}

```

