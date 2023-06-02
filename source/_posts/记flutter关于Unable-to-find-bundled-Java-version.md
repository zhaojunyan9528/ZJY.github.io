---
title: 记flutter关于Unable to find bundled Java version
tags:
  - 前端
categories:
  - - 问题&总结
    - Flutter
date: 2023-05-31 15:53:01
---

先查看flutter版本：

```cmd
flutter --version
```

结果：

```cmd
  Flutter 1.20.2 • channel stable • <https://github.com/flutter/flutter.git>
  Framework • revision bbfbf1770c (2 年 10 个月前) • 2020-08-13 08:33:09 -0700
  Engine • revision 9d5b21729f
  Tools • Dart 2.9.1
```

感觉是版本太低，因为很久没更新，尝试更新：

```cmd
flutter upgrade
```

结果：

![flutter](/images/flutter_version.jpg)

解决cmdline-tools component is missing问题，在Androidstudio安装这个东西：

![flutter](/images/flutter_comline.jpg)

最后apply，等待安装即可
