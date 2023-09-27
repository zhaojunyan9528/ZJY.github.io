---
title: >-
  记打包内存溢出Last few GCs（FATAL ERROR: Reached heap limit Allocation failed -
  JavaScript heap out of memory）
tags:
  - 前端
categories:
  - - 问题&总结
    - Javascript
date: 2023-09-27 16:56:39
---

项目环境：
node：16.15.1
框架：vue3、typescript
打包工具：vite

当运行npm run build时打包报错：

```shell
Last few GCs [ 8844 ： 999992A17EC9D459 ] 52995 ms ： (average mu = 9 ． 489 ， current mu [ 8844 ： 999992A17EC9D459 ] 52196 ms ： (average mu = 9 ． 489 ， current mu [ 8844 ： 999992A17EC9D459 ] 57289 ms ： Scavenge 2922 ． 1 （ 2979 ． 8 ） 9 ． 229 ） allocation failure Scavenge 2929 ． 4 （ 2981 ． 6 ） 9 ． 229 ） allocation failure Scavenge 2939 ． 9 （ 2983 ． 6 ） 一 > 2921 ． 3 （ 2981 ． 1 ） 凹 B ， 一 > 2927 ． 2 （ 2983 ． 6 ） 凹 B ， 一 > 2929 ． 4 （ 2193 ． 3 ） 凹 B ， 81 ． 6 / 9 ． 9 ms 69 ． 9 / 9 ． 9 ms 5996 ． 8 / 9 ． 9 ms (average mu = 9 ． 489 ， current mu = 9 ． 229 ） allocation failure 
<--- JS stacktrace --->  FATAL ERROR: Reached heap limit AIIocation failed - JavaScript heap out Of memory ...
```

解决思路：

方式一：
vscode终端运行：

```node
$env:NODE_OPTIONS="--max-old-space-size=8192"
```

方式二：
在打包时加上：npm run build --max_old_space_size=8192

方式三：
在package.json的script的打包命令添加：

```json
"build": "vite build && node --max_old_space_size=8192"
```