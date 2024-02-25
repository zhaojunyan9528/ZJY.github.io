---
title: javascript-前端路由
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-01-19 23:03:20
---

### 什么是路由?

简单的说 路由就是根据不同的url地址来展示不同的内容或页面.

### 前端路由的来源

在很久很久以前~ 用户的每次更新操作都需要重新刷新页面,非常的影响交互体验,后来,为了解决这个问题,便有了Ajax(异步加载方案),Ajax给体验带来了极大的提升。
虽然Ajax解决了用户交互时体验的痛点,但是多页面之间的跳转一样会有不好的体验,所以便有了spa(single-page application)使用的诞生。而spa应用便是基于前端路由实现的,所以便有了前端路由
如今比较火的vue-router/react-router 也是基于前端路由的原理实现的~

### 前端路由的两种实现原理

1.Hash模式

    window对象提供了onhashchange事件来监听hash值的改变,一旦url中的hash值发生改变,便会触发该事件。
    window.onhashchange = function(){}

2.history模式

    HTML5的History API 为浏览器的全局history对象增加的扩展方法。简单来说,history其实就是浏览器历史栈的一个接口
    window对象提供了onpopstate事件来监听历史栈的改变,一旦历史栈信息发生改变,便会触发该事件。
    需要特别注意的是,调用history.pushState()或history.replaceState()不会触发popstate事件。只有在做出浏览器动作时，才会触发该事件。
    window.onpopstate = function(){
    // 历史栈 信息改变
    // do you want
    }
    history提供了两个操作历史栈的API:history.pushState 和 history.replaceState：
    history.pushState(data[,title][,url]);//向历史记录中追加一条记录
    history.replaceState(data[,title][,url]);//替换当前页在历史记录中的信息。
    // data: 一个JavaScript对象，与用pushState()方法创建的新历史记录条目关联。无论何时用户导航到新创建的状态，popstate事件都会被触发，并且事件对象的state属性都包含历史记录条目的状态对象的拷贝。

    //title: FireFox浏览器目前会忽略该参数，虽然以后可能会用上。考虑到未来可能会对该方法进行修改，传一个空字符串会比较安全。或者，你也可以传入一个简短的标题，标明将要进入的状态。

    //url: 新的历史记录条目的地址。浏览器不会在调用pushState()方法后加载该地址，但之后，可能会试图加载，例如用户重启浏览器。新的URL不一定是绝对路径；如果是相对路径，它将以当前URL为基准；传入的URL与当前URL应该是同源的，否则，pushState()会抛出异常。该参数是可选的；不指定的话则为文档当前URL。
    pushState操作：
        history.pushState({"foo":bar},"page 2","apply")

        history.state;   //输出 {foo:"bar"}

        window.onpopstate = function(){
            console.log('router');
        }
        history.pushState({"foo":"bar1"},"page2","apply");// 此时执行不触发onpopstate方法

        history.state； //输出 {foo:'bar1'}

        history.back(); //做出后退操作，触发onpopstate事件，此时输出 'router'
        history.state; //输出 {foo:"bar"}

        history.forward(); //做前进操作，触发onpopstate事件，此时输出 'router'
        history.state; //输出 {foo:"bar1"}

    replaceState操作：
        history.back();
        history.state; //输出 {foo:bar} 当前是bar

        
        history.replaceState({"foo":"barreplace"},"page2","test");//不触发onpopstate事件， 此时浏览器地址改为test,并不重新加载
        history.state; //输出：{foo:"barreplace"}

        history.forward(); //触发onpopstate事件，输出'router'
        history.state;  //输出： {foo:"bar1"}

        history.back();//触发onpopstate事件，输出'router',重新加载test
        history.state; //输出：{foo:"barreplace"}
