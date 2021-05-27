---
title: git知识点大全
date: 2021-05-09 23:11:17
type: tags
tags: git
categories: git
---
## git是什么？

* 分布式
* 版本控制
  * 文件
  * 本地 ： 1个文件 ----> 保留
  * 集中化版本控制（SVN)  -- 本地没有版本控制
  * 分布式版本控制  -- 本地版本控制，版本更新先提交到本地，然后提交到服务器
* 软件，安装在电脑中

## 安装git

安装git只能在本地自己做版本控制，如果要在云端，需要github仓库支持

* 版本控制步骤

  * 进入要管理的文件夹
  * 初始化(git init)
  * 管理
  * 生成版本

* 第一次提交过程

  * git init    //初始化项目目录，生成.git文件夹
  * git status   //检查目录中所有文件的状态
  * git add .    //将上一步标红的（未提交版本控制的文件）文件提交，再次执行git status可以看到上一次标红的文件全部变绿了，代表已经对这些文件进行了版本控制
  * git commit -m 'v1'      //将之前add的文件作为v1版本的内容进行提交
  * git status                     //查看，提示On branch master nothing to commit working tree clean，没有可提交的文件

  

* 修改后第二次提交

  * 这时候如果我对index.html的文件内容进行了修改，执行git status，它会将index.html与v1版本中的进行对比，发现不一样，标红
  * 执行 git add index.html  //添加修改修改后的index.html到暂存区
  * 执行git commit -m 'v2'   //新生成一个版本v2（包含修改过后的index.html)
  * 执行git log 命令查看提交版本记录v1和v2

  

* 第一次执行git commit会提示让你配置用户信息（这个用户只是一个标识，而不是github账户名密码，随便填也可以，最好和github账号一致）

  * git config --global user.email 'your@example.com'
  * git config --global user.name 'your name'
  * 按照步骤走了就可以

## github和码云切换登录

github和码云的登录凭证存储在 控制面板\所有控制面板项\凭据管理器 -------windows凭证

![切换登录](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204019.JPG)



## 安装TortoiseGIT（git的gui界面）

* TortoiseGIT和TortoiseSVN不是一个东西
* SVN和GIT版本控制的区别？
  * GIT分布式，SVN集中式
  * GIT概念多，SVN简单易上手
  * GIT分支廉价，SVN分支昂贵
* 安装包和中文包地址都在：https://tortoisegit.org/download/
* 安装完了以后要**重启电脑**，才能看到红绿图标
* 使用git作为版本控制软件的项目，受控文件是绿色，修改或者新增的文件是红色。和git status颜色显示一致
* 拉取pull 
* 获取fetch
* 使用tortoisegit能够以图形界面方式进行代码clone、pull、push 、merge等等操作

![tortoisegit](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204114.JPG)



* TortoiseGit 克隆项目时会自动设置origin为克隆地址，方便使用
* 码云可以创建发行版，发行版根据tag发行，v1.0   v2.0根据tag发行以后会将该版本的代码打包成一个压缩包，可以下载并部署。
* 码云可以部署master做简单网页预览，在服务--》gitee pages页面中可以部署，部署成功后会生成一个网址可供访问。这个网址不是跟随master分支随时变化的，需要手动更新一下

## git三大区域

* 工作区 
  * 已管理
  * 修改   ---- 红色 
  * 新增  ---- 红色
* 工作区执行git add将数据推向暂存区
* 暂存区  绿色
* 暂存区执行git commit -m 'vname'将数据推向版本库
* 版本库  

![三大区域](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204136.png)





![四大区域](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204140.png)

git pull origin dev 等价于 git fetch origin dev   git merge origin/dev 两句语句

## 回滚

* 先执行git log获取要回滚版本的唯一id

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204143.JPG" alt="git log" style="zoom:50%;" />

* 执行 git reset --hard 版本号
  * git reset --hard 478f849dac78a6a1b0ee17e86f42c81d52e20392 回滚到v1版本



这个时候执行git log只能看到v1版本，那么如果有一天想要恢复v3版本？该怎么做？

* 执行git reflog，可以查看更详细的版本更新记录
* ![git reflog](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204146.JPG)
* 可以查看到v3版本的id，执行 git reset --hard c485caf即可回滚到v3
* 执行git log，可以看到版本v1 v2 v3都出现了，v3是当前的版本



* git checkout index.html，将从版本库中下载最新的index.html，覆盖本地



* v2版本相对v1版本只保留修改的或者新增的部分，不变的文件用指针指向v1



## 分支

![分支原理图](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204151.png)

* git branch查看目前所处的分支 ：输出结果是 主分支master，我们已经在主分支上开发到了v3版本

  ![](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204155.JPG)

* git branch dev，新增一个分支，分支名是dev

* 再次执行查看git branch所有分支，输出结果有一个master和一个dev，master分支高亮显示，表示当前是master分支

  ![git branch dev](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204158.JPG)

* 如果要跳到dev分支开发新功能，执行 git checkout dev 切换到dev分支

  ![git checkout dev切换分支](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204201.JPG)

* 这时可以打开index.html编写dev分支的代码，如果要切换到master分支，必须要将修改后得的代码 git add 和git commit到版本库，否则不能切换，切换会有提示

* 执行git checkout master，跳转到主分支

  ![git checkout master](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204205.JPG)

* 这时如果（线上环境）主分支代码出现bug，需要紧急修改，再新增一个分支，执行git branch bug，分支名是bug

  ![git branch bug](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204208.JPG)

* 切换到bug分支 `git checkout bug`在bug分支中修改代码，解决bug

* 解决完bug以后在bug分支执行 git add index.html   git commit -m 'resolve bug'，将解决后的代码保存到版本库，注意，此时代码仍然处于bug分支中，而不是主分支master中，要想解决线上bug(master分支)，需要将bug分支合并到master分支中

* 切换到master分支(git checkout master) ，执行git merge bug，将bug分支与主分支master合并，合并完成后bug分支已经没用了，需要执行 `git branch -d bug`删除该分支

  ![git merge bug](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204210.JPG)



* bug解决完毕以后，切换到dev分支继续开发新功能，新功能开发完毕以后 执行 git add和git commit添加到版本库，然后切换到master分支执行git merge dev进行合并

  ![git merge dev](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204214.JPG)

* 合并时由于bug分支和dev分支修改了同一处代码，导致冲突，需要打开该文件，手动修改冲突

  ![手动解决冲突](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204217.png)

  * 修改完后，执行`git add index.html`    `git commit -m 'V final'`，将修改后的文件推送到版本库，此时已经解决冲突并且将dev分支成功合并到master分支

* 总结：以后开发新功能，必须新建一个分支开发。master分支一般就是项目上线环境，出bug的话就在master上新建一个分支进行修改，修改后合并到主分支中

## git工作流

![git简单工作流](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204221.JPG)

master相当于上线版本，只放开发测试好的稳定功能

dev分支上面写代码，开发新功能，功能通过测试后合并到master分支上



## 基于github做代码托管

### 上班第一天早上（家里）

注：2019年开始github私有仓库也不收费了。

![github创建仓库](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204225.png)



创建好仓库以后，获得仓库的地址 https://github.com/Hexi1997/dbhot.git 

执行 

`git remote add origin https://github.com/Hexi1997/dbhot.git `  为这个仓库地址起一个别名，叫origin

执行`git push -u origin master`  将master分支推送到 github 仓库。如果是第一次推送，会让你登录github账号，登录即可



github现在默认分支是main，可能会造成一些问题，就是你更新了但是不能第一时间看到，需要手动切换到master分支才能看到

![github man](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204232.JPG)



这里需要设置一次default branch为master，舒服了

![github default branch](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204234.JPG)

以后推送都可以看到



执行 `git push -u orgin dev`将dev分支推送到远端



### 上班第一天白天（公司）

现在在家里将代码推送到服务器端，如果我上班了，换一台公司空白电脑，需要执行 `git clone https://github.com/Hexi1997/dbhot.git` 初次获取仓库代码，继续编写，在clone的文件夹中执行git branch发现只有master分支，其实不是这样的，dev分支也被clone下来了。可以执行 `git checkout dev`切换到dev分支



在公司里，要继续开发肯定是在dev分支上继续开发，之前在家里将dev分支merge到master分支，master分支是最新的代码，肯定是在最新的代码进行开发。需要将dev分支与master分支保持一致，在dev分支下执行 `git merge master`，继续在dev分支中开发即可




### 上班第一天下班前（公司）

公司下班时候，如果dev分支功能没有开发完毕，首先需要git add和git commit，将代码推送到本地版本库，然后需要代码推送到远程仓库dev分支中 `git push -u origin dev`

![git第一天工作](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204238.JPG)

### 上班第一天下班后（家里）

继续在dev分支中开发，需要拉取dev中最新代码。执行  `git pull origin dev`即可

![git第一天傍晚在家里](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204240.JPG)

加班加点写代码，写完以后和下班前执行一样步骤 git add git commit git push，睡觉

### 上班第二天（公司）

继续在dev中开发，这次不能直接git clone，应该使用 `git pull origin dev`来拉取最新代码，然后再继续开发，下班前提交，如此往复



* https://www.cdnfine.com/a/news/mtbd/1229.html 解决github访问速度慢、pull push clone速度慢的一种解决办法。
* GitHub在国内访问速度慢的问题原因有很多，但最直接和最主要的原因是GitHub的分发加速网络的域名遭到dns污染 。为了解决这个问题可以通过ipaddress.com查询github.com的CDN节点ip，添加到host文件中。但是CDN节点ip是会变化的，有时候访问github慢可能是**CDN节点变化**了，需要重新按照上面链接的步骤执行

### 开发完毕要上线

* 切换到master分支，执行`git merge dev`  ,  执行 `git push origin master`将最新的master分支推送到远程。
* 切换到dev分支，执行`git merge master git push origin dev`



## 突发事件

### 上班第三天下班前（公司）

白天在dev分支上继续开发新功能（index.html）中，只进行到50%。由于有**急事**，临走前只进行了git add和git commit，没有推送到远程仓库。

### 上班第三天下班后（家里）

事情解决后回到家中想继续开发新功能，执行git pull origin dev，发现没有白天写的代码。于是他进行了两步操作

	* 凭借记忆在index.html中继续开发新功能 
	* 新增一个another.html写另一个功能 

睡觉前他将所有新写的代码执行 git add / git commit , 然后执行`git push origin dev`将家中的代码推送到远程仓库

### 上班第四天白天（公司）

回到公司开始写代码之前第一步时拉取代码`git pull origin dev`，此时 新增的功能(another.html)和不冲突的代码会进行自动合并。但是index.html代码是冲突的，会提示conflict，需要手动编辑。

![github 忘记push导致conflict](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204247.JPG)

![忘记push导致手动修改冲突对比](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204250.png)

修改冲突后，继续在dev中开发，下班前执行git add / git commit , 最后执行git push origin dev 推送到远程仓库



### 上班第四天下班后（家里）

要继续开发，执行`git pull origin dev`获取最新代码即可



## rebase应用场景（面试常问)



* rebase翻译过来就是变基
* rebase的作用是让代码的提交记录变得简洁
* **注意事项**：
  * 如果这条commit记录已经push到远程仓库了，最好不要将这条commit记录执行rebase，否则本地和远程版本库不一致，会很麻烦

### 场景一

如果某个大功能开发周期长，在dev分支提交commit了很多次，有很多记录。可以在push前通过rebase将多个提交记录整合成一个记录。

```
新建一个rebase文件夹测试
mkdir rebase
进入rebase文件夹
cd rebase
git版本控制初始化
git init

接下来重复操作创建多次commit记录

默认是在master分支，执行touch 1.py创建一个1.py文件
touch 1.py
将1.py 添加到暂存区
git add 1.py
将1.py提交到版本库
git commit -m 'v1'

touch 2.py
git add 2.py
git commit -m 'v2'

touch 3.py
git add 3.py
git commit -m 'v3'

touch 4.py
git add 4.py
git commit -m 'v4'

以图形形式查看分支
git log --graph
可以看到有四次提交记录
v4 834f67c348ddf9cb276d4a67c315edf6d18ece38  (HEAD --> master)
v3 97fcfe262c74659259e477b37b76876226a911a1
v2 bd04b1001cdfddcc8679d327d34360af1f24420c
v1 700d7160efbf757fa0fef3b245a6786087c9a400

在要合并记录的分支执行 git rebase i [startpoint] [endpoint],其中i的意思是--interactive，即弹出交互式的界面让用户编辑完成合并操作，[startpoint] [endpoint]则指定了一个编辑区间，如果不指定[endpoint]，则该区间的终点默认是当前分支HEAD所指向的commit(注：该区间指定的是一个前开后闭的区间)。

由于前开后闭，这里实际上是将v2-v4提交记录合并，等价于 git rebase -i HEAD~3，代表从HEAD(V4)开始的最近3条提交记录
git rebase -i 700d7160efbf757fa0fef3b245a6786087c9a400
会弹出如下的界面
```

![rebase_vim1](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204300.JPG)

这个界面是vim编辑界面，让v2-v4之间如何合并

git 为我们提供了以下几个命令:

> - pick：保留该commit（缩写:p）
> - reword：保留该commit，但我需要修改该commit的注释（缩写:r）
> - edit：保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e）
> - squash：将该commit和前一个commit合并（缩写:s）
> - fixup：将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f）
> - exec：执行shell命令（缩写:x）
> - drop：我要丢弃该commit（缩写:d）



 根据我们的需求，我们将commit内容编辑如下（注意：

**vim进入编辑是按键盘小写 i**

vim编辑完后 按**esc 后按住 shift和: 输入 wq**表示保存并退出 。 



然后弹出注释修改界面:

![rebase_vim1_3](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204304.JPG)

编辑完保存即可完成commit的合并了，执行git log可以查看最新的commit记录

![rebase_success](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204306.JPG)

是不是简洁了许多



### 场景二

将dev分支合并到主分支master

![rebase_2](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204309.png)

```
创建dev分支
git branch dev
切换到dev分支
git checkout dev
在dev分支中新建一个文件
touch dev1.py
添加到暂存区
git add dev1.py
提交到版本库
git commit -m 'dev v1'

切换到master分支
git checkout master
添加一个文件
touch master.py
git add master.py
git commit -m 'master funciton'
```

#### 使用merge方式

如果不用rebase，使用merge那么就是在master分支执行 `git merge dev`  ,  由于merge后会产生新版本，所以执行merge时会弹出vim框让你写新版本的注释，按照前面一样的编辑vim方法输入注释保存即可

![rebase_merge_dev_to_master](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204313.JPG)

执行git log --graph可以看到最新版本树，实际上使用merge仍然是两个分支

![git log --graph2](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204315.JPG)

git log --graph --pretty=format:"%h %s"

![git merge 小树](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204319.JPG)



#### 使用rebase方式

```
由于之前dev分支的commit记录已经被merge。再在dev分支创建一个commit记录
git checkout dev
touch dev2.py
git add dev2.py
git commit -m 'dev v2'

给master分支新增一条commit记录
git checkout master
touch master111.py
git add master111.py
git commit -m 'master111'

切换到dev分支执行git rebase master,将master分支中的内容放到dev分支中（应该指的是master中新增的commit记录）
git checkout dev
git rebase master

切换到master分支，把dev分支merge到master分支中
git checkout master
git merge dev
```

这样就rebase完毕，执行 git log --graph可以看到最新的提交版本树状图，通过上面一段和下面一段的对比可以很明显的看到merge和rebase的区别，merge会分叉，rebase不会分叉

![git rebase final](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510204327.JPG)





### 场景三

就是对应上面的突发事件：前一天晚上在公司忘了push，回家继续改然后push到远程仓库。第二天到公司先执行git pull 拉取代码，这时会进行merge操作。产生新的分支。如何解决这个问题呢？用rebase

前面说了 

​	git pull origin dev                 远程仓库 -->工作区

​    等价于 

​	git fetch origin dev             远程仓库 -->  版本库

   git merge origin/dev            版本库  -->  工作区

两句语句。所以在第二天工作的时候不用git pull拉取代码，而用

   git fetch origin dev 

   git checkout dev 

   git rebase origin/dev            origin/dev代表的是远程仓库的dev分支，将远程仓库的dev分支rebase到当前dev分支，即可获取最新代码。不会产生新的分支



* 执行git rebase产生冲突的时候？
  * 手动解决冲突
  * git add ....
  * git rebase --continue
* 代码举例

还是继续用场景二的工程

```shell
dev中修改1.py的值并commit
git checkout dev
使用vim修改1.py的内容
vim 1.py
git add 1.py
git commit -m 'dev中修改1.py'

master中修改1.py的值并commit
git checkout master
使用vim修改1.py的内容
vim 1.py
git add 1.py
git commit -m 'master中修改1.py'

切换到dev执行rebase
git checkout dev
git rebase master
这时会提示冲突，使用vim手动解决冲突
vim 1.py
解决冲突后执行git add
git add 1.py
继续rebase
git rebase --continue

切换到master分支执行merge
git checkout master
git merge dev

这样才是一个完整的解决rebase冲突的流程
```



## beyond compare快速解决冲突

安装beyond compare，简称bc



给git配置bc4作为对比工具：可参考 https://segmentfault.com/a/1190000019606428?utm_source=tag-newest



应用beyond compare解决冲突

`git mergetool `，会自动弹出bc4的对比界面，对比保存即可

![git mergetool](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510215042.png)

解决冲突后仍然处于merging状态，那是因为没有git add和git commit



## 多人协同开发

### gitflow工作流

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510213826.png" alt="gitflow" style="zoom:50%;" />

项目经理也可以理解成小组长

这是一个正规的流程图，实际工作过程中往往不那么规范

在小公司，release阶段可能没有。

​					code review可能没有

### 实际案例

从头开始，新建一个项目，初始化代码和，在master提交一次到版本库。

```shell
git init 
touch index.html
手动或者使用vim编辑内容
git add index.html
git commit -m '第一次提交，新增index.html'
```

接下来要新建远程仓库地址来推送 ，有两种方式

#### 第一种 

![dbhot2](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510213850.png)

```shell
然后执行如下代码推送main分支到远程仓库
设置当前文件夹内origin代表这个地址，相当于--local
git remote add origin https://github.com/Hexi1997/dbhot2.git
将主分支的命名由master改为main(原因：master涉嫌歧视)
git branch -M main
将main分支推送到远程仓库
git push -u origin main
```

推送成功以后，如果想让别人合作开发，需要如下图去邀请github账户，那么对方就是收到一条邮件，点击确认即可加入团队。

![toolong](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214240.JPG)

对方邮箱收到邀请

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214409.JPG" alt="nowrap" style="zoom:50%;" />

点击接受邀请即可

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214446.JPG" alt="invite2" style="zoom:50%;" />

#### 第二种

第一种方式不适用于公司，只适用于个人开发或者小团队开发

公司会先创建一个组织（类似部门），在组织里面再创建项目，让好多人都属于共同一个组织。协同开发

![new origization](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214458.JPG)

填写组织名 邮箱 组织属于

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214520.JPG" alt="创建组织表单填写" style="zoom:50%;" />



邀请用户，暂时可不邀请，之后再邀请

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214541.JPG" alt="创建组织表单填写2" style="zoom:50%;" />



组织创建成功

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214552.JPG" alt="组织创建成功" style="zoom:50%;" />

在组织内新建一个仓库dbhot3(步骤同上面一样)

推送代码到远程仓库

```she
然后执行如下代码推送main分支到远程仓库
设置当前文件夹内origin代表这个地址，相当于--local
git remote add origin https://github.com/hexi1990/dbhot3.git
将主分支的命名由master改为main(原因：master涉嫌歧视)
git branch -M main
将main分支推送到远程仓库
git push -u origin main
```



github还有一个概念是team，一个team内有多个成员，属于一个团队。

team和项目都属于organization。team和项目是多对多的关系



题外话，现在区分版本号要么是用一长串的id，要么是用更新描述来区分，很不方便

所以引入tag概念，tag的命名一般是1.0.1之类



git tag是命令 1.0.1是版本号 -m后面是注释
**打标签的操作发生在我们commit修改到本地仓库之后，push之前**

```shell
git tag -a 1.0.1 -m '第一次提交'
```

推送标签到远程服务器

```she
推送所有tag到远程服务器，普通的push不会推送tag，tag要单独推送git push origin --tags
```

删除标签

```she
删除本地标签git tag -d 1.0.1删除远程仓库标签git push origin:refs/tags/1.0.1
```

推送标签后再git中可以看到

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214559.JPG" alt="tags" style="zoom:50%;" />



现在完成了初始版本v1的内容推送。开始进行多人协同开发

##### 第一步：明确关系

当前工程项目的创建者是 hexi1997，他拥有最高的权限

项目经理(hexi1997)   -- 负责项目任务分配 代码检查code review

程序员(hexixi2021)  -- 对应gitflow工作流图中 小弟A ，开发新功能

测试人员：由于没有那么多git账号，这里就用项目经理账号hexi1997进行测试，也是可以的

##### 第二步：邀请成员

只需要邀请hexixi2021即可，hexixi2021同意即可加入



目前项目分支只有一个master，开发都是基于dev分支进行的，所以需要项目经理（hexi1997)创建一个dev分支并且推送到远程仓库

```shell
创建dev分支并且切换到dev分支git checkout -b dev推送dev分支到远程git push origin dev
```



##### 第三步：新成员在新分支开发新功能

hexixi2021用户入职第一天执行的操作

```she
第一次需要clone仓库，将master分支和dev分支全部clone下来git clone https://github.com/hexi1990/dbhot3.gitcd dbhot3切换到dev分支git checkout dev在dev分支下新增一个分支（zjh)git checkout -b zjh新增一个zjh.html文件，用于开发功能Atouch zjh.htmlvim zjh.htmlgit add zjh.htmlgit commit -m 'zjh功能开发进度50%'执行git commit以后远程仓库中同步了一个分支zjh，回家
```

hexixi2021回家晚上继续开发

```shell
回家，首先拉取代码git pull origin dev继续编辑zjh.html，开发zjh功能vim zjh.html开发完成后推送到远程git add zjh.htmlgit commit -m 'zhj功能开发进度100%'git push origin zjh
```

睡觉，第二天早上起来到公司上班，昨晚已经加班加点把zjh功能开发完毕，现在需要将zjh分支的代码合并到dev分支。因此找项目经理做code review，



##### 第四步：code review

项目经理（小组长 hexi1997）如果想做code review，需要在github中先对dev分支进行规则设置（设置一次即可）

![code_review_1](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214609.png)



设置完成之后，hexixi2021需要到自己的github账户上去pull request

![code_review_2](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214623.png)

![code_review_3](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214658.JPG)

![code_review_4](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214711.JPG)

如此hexixi2021就已经提交code review请求完毕



项目经理 hexi1997登录github查看pull request可以看到刚刚发起的request，点击add your review执行code review



![code_review_5](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214819.JPG)

左边会罗列出zjh分支到master分支中的所有改变，如果全部确认无误后，点击 viewed代表已经做了code review

![code_review_6](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214827.JPG)

执行merge pull request命令，在远程仓库将zjh分支和合并到dev分支

![code_review_7](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214838.JPG)

如此，可以看到显示Merged，同时该request也会被关闭，注意下面有一个Delete branch分支，因为分支zjh已经开发完毕并且合并到dev分支，因此可以删除该分支，也可以不删

![code_review_8](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214844.JPG)

可以看到dev分支中已经包含zjh.html

![code_review_9](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214851.JPG)



项目经理在dev分支中执行	`git pull origin dev`     即可将远程仓库合并的代码更新到本地。



##### 第五步：代码测试 、预发布(release)

项目经理hexi1997，操作生成一个release分支，并push到远程仓库，让测试员可以pull 并且进行测试，修改小bug（不能添加新功能）

```shell
切换到dev分支（重要）git checkout dev在dev分支中生成新分支release，专门用来做测试git checkout -b release将release分支推送到远程仓库，供测试人员测试git push origin release
```

此时可以再安排一个人进行release分支代码测试和小bug修复，但是github账号有限，直接让项目经理（hexi1997 小组长)进行代码测试

```shell
切换到release分支git checkout release拉取最新代码git pull origin release
```

在测试过程中如果代码没问题，没对release分支做修改，只需要将release分支合并到master分支上线即可。

在测试过程中如果代码有问题，release分支做了修改，需要将release分支合并master分支，同时也需要将release分支合并到dev分支。merge的操作通过github或者git命令都可以进行。

合并完成后需要将release分支删除

```shell
发现zjh.html有bug，解决该bugvim zjh.htmlgit add zjh.htmlgit commit -m 'release阶段发现zjh小Bug,已解决'切换到主分支git checkout main 将release分支合并到main分支git merge release打版本taggit tag -a v1.0.2 -m '新增扎金花功能'推送所有tag到远程服务器，普通的push不会推送tag，tag要单独推送git push origin --tags将main分支推送到远程仓库并且会被标记为v1.0.2git push origin main切换到dev分支git checkout dev将release分支合并到dev分支git merge release将dev分支push到远程仓库git push origin dev删除release分支git branch -d release
```

## 给开源项目贡献代码

* 第一步：github上fork项目，别人的代码就进入了我的远程仓库
* 第二步：在本地git clone
* 修改代码，并git add / git commit 
* 将修改的版本推送到远程仓库（git push)
* 在github上pull request，发起merge请求

## 配置文件存放的三个位置

```
配置信息存储在.git/config文件中，项目配置文件git config --local user.name ''git config --local user.email ''全局配置文件，存储在当前用户的home目录下gitconfig中（所有git项目目录通用）git config --global user.name ''git config --global user.email ''系统配置文件 /etc/.gitconfig ,需要有root权限git config --system user.name ''git config --system user.email ''
```

优先级 项目>全局>系统

## git免密登录

* 修改url

```shell
原来的地址 https://github.com/hexi1997/dbhot3.git修改的地址 https://用户名:密码@github.com/hexi1997/dbhot3.gitvim ./git/config修改配置
```

* ssh实现（企业中使用较多）

* git凭证管理（目前一般都是这样的）

## gitignore忽略文件

让git不在管理指定文件/文件夹

```shell
vim .gitignore   创建（如果没有）并编辑.gitignorea.py  排除a.pyb.py  排除b.py*.h   排除.h后缀名!a.h  a.h仍然被管理files  排除files文件夹*.py[c|a|d]  排除.pyc .pya .pyd后缀名文件
```

一般可以github上搜索gitignore，根据语言选择即可

https://github.com/github/gitignore



## 任务管理相关

* issues-

  * bug管理

  ![issues](https://gitee.com/chen_hexi/image-source/raw/master/img/20210510214902.JPG)

* wiki

  * 项目文档说明

## github创建静态网页

略

## 码云

和github使用基本一致

 不过码云个人私有仓库最多支持 5 人协作（如个人拥有多个私有仓库，所有协作人数总计不得超过 5 人） 

github访问速度慢

码云部署静态服务很方便，点击服务-->git pages即可，例如 http://chen_hexi.gitee.io/dbhot/

## 删除分支命令

### 删除远程

```shell
git push origin -d nav
```

### 删除本地

```shell
git branch -d nav
```

## 补充知识
* 使用source tree图形化管理git分支
* 注意git reset --hard的使用，会**覆盖**本地未commit的代码
* git submodules 子模块使用 https://zhuanlan.zhihu.com/p/97761640
子module通过设置.gitmodules来配置

* 使用ssh，https://juejin.cn/post/6844904071757824007，ssh的clone和push成功率更高，https方式由于DNS污染成功率很低

