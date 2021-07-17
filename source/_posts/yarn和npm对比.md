---
title: npm和yarn命令总结
date: 2021-07-17 10:04:07
type: tags
tags: [大前端, 包管理工具]
categories: yarn
---

```text
npm                                     yarn

npm init                                yarn init              // 初始化
npm i | install                         yarn  (install)        // 安装依赖包
npm i x --S | --save                    yarn add  x            // 安装生产依赖并保存包名
npm i x --D | --save-dev                yarn add x -D | --dev  // 安装开发依赖并保存包名
npm un | uninstall  x                   yarn remove            // 删除依赖包
npm i -g | npm -g i x                   yarn global add x      // 全局安装
npm un -g x                             yarn global remove x   // 全局下载
npm run dev                             yarn dev | run dev     // 运行命令
```

优先使用yarn

```text
全局安装卸载
yarn global add xxx
yarn global remove xxx

局部安装卸载
yarn add xxx
yarn add xxx --dev
yarn add xxx -D
yarn remove xxx
yarn remove --dev
yarn remove -D

安装依赖
yarn
yarn install
```

