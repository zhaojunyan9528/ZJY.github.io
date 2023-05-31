---
title: 记flutter关于Flutter plugin not installed
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2023-05-31 15:47:32
---

![flutter](/images/flutter_plugins.jpg)

Android Studio 安装好flutter插件，执行flutter doctor -v 还是提示了图中错误：

```cmd
✗ Flutter plugin not installed; this adds Flutter specific functionality.
✗ Dart plugin not installed; this adds Dart specific functionality.
```

解决方法:

```cmd
sudo ln -s ~/Library/Application\ Support/Google/AndroidStudio2022.2/plugins ~/Library/Application\ Support/AndroidStudio2022.2
```

结果：
![flutter](/images/flutter_plugins_installed.jpg)
