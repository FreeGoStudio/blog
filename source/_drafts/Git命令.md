---
title: Git命令
mathjax: true
categories: Git
tags: 代码管理
---

## Git 强制回退到某个历史版本再推送到远程

1. 使用 git log 命令历史版本记录回退版本
```Bash
git reset --hard f6a7c803a6931a9eca011d4e097389e0845cbe49
```

2. 推送到远程
```Bash
git push -f -u origin master
```