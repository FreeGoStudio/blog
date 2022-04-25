---
title: 关于js字符串拼接与三元表达式的坑
mathjax: true
date: 2022-04-24 01:37:16
categories: 前端
tags: JS
---

在学react的代码的入门例子的时候遇到了以下代码
```
const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
```

当时想当然的认为并不需要括号,如以下代码

```
const status = 'Next player: ' + this.state.xIsNext ? 'X' : 'O';
```

但显示结果会为`X`

但是该代码实际上在类型严格的语言如C#会报错, 因为代码在没有加括号会从左到右来执行, 这样会将`'Next player: ' + this.state.xIsNext`先计算, 因此得到一个string类型, 但强类型语言的string没法转换成bool类型, 因此会编译不通过.

但是js是个弱类型的语言, 因此string类型也能进行true和false判断, string不为null则为true, 反之为false, 因此`'Next player: ' + this.state.xIsNext`先计算, 进行真假判断, 得到结果为true, 所以界面会显示`X`