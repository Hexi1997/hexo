---
title: hexo使用大全
date: 2021-05-12 20:42:55
tags: [hexo,博客相关]
categories: 博客相关
cover: https://patchwiki.biligame.com/images/sssj/1/13/tilswksxlbw1rgrzebqxu2ishz7m677yi
---

## 一、hexo的butterfly主题支持moc3格式的l2d

* 本教程参考了 guoshidu( https://github.com/guoshidu ) 的实现，不过他是在yilia主题的基础上实现的，详见issue: https://github.com/guoshidu/hexo-live2d-moc3/issues/1
* 本教程需要魔改butterfly，本教程基于hexo-theme-butterfly 3.7.6版本，安装方式是通过github安装到themes文件夹下面。而不是通过npm install进行的安装，其注意区别
* 本教程中将butterfly文件夹中的_config.yml的内容拷贝到了主项目根目录的_`_config.butterfly.yml`文件中，hexo会将主目录的`_config.butterfly.yml、_config.yml和butterfly文件夹中的_config.yml`自动进行合并，重复的键优先级主目录>butterfly文件夹，`_config.butterfly.yml`优先级高于`_config.yml`

### （一）引入js

* 修改`_config.butterfly.yml的`配置如下,在head标签中插入要使用的l2d相关js文件

```yaml
# Inject
# Insert the code to head (before '</head>' tag) and the bottom (before '</body>' tag)
# 插入代码到头部 </head> 之前 和 底部 </body> 之前
inject:
  head:
    # - <link rel="stylesheet" href="/xxx.css">
    - <script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script> 
    - <script src="https://cdn.jsdelivr.net/gh/litstronger/live2d-moc3@master/js/frame/live2dcubismcore.min.js"></script>
    - <script src="https://cdnjs.cloudflare.com/ajax/libs/pixi.js/4.6.1/pixi.min.js"></script>
    - <script src="https://cdn.jsdelivr.net/gh/litstronger/live2d-moc3@master/js/live2dcubismframework.js"></script>
    - <script src="https://cdn.jsdelivr.net/gh/litstronger/live2d-moc3@master/js/live2dcubismpixi.js"></script>
    - <script src="https://cdn.jsdelivr.net/gh/litstronger/live2d-moc3@master/js/l2d.js"></script>
  bottom:
    - <div class="aplayer no-destroy" data-id="3083316116" data-server="netease" data-type="playlist" data-fixed="true" data-mini="true" data-listFolded="false" data-order="random" data-preload="none" data-autoplay="true" muted></div>
    # - <script src="xxxx"></script>
```

* 



