---
title: script标签的加载和执行
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-04-11 14:36:45
---

### 1.script标签是如何加载的？

+ 当浏览器遇到一个script标签时，会暂停页面渲染，运行javascript代码，然后再继续解析渲染页面；
+ 当遇到外部javascript脚本时，浏览器首先下载外部js代码，会占用一部分时间，然后运行js代码，此过程中，页面解析渲染和用户交互是阻塞的。

### 2.script标签应放在何处？

```html
<head>
<script type="text/javascript" src="file1.js" />
<script type="text/javascript" src="file2.js" />
<script type="text/javascript" src="file3.js" />
</head>
```

可以看出，在head中引入3个外部js文件，首先第一个js文件开始下载，占用一部分时间，并阻塞其他js文件的下载。file1.js文件下载完成后，又消耗一部分时间用来完全执行此js代码。然后才是file2.js文件下载，执行，file3.js文件下载，执行。每个文件都需要等到前一个js文件下载完成并执行完毕才能开始自己的下载过程。这些文件下载时，用户屏幕会出现空白，有明显的延迟。且浏览器遇到body标签之前不会渲染页面任何部分。将外部js放在head会导致一个明显的延迟，打开页面首先会出现空白，用户既不能阅读也不能与之交互，体验非常不好。

随着浏览器的发展，浏览器允许并行下载js文件。当一个script标签正在下载资源时，不必阻塞别的script标签。
但是script标签的下载仍要阻塞其他资源的下载，如图片，样式表。即使脚本下载过程不互相阻塞，页面仍需等到所有js资源下载并执行完毕才能继续。所以，当浏览器通过允许并行下载提高性能之后，该问题并没有完全解决。脚本阻塞仍旧是一个问题。

脚本阻塞其他资源的下载过程，推荐将script标签放在body结束标签的前面。

### 3.非阻塞脚本

非阻塞脚本就是等页面加载完成之后，再加载js。

1.defer属性

```html
<script type="text/javascript" src="file1.js" defer />
```

此script标签可以放在页面任何位置，解析到script标签时会立即下载此js文件，但会等到页面加载完成后再执行此js文件
只对外部js脚本有效，HTML5规范要求脚本按照它们出现的先后顺序执行。
延迟脚本会按他们在文档里的出现顺序执行

2.async属性

```html
<script type="text/javascript" src="file1.js" async />
```

不让页面等到脚本的下载和执行，从而异步加载页面其他内容。
下载完立即执行。不保证按照指定的先后顺序执行。只对外部js脚本有效
异步脚本在它们载入后执行，但是不能保证执行顺序。

图片说明script，defer，async：
![image](/images/script.jpg)

3.动态创建script标签

```html
<script type="text/javascript">
function downloadJSAtOnload(){
  var element = document.createElement('script');
  element.src = 'test.js';
  document.body.appendChild(element);
}
if(window.addEventListener){
  window.addEventListener("load",downloadJSAtOnload, false);
}else if(window.attachEvent){
  window.attachEvent("onload",downloadJSAtOnload);  
}else{
  window.onload =downloadJSAtOnload; 
}
</script>
```

创建script，插入到DOM中，加载完毕后callBack

### 4.为什么需要顺序加载和执行？

因为js有可能修改dom，如果不阻塞后续资源下载，dom的操作顺序不可控

### 5.外部样式表会不会阻塞文档解析？

不会
style-sheets 不会修改 DOM 树，没有理由为了解析 style-sheets 而阻塞文档解析（即 style-sheets 不会阻塞文档解析）。但如果在解析文档过程中有脚本需要访问样式信息时，为了保证访问样式信息的正确性。Firefox 会阻塞所有脚本直到 style-sheets 下载解析完为止。而 WebKit 只在访问的样式属性没有被加载解析时，才会阻塞脚本。

即 style-sheet 不会直接阻塞文档解析，它只阻塞 script 的解析执行，才导致 style-sheet 间接阻塞文档解析