---
title: 记flutter关于Android license status unknown
tags:
  - 前端
categories:
  - - 问题&总结
    - Flutter
date: 2023-05-31 16:43:37
---

运行`flutter doctor -v`显示：

```cmd
✗ Android license status unknown.
      Run `flutter doctor --android-licenses` to accept the SDK licenses.
      See https://flutter.dev/docs/get-started/install/macos#android-setup for
      more details.
```

按照提示运行`flutter doctor --android-licenses`：

![flutter](/images/flutter_license.jpg)

显示java Runtime版本不匹配，查看android sdk命令行工具版本是否和java版本匹配：

查看java版本：

```cmd
java -version

java version "1.8.0_251"
Java(TM) SE Runtime Environment (build 1.8.0_251-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.251-b08, mixed mode)
```

查看android sdk命令行工具版本：

![flutter](/images/flutter_java_version.jpg)

重新选择命令行版本：

![flutter](/images/flutter_line_version.jpg)

然后apply，重新运行`flutter doctor --android-licenses`：

![flutter](/images/flutter_license_done.jpg)

重新运行`flutter doctor -v`:

![flutter](/images/flutter_doctor_done.jpg)