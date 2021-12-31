---
title: 记nuxt服务端请求接口无法通过store获取token
tags:
  - 前端
categories:
  - - 问题&总结
    - vue
date: 2021-12-31 13:35:07
---

1.问题

使用nuxt框架,接口在fetch或者asyncData中调用时,导致无法获取登录凭证token

2.原因

页面正常跳转进入或第一次加载时,asyncData和fetch方法都是在客户端执行,页面手动刷新时asyncData和fetch是在服务端执行,因此获取不到存储在客户端数据

3.解决

在store状态树中指定nuxtServerInit 方法, Nuxt.js 调用它的时候会将页面的上下文对象作为第2个参数传给它（服务端调用时才会）.可以将数据存储在服务端在nuxtServerInit中将数据传到客户端.

假设登录后将登录凭证token存在cookie中

```vue
login(params).then(res => {
  this.$store.commit('setToken', token)
  this.$cookies.set('token', token)
})

// 在store中actions中添加nuxtServerInit
nuxtServerInit({ commit }, { app, req, store }) {
  console.log(
    '打印nuxtServerInit3',
    req.headers.cookie,
    app.$cookies.getAll()
  )
  store.commit('setToken', app.$cookies.get('token'))
}

// 在axios请求拦截中设置authroization
$axios.onRequest((config) => {
  config.headers['authroization'] = store.state.token
})
```

4.nuxt 使用cookie持久化

```js
// 1.安装
yarn add cookie-universal-nuxt

// 2.config.nuxt.js
modules: [
  'cookie-universal-nuxt',
]
```

