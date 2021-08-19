---
title: git分支开发rebase实践
date: 2021-08-15 12:48:40
tags: [git]
categories: git
---

之前在进行分支开发的时候往往都使用`merge`，提交记录比较混乱。这里介绍一下使用`rebase`进行分支开发的实践。

- 1、公司的项目一般都是一个`main`分支，代表线上环境。一个`develop`分支，是开发主分支。如果要开发新功能，一般是在`develop`分支的基础上切出一个`feature`分支。

```shell
# 切换到develop分支
git checkout develop
# 将develop分支拉取到最新
git pull origin develop
# 基于develop分支创建一个feature/test分支
git checkout -b feature/test
```

- 2、在`feature/test`分支上进行代码开发，形成多次`commit`记录（注：开发过程中不要将`develop`分支合并到`feature/test`分支，开发完毕后再统一处理）

```shell
touch index.js
git add .
git commit -m 'feat: add index.js'

touch index2.js
git add .
git commit -m 'feat: add index2.js'

touch index3.js
git add .
git commit -m 'feat: add index3.js'

touch index4.js
git add .
git commit -m 'feat: add index4.js'

# 开发完毕，先不整理代码格式，直接将代码推送到origin，触发ci/cd自动部署，给提出需求的人测试功能
git push origin feature/test
```

- 3、开发完毕并测试通过以后，首先使用`rebase`整理本地分支代码`commit`记录

```shell
# 使用rebase交互式的编辑最近4次提交记录
git rebase -i HEAD~4
```

弹出一个`git-rebase-todo`的文件，选择要保留的`commit`记录。

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/1629007414029-1.png" alt="1" style="zoom:50%;" />

修改如下，`index1和2`合并为一条`commit`。`index3和4`合并为一条`commit`。后面`commit message`信息先不改，编辑完保存然后关闭当前文件，会进行下一步合并后的`commit`的`message`修改

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/1629007626544-2.png" alt="2" style="zoom:50%;" />

编辑器打开了第一个`COMMIT_EDITMSG`的文件，这是让你编写`index1 和 index2`合并后的`commit`信息

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/1629007658598-3.png" alt="3" style="zoom:50%;" />

编辑完毕后，点击保存

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/1629007813802-4.png" alt="4" style="zoom:50%;" />

关闭当前页面，编辑器弹出了`index3 和 index4`合并`commit`的`message`信息填写界面。同上，编辑保存关闭即可

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/1629007910249-5.png" alt="5" style="zoom:50%;" />

全部编辑完毕后控制台弹出如下信息，代表`rebase`成功

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/1629007928605-6.png" alt="6" style="zoom:50%;" />

执行`git log --graph`可以看待一条直线的记录。很舒服

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/1629008110262-7.png" alt="7" style="zoom:50%;" />

- 4、在`feature/test`分支的开发过程中，`develop`分支不可能一直不变，有些成员可能会直接基于`develop`分支进行开发和`push`。也有别的分支`pull request`合并到`develop`分支。因此需要切换到的`develop`分支，拉取最新代码。然后切换到`feature/test`分支，进行`rebase`

```shell
git checkout develop
git pull origin develop
git checkout feature/test
git rebase develop

# rebase 的过程中如果遇到冲突，首先编辑文件解决冲突，
# 然后git add .
# 最后git rebase --continue即可完成rebase
```

- 5、由于之前为了测试已经把代码推送到`origin`，然后对本地又做了`rebase`。因此执行`git push orign feature/test`会报错。应该执行`git push --force origin feature/test`强制推送（注意：**只有在自己的分支、单人开发的分支上才能使用 --force 强制推送**）

```shell
git push --force origin feature/test
```

- `github`上`pull request`到`develop`分支。代码进行`review`。如果仍然有冲突（`reivew`时间过长，或者刚好有人`push`或者`merge`新的代码到`develop`分支），需要重新执行 4、5 两步。`review`通过以后选择`rebase and merge`合并 pr。
