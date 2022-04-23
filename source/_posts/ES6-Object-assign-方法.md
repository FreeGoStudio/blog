---
title: ES6 Object.assign()方法
mathjax: true
date: 2022-04-23 20:23:28
categories: 前端
tags: ES6
---

#### 将多个对象的所有枚举属性拷贝到目标对象上
```
var target={a:1,b:2};
var source1={b:3,c:4};
var source2={c:5,d:6};

var newTarget=Object.assign(target,source1,source2);

console.log('target '+target);
console.log('newTarget '+newTarget);
```

输出:
```
target {a:1,b:3,c:5,d:6}
newTarget {a:1,b:3,c:5,d:6}
```


#### 拷贝出新对象, 不修改原对象
```
var target={a:1,b:2};
var newTarget=Object.assign({},target,{c:3});

console.log('target '+ target);
console.log('newTarget '+ newTarget);
```

输出:
```
target {a:1,b:2}

newTarget {a:1,b:2,c:3}
```