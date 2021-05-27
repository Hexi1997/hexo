---
title: nodejs+serverless开发自己的短网址服务
date: 2021-05-14 21:51:11
tags: [大前端,serverless]
categories: serverless

---

## Serverless简介

### 三个阶段

#### 物理机房

硬件软件、数据库都需要自己做

#### 云服务器

只需要关心软件和数据库，不需要担心硬件

#### Serverless

只需要写代码（专注业务本身）

### 优势

* 不需要考虑物理机/虚拟机，结合工作流，代码自动提交部署
* 没有服务器，维护成本大大降低，安全稳定性更高
* 都是弹性伸缩云，不用担心性能问题
* 大多数Serverless服务商都是按照使用情况计费

## 功能简要概述

 <img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210514215801.png" alt="image-20210514215748740" style="zoom:50%;" />

使用腾讯serverless和云函数

