---
title: 记Clipboard.js触发多次复制成功问题
tags:
  - 前端
categories:
  - - 问题&总结
    - Javascript
date: 2021-12-17 09:49:13
---

### 1.问题

复制文字到剪切板,触发多次成功事件,导致多次提示复制成功

```vue
const that = this
clipboard = new Clipboard('#copyBtn')
clipboard.on('success', function(e) {
  console.log('success', e.text)
  that.$message.success('复制成功')
  e.clearSelection()
})
clipboard.on('error', function(e) {
  that.$message.error('复制失败')
  e.clearSelection()
})
```

### 2.原因

因项目是单页应用,没有及时清理事件及创建的对象.

### 3.解决

```vue
beforeDestroy() {
  console.log('打印', 'destory')
  clipboard.destroy()
}
```

### 4.clipboard简单使用

[clipboard.js中文文档](https://www.kancloud.cn/luponu/clipboardjs_zh/988266)