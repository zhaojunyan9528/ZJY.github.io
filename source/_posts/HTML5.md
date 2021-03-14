---
title: HTML5基础
tags:
  - 前端
categories:
  - - 笔记
    - HTML5
date: 2021-02-19 11:30:37
---

## HTML5

### 开始学习HTML5

#### HTML5 简介

+ HTML5是HTML最新的修订版本，2014年10月由万维网联盟（W3C）完成标准制定。
+ HTML5的设计目的是为了在移动设备上支持多媒体。

#### 什么是 HTML5?

+ HTML5 是下一代 HTML 标准。
+ HTML , HTML 4.01的上一个版本诞生于 1999 年。自从那以后，Web 世界已经经历了巨变。
+ HTML5 仍处于完善之中。然而，大部分现代浏览器已经具备了某些 HTML5 支持。
+ HTML5 受包括Firefox（火狐浏览器），IE9及其更高版本，Chrome（谷歌浏览器），Safari，Opera等国外主流浏览器的支持；国内的傲游浏览器（Maxthon）， 360浏览器、搜狗浏览器、QQ浏览器、猎豹浏览器等同样具备支持HTML5的能力。

#### HTML5 <!DOCTYPE>

<!doctype> 声明必须位于 HTML5 文档中的第一行,使用非常简单:`<!doctype html>`

#### 最小的HTML5文档

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

#### HTML5的改进

+ 新元素
+ 新属性
+ 完全支持 CSS3
+ Video 和 Audio
+ 2D/3D 制图
+ 本地存储
+ 本地 SQL 数据
+ Web 应用

#### HTML5多媒体

使用 HTML5 你可以简单的在网页中播放 视频(video)与音频 (audio) 。

+ html5 `<video/>`
+ html5 `<audio/>`

#### HTML5 图形

使用 HTML5 你可以简单的绘制图形:

+ 使用 `<canvas>` 元素
+ 使用内联 SVG
+ 使用 CSS3 2D/CSS 3D

#### HTML5 使用 CSS3

+ 新选择器
+ 新属性
+ 动画
+ 2D/3D 转换
+ 圆角
+ 阴影效果
+ 可下载的字体
  
#### 语义要素

HTML5 添加了很多语义元素如下所示：

+ article: 定义页面独立的内容区域。
+ aside:定义页面的侧边栏内容
+ bdi:允许您设置一段文本，使其脱离其父元素的文本方向设置。
+ command:定义命令按钮，比如单选按钮、复选框或按钮
+ details:用于描述文档或文档某个部分的细节
+ dialog:定义对话框，比如提示框
+ summary: 标签包含 details 元素的标题
+ figure:规定独立的流内容（图像、图表、照片、代码等等）
+ figcaption:定义 `<figure> `元素的标题
+ footer:定义 section 或 document 的页脚。
+ header:定义了文档的头部区域
+ mark:定义带有记号的文本。
+ meter:定义度量衡。仅用于已知最大和最小值的度量
+ nav:定义导航链接的部分。
+ progress:定义任何类型的任务的进度
+ ruby:定义 ruby 注释（中文注音或字符）
+ rt:定义字符（中文注音或字符）的解释或发音。
+ rp:在 ruby 注释中使用，定义不支持 ruby 元素的浏览器所显示的内容
+ section:定义文档中的节（section、区段）
+ time:定义日期或时间。
+ wbr:规定在文本中的何处适合添加换行符

#### HTML5 - 新特性

+ 用于绘画的 canvas 元素
+ 用于媒介回放的 video 和 audio 元素
+ 对本地离线存储的更好的支持
+ 新的特殊内容元素，比如 article、footer、header、nav、section
+ 新的表单控件，比如 calendar、date、time、email、url、search

### HTML5 浏览器支持

+ HTML5 浏览器支持
    现代的浏览器都支持 HTML5。
    此外，所有浏览器，包括旧的和最新的，对无法识别的元素会作为内联元素自动处理。
    正因为如此，你可以 "教会" 浏览器处理 "未知" 的 HTML 元素。
+ 将 HTML5 元素定义为块元素
    HTML5 定了 8 个新的 HTML 语义（semantic）  元素。所有这些元素都是块级 元素。
    为了能让旧版本的浏览器正确显示这些元素，你可以设置 CSS 的 display 属性值为 block:
    header, section, footer, aside, nav, main, article, figure {display: block;}
+ 为 HTML 添加新元素
  
```html
<script>
    window.onload=function(){
        var newEle = document.createElement('myHero');
        var content = document.createTextNode('myHero');
        newEle.appendChild(content)
        document.body.appendChild(newEle);
    }
    
</script>
<style>
    myHero{
        background-color:red;
        padding:30px;
    }
</style>
```

+ Internet Explorer 浏览器问题
  + 你可以使用以上的方法来为 IE 浏览器添加 HTML5 元素，但是Internet Explorer 8 及更早 IE 版本的浏览器不支持以上的方式.针对IE浏览器html5shiv 是比较好的解决方案。html5shiv主要解决HTML5提出的新的元素不被IE6-8识别，这些新元素不能作为父节点包裹子元素，并且不能应用CSS样式。
  
```js
  <!-- [if It IE 9] -->
  <script src="/html5shiv.min.js"></script>
  <!-- [endif] -->
```

+ 完美的 Shiv 解决方案
  + html5shiv.js 引用代码必须放在  `<head>` 元素中，因为 IE 浏览器在解析 HTML5 新元素时需要先加载该文件。
  
### HTML5 新元素

#### canvas

什么是canvas？
  标签定义图形，比如图表和其他图像。该标签基于javascript的绘图API。
创建画布
  画布在网页中是一个矩形框，通过`<canvas>`元素来绘制

  ```html
  <canvas id="myCanvas" width="200" height="100" />
  ```
使用 JavaScript 来绘制图像
  canvas本身没有绘图能力，所以绘图工作必须在js内完成：

  ```js
  var c = document.getElementById('myCanvas');
  var ctx = c.getContext('2d');
  ctx.fillStyle="#ffffff";
  ctx.fillRect(0,0,150,75);
  ```

canvas-坐标
  canvas是一个二维网络，左上角坐标为（0，0）
  ctx.fillRect(0,0,150,75);在画布上绘制150*75的矩形，从左上角开始

canvas-路径
  moveTo(x,y) 定义线条开始坐标
  lineTo(x,y) 定义线条结束坐标

  ```js
  ctx.moveTo(0,0);
  ctx.lineTo(200,100);
  ctx.stroke();//绘制线条
  ```

  绘制圆形：arc(x,y,r,start,stop)

  ```js
  ctx.begainPath();
  ctx.arc(0,0,40,0,2*Math.PI);
  ctx.stroke();
  ```

canvas-文本
  fillText(text,x,y,[maxWidth]);

  ```js
  ctx.font = '30px arial';
  ctx.fillText('hello,world",10,50)
  ```

canvas-渐变

  渐变可以填充在矩形，圆形，线条，文本等，可以定义不同的颜色
  createLinearGradient(x,y,x1,y1);//线条渐变
  createRadialGradient(x,y,r,x1,y1,r1);//径向/圆渐变
  当我们使用渐变对象，必须使用两种或两种以上的停止颜色
  addColorStop()方法指定颜色停止，参数使用坐标来描述，可以是0至1.
  设置fillStyle或strokeStyle的值为渐变，然后绘制形状，如矩形，文本，或一条线。
  使用createLinearGradient：创建一个线性渐变，使用渐变填充矩形

  ```js
  //create gradient
  var grd = ctx.createLinearGradient(0,0,200,0);
  grd.addColorStop(0,"red");
  grd.addColorStop(1,'white');
  //fill width gradient
  ctx.fillStyle = grd;
  ctx.fillRect(10,10,150,80);
  ```

  使用createRadialGradient：创建一个径向/圆渐变，使用渐变填充矩形
  
  ```js
  var grd = ctx.createRadialGradient(75,50,5,90,60,100);
  grd.addColorStop(0,"red");
  grd.addColorStop(1,'white');

  ctx.fillStyle= grd;
  ctx.fillRect(10,10,150,80);
  ```

  createRadialGradient(x , y , r , x1 , y1 , r1) 括号内的参数有如下的含义:
  x：表示渐变的开始圆的 x 坐标
  y：表示渐变的开始圆的 y 坐标
  r：表示开始圆的半径
  x1：表示渐变的结束圆的 x 坐标
  y1：表示渐变的结束圆的 y 坐标
  r1：表示结束圆的半径

canvas-图像
  drawImage(image,x,y)

  ```js
  var img = document.getElementById("img");
  ctx.drawImage(img,10,10);
  ```

#### 新多媒体元素

+ audio音频

```html
<audio controls>
  <source src="audio.ogg" type="aduio/ogg">
  <source src="audio.mp3" type="aduio/mpeg">
  您的浏览器不支持audio元素
</audio>
```

control 属性供添加播放、暂停和音量控件
在`<audio>` 与 `</audio>` 之间你需要插入浏览器不支持的`<audio>`元素的提示文本 。
`<audio>` 元素允许使用多个 `<source>` 元素. `<source>` 元素可以链接不同的音频文件，浏览器将使用第一个支持的音频文件

+ video视频
  
```html
<video controls width="320" height="240">
  <source src="movie.ogg" type="video/ogg">
  <source src="movie.mp4" type="video/mp4">
  您的浏览器不支持video元素
</video>
```

`<video>`元素提供了播放、暂停和音量控件来控制视频。
同时`<video>`元素也提供了 width 和 height 属性控制视频的尺寸.如果设置的高度和宽度，所需的视频空间会在页面加载时保留。。如果没有设置这些属性，浏览器不知道大小的视频，浏览器就不能再加载时保留特定的空间，页面就会根据原始视频的大小而改变。
元素支持多个source元素. 元素可以链接不同的视频文件。浏览器将使用第一个可识别的格式
元素支持三种视频格式： MP4, WebM, 和 Ogg:

+ source
  `<source>` 标签可以为`<picture>`、`<audio>`或`<video>`元素指定一个或者多个的媒体资源

+ embed
  定义嵌入的内容，比如插件，定义了一个容器，用来嵌入外部应用或者互动程序（插件）
  属性：
  height：规定嵌入内容的高度
  src: 规定嵌入内容的url
  type: 规定嵌入内容的MIME类型
  width: 规定嵌入内容的宽带

+ track
  为诸如 `<video>` 和 `<audio>` 元素之类的媒介规定外部文本轨道
  `<track>` 标签用作 `<audio>` 元素和 `<video> `元素的子级，它允许您指定定时文本轨道（或基于时间的数据），采用 WebVTT 格式（.vtt 文件）

```html
<video controls width="320" height="240">
  <source src="movie.ogg" type="video/ogg">
  <source src="movie.mp4" type="video/mp4">
  <track src="subtitle_en.vtt" kind="subtitles" srclang="en" label="English">
  <track src="subtitle_zh.vtt" kind="subtitles" srclang="zh" label="Zh">
  您的浏览器不支持video元素
</video>
```

这个元素用于规定字幕文件或其他包含文本的文件，当媒体播放时，这些文件是可见的。

#### 新表单元素

+ datalist
  定义选项列表。请与 input 元素配合使用该元素，来定义 input 可能的值
  + `<datalist>` 标签规定了 `<input>` 元素可能的选项列表。
  + `<datalist>` 标签被用来在为 `<input>` 元素提供"自动完成"的特性。用户能看到一个下拉列表，里边的选项是预先定义好的，将作为用户的输入数据。
  + 请使用 `<input>` 元素的 list 属性来绑定 `<datalist>` 元素。
  + 提示：不能控制 datalist 的位置，并且不能将其与服务器的数据进行绑定。
  
```html
<input list="browser" name="browser">
<datalist id="browser">
    <option value="IE"></option>
    <option value="Firefox"></option>
    <option value="chrome"></option>
    <option value="Opera"></option>
    <option value="safari"></option>
</datalist>
```

+ keygen
  + 规定用于表单的密钥对生成器字段
  + 当提交表单时，私钥存储在本地，公钥发送到服务器。

+ output
  + 定义不同类型的输出，比如脚本的输出

#### 新的语义和结构元素

+ article：定义页面独立的内容区域，主要是布局文章、内容方面的内容
+ aside：定义页面的侧边栏内容。`<aside>` 标签定义 `<article>` 标签外的内容
+ bdi:标签允许您设置一段文本，使其脱离其父元素的文本方向设置
+ figure: 标签规定独立的流内容（图像、图表、照片、代码等等）,figure 元素的内容应该与主内容相关，但如果被删除，则不应对文档流产生影响
+ footer:标签定义文档document或节section的页脚。页脚通常包含文档的作者、版权信息、使用条款链接、联系信息等等。您可以在一个文档中使用多个 `<footer> `元素。
+ header:定义了文档的头部区域,表示介绍性的内容，可以让您了解页面涉及的内容，具有导航性.在一个文档中，您可以定义多个 `<header>` 元素。注释：`<header>` 标签不能被放在 `<footer>`、`<address>` 或者另一个 `<header>` 元素内部。
+ mark:带有记号的文本，请在需要突出显示文本时使用 `<mark> `标签。
+ meter：定义度量衡。仅用于已知最大和最小值的度量`<meter min="0" max="10" value="5">5 out of 10</meter>`
+ nav:标签定义导航链接的部分,如果文档中有“前后”按钮，则应该把它放到 `<nav> `元素中。
+ progress:标签标示任务的进度
+ ruby:标签定义 ruby 注释（中文注音或字符）。
  
  ```html
  <ruby>
      王 <rp>(</rp><rt> wang </rt><rp>)</rp>
  </ruby>
  ```

  将 `<ruby>` 标签与 `<rt>` 和 `<rp> `标签一起使用
  rt:定义字符（中文注音或字符）的解释或发音
  rp:在 ruby 注释中使用，定义不支持 ruby 元素的浏览器所显示的内容

+ section:定义文档中的节（section、区段）。比如章节、页眉、页脚或文档中的其他部分。
+ time:用来表示HTML网页中出现的日期和时间，目的是让搜索引擎等其它程序更容易的提取这些信息。`<time>` 标签不会在任何浏览器中呈现任何特殊效果，只是用来给机器识别的。
+ wbr:可以用来定义HTML文档中需要进行换行的位置，与`<br>`标签不同，如果浏览器窗口的宽度足够，则不换行；反之，则在添加了 `<wbr>` 标签的位置进行换行


### HTML5 内联 SVG

SVG表示可缩放矢量图形，是基于可扩展标记语言（标准通用标记语言的子集），用于描述二维矢量图形的一种图形格式，它在2003年1月14日成为W3C推荐标准。
什么是SVG?

+ SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
+ SVG 用于定义用于网络的基于矢量的图形
+ SVG 使用 XML 格式定义图形
+ SVG 图像在放大或改变尺寸的情况下其图形质量不会有损失
+ SVG 是万维网联盟的标准
+ SVG 与 DOM 和 XSL 之类的 W3C 标准是一个整体
  
与其他图像格式相比（比如 JPEG 和 GIF），使用 SVG 的优势在于：

+ SVG 图像可通过文本编辑器来创建和修改
+ SVG 图像可被搜索、索引、脚本化或压缩
+ SVG 是可伸缩的
+ SVG 图像可在任何的分辨率下被高质量地打印
+ SVG 可在图像质量不下降的情况下被放大

SVG 与 Canvas两者间的区别:

+ SVG 是一种使用 XML 描述 2D 图形的语言。
+ Canvas 通过 JavaScript 来绘制 2D 图形
+ SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器
+ 在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形
+ Canvas 是逐像素进行渲染的。在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。

Canvas与SVG的比较
canvas：

+ 依赖分辨率
+ 不支持事件处理器
+ 弱的文本渲染能力
+ 能够以png或jpg格式保存结果图像
+ 最适合图像密集型游戏，其中的许多对象会被频繁重绘
  
svg：

+ 不依赖分辨率
+ 支持事件处理器
+ 最适合带有大型渲染区域的应用程序（比如地图）
+ 复杂度稿会减慢渲染速度（任何过度使用DOM的应用都不快）
+ 不适合游戏应用


### HTML5 拖放

+ HTML5 拖放（Drag 和 Drop）
  + 拖放的目的是可以让你将某个对象放置到你想要放置的位置。
  + 拖放（Drag 和 drop）是 HTML5 标准的组成部分。
  + 拖放是一种常见的特性，即抓取对象以后拖到另一个位置。
  + 在 HTML5 中，拖放是标准的一部分，任何元素都能够拖放。

```js
<script type="text/javascript">
function allowDrop(ev)
{
ev.preventDefault();
}

function drag(ev)
{
ev.dataTransfer.setData("Text",ev.target.id);
}

function drop(ev)
{
ev.preventDefault();
var data=ev.dataTransfer.getData("Text");
ev.target.appendChild(document.getElementById(data));
}
</script>


<p>请把 W3School 的图片拖放到矩形中：</p>

<div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
<br />
<img id="drag1" src="/i/eg_dragdrop_w3school.gif" draggable="true" ondragstart="drag(event)" />
```

设置元素为可拖放:`<img draggable="true">`
拖动什么 - ondragstart 和 setData()
放到何处 - ondragover,ondragover 事件规定在何处放置被拖动的数据。
默认地，无法将数据/元素放置到其他元素中。如果需要设置允许放置，我们必须阻止对元素的默认处理方式。
这要通过调用 ondragover 事件的 event.preventDefault() 方法
进行放置 - ondrop

### HTML5 地理定位

HTML5 Geolocation（地理定位）用于定位用户的位置
Geolocation 通过请求一个位置信息，用户同意后，浏览器会返回一个包含经度和维度的位置信息

定位用户的位置:
  HTML5 Geolocation API 用于获得用户的地理位置
  鉴于该特性可能侵犯用户的隐私，除非用户同意，否则用户位置信息是不可用的。

```js
<p id="demo">点击按钮获取您当前坐标：</p>
<button onclick="getLocation()">click me</button>
<script>
    var x = document.getElementById('demo');
    function getLocation(){
        if(navigator.geolocation){
            navigator.geolocation.getCurrentPosition(showPosition);
        }else{
            x.innerHTML ='该浏览器不支持获取地理位置'
        }
    }
    function showPosition(position){
        console.log(position)
        x.innerHTML = '纬度：'+ position.coords.latitude+'<br>经度：'+position.coords.longitude
    }
</script>
```