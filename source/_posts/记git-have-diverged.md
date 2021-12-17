---
title: 记git have diverged
tags:
  - 前端
categories:
  - - 问题&总结
    - git
date: 2021-12-17 13:22:17
---

### 1.问题

git出现Your branch and 'origin/branch' have diverged

如果不需要保留本地的修改:

```
git fetch origin
git reset --hard origin/branch
```

如果需要保留本地的修改:

```
git rebase origin/branch
git pull --rebase
解决冲突
git rebase --continue
git push origin branch
```