---
title: gulp-gulp基础使用
tags:
  - 前端
categories:
  - - 笔记
    - gulp
date: 2021-01-19 18:03:44
---

1.前置条件：测试node /npm /npx是否安装
2.安装gulp命令行工具

```javascript
    npm install --global gulp-cli
```

3.创建项目目录

```javascript
    npx mkdirp my-project
    cd my-project
```

4.在项目目录下创建package.json文件

```javascript
    npm init
```

5.安装gulp,作为开发时依赖项

```javascript
    npm install --save-dev gulp
```

6.检查gulp版本

```javascript
    gulp --version
```

7.创建gulpfile文件

8.测试

在项目根目录下执行 gulp 命令：gulp

9.创建任务（task）:

每个 gulp 任务（task）都是一个异步的 JavaScript 函数，此函数是一个可以接收 callback 作为参数的函数，或者是一个返回 stream、promise、event emitter、child process 或 observable (后面会详细讲解) 类型值的函数

10.导出任务：

任务可以是公开或私有的：
公开任务： 从gulpfile中被导出，可以通过gulp命令直接调用
私有任务：被设计在内部使用，通常作为series（）或parallel()组合的组成部分。
一个私有（private）类型的任务（task）在外观和行为上和其他任务（task）是一样的，但是不能够被用户直接调用。如需将一个任务（task）注册为公开（public）类型的，只需从 gulpfile 中导出（export）即可

11.组合任务

两个组合方法： series()和parallel(),允许多个独立的任务组合为一个更大的操作。这两个方法都可以接受任意数目的任务函数和已组合的操作。
series()和parallel()可以互相嵌套至任意深度。
如果需要让任务按顺序执行，请使用series()方法。

```javascript
        const { series } = require('gulp');
        function transpile(cb){
            cb();
        }
        function bundle(cb) {
            cb();
        }
        exports.build = series(transpile, bundle);
```

对于希望以最大并发来允许的任务，可以使用parallel()方法将他们组合：

```javascript
        cosnt { parallel } = require('gulp');
        function javascript(cb) {
            cb();
        }
        functin css(cb) {
            cb();
        }
        exports.build = parallel(javascript, css);
```

当series()和parallel()被调用时，任务会被立即组合在一起，这就允许在组合中进行改变，而不需要在单个任务中进行条件判断：

```javascript
        const { series } = require('gulp');
        function minify(cb){
            cb();
        }
        function overload(cb) {
            cb();
        }
        function transpile(cb) {
            cb();
        }
        if(process.env.NODE_ENV == 'production') {
            exports.build = series(minify, overload);
        }else {
            exports.build = series(transpile,overload);
        }
```

series()和parallel()可以被嵌套任意深度：

```javascript
        const { series, parallel } = requier('gulp');
        function a(cb) {
            cb();
        }
        function b(cb) {
            cb();
        }
        function c(cb) {
            cb();
        }
        function d(cb) {
            cb();
        }
        function e(cb) {
            cb();
        }
        exports.build = series(
            a,
            parallel(b
            ,
            series(c,d)
            ),
            e
        )
```

如果在两个不同任务间调用同一个任务将被执行两次，并产生不可预期结果：

```javascript
        const { series, parallel} = require('gulp');
        const clean = function(cb){
            cb();
        }
        const javascript = series(clean,function(cb){
            cb();
        })
        const css = series(clean, function(cb){
            cb();
        })
        exports.build = parallel(javascript,css)
```

重构为：

```javascript
        const { series, parallel } = require('gulp');
        function clean(cb) {
            cb();
        }
        function css(cb) {
            cb();
        }
        function javascript(cb) {
            cb();
        }
        exports.build = series(clean, parallel(css, javascript));
```

12.异步执行：

异步执行方式：
1.返回stream

```javascript
        const { src, dest} = require('gulp');
        function streanTask() {
            return src("*.js")
            .pipe(dest('output'));
        }
```

2.返回promise:

```javascript
        function promiseTask(){
            return Promise.resolve("test");
        }
```

3.返回event emitter

```javascript
        const { EventEmitter } = require('events');
        function eventEmitterTask(){
            const emitter = new EventEmitter();
            setTimeout(()=>{ emitter.emit('finish')},250);
            return emitter;
        }
```

4.返回 child process

```javascript
        const { exec } = require('gulp');
        function childProcessTask() {
            return exec('date');
        }
```

5.返回observable

```javascript
        const { Observale} = require('gulp');
        function observaleTask() {
            return Observable.of(1,2,3);
        }
```

6.使用callback
如果任务不返回任何内容，则必须用callback来指示任务已完成

7.使用async/await

```javascript
        const fs = require('fs');
        async function asyncAwaitTask() {
            const { version } = fs.readFileSync('package.json');
            console.log(version);
            await Promise.resolve('some result');
        }
```

13.处理文件

gulp暴露了src()和dest()方法用于处理计算机上存放的文件
src()接受glob参数，并从文件系统中读取文件然后生成一个node流
dest()接收一个输入目录作为参数，并且会产生一个node流作为终止流，当他接受到管道pipe传输的文件时，他会将文件内容及属性写到目录中

14.glob

 glob 是由普通字符和/或通配字符组成的字符串，用于匹配文件路径。可以利用一个或多个 glob 在文件系统中定位文件

 src() 方法接受一个 glob 字符串或由多个 glob 字符串组成的数组作为参数，用于确定哪些文件需要被操作。glob 或 glob 数组必须至少匹配到一个匹配项，否则 src() 将报错
 + 分隔符： /
 + 特殊符号： *
在一个字符串片段中匹配任意数量的字符，包括零个匹配。对于匹配单级目录下的文件很有用： '*.js'
+ 特殊字符： ** (两个星号)
在多个字符串片段中匹配任意数量的字符，包括零个匹配。 对于匹配嵌套目录下的文件很有用 'scripts/**/*.js'
+ 特殊字符： ! (取反)
由于 glob 匹配时是按照每个 glob 在数组中的位置依次进行匹配操作的，所以 glob 数组中的取反（negative）glob 必须跟在一个非取反（non-negative）的 glob 后面
        ['script/**/*.js', '!scripts/vendor/']
+ 匹配重叠：
两个或多个 glob 故意或无意匹配了相同的文件就被认为是匹配重叠（overlapping）了。如果在同一个 src() 中使用了会产生匹配重叠的 glob，gulp 将尽力去除重叠部分，但是在多个 src() 调用时产生的匹配重叠是不会被去重的

15.使用插件

Gulp 插件实质上是 Node 转换流（Transform Streams），它封装了通过管道（pipeline）转换文件的常见功能，通常是使用 .pipe() 方法并放在 src() 和 dest() 之间。他们可以更改经过流（stream）的每个文件的文件名、元数据或文件内容

16.文件监控

ulp api 中的 watch() 方法利用文件系统的监控程序（file system watcher）将 globs 与 任务（task） 进行关联。它对匹配 glob 的文件进行监控，如果有文件被修改了就执行关联的任务（task）。如果被执行的任务（task）没有触发 异步完成 信号，它将永远不会再次运行了。

```javascript
    function css(cb) {
        cb();
    }
    watch('src/*.css',css);
```

与文件监控程序关联的任务不能时同步任务
默认情况下，只要创建、更改或删除文件，文件监控程序就会执行关联的任务（task）。 如果你需要使用不同的事件，你可以在调用 watch() 方法时通过 events 参数进行指定。可用的事件有 'add'、'addDir'、'change'、'unlink'、'unlinkDir'、'ready'、'error'。此外，还有一个 'all' 事件，它表示除 'ready' 和 'error' 之外的所有事件。

```javascript
    const { watch } = require('gulp');

    // 所有事件都将被监控
    watch('src/*.js', { events: 'all' }, function(cb) {
        cb();
    });
```

初次执行：
调用watch之后，关联的任务是不会被立即执行，要等到第一次文件修改之后才执行
如果需要在第一次文件修改之前执行，也就是调用watch之后立即执行，设置参数： ignoreInitial: false

```javascript
watch('src/*.js', { ignoreInitial: false }, function(cb) {
    cb();
});
```

队列： queue: false//禁止队列
延迟： delay: Nubmer //文件更改之后延迟多久被执行