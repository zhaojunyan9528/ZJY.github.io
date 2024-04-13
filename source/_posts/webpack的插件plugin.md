---
title: webpack的插件plugin
tags:
  - 前端
categories:
  - - 笔记
    - webpack
date: 2021-03-26 21:08:02
---

### 插件(plugin)

loader用来转换文件，插件用来执行更广泛的任务。包括：打包优化、资源管理，注入环境变量。
使用一个插件，只需要require()它，添加到plugins数组中，多数插件可以通过option选项自定义。
你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建一个插件实例

webpack插件是一个具有apply方法的js对象，apply 方法会被 webpack compiler 调用，并且在__整个__编译生命周期都可以访问 compiler 对象。

```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
module.exports = {
  plugins:[
    new HtmlWebpackPlugin({ template: './src/index.html' });
  ]
}
```

HtmlWebpackPlugin插件为应用程序生成一个html文件，并自动注入所以生成的bundle

clean-webpack-plugin:打包之前删除 dist。

```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

plugins:[
  new CleanWebpackPlugin();
]
```

CommonsChunkPlugin:提取chunks间共享的通用模块

MiniCssExtractPlugin：为每个引入css的js文件创建一个css文件

HotModuleReplacementPlugin：启用模块热更替

uglifyjs-webpack-plugin：压缩js文件的体积

为什么使用webpack？

框架、ES6模块化语法，预处理器scss等需要转换为浏览器能识别的js、css等

webpack静态打包工具, 处理非js文件

功能：

开发模式-development：仅能编译JS中的ES6模块化语法
生产模式-production：能编译JS中的ES6模块化语法，还能压缩js代码

安装依赖:

```shell
npm init -y
npm i webpack webpack-cli -D
```

webpack编译:

```shell
npx webpack ./src/main.js --mode=development
```

核心概念：

entry：入口，指定webpack从哪个文件开始打包
output: 输出，打包完的代码输出到哪里如何命名等
loader：加载器，webpack本身只能处理js、json等资源，其他资源需要借助loader，webpack才能解析
plugins：插件，扩展webpack的功能
mode：模式，开发:development, 生产production

webpack配置文件webpack.config.js

根目录下创建webpack.config.js文件：

```js
const path = require('path')
module.exports = {
  entry: './src/main.js', // 相对路径
  // 输出
  output: {
    path: path.resolve(__dirname, 'dist'), // 绝对路径，__dirname当前目录的目录名
    filename: 'main.js'
  },
  // loaders
  module: {
    rules: []
  },
  // 插件
  plugins: [],
  // 模式
  mode: 'development'
}
```

运行打包：

```shell
npx webpack
```

开发模式：
1、编译代码，使浏览器能识别：样式资源、字体图标、图片、html等）
2、代码质量检查，eslint等

处理css资源：css-loader

安装css-loader:

```shell
npm install --save-dev css-loader style-loader
```

配置loader：

在 webpack 的配置中，loader 有两个属性：

+ test 属性，识别出哪些文件会被转换。
+ use 属性，定义出在进行转换时，应该使用哪个 loader。

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'style-loader', // 将css资源创建style标签添加到html文件中生效
          'css-loader' // 将css资源编译成commonjs的模块到js中
        ]} // use执行顺序从右到左
    ]
  },
}
```

使用less-loader：

首先，你需要先安装 less 和 less-loader：

```shell
npm install less less-loader --save-dev
```

然后将该 loader 添加到 webpack 的配置中去:

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.less$/,
        use: [
          'style-loader', // 将css资源创建style标签添加到html文件中生效
          'css-loader', // 将css资源编译成commonjs的模块到js中
          'less-loader'
        ]} // use执行顺序从右到左
    ]
  },
}
```

使用sass-loader
首先，你需要安装 sass-loader：

```shell
npm install sass-loader sass webpack --save-dev
```

然后将本 loader 添加到你的 Webpack 配置:

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: [
          // 将 JS 字符串生成为 style 节点
          'style-loader',
          // 将 CSS 转化成 CommonJS 模块
          'css-loader',
          // 将 Sass 编译成 CSS
          'sass-loader',
        ],
      },
    ],
  },
}
```

使用stylus-loader，首先，先安装stylus和stylus-loader：

```shell
npm install stylus stylus-loader --save-dev
```

将stylus-loader添加到配置中：

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.styl$/,
        use: [
          // 将 JS 字符串生成为 style 节点
          'style-loader',
          // 将 CSS 转化成 CommonJS 模块
          'css-loader',
          // 将 stylus 编译成 CSS
          'stylus-loader',
        ],
      },
    ]
  }
}
```

处理图片资源，webpack4处理图片资源使用file-loader和url-loader,现在webpack5，使用资源模块类型(asset module type)，通过添加 4 种新的模块类型，来替换所有这些 loader：

+ asset/resource 发送一个单独的文件并导出 URL。之前通过使用 file-loader 实现。
+ asset/inline 导出一个资源的 data URI。之前通过使用 url-loader 实现。
+ asset/source 导出资源的源代码。之前通过使用 raw-loader 实现。
+ asset 在导出一个 data URI 和发送一个单独的文件之间自动选择。之前通过使用 url-loader，并且配置资源体积限制实现

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|webp|svg|jpeg)$/,
        type: 'asset', //asset通用资源类型  自动地在 resource 和 inline 之间进行选择：
        // 小于 8kb 的文件，将会视为 inline 模块类型，否则会被视为 resource 模块类型。
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024 // 8kb
          }
        }
      },
    ]
  }
}
```

将文件输出到指定资源目录：

```js
{
  test: /\.(png|jpg|webp|svg|jpeg)$/,
  type: 'asset', //asset通用资源类型  自动地在 resource 和 inline 之间进行选择：
  // 小于 8kb 的文件，将会视为 inline 模块类型，否则会被视为 resource 模块类型。
  parser: {
    dataUrlCondition: {
      maxSize: 10 * 1024 // 8kb
    }
  },
  generator: {
    filename: 'static/images/[hash][ext][query]' // 输出文件名,将某些资源发送到指定目录：
  }
},
```

[hash:10]代表哈希值取10位

每次构建前清理dist文件夹：

```js
module.exports = {
// 输出
  output: {
    path: path.resolve(__dirname, 'dist'), // 绝对路径，__dirname当前目录的目录名
    filename: 'main.js',
    clean: true // 在每次构建前清理 /dist 文件夹
  },
}
```

使用字体资源：

```js
{
  test: /\.(ttf|woff|woff2)$/, // 字体资源
  type: 'asset/resource',
  generator: {
    filename: 'static/media/[hash][ext][query]'
  }
}
```

其他资源比如map3等也可以像字体资源处理

处理js资源：
针对js兼容性处理，使用babel完成
针对代码个格式，使用eslint处理

Eslint配置文件写法：

+ .eslintrc.*: 新建文件，位于根目录
  + .eslintrc
  + .eslintrc.js
  + .eslintrc.json
  + 区别在于配置格式不一样

+ package.json中的eslintConfig中配置

eslint在webpack4是eslint-loader，在webpack5是plugins

使用eslint需要安装 eslint-webpack-plugin：

```js
npm install eslint-webpack-plugin --save-dev
```

注意: 如果未安装 eslint >= 7 ，你还需先通过 npm 安装：

```js
npm install eslint --save-dev
```

然后把插件添加到你的 webpack 配置:

```js
const ESLintPlugin = require('eslint-webpack-plugin');

module.exports = {
  // ...
  plugins: [
    new ESLintPlugin({
      // 指定文件根目录，类型为字符串,检测哪些文件
      context: path.resolve(__dirname, 'src')
    })
  ],
  // ...
};
```

创建eslint配置文件：

```js
module.exports = {
  extends: ['eslint:recommended'],
  env: {
    node: true, // 启动node全局变量
    browser: true // 启用浏览器全局变量
  },
  parserOptions: {
    ecmaVersion: 6, // es6
    sourceType: "module" // es module
  },
  rules: {
    "no-var": 2 // 不能使用var定义变量
  }
}
```

Babel将es6语法编写的代码转换为向后兼容的js语法，能够运行在当前和旧版本的浏览器或其他环境中。

配置文件有多种写法：

+ babel.config.*: 新建文件，位于根目录
  + babel.config.js
  + babel.config.json
+ .babelrc.*:新建文件，位于根目录
  + .babelrc
  + .babelrc.js
  + .babelrc.json
+ package.json中的label中，不需要创建文件

使用babel,先安装：

```shell
npm install -D babel-loader @babel/core @babel/preset-env webpack
```

webpack 配置对象中，需要将 babel-loader 添加到 module 列表中:

```js
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
  ]
}
```

HtmlWebpackPlugin 简化了 HTML 文件的创建，以便为你的 webpack 包提供服务。这对于那些文件名中包含哈希值，并且哈希值会随着每次编译而改变的 webpack 包特别有用。

安装：

```shell
npm install --save-dev html-webpack-plugin
```

该插件将为你生成一个 HTML5 文件， 在 body 中使用 script 标签引入你所有 webpack 生成的 bundle.

```js
module.exports = {
  plugins: [
    new HtmlWebpackPlugin()
  ]
}
```

webpack-dev-server监听文件改变，自动打包

```shell
npm i webpack-dev-server -D
```

配置webpack.config.js：

```js
module.exports = {
   // 开发服务器
  devServer: {
    host: "localhost",
    port: 3000,
    open: true
  }
}
```

运行命令：`npx webpack serve`

**生产模式：**

优化代码运行性能
优化打包速度

根目录下创建config目录，创建webpack.dev.js和webpack.prod.js

然后运行：

```shell
npx webpack serve --config ./config/webpack.dev.js // dev
npx webpack --config ./config/webpack.prod.js // prod
```

或者加入package.json：

```json
"scripts": {
  "start": "npm run dev",
  "dev": "webpack serve --config ./config/webpack.dev.js",
  "build": "webpack --config ./config/webpack.prod.js"
}
```

**css处理，提取css文件.**

css文件目前被打包到js文件，当js文件加载时，会创建一个style标签来生成样式

这样对网站不好，会出现闪屏

单独将css文件分离，用link标签加载性能更好

安装：

```shell
npm install --save-dev mini-css-extract-plugin
```

建议 mini-css-extract-plugin 与 css-loader 一起使用。

```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  plugins: [new MiniCssExtractPlugin()],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
    ],
  },
};
```

运行`npm run build` 打包后的index.html会link引入css文件
不要同时使用 style-loader 与 mini-css-extract-plugin。

**css样式兼容性处理.**
需要安装 postcss-loader 和 postcss, postcss-preset-env

```shell
npm install --save-dev postcss-loader postcss postcss-preset-env
```

配置：

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [
          'style-loader',
          'css-loader',
          {
            loader: 'postcss-loader',
            options: {
              postcssOptions: {
                plugins: [
                  [
                    'postcss-preset-env',
                    {
                      // 其他选项
                    },
                  ],
                ],
              },
            },
          },
        ],
      },
    ],
  },
};
```

或者使用postcss本身的配置文件postcss.config.js:

```js
module.exports = {
  plugins: [
    [
      'postcss-preset-env',
      {
        // 其他选项
      },
    ],
  ],
};
```

在package.json中配置需要浏览器兼容的程度：

```json
"browserslist": [
  "> 1%",
  "last 2 versions",
  "not ie <= 8"
]
```

比如display:flex,打包编译会变成

```css
display: -webkit-box;
display: -ms-flexbox;
```

**优化和压缩css：**

你需要安装 css-minimizer-webpack-plugin：

```shell
npm install css-minimizer-webpack-plugin --save-dev
```

接着在 webpack 配置中加入该插件:

```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

module.exports = {
  module: {
    rules: [
      {
        test: /.s?css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
      },
    ],
  },
  optimization: {
    minimizer: [
      // 在 webpack@5 中，你可以使用 `...` 语法来扩展现有的 minimizer（即 `terser-webpack-plugin`），将下一行取消注释
      // `...`,
      new CssMinimizerPlugin(),
    ],
  },
  plugins: [new MiniCssExtractPlugin()],
};
```

这将仅在生产环境开启 CSS 优化。

如果还想在开发环境下启用 CSS 优化，请将 optimization.minimize 设置为 true:

```js
module.exports = {
  optimization: {
    // [...]
    minimize: true,
  },
};
```

默认生产模式已开启html压缩和js压缩

SourceMap: 开发环境中，运行代码source是编译过的。
sourcemap是源代码和编译后的代码的映射文件，代码出错后找到源代码出错位置，从而让浏览器提示出错误。

devtool选项配置sourcemap，不同的值会明显影响到构建(build)和重新构建(rebuild)的速度。
值有很多种，但是一般只关心开发和生产模式的值：

开发模式：devtool: cheap-module-source-map
优点：打包编译速度快，只包含行映射
缺点：没有列映射

生产模式：devtool: source-map
优点：包含行和列映射
缺点：打包编译速度更慢

**启用 HMR: 提升打包构建速度。**

开发时更改一个模块的代码，webpack会默认所有模块全部打包编译，速度很慢。
HotModuleReplacement：HMR热模块替换，在程序运行中添加、替换、删除模块而无需重新加载整个页面

```js
module.exports = {
  //...
  devServer: {
    hot: true, // 启用 webpack 的 热模块替换
  },
};
```

但是hot: true不支持js热模块替换，js更改整个页面都重新刷新

实际使用框架，vue-loader、react-hot-loader解决热更替

**oneOf: 每个文件只能被一个loader配置处理.**

```js
module: {
  rules: [
    {
      // 每个文件只能被一个loader配置处理
      oneOf: [
        {
          test: /\.css$/,
          use: [
          'style-loader', // 将css资源创建style标签添加到html文件中生效
          'css-loader', // 将css资源编译成commonjs的模块到js中
          'less-loader'
          ]
        }, // use执行顺序从右到左
        {
          test: /\.less$/,
          use: [
            // compiles Less to CSS
            'style-loader',
            'css-loader',
            'less-loader',
          ],
        },
        {
          test: /\.s[ac]ss$/,
          use: [
            // 将 JS 字符串生成为 style 节点
            'style-loader',
            // 将 CSS 转化成 CommonJS 模块
            'css-loader',
            // 将 Sass 编译成 CSS
            'sass-loader',
          ],
        },
        {
          test: /\.styl$/,
          use: [
            // 将 JS 字符串生成为 style 节点
            'style-loader',
            // 将 CSS 转化成 CommonJS 模块
            'css-loader',
            // 将 stylus 编译成 CSS
            'stylus-loader',
          ],
        },
        {
          test: /\.(png|jpg|webp|svg|jpeg)$/,
          type: 'asset', //asset通用资源类型  自动地在 resource 和 inline 之间进行选择：
          // 小于 8kb 的文件，将会视为 inline 模块类型，否则会被视为 resource 模块类型。
          parser: {
            dataUrlCondition: {
              maxSize: 10 * 1024 // 8kb
            }
          },
          generator: {
            filename: 'static/images/[hash][ext][query]' // 输出文件名,将某些资源发送到指定目录：
          }
        },
        {
          test: /\.(ttf|woff|woff2)$/, // 字体资源
          type: 'asset/resource',
          generator: {
            filename: 'static/media/[hash][ext][query]'
          }
        },
        {
          test: /\.js$/,
          exclude: /node_modules/,
          loader: 'babel-loader'
        }
      ]
    }
  ]
},
```

**Include/Exclude.**

Include: 包含，只处理xxx文件
Exclude: 排除，除列xxx文件都处理

```js
{
  test: /\.js$/,
  exclude: /node_modules/, // 排除node_modules文件
  // include: path.resolve(__dirname, '../src')
  loader: 'babel-loader'
}
```

**Cache: 开启缓存。**

每次打包都要经过babel编译和eslint检查，速度慢
可以缓存之前的babel编译和eslint检查，这样第二次打包速度会更快

```js
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /node_modules/,
      loader: 'babel-loader',
      options: {
        cacheDirectory: true, // 开启babel缓存
        cacheCompression: false // 关闭缓存文件压缩
      }
    }
  ]
},

plugins: [
  new EslintPlugin({
    // 指定文件根目录，类型为字符串,检测哪些文件
    context: path.resolve(__dirname, '../src'),
    exclude: "node_modules",
    cache: true, // 开启缓存
    cacheLocation: path.resolve(__dirname, '../node_modules/.cache/eslint-webpack-plugin/.eslintcache')
  }),
]
```

**Thread-loader: 多进程。**

项目越来越大时，打包速度更慢，可以开启多进程同时处理js文件。

使用时，需将此 loader 放置在其他 loader 之前。放置在此 loader 之后的 loader 会在一个独立的 worker 池中运行。

每个 worker 都是一个独立的 node.js 进程，其开销大约为 600ms 左右。同时会限制跨进程的数据交换。

请仅在耗时的操作中使用此 loader！
安装：

```shell
npm install --save-dev thread-loader
```

使用：

```js
// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        include: path.resolve('src'),
        use: [
          "thread-loader",
          // 耗时的 loader （例如 babel-loader）
        ],
      },
    ],
  },
};
// with options
use: [
  {
    loader: "thread-loader",
    // 有同样配置的 loader 会共享一个 worker 池
    options: {
      // 产生的 worker 的数量，默认是 (cpu 核心数 + 1)，或者，
      // 在 require('os').cpus() 是 undefined 时回退至 1
      workers: 2,

      // 一个 worker 进程中并行执行工作的数量
      // 默认为 20
      workerParallelJobs: 50,

      // 额外的 node.js 参数
      workerNodeArgs: ['--max-old-space-size=1024'],

      // 允许重新生成一个僵死的 work 池
      // 这个过程会降低整体编译速度
      // 并且开发环境应该设置为 false
      poolRespawn: false,

      // 闲置时定时删除 worker 进程
      // 默认为 500（ms）
      // 可以设置为无穷大，这样在监视模式(--watch)下可以保持 worker 持续存在
      poolTimeout: 2000,

      // 池分配给 worker 的工作数量
      // 默认为 200
      // 降低这个数值会降低总体的效率，但是会提升工作分布更均一
      poolParallelJobs: 50,

      // 池的名称
      // 可以修改名称来创建其余选项都一样的池
      name: "my-pool"
    },
  },
  // 耗时的 loader（例如 babel-loader）
];
```

```js
const threads = os.cpus().length // 获取cpu的核数量

module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /node_modules/,
      use: [
        {
          loader: 'thread-loader',
          options: {
            works: threads // 进程数量
          }
        },
        {
          loader: 'babel-loader',
          options: {
            cacheDirectory: true, // 开启babel缓存
            cacheCompression: false // 关闭缓存文件压缩
          }
        }
      ]
    }
  ]
},
plugins: [
  new EslintPlugin({
    // 指定文件根目录，类型为字符串,检测哪些文件
    context: path.resolve(__dirname, '../src'),
    exclude: "node_modules",
    cache: true, // 开启缓存
    cacheLocation: path.resolve(__dirname, '../node_modules/.cache/eslint-webpack-plugin/.eslintcache'),
    threads // 开启多进程和设置多进程数量
  }),
]
```

TerserWebpackPlugin:该插件使用 terser 来压缩 JavaScript。
webpack v5 开箱即带有最新版本的 terser-webpack-plugin。如果你使用的是 webpack v5 或更高版本，同时希望自定义配置，那么仍需要安装 terser-webpack-plugin。如果使用 webpack v4，则必须安装 terser-webpack-plugin v4 的版本

```shell
npm install terser-webpack-plugin --save-dev
```

```js
const TerserPlugin = require("terser-webpack-plugin");

module.exports = {
  // css压缩优化
  optimization: {
    minimizer: [
      // 在 webpack@5 中，你可以使用 `...` 语法来扩展现有的 minimizer（即 `terser-webpack-plugin`），将下一行取消注释
      // `...`,
      new CssMinimizerPlugin(),
      new TerserPlugin({
        parallel: threads // 开启多进程和设置多进程数量
      }) // 压缩js
    ],
  },
};
```

**减少代码体积： tree shaking.**

开发时定义了一些工具函数库，或引入第三方工具函数库或组件库

可能只使用极小一部分功能确需要引入整个库，打包进来体积很大

Tree shaking一个术语，用于描述移除js中未使用的代码

webpack默认开启这个功能

减少babel代码体积：@babel/plugin-transform-runtime

安装：

```shell
npm install @babel/plugin-transform-runtime -D
```

```js
{
  test: /\.js$/,
  exclude: /node_modules/,
  use: [
    {
      loader: 'thread-loader',
      options: {
        works: threads // 进程数量
      }
    },
    {
      loader: 'babel-loader',
      options: {
        cacheDirectory: true, // 开启babel缓存
        cacheCompression: false, // 关闭缓存文件压缩
        plugins: ["@babel/plugin-transform-runtime"] // 减少代码体积

      }
    }
  ]
}
```

**Image minimizer： 图片压缩。**

本地静态图片压缩，减少代码体积

```shell
npm install image-minimizer-webpack-plugin --save-dev
```

无损压缩模式，需要安装：

```shell
npm install imagemin-gifsicle imagemin-jpegtran imagemin-optipng imagemin-svgo imagemin --save-dev
```

有损压缩：

```shell
npm install imagemin-gifsicle imagemin-mozjpeg imagemin-pngquant imagemin-svgo imagemin --save-dev
```

配置webpack.config.js：

```js
const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");
const { extendDefaultPlugins } = require("svgo");
// ...
new ImageMinimizerPlugin({
  minimizer: {
    implementation: ImageMinimizerPlugin.imageminGenerate,
    options: {
      // Lossless optimization with custom option
      // Feel free to experiment with options for better result for you
      plugins: [
        ["gifsicle", { interlaced: true }],
        ["jpegtran", { progressive: true }],
        ["optipng", { optimizationLevel: 5 }],
        // Svgo configuration here https://github.com/svg/svgo#configuration
        [
          "svgo",
          {
            plugins: [
              "preset-default",
              "prefixIds"
            ],
          },
        ],
      ],
    }
  },
}),
```

**Code Split：代码分割。**
打包代码时会将所有js文件打包到一个文件中，体积太大了，如果渲染首页，就应该只加载首页的js。

对js文件进行分割，渲染哪个页面就加载对应js，加载资源少速度更快

代码分割：
1、分割文件，将打包生成的文件进行分割，分为多个js文件
2、按需加载，需要哪个文件加载哪个

配置：optimization.splitChunks

```js
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'async',
      minSize: 20000,
      minRemainingSize: 0,
      minChunks: 1,
      maxAsyncRequests: 30,
      maxInitialRequests: 30,
      enforceSizeThreshold: 50000,
      cacheGroups: {
        defaultVendors: {
          test: /[\\/]node_modules[\\/]/,
          priority: -10,
          reuseExistingChunk: true,
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true,
        },
      },
    },
  },
};
```

**自定义loader：**

clean-log-loader

```js
// clean-log-loader
// 清理console.log
module.exports = function(context) {
  return context.replace(/console\.log\(.*\);?/g, "")
}
```

使用：

```js
{
  test: /\.js$/,
  use: ['./loaders/clean-log-loader']
} // 自定义loader
```

loader分为：

+ pre 前置loader
+ normal 普通loader
+ inline 内联loader
+ post 后置loader

优先级：前置loader > 普通loader > 内联loader > 后置loader

同步loader：

```js
module.exports = function(context) {
  return context
}
```

异步loader：

```js
module.exports = function (content, map, meta) {
  var callback = this.async();
  someAsyncOperation(content, function (err, result) {
    if (err) return callback(err);
    callback(null, result, map, meta);
  });
};
```

"Raw" Loader:默认情况下，资源文件会被转化为 UTF-8 字符串，然后传给 loader。通过设置 raw 为 true，loader 可以接收原始的 Buffer

```js
module.exports = function (content) {
  assert(content instanceof Buffer);
  return someSyncOperation(content);
  // 返回值也可以是一个 `Buffer`
  // 即使不是 "raw"，loader 也没问题
};
module.exports.raw = true;
```

Pitching Loader:loader 总是 从右到左被调用。有些情况下，loader 只关心 request 后面的 元数据(metadata)，并且忽略前一个 loader 的结果。在实际（从右到左）执行 loader 之前，会先 从左到右 调用 loader 上的 pitch 方法。

```js
module.exports = function (content) {
  return someSyncOperation(content, this.data.value);
};

module.exports.pitch = function (remainingRequest, precedingRequest, data) {
  data.value = 42;
};
```

**plugin: 自定义plugin。**

```js
/**
 * webpack会加载配置文件和命令语句初始化参数，生成compoliler对象，加载配置的插件
 * 变量所有插件，调用的插件的apply方法
 * 开始编译（触发各个hooks钩子事件）
 */
class TestPlugin {
  constructor() {
    console.log('testPlugin constructor')
  }
  apply (complier) {
    console.log('testPlugin apply')
  }
}
// testPlugin constructor
// testPlugin apply
module.exports = TestPlugin

// 在webpack.config.js配置使用：
const TestPlugin = require('../plugins/test-plugin') // 自定义插件
//...
plugins: [
  new TestPlugin()
]
// ...
```

complier模块是webpack的主要引擎，通过cli或者node api传递的所有选项创建一个compilation 实例。
它扩展（extends）自 Tapable 类，用来注册和调用插件。大多数面向用户的插件会首先在 Compiler 上注册。

根据使用不同的钩子(hooks)和tap方法，插件可以以不同的方式运行。

同步tap

```js
compiler.hooks.compile.tap('MyPlugin', (params) => {
  console.log('以同步方式触及 compile 钩子。');
});
```

对于可以使用 AsyncHook 的 run 阶段， 则需使用 tapAsync 或 tapPromise（以及 tap）方法。

```js
compiler.hooks.run.tapAsync(
  'MyPluginName',
  (source, target, routesList, callback) => {
    console.log('以异步方式触及运行钩子。');
    callback();
  }
);

compiler.hooks.run.tapPromise('MyPluginName', (source, target, routesList) => {
  return new Promise((resolve) => setTimeout(resolve, 1000)).then(() => {
    console.log('以异步的方式触发具有延迟操作的钩子。');
  });
});

compiler.hooks.run.tapPromise(
  'MyPluginName',
  async (source, target, routesList) => {
    await new Promise((resolve) => setTimeout(resolve, 1000));
    console.log('以异步的方式触发具有延迟操作的钩子。');
  }
);
```

Compilation 模块会被 Compiler 用来创建新的 compilation 对象（或新的 build 对象）。 compilation 实例能够访问所有的模块和它们的依赖（大部分是循环依赖）。 它会对应用程序的依赖图中所有模块， 进行字面上的编译(literal compilation)。 在编译阶段，模块会被加载(load)、封存(seal)、优化(optimize)、 分块(chunk)、哈希(hash)和重新创建(restore)。

添加注释插件：

```js
const path = require('path')
const os = require('os')
const EslintPlugin = require('eslint-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const MiniCssExtractPlugin = require("mini-css-extract-plugin")
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin')
const TerserPlugin = require("terser-webpack-plugin")
const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin")
// const TestPlugin = require('../plugins/test-plugin') // 自定义插件
const BannerWebpackPlugin = require('../plugins/banner-webpack')
const threads = os.cpus().length // 获取cpu的核数量

function getStyleLoader(loader) {
  return [
    MiniCssExtractPlugin.loader, // 将css文件单独引入到link
    // 'style-loader', // 将css资源创建style标签添加到html文件中生效
    'css-loader', // 将css资源编译成commonjs的模块到js中
    {
      loader: 'postcss-loader',
      options: {
        postcssOptions: {
          plugins: [
            [
              'postcss-preset-env'
            ],
          ],
        },
      },
    },
    loader
  ].filter(Boolean)
}

module.exports = {
  entry: './src/main.js', // 相对路径
  // 输出
  output: {
    path: path.resolve(__dirname, '../dist'), // 绝对路径，__dirname当前目录的目录名
    filename: 'static/js/main.js',
    clean: true // 在每次构建前清理 /dist 文件夹
  },
  // loaders
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.css$/,
            use: getStyleLoader()
          }, // use执行顺序从右到左
          {
            test: /\.less$/,
            use: getStyleLoader('less-loader')
          },
          {
            test: /\.s[ac]ss$/,
            use: getStyleLoader('sass-loader')
          },
          {
            test: /\.styl$/,
            use: getStyleLoader('stylus-loader')
          },
          {
            test: /\.(png|jpg|webp|svg|jpeg)$/,
            type: 'asset', //asset通用资源类型  自动地在 resource 和 inline 之间进行选择：
            // 小于 8kb 的文件，将会视为 inline 模块类型，否则会被视为 resource 模块类型。
            parser: {
              dataUrlCondition: {
                maxSize: 10 * 1024 // 8kb
              }
            },
            generator: {
              filename: 'static/images/[hash][ext][query]' // 输出文件名,将某些资源发送到指定目录：
            }
          },
          {
            test: /\.(ttf|woff|woff2)$/, // 字体资源
            type: 'asset/resource',
            generator: {
              filename: 'static/media/[hash][ext][query]'
            }
          },
          {
            test: /\.js$/,
            exclude: /node_modules/,
            use: [
              {
                loader: 'thread-loader',
                options: {
                  works: threads // 进程数量
                }
              },
              {
                loader: 'babel-loader',
                options: {
                  cacheDirectory: true, // 开启babel缓存
                  cacheCompression: false, // 关闭缓存文件压缩
                  plugins: ["@babel/plugin-transform-runtime"] // 减少代码体积
                }
              }
            ]
          },
          // {
          //   test: /\.js$/,
          //   use: ['./loaders/clean-log-loader']
          // } // 自定义loader
        ]
      }
    ],
  },
  // css压缩优化
  optimization: {
    minimizer: [
      // 在 webpack@5 中，你可以使用 `...` 语法来扩展现有的 minimizer（即 `terser-webpack-plugin`），将下一行取消注释
      // `...`,
      new CssMinimizerPlugin(),
      new TerserPlugin({
        parallel: threads // 开启多进程和设置多进程数量
      }) // 压缩js
    ],
  },
  // 插件
  plugins: [
    // new TestPlugin(),
    new BannerWebpackPlugin({
      author: 'zjy'
    }),
    new EslintPlugin({
      // 指定文件根目录，类型为字符串,检测哪些文件
      context: path.resolve(__dirname, '../src'),
      exclude: "node_modules",
      cache: true, // 开启缓存
      cacheLocation: path.resolve(__dirname, '../node_modules/.cache/eslint-webpack-plugin/.eslintcache'),
      threads // 开启多进程和设置多进程数量
    }),
    new HtmlWebpackPlugin({
      // 以public/index.html为模版，自动引入bundles到body的script
      template: path.resolve(__dirname, '../public/index.html')
    }),
    new MiniCssExtractPlugin({
      filename: "static/css/main.css"
    }),
    new ImageMinimizerPlugin({
      minimizer: {
        implementation: ImageMinimizerPlugin.imageminGenerate,
        options: {
          // Lossless optimization with custom option
          // Feel free to experiment with options for better result for you
          plugins: [
            ["gifsicle", { interlaced: true }],
            ["jpegtran", { progressive: true }],
            ["optipng", { optimizationLevel: 5 }],
            // Svgo configuration here https://github.com/svg/svgo#configuration
            [
              "svgo",
              {
                plugins: [
                  "preset-default",
                  "prefixIds"
                ],
              },
            ],
          ],
        }
      },
    })
  ],
  // 模式
  mode: 'production',
  devtool: 'source-map'
}
// webpack.config.js
new BannerWebpackPlugin({
  author: 'zjy'
}),
```
