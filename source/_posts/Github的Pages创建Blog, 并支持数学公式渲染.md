---
title: Github的Pages创建Blog, 并支持数学公式渲染
mathjax: true
date: 2022-04-22 09:17:10
categories: 软件配置
tags: Hexo
---
# github创建Pages的网页静态页面仓库
github仓库名为 `github用户名.github.io`

# github创建blog的仓库
名字随便

# hexo 安装
安装nodejs

安装git

修改npm镜像源为淘宝镜像源
```
npm config set registry https://registry.npm.taobao.org
```

安装hexo
```
npm install -g hexo-cli
```

# hexo 初始化
选择存放blog的位置
```
hexo init blog
```
将代码提交到github的blog的仓库中

进入文件夹, 运行hexo服务
```
hexo server
```

# 安装hexo通过git发布的插件
```
npm install hexo-deployer-git --save
```

修改`_config.yml`文件

```
deploy:
  type: git
  repository: https://github.com/github用户名/github用户名.github.io.git
  branch: main
```

# 安装 NexT 模板
```
npm install hexo-theme-next
git clone https://github.com/next-theme/hexo-theme-next themes/next
```

修改`_config.yml`文件
`theme: next`

# hexo 配置
修改`_config.yml`文件

同步生成文件夹
`post_asset_folder: true`

修正路径（渲染器设置）
```
marked:
  prependRoot: true
  postAsset: false
```

更换渲染器
```
npm un hexo-renderer-marked --save
npm i hexo-renderer-pandoc --save
```

# 配置数学公式
```
npm install hexo-filter-mathjax
hexo clean
```

如果需要使用 MathJax 来加载公式，对应文档的的文件头加入一个选项：`mathjax: true`，可以根据个人需求是否加入位于 `scaffolds/post.md` 文件模板中。

# 生成静态网页和发布
```
hexo g
hexo d
```

# 参考资料
https://hexo.io/docs/

http://home.ustc.edu.cn/~liujunyan/blog/hexo-next-theme-config/