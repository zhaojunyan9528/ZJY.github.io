---
title: git常用命令
tags:
  - 前端
categories:
  - - 笔记
    - git
date: 2021-12-17 14:57:40
---

1.克隆项目 

```
git clone url
```

2.创建分支

创建dev分支对应远程dev分支,并切换到dev分支

```
git checkout -b dev origin/dev
```

创建本地分支并切换

```
git checkout -b dev
```

提交本地分支到远程仓库,创建远程分支dev

```
git push origin dev
```

3.切换分支

```
git checkout dev
```

4.提交代码

```
//暂存代码
git add .
//提交暂存代码到本地仓库
git commit -m "commit info"
//将本地仓库代码推送到远程仓库
git push
```

5.删除分支

```
// 首先要切换到别的分支
git branch -d dev
git branch -D dev // 强制删除
git push origin --delete dev // 删除远程分支
```

6.打标签

列出标签

```
git tag
```

创建轻量标签

```
git tag v1.0
```

后期打标签,可以对过去的提交打标签

```
// 先查看提交历史
git log --pretty

7d58683af833a44637800f89f2ee5fbacec436d8 (HEAD -> develop, origin/develop) 🐞 fix(input): 修复input-confirm-type:search时出现搜索图标
aa3b910be51a9e682e5646f108f340d1248d6233 (tag: v1.0.0, origin/build, build) 🐞 fix(招标公告详情): 公告进度样式处理16f0d68366724fcbc7b9121522d71724bdc70251 ✨ feat(注册): 1.新增注册功能 2.我的名称兼容
648e1ef9701ec3bd8f48ec9f9dd9c97468dba3cc ✨ feat(app): 1.去掉耳机页面下拉刷新 2.去掉调试工具
c2a3a051ef35e3babe39ef5127db78d337cc4f4e ✨ feat(设置): 去掉手机号
7acae11b4aab2276efcec9ca54a4e7b100e8cd37 🐞 fix(tabbar页面): 1.修复tabbar页面接口执行2次
b9975fca9ee199dd59b48457d37fd782e9be7721 🐞 fix(工作台): 招标列表详情取招标名称
e3686c0c9892d205a44e1e9379ea546d50f38cb6 🐞 fix(时间格式化): 修复时间戳转换
da2e37df1aabf25af302160db7596b55370bc58f 🐞 fix(资质认证): 认证状态处理
281b56bc78d79782fcd1eb42ae98721cea2ac49c ✨ feat(tabar页面): 添加下拉刷新功能
```

给最新一次提交打上v1.2标签,也就是7d58683af833a44637800f89f2ee5fbacec436d8,需要再命令的末尾指定提交的校验和(或部分校验和):

```
git tag -a v1.2 7d58683af833
```

可以看到打上标签了

```
git tag
v1.1
v1.2
```

默认情况下,git push不会传送标签到远程仓库,创建完标签后必须显示推送到服务器上

```
git push origin v1.2
```

如果想要一次性推送很多标签，也可以使用带有 --tags 选项的 git push 命令。 这将会把所有不在远程仓库服务器上的标签全部传送到那里。

```
git push origin --tags
```

7.删除标签

删除本地仓库的标签,用git tag -d <tagname>

```
git tag -d v1.2
```

上述命令不会从远程仓库删除这个标签,需用git push <remote> :refs/tags/<tagname>来更新远程仓库

```
git push origin :refs/tags/v1.2
```

或者用以下命令删除远程标签

```
git push origin --delete <tagname>
```


8.回退已经提交(git push)到远程仓库的代码

git log找到commit id

```
git log
commit 7d58683af833a44637800f89f2ee5fbacec436d8 (HEAD -> develop, origin/develop)
Author: zhaojunyan <zhao.junyan@hxss.com.cn>
Date:   Fri Dec 3 09:40:22 2021 +0800
...
commit aa3b910be51a9e682e5646f108f340d1248d6233 (tag: v1.0.0, origin/build, build)
Author: zhaojunyan <zhao.junyan@hxss.com.cn>
Date:   Wed Dec 1 15:35:20 2021 +0800
...
```

将代码回退到aa3b910be51a9e682e5646f108f340d1248d6233

```
git reset --hard aa3b910be51a9e682e5646f108f340d1248d6233 // 回退本地提交
git push --force // 本地修改强制推送到远程仓库
```

9.撤销git commit

1.git reset --soft 版本号<commit>

```
git reset --soft HEAD^  //回到上一个版本
```

不删除工作区改动的代码，撤销commit，不撤销git add .

2.git reset --mixed 版本号 

```
git reset --mixed HEAD^  //回到上一个版本
```

不删除工作区改动的代码，撤销commit，撤销git add .

3.git reset --hard 版本号 

```
git reset --hard HEAD^  //回到上一个版本
```

删除工作区的代码，撤销commit，撤销git add . 回到上一次commit的状态

10.仓库b 分支a同步 仓库a 分支a 的代码

1.仓库b添加仓库a连接

```
git remote add 仓库a的项目名 仓库a的git地址
```

2.仓库b 拉取仓库a分支a 的最新代码

```
git pull 仓库a的项目名 branch-a
```

3.仓库b根据仓库a的分支a创建分支a

```
git checkout -b branch-a 仓库a的项目名/branch-a
```

例子：

```
git remote add storeA storeA.git
git pull storeA branch-a
git checkout -b branch-a storeA/branch-a
```

