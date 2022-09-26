### 新建目录

```js
npm init 

安装
npm i webpack@4.44.2 webpack-cli -D
安装 4.x版本
```

### webpack 有默认的配置     入口src目录下的index.js

````
默认配置 
	入口文件：
		src/index.js
	出口
		dist/main.js
````

### 自定义webpack配置

默认配置文件 是在 根目录下 webpack.config.js

```js
const path = require('path')
module.exports = {
  //  模式 开发环境生产环境
  mode:'production',
  // 指定入口
  entry: './src/main.js',
  // 出口
  output: {
    path: path.join(__dirname, 'dist') ,// 指定出口 注意 必须是 绝对路径
    filename: 'main.[hash:8].js'  // []模板变量 hash自动加hash（随机数）值 hash根据内容生成
  }
}

```

### webpack配置多入口

```js
const path = require('path')
module.exports = {
  //  模式 开发环境生产环境
  mode:'production',
  // 指定入口 多入口
  entry: {
    a: './src/main.js',
    b: './src/dev.js'
  },
  // 出口
  output: {
    path: path.join(__dirname, 'dist') ,
    filename: '[name].[hash:8].js'  
    // 注意： 多入口 出口不能同名 变量name 出口文件自动和出口名字重名
  }
}

```

### 配置 html 模板

```js
src
	public
    	index.html
cnpm i html-webpack-plugin
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
  //  模式 开发环境生产环境
  mode:'production',
  // 指定入口 多入口
  entry: './src/main.js',
  // 出口
  output: {
    path: path.join(__dirname, 'dist') ,
    filename: 'chunk.[hash:8].js'
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: '我不好', 
      template:  path.join(__dirname, 'src/public/index.html'), // 指定模板路径
      filename: path.join(__dirname, 'dist/index.html') // 指定输出路径
    })
  ]
}

<title><%= htmlWebpackPlugin.options.title %></title>
可以在 index.html中通过 <%= htmlWebpackPlugin.options.title %> 获取 插件中配置的title属性值


```



### 处理css

```js
一切的资源都在 入口函数中引入 
支持 es6 以及 nodejs模块引入
main.js中
import './assets/css/common.css'
报错：css相当于在js中引入，js中不允许出现 css代码
怎么解决：在入口函数中引入 非js 文件
loader解析 非js的文件  不同的 文件有不同的loader
loader写法

cnpm i style-loader css-loader -D


const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  //  模式 开发环境生产环境
  mode:'production',
  // 指定入口 多入口
  entry: './src/main.js',
  // 出口
  output: {
    path: path.join(__dirname, 'dist') ,
    filename: 'chunk.[hash:8].js'
  },
  module: {
    rules: [  // 配置loader
        {
            test: /\.css$/,
            use: ['style-loader', 'css-loader']
            // loader执行顺序是从右往左 css-loader将 css文件解析为js style-loader将css代码动态创建style标签通过内部样式表引入 起效果
        }
    ]
  }
  plugins: [
    new HtmlWebpackPlugin({
      title: '我不好', 
      template:  path.join(__dirname, 'src/public/index.html'), // 指定模板路径
      filename: path.join(__dirname, 'dist/index.html') // 指定输出路径
    })
  ]
}

```

### 处理less (解析sass/scss)

```js
npm i less less-loader -D

{
    {
            test: /\.less/,
            use: ['style-loader', 'css-loader', 'less-loader']
        }
}
```

### clean-webpack-plugin

> 每次执行npm run build 会发现dist文件夹里会残留上次打包的文件，这里我们推荐一个plugin来帮我们在打包输出前清空文件夹`clean-webpack-plugin`

```
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
module.exports = {
    // ...省略其他配置
    plugins:[new CleanWebpackPlugin()]
}
复制代码
```

### 拆分csss

style loader并没有单独创建 css文件，引入css,而是将css代码 嵌入 生成chunk.js中 

其实 这样 性能更高（http请求减少），单独 单独拆分css文件引入 更利于维护

```
npm i -D mini-css-extract-plugin
复制代码
```

> webpack 4.0以前，我们通过`extract-text-webpack-plugin`插件，把css样式从js文件中提取到单独的css文件中。webpack4.0以后，官方推荐使用`mini-css-extract-plugin`插件来打包css文件

配置文件如下

```
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
module.exports = {
  //...省略其他配置
  module: {
    rules: [
      {
        test: /\.less$/,
        use: [
           MiniCssExtractPlugin.loader,
          'css-loader',
          'less-loader'
        ],
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
        filename: "[name].[hash].css",
        chunkFilename: "[id].css",
    })
  ]
}
复制代码
```

###  拆分多个css

> 这里需要说的细一点,上面我们所用到的`mini-css-extract-plugin`会将所有的css样式合并为一个css文件。如果你想拆分为一一对应的多个css文件,我们需要使用到`extract-text-webpack-plugin`，而目前`mini-css-extract-plugin`还不支持此功能。我们需要安装@next版本的`extract-text-webpack-plugin`

```
npm i -D extract-text-webpack-plugin@next
复制代码
// webpack.config.js

const path = require('path');
const ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
let indexLess = new ExtractTextWebpackPlugin('index.less');
let indexCss = new ExtractTextWebpackPlugin('index.css');
module.exports = {
    module:{
      rules:[
        {
          test:/\.css$/,
          use: indexCss.extract({
            use: ['css-loader']
          })
        },
        {
          test:/\.less$/,
          use: indexLess.extract({
            use: ['css-loader','less-loader']
          })
        }
      ]
    },
    plugins:[
      indexLess,
      indexCss
    ]
}
复制代码
```

### 资源引入

如图片 音频 视频  字体图标、、、

在main.js中引入

file-loader解析

如果css中写背景图片，只使用file-loader main.js解析图片，但是 会出现路径问题

解析url-loader解决路径引入的问题

url-loader除了解决路径的问题，还可以将 文件，解析成字符串（base64格式）

url-loader包含file-loader

```js
	{
        test: /\.(jpe?g|png|gif)$/i, //图片文件
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 10240,
              fallback: {
                loader: 'file-loader',
                options: {
                    name: 'img/[name].[hash:8].[ext]'
                }
              }
            }
          }
        ]
      }
```



```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const MiniCssExtractPlugin = require("mini-css-extract-plugin")
module.exports = {
  //  模式 开发环境生产环境
  mode:'production',
  // 指定入口 多入口
  entry: './src/main.js',
  // 出口
  output: {
    path: path.join(__dirname, 'dist') ,
    filename: 'chunk.[hash:8].js'
  },
  module: {
    rules: [
      /* {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }, */
      {
        test: /\.css$/,
        use: [{
          loader: MiniCssExtractPlugin.loader,
          options: {
            publicPath: '../' // css拆分后路径的问题 拆出css放在 dist/css目录
          }
        }, 'css-loader']
      },
      {
        test: /\.less$/,
        use: [ {
          loader: MiniCssExtractPlugin.loader,
          options: {
            publicPath: '../'
          }
        }, 'css-loader', 'less-loader']
      },
      {
        test: /\.(jpe?g|png|gif)$/i, //图片文件
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 1024,
              fallback: {
                loader: 'file-loader',
                options: {
                    name: 'imgs/[name].[hash:8].[ext]'
                }
              }
            }
          }
        ]
      }
     
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: '我不好',
      template:  path.join(__dirname, 'src/public/index.html'),
      filename: path.join(__dirname, 'dist/index.html')
    }),
    new CleanWebpackPlugin(),
    new MiniCssExtractPlugin({
      filename: "css/[name].[hash:8].css",
      chunkFilename: "[id].css",
  })
  ]
}

```

### css3 加浏览器前缀   post-css   



```
npm i -D postcss-loader autoprefixer  
复制代码
```

配置如下

```
// webpack.config.js
module.exports = {
    module:{
        rules:[
            {
                test:/\.less$/,
                use:['style-loader','css-loader','postcss-loader','less-loader'] // 从右向左解析原则
           }
        ]
    }
} 
复制代码
```

接下来，我们还需要引入`autoprefixer`使其生效,这里有两种方式

在项目根目录下创建一个`postcss.config.js`文件，配置如下：

```
module.exports = {
    plugins: [require('autoprefixer')]  // 引用该插件即可了
}

复制代码
```

### es6转es5

```js
npm install -D babel-loader @babel/core @babel/preset-env
						转es5核心代码    转换版本

```

### webpack-dev-server 启动服务器

```js
npm i webpack-dev-server -D

webpack.config.js
module.exports = {
    mode,
    entry,
    output,
    module,
    plugins,
    
}
```

package.json

```js
{
  "name": "pro1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack",
    "serve": "webpack-dev-server"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.12.3",
    "@babel/preset-env": "^7.12.1",
    "autoprefixer": "^10.0.1",
    "babel-loader": "^8.1.0",
    "clean-webpack-plugin": "^3.0.0",
    "css-loader": "^5.0.0",
    "file-loader": "^6.1.1",
    "graceful-fs": "^4.2.4",
    "html-webpack-plugin": "^4.5.0",
    "less": "^3.12.2",
    "less-loader": "^7.0.2",
    "mini-css-extract-plugin": "^1.2.0",
    "postcss-loader": "^4.0.4",
    "style-loader": "^2.0.0",
    "url-loader": "^4.1.1",
    "webpack": "^4.41.2",
    "webpack-bundle-analyzer": "^3.6.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.11.0",
    "webpack-merge": "^4.2.2",
    "webpack-version-file-plugin": "^0.4.0"
  }
}

```

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
module.exports = {
  //  模式 开发环境生产环境
  mode: "development",
  // 指定入口 多入口
  entry: "./src/main.js",
  // 出口
  output: {
    path: path.join(__dirname, "dist"),
    filename: "chunk.[hash:8].js",
  },
  module: {
    rules: [
      /* {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }, */
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: "../",
            },
          },
          "css-loader",
          "postcss-loader",
        ],
      },
      {
        test: /\.less$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: "../",
            },
          },
          "css-loader",
          "postcss-loader",
          "less-loader",
        ],
      },
      {
        test: /\.(jpe?g|png|gif)$/i, //图片文件
        use: [
          {
            loader: "url-loader",
            options: {
              limit: 1024,
              fallback: {
                loader: "file-loader",
                options: {
                  name: "imgs/[name].[hash:8].[ext]",
                },
              },
            },
          },
        ],
      },
      {
        test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/, //媒体文件
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 10240,
              fallback: {
                loader: 'file-loader',
                options: {
                  name: 'media/[name].[hash:8].[ext]'
                }
              }
            }
          }
        ]
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/i, // 字体
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 10240,
              fallback: {
                loader: 'file-loader',
                options: {
                  name: 'fonts/[name].[hash:8].[ext]'
                }
              }
            }
          }
        ]
      },
      {
        test: /\.js$/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"],
          },
        },
        exclude: /node_modules/,
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: "我不好",
      template: path.join(__dirname, "src/public/index.html"),
      filename: path.join(__dirname, "dist/index.html"),
    }),
    new CleanWebpackPlugin(),
    new MiniCssExtractPlugin({
      filename: "css/[name].[hash:8].css",
      chunkFilename: "[id].css",
    }),
  ],
  resolve:{  // 解析文件
        alias:{  // 路径别名
          'vue$':'vue/dist/vue.runtime.esm.js',
          ' @':path.resolve(__dirname,'../src')
        },
        extensions:['.js','.json','.vue']  // 省略文件后缀
   },
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    port: 3000,
    inline: true,
    open: true
  }
}

```

### 配置vue开发环境

```js
npm i -D vue-loader vue-template-compiler vue-style-loader
	vue-loader 解析.vue结尾 文件
    vue-template-compiler 解析 单文件模板的 template属性 放到 组件对象template属性
    vue-style-loader 解析 style标签样式
    
    
    
    
    
    const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
// vueloader插件
const vueLoaderPlugin = require("vue-loader/lib/plugin");
module.exports = {
  //  模式 开发环境生产环境
  mode: "development",
  // 指定入口 多入口
  entry: "./src/main.js",
  // 出口
  output: {
    path: path.join(__dirname, "dist"),
    filename: "chunk.[hash:8].js",
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        use: ["vue-loader"],
      },
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: "../",
            },
          },
          "css-loader",
          "postcss-loader",
        ],
      },
      {
        test: /\.less$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: "../",
            },
          },
          "css-loader",
          "postcss-loader",
          "less-loader",
        ],
      },
      {
        test: /\.(jpe?g|png|gif)$/i, //图片文件
        use: [
          {
            loader: "url-loader",
            options: {
              limit: 1024,
              fallback: {
                loader: "file-loader",
                options: {
                  name: "imgs/[name].[hash:8].[ext]",
                },
              },
            },
          },
        ],
      },
      {
        test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/, //媒体文件
        use: [
          {
            loader: "url-loader",
            options: {
              limit: 10240,
              fallback: {
                loader: "file-loader",
                options: {
                  name: "media/[name].[hash:8].[ext]",
                },
              },
            },
          },
        ],
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/i, // 字体
        use: [
          {
            loader: "url-loader",
            options: {
              limit: 10240,
              fallback: {
                loader: "file-loader",
                options: {
                  name: "fonts/[name].[hash:8].[ext]",
                },
              },
            },
          },
        ],
      },
      {
        test: /\.js$/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"],
          },
        },
        exclude: /node_modules/, // 排除 node_modules目录
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: "我不好",
      template: path.join(__dirname, "public/index.html"),
      filename: path.join(__dirname, "dist/index.html"),
    }),
    new CleanWebpackPlugin(),
    new MiniCssExtractPlugin({
      filename: "css/[name].[hash:8].css",
      chunkFilename: "[id].css",
    }),
    new vueLoaderPlugin(),
  ],
  resolve: {
    // 解析文件
    alias: {
      // 路径别名
      vue$: "vue/dist/vue.runtime.esm.js",
      "@": path.join(__dirname, "src"),
    },
    extensions: [".js", ".json", ".vue"], // 省略文件后缀
  },
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    port: 3000,
    inline: true,
    open: true,
  },
};

```

