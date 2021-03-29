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

