---
title: 关于husky+lint-staged代码规范
tags:
  - 前端
categories:
  - - 问题&总结
    - vue
date: 2022-09-29 14:00:43
---


## 一.husky

文档：<https://typicode.github.io/husky/#/?id=install>

### 1.安装配置

安装老版本husky4.x:

```shell
npm install husky@4.3.8 lint-staged --dev
```

配置：

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.js": "yarn lint"
  }
}
```

git commit时触发pre-commit钩子，运行lint-staged命令，对*.js执行eslint命令。lint要在scripts提前配置好

husky新版本

1.安装

```shell
npm install husky --save-dev
```

2.使git hooks生效

```shell
npx husky install
```

3.在安装后自动启用 git hooks

```shell
npm set-script prepare "husky install"
```

```json
// package.json
{
  "scripts": {
    "prepare": "husky install"
  }
}
```

也可以手动添加prepare
若是使用yarn 安装则需要将prepare换成postinstall， 见husky文档

4.创建一个precommit hook

```shell
npx husky add .husky/pre-commit "npm run lint" 
# 执行完之后在 .husky 目录下会多一个 pre-commit 的文件，里面的 npm run lint 就是这个 hook 要执行的命令，后续要改也可以直接改这个文件。
git add .husky/pre-commit
```

提交代码

```shell
git commit -m "feat(husky): test"
```

如果npm run lint失败会自动停掉代码提交并给出错误提示信息
lint的是全部文件，校验很慢，如果需要校验暂存文件，需要使用lint-staged

## 二.lint-staged

### 1.安装

```shell
npm install lint-staged --save-dev
```

### 2.配置

```json
{
  "lint-staged": {
    "*.{js,vue}": "npm run lint"
  }
}
```

### 3.设置precommit运行为lint-staged

```shell
npx husky add .husky/pre-commit "npx lint-staged"
```

或者手动修改.husky/pre-commit

## 三.uninstall husky

```shell
npm uninstall husky && git config --unset core.hooksPath
```
