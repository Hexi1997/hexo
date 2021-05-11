---
title: vercel中报错Failed to fetch one or more git submodules
date: 2021-05-11 23:39:19
tags:
---

* 博客使用hexo安装hexo-theme-butterfly的时候，使用git clone将其安装在themes路径下(大概是为了方便修改？但是不知道能不能修改呢？)。
* 在git commit的时候会提示仓库内包含另一个仓库，建议使用git submodules将被包含的仓库添加为子仓库。执行完git submodules，再push到github，然后vercel自动构建后网站就无法打开，打开vercel Dashboard查看Deploy日志，发现报警告`Warning: Failed to fetch one or more git submodules`
* 解决办法是在主项目目录下新建一个.gitmodules文件，里面指定子仓库的安装路径和git地址以及分支
```
[submodule "themes/butterfly"]
          path = themes/butterfly
          url = https://github.com/jerryc127/hexo-theme-butterfly.git
          branch = master
```