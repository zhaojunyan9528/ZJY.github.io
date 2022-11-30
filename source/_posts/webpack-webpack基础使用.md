---
title: webpack-webpack基础使用
tags:
  - 前端
categories:
  - - 笔记
    - webpack
date: 2021-01-19 22:43:37
---

##### 使用lodash:是一个一致性、模块化、高性能的 JavaScript 实用工具库

要使用lodash 需要本地安装

```javascript
--npm install --save lodash
```

##### 在安装一个要打包到生产环境的安装包时，你应该使用 npm install --save，如果你在安装一个用于开发环境的安装包（例如，linter, 测试库等），你应该使用 npm install --save-dev。请在 npm 文档 中查找更多信息

##### 加载css

为了从 JavaScript 模块中 import 一个 CSS 文件，你需要在 module 配置中 安装并添加 style-loader 和 css-loader：

```javascript
    --npm install --save-dev style-loader css-loader
```

在webpack.config.js中配置

```javascript
module:{
        rules:[
            {
                test:/\.css$/,
                use:[
                    'style-loader',
                    'css-loader'
                ]
            }
        ]
    }
```

### 加载图片：  在Css中有背景和图标等图片，需要安装file-loader来处理

```javascript
    npm install --save-dev file-loader
```

在配置文件中配置：

```javascript
{
        test:/\.(png|svg|jpg|gif)&/,
        use:[
            'file-loader'
        ]
    }
```

### 加载字体： file-loader 和 url-loader 可以接收并加载任何文件，然后将其输出到构建目录

在配置文件中配置：

```javascript
{
        test:/\.(woff|woff2|eot|ttf|otf)$/,
        use:[
            'file-loader'
        ]
    }
```

此外，可以加载的有用资源还有数据，如 JSON 文件，CSV、TSV 和 XML。类似于 NodeJS，JSON 支持实际上是内置的，也就是说 import Data from './data.json' 默认将正常运行。要导入 CSV、TSV 和 XML，你可以使用 csv-loader 和 xml-loader：

```javascript
    npm install --save-dev csv-loader xml-loader
```

在配置文件中配置：

```javascript
{
        test:/\.(csv|tsv)$/,
        use:[
            'csv-loader'
        ]
    },{
        test:/\.xml$/,
        use:[
            'xml-loader'
        ]
    }
```

### html-webpack-plugin

如果我们更改了我们的一个入口起点的名称，甚至添加了一个新的名称，会发生什么？生成的包将被重命名在一个构建中，但是我们的index.html文件仍然会引用旧的名字。我们用 HtmlWebpackPlugin 来解决这个问题。首先安装插件，并且调整 webpack.config.js 文件

```javascript
    npm install --save-dev html-webpack-plugin
    webpack.config.js:
        const HtmlWebpackPlugin = require('html-webpack-plugin');
        plugins: [
           new HtmlWebpackPlugin({
           title: 'Output Management'
         })
       ],
```

### clean-webpack-plugin

清理 /dist 文件夹:

```javascript
    npm install clean-webpack-plugin --save-dev
    webpack.config.js:
        const {CleanWebpackPlugin} = require('clean-webpack-plugin');
        plugins: [
          new CleanWebpackPlugin(), /+
        ]
```

通过 manifest，webpack 能够对「你的模块映射到输出 bundle 的过程」保持追踪

### source map

使用 source map: 为了更容易地追踪错误和警告，JavaScript 提供了 source map 功能，将编译后的代码映射回原始源代码。如果一个错误来自于 b.js，source map 就会明确的告诉你。ource map 有很多不同的选项可用,对于本指南，我们使用 inline-source-map 选项

devtool: 'inline-source-map'

### webpack编译模式

每次要编译代码时，手动运行 npm run build 就会变得很麻烦,webpack 中有几个不同的选项，可以帮助你在代码发生变化后自动编译代码：

+ 1.webpack's Watch Mode 使用观察模式 依赖图中的所有文件以进行更改。如果其中一个文件被更新，代码将被重新编译，所以你不必手动运行整个构建。
    scripts中添加"watch": "webpack --watch",唯一的缺点是，为了看到修改后的实际效果，你需要刷新浏览器。如果能够自动刷新浏览器就更好了，
    可以尝试使用 webpack-dev-server，恰好可以实现我们想要的功能

+ 2.webpack-dev-server: 为你提供了一个简单的 web 服务器，并且能够实时重新加载(live reloading)让我们设置以下：npm install --save-dev webpack-dev-server
    修改配置文件，告诉开发服务器(dev server)，在哪里查找文件,webpack.config.js:
    devServer: {
     contentBase: './dist'
    },
    以上配置告知 webpack-dev-server，在 localhost:8080 下建立服务，将 dist 目录   下的文件，作为可访问文件。在package.json中添加一个script脚本可直接运行开发服务器。

+ 3.webpack-dev-middleware 是一个容器(wrapper)，它可以把 webpack 处理后的文件传递给一个服务器(server)。 webpack-dev-server 在内部使用了它，同时，它也可以作为一个单独的包来使用，以便进行更多自定义设置来实现更多的需求。接下来是一个 webpack-dev-middleware 配合 express server 的示例:
    首先，安装 express 和 webpack-dev-middleware:
    npm install --save-dev express webpack-dev-middleware
    webpack.config.js: output:{publicPath:'/'}
    新建server.js,再添加一个npm script :"server": "node server.js",运行npm run server

### tree shaking是一个术语，通常用于描述移除 JavaScript 上下文中的未引用代码(dead-code)

### glifyJsPlugin 用来对js文件进行压缩，从而减小js文件的大小，加速load速度

uglifyJsPlugin会拖慢webpack的编译速度，所有建议在开发简单将其关闭，部署的时候再将其打开。

```javascript
    --npm install uglifyjs-webpack-plugin --save-dev
```

通过webpack-merge配置不同环境的构建配置：

```javascript
    --npm install --save-dev webpack-merge
```

多数情况下，你也可以进行 CSS 分离，以便在生产环境中节省加载时间:

```javascript
    --npm install --save-dev extract-text-webpack-plugin
```

它会将所有的入口 chunk(entry chunks)中引用的 *.css，移动到独立分离的 CSS 文件。因此，你的样式将不再内嵌到 JS bundle 中，而是会放到一个单独的 CSS 文件（即 styles.css）当中。 如果你的样式文件大小较大，这会做更快提前加载，因为 CSS bundle 会跟 JS bundle 并行加载。

代码分离是 webpack 中最引人注目的特性之一。此特性能够把代码分离到不同的 bundle 中，然后可以按需加载或并行加载这些文件。代码分离可以用于获取更小的 bundle，以及控制资源加载优先级，如果使用合理，会极大影响加载时间。有三种常用的代码分离方法：

+ 1.入口起点：使用 entry 配置手动地分离代码。
+ 2.防止重复(prevent duplication): 通过使用 CommonsChunkPlugin 来移除重复的模块
+ 3.动态导入(dynamic imports):

    涉及到动态代码拆分时，webpack 提供了两个类似的技术。对于动态导入，第一种，也是优先选择的方式是，使用符合 ECMAScript 提案 的 import() 语法。第二种，则是使用 webpack 特定的 require.ensure
    import() 调用会在内部用到 promises:使用： import().then(),由于import（）会返回一个promise函数，因此可以和async函数一起使用，但是需要使用像babel这样的预处理器和Syntax Dynamic Import Babel Plugin：

```javascript
--npm install --save-dev @babel/plugin-syntax-dynamic-import
```
