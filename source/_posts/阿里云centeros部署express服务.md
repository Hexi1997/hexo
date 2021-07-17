---
title: 阿里云centOS部署express服务小结
date: 2021-07-17 10:04:07
type: tags
tags: [大前端, centOS, express]
categories: centOS
---

## centos 基础环境搭建

1、yum安装git，和nodejs版本控制工具n

2、通过n安装最新版本的nodejs（yum安装的版本太老）

3、安装nodejs时会附带安装npm（npm是nodejs官方自带的包管理工具)

4、通过npm全局安装yarn

5、使用npm或者yarn全局安装pm2（全局服务管理工具，防止命令行退出，服务关闭）

## 部署代码并开启服务

1、将express服务端代码push到github仓库上

2、cd /home文件夹，git clone服务端代码

3、进入到项目文件夹根目录，执行yarn安装依赖

4、pm2 start app.js 启动线程，然后就可以通过外网ip加端口号访问了。

## pm2使用

因为node.js 是单进程，进程被杀死后整个服务就跪了，所以需要进程管理工具，但是pm2 远远不止这些。

```text
 npm install pm2 -g     # 命令行安装 pm2 
 pm2 start app.js -i 4 #后台运行pm2，启动4个app.js 
                               # 也可以把'max' 参数传递给 start
                               # 正确的进程数目依赖于Cpu的核心数目
 pm2 start app.js --name my-api # 命名进程
 pm2 list               # 显示所有进程状态
 pm2 monit              # 监视所有进程
 pm2 logs               #  显示所有进程日志
 pm2 stop all           # 停止所有进程
 pm2 restart all        # 重启所有进程
 pm2 reload all         # 0秒停机重载进程 (用于 NETWORKED 进程)
 pm2 stop 0             # 停止指定的进程
 pm2 restart 0          # 重启指定的进程
 pm2 startup            # 产生 init 脚本 保持进程活着
 pm2 web                # 运行健壮的 computer API endpoint (http://localhost:9615)
 pm2 delete 0           # 杀死指定的进程
 pm2 delete all         # 杀死全部进程
```

## node版本管理工具n的使用

安装 node

```bash
n 12
n 12.16.3
n lts  # 最新稳定版
n latest   # 最新发布
```

卸载 node

```bash
n rm 0.9.4 v0.10.0
```

查看&切换

```bash
n
```

