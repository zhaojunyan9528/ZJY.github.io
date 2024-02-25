---
title: vue虚拟dom和diff算法
tags:
  - 前端
categories:
  - - 笔记
    - vue
date: 2021-04-10 09:56:01
---

1.虚拟dom
用js对象描述dom的层级结构。虚拟dom属性和真实dom的属性一一对应。
vue虚拟dom参考snabbdom库实现的

diff是发生在虚拟dom上的
新的虚拟dom和旧的虚拟dom进行diff，算出如何最小量更新，最后反映在真正的dom上。

数据更新->虚拟dom计算变更->操作真实的dom->视图更新

一个虚拟节点有哪些属性？

{
  tag:'div',
  props:{
    id:'app',
    className:''
  },
  children:[]
}

```js
{
  children: undefined , //子元素
  data: {}, //属性，样式等
  elm: undefined, //真实的dom节点
  key: undefined, //唯一标识
  sel: "div", //选择器
  text: '文本'
}
```

```js
import {
    init,
    classModule,
    propsModule,
    styleModule,
    eventListenersModule,
    h,
} from "snabbdom";

// 创建出patch函数
const patch = init([
    // Init patch function with chosen modules
    classModule, // makes it easy to toggle classes
    propsModule, // for setting properties on DOM elements
    styleModule, // handles styling on elements with support for animations
    eventListenersModule, // attaches event listeners
]);

const container = document.getElementById("container");
// 创建虚拟节点 h函数
const vnode = h("div#container.two.classes", { on: { click: function(){} } }, [
    h("span", { style: { fontWeight: "bold" } }, "This is bold"),
    " and this is just normal text",
    h("a", { props: { href: "/foo" } }, "I'll take you places!"),
]);
// console.log(vnode)
// Patch into empty DOM element – this modifies the DOM as a side effect
// 让虚拟节点上树
patch(container, vnode);

const newVnode = h(
    "div#container.two.classes",
    { on: { click: function(){} } },
    [
        h(
            "span",
            { style: { fontWeight: "normal", fontStyle: "italic" } },
            "This is now italic type"
        ),
        " and this is still just normal text",
        h("a", { props: { href: "/bar" } }, "I'll take you places!"),
    ]
);
// Second `patch` invocation
patch(vnode, newVnode); // Snabbdom efficiently updates the old view to the new state

const vnode3 = h(
    'ul',[
        h('li','one'),
        h('li',"two")
    ]
)
console.log(vnode3);
// {
//   children:[
//     {
//       sel:'li',
//       data:{},
//       text: 'one',
//       elm: li,
//       key: undefined,
//       children: undefined
//     },
//     {
//       sel:'li',
//       data:{},
//       text: 'two',
//       elm: li,
//       key: undefined,
//       children: undefined
//     }
//   ],
//   sel:'ul',
//   elm: ul,
//   data:{},
//   key: undefined,
//   text: undefined
// }


patch(vnode,vnode3)
```

+ key是节点的唯一标识，告诉diff算法，在更改前后他们是同一个dom节点
+ 只比较同一层级，不跨级作比较
+ 只有是同一个虚拟节点才进行精细比较，否则就是暴力删除旧的，插入新的。

diff算法：

+ patch函数，首先判断oldvnode是不是虚拟节点，如果不是，转换为虚拟节点
+ 接着判断是不是同一个虚拟节点（选择器相同且key相同），如果不是，则创建新的节点并插入并删除旧的节点
+ 如果是同一个虚拟节点则进行精细比较，调用patchVnode函数，首先判断新虚拟节点vnode和旧虚拟节点oldVnode是否指向同一个对象，如果是直接返回
+ 如果他们都有文本节点并且不相等，那么将vnode的文本节点设置为真实dom的文本节点
+ 如果oldVnode有子节点而vnode没有子节点，则删除对应真实dom的子节点
+ 如果oldVnode没有子节点而vnode有子节点，将vnode的子节点真实化后添加到真实dom
+ 如果oldVnode和vnode都有子节点则执行updateChildren函数比较子节点

diff算法对比同级节点：
为了保证dom顺序和新节点保持一致使用while循环和首尾节点进行对比，对新旧虚拟节点数组的开始和结束节点设置索引，遍历的过程向中间移动索引，这样既能实现排序也减小了时间复杂度。两节点比较，有四种比对方式：

+ 旧开始节点  和  新结束节点  比较
+ 旧结束节点  和  新开始节点  比较
+ 旧开始节点  和  新开始节点  比较
+ 旧结束节点  和  新结束节点  比较

旧节点：oldStartIndex---->oldEndIndex
新节点：newStartIndex---->newEndIndex

比如：如果oldStartIndex和newEndIndex匹配上了，那么真实dom的第一个节点会移动到最右；如果newStartIndex和oldEndIndex匹配上了，那么真实dom的最后一个元素会移动到最左，匹配上的两个指针向中间移动，直到while循环结束

如果四种匹配都没有成功：

+ 那么遍历新节点，用新节点的key去老节点数组中查找相同节点
+ 如果没有找到，说明当前节点是新节点，创建对应dom元素并插入到真实dom
+ 如果找到了，比对新旧节点的sel是否相同
  + 如果相同，说明是同一个节点，移动到dom元素的最左边
  + 如果不相同，说明修改了节点，创建对应的dom元素并插入到真实dom树中
  
循环结束：

+ 如果老节点的子节点先遍历完，那么说明新节点有新增节点，将新增节点批量加入
+ 如果新节点的子节点先遍历完，那么说明老节点有剩余，将剩余节点批量删除

例子：

![image](/images/domdiff.jpeg)

+ 先对首尾进行4种比较:b和a,b和e,a和e,e和e. 得出e节点匹配在真实DOM最右侧中得出第1步. 索引向中间移动,剩下old: b a d f 剩下new: a b
+ 对剩余的同样进行收尾比较,匹配出b匹配在真实DOM的最左侧.剩下old: a d f 剩下new: a
+ 同上,匹配出a匹配在真实DOM的最左侧.剩下old:d f 剩下new: 空
+ 最后old长度大于new的长度,把真实dom中对应的删除