---
layout: drafts
title: Git版本分支管理-TBD模式
tags: Git, TBD
mathjax: true
categories: 代码管理
date: 2022-05-08 15:51:07
---


# TBD Flow
Trunk based Development，又叫 **主干开发** ，是一套代码分支管理策略，开发人员之间通过约定向被指定为 **主干** 的分支提交代码，以此抵抗因为长期存在的多分支导致的开发压力。此举可 **避免分支合并的困扰，保证随时拥有可发布的版本** 。“主干”这个词隐喻了树木生长的场景，树木最粗最长的部位是主干，分支从主干分离出来但是长度有限。

{% asset_img TBD_1.jpg TBD Flow %}

使用主干开发后，我们的代码库原则上就只能有一个 Trunk 分支即 master 分支了，所有新功能的提交也都提交到 master 分支上，保证每次提交后 master 分支都是可随时发布的状态。没有了分支的代码隔离，测试和解决冲突都变得简单，持续集成也变得稳定了许多，但也有如下几个问题：

- 如何避免发布引入未完成 Feature，答案是使用 **Feature Toggle** 。在代码库里加一个特性开关来随时打开和关闭新特性是最容易想到的也是最容易被质疑的解决方案。Feature Toggle 是有成本的，不管是在加 Toggle 时的代码设计，还是在移除 Toggle 时的人力成本和风险，都是需要和它带来的价值进行衡量的。
- 如何进行线上 Bug Fix，答案是在发布时打上 Release Tag，一旦发现这个版本有问题，如果此时 master 分支还没有其他提交，那可以直接在 master 分支上 Hot Fix 然后合并至 release 分支；如果 master 分支已经有了提交就需要做以下三件事：
   - 从 Release Tag 创建发布分支。
   - 在 master 上做 Fix Bug 提交。
   - 将 Fix Bug 提交 **Cherry Pick** 到 release 分支。
   - 为 release 分支打上新的 Tag 并做一次发布。

## 优点

- 分支少，与现有工作流程差别不大，易于上手
- 没有长期分离的其他开发分支，合并冲突少
- 利于持续部署和持续交付的支持
- 在不满足现有需求后方便扩展到其他管理策略，如：TBD++ Flow，GitLab Flow

## 版本号管理
### 版本规则
发布release的命名
```
release-主版本号.次版本号
```

如: 
```
release-0.1
```

Release版本需要一个或多个Bugfix，在交付给测试的时候需要将修订号+1

```
主版本号.次版本号.修订号
```

如:
```
0.1.0
0.1.1
```

## 注意事项

- release的bug修复代码，需要与功能迭代的代码分开提交，并及时pick到release上。
- 提交代码需要先拉取再提交
- 确保提交的代码项目能够编译通过与正常运行。

## 扩展
如不满足现有的需求可以灵活扩展成TBD++ Flow

{% asset_img TBD_2.jpg TBD++ Flow %}