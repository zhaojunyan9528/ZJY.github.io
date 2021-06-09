---
title: DomContentLoaded事件解析
tags:
  - 前端
categories:
  - - 问题&总结
    - HTML
date: 2021-06-09 10:50:42
---

### 1.DOMContentLoaded事件

当初始的HTML文档被完全加载和解析完成之后，DOMContentLoaded事件被触发，而无需等待样式表、图像和子框架的完全加载。

### 2.load事件

当整个页面及所有依赖资源如样式表和图片都已完成加载时，将触发load事件

<b>DOMContentLoaded事件和load事件区别：</b>
DOMContentLoaded在HTML文档被解析完成之后触发，而load是在HTML所有相关资源被加载完成后触发

### 3.HTML解析过程与DOMContentLoaded触发时机

#### 1.在既没有css也没有js情况下，HTML文档的解析过程为：

![image](/images/domcontentload/domcontentload1.jpg)

DOMContentLoaded事件的触发时机为:HTML解析为DOM之后

#### 2.有css无js情况下，HTML文档的解析过程为：

![image](/images/domcontentload/domcontent2.jpg)

渲染树的生成是基于DOM和CSSOM的。但是触发DOMContentLoaded的时间依然是在HTML解析为DOM后，无论此时CSS解析为CSSOM的过程是否完成

#### 3.当有js和css时，HTML文档的解析过程为：

![image](/images/domcontentload/domcontent3.jpg)

网页从空白到出现内容所花费的时间，就是html文档加载和解析的时间，也就是DOMContentLoaded事件触发之前所经历的时间。
所以对于首屏时间而言，js放在html文档的开头和结尾效果是一样的。而js放在结尾的目的并不是为了减少首屏时间，而是由于js经常需要操作dom，放在后面才更能保证找到dom节点。

#### 4.同步/异步脚本

同步脚本：
html文档解析时如果遇见同步脚本，则停止解析，先加载脚本再执行，执行结束后继续解析HTML文档。HTML文档解析完毕后触发DOMContentLoaded事件

异步脚本：
+ defer

当 HTML 文档被解析时如果遇见 defer 脚本，则在后台加载脚本，文档解析过程不中断，而等文档解析结束之后，defer 脚本执行。另外，defer 脚本的执行顺序与定义时的位置有关。在前面的script会先执行

+ async

当 HTML 文档被解析时如果遇见 async 脚本，则在后台加载脚本，文档解析过程不中断。脚本加载完成后，文档停止解析，脚本执行，执行结束后文档继续解析

带async的脚本一定会在load事件之前执行，可能会在DOMContentLoaded之前或之后执行。

1.当HTML还没有被解析完的时候，async脚本已经加载完了，那么HTML停止解析，去执行脚本，脚本执行完后继续解析HTML文档，然后触发DOMContentLoaded事件

![image](/images/domcontentload/domcontent4.jpg)

2.HTML 解析完了之后，async脚本才加载完，然后再执行脚本，那么在HTML解析完毕、async脚本还没加载完的时候就触发DOMContentLoaded事件。如下图所示：

![image](/images/domcontentload/domcontent5.jpg)

总之， DomContentLoaded 事件只关注 HTML 是否被解析完，而不关注 async 脚本。

defer
如果script标签含有defer，那么这一块脚本将不会影响 HTML 文档的解析，而是等到 HTML 解析完成后才会执行。而 DOMContentLoaded 只有在 defer 脚本执行结束后才会被触发。

defer脚本同样包含两种情况：

1.HTML还没解析完成时，defer脚本已经加载完毕，那么defer脚本将等待HTML解析完成后再执行。defer脚本执行完毕后触发DOMContentLoaded事件。如下图所示

![image](/images/domcontentload/domcontent6.png)

2.HTML解析完成时，defer脚本还没加载完毕，那么defer脚本继续加载，加载完成后直接执行，执行完毕后触发DOMContentLoaded事件。如下图所示:

![image](/images/domcontentload/domcontent7.png)

<b>defer与DOMContentLoaded</b>

如果 script 标签中包含 defer，那么这一块脚本将不会影响 HTML 文档的解析，而是等到 HTML 解析完成后才会执行。而 DOMContentLoaded 只有在 defer 脚本执行结束后才会被触发。 所以这意味着什么呢？HTML 文档解析不受影响，等 DOM 构建完成之后 defer 脚本执行，但脚本执行之前需要等待 CSSOM 构建完成。在 DOM、CSSOM 构建完毕，defer 脚本执行完成之后，DOMContentLoaded 事件触发。

<b>async与DOMContentLoaded</b>
 DomContentLoaded 事件只关注 HTML 是否被解析完，而不关注 async 脚本。