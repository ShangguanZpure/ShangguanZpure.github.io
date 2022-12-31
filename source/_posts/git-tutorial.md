---
title: git-tutorial
date: 2020-04-28 22:14:51
tags:
- 管理工具
categories: 基础工具使用
description: git使用总结（记下来的才是自己的）
photos:
---

git网上有很多经典的教程，比如廖雪峰的[Git教程](https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496)

不过，秉承“只有自己总结的东西才是自己的”观念，还是做了自己的笔记。

学习的顺序应该为：

- 学习廖的课程
- 做自己的笔记，形成自己的知识
- ***再回去好好细读廖的教程，发现一些之前未发现的、未理解的内容***

# git help 的使用

工具，都是熟能生巧。掌握基本方法，多看官方git help 才是正道。

```shell
# help 功能全览
git help
# 查看帮助手册
git help git
# 如果不知道下一步怎么操作
git status #git status会列出当前的状态，并提示接下来的操作！！ 
```

# git的优势

1. 分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！
2. 远程库既可以作为备份，又可以让其他人通过该仓库来协作。
3. 因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

# git线上网页版使用说明

git本地提交之后，线上有个最后审核步骤，有实操后总结在这里。

# git的基本操作

## 安装git

官网下载，一路next。（windows）

## 设置username和email

```shell
# --global是全局参数，对局部仓库也可以单独设置name和email
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

## 使用SSH协议连接本地库与远程库

`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

   ```shell
# 本地的git仓库与远程库使用SSH（secure shell）进行加密传输
# 本地创建SSH Key
	# 查看根目录下是否有.ssh文件，以及.ssh/id_rsa 和.ssh/id_rsa_pub文件是否存在
		#不存在
		ssh -keygen -t rsa -C "youremail@example.com"
# 将公钥放在GitHub上面
   ```

> 为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。
>
> 当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。
>
> 最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。
>
> 摘抄自https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416



***Q:*** 远程库有什么用呢？

***A:*** GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作



## 先有本地库，再有远程库

### 本地仓库初始化

   ```shell
   # 仓库 repository
   # 本地仓库初始化
   git init
   # 查看git配置文件
   ls -ah
   # 或者克隆远程库git clone（TODO）
   ```

### 本地文件新建提交仓库

   ```shell
   # 多次add 一次提交
   git add fileName1
   git add fileName2
   git commit -m "comment content"
   ```

### 提交完毕复核

   ```shell
   # 修改了fileName1
   git status
   # 查看某个文件修改前后差别（已经 add了会回退到未add状态）
   # 脑中要有文件状态转换图（第一幅图）
   # ！！diff 比较的是 add之前和之后文件的差异！！
   git diff fileName1
   git add fileName1
   git commit -m "revise fileName1"
   ```

### 版本回退（游戏存档读档）

   ```shell
   # commit的作用类似于*存档*，当出现意外，可以回到这个存档点重新开始
   # 查看存档点,存档点是一个HEAD指针，HEAD^表示当前存档点的前一个存档点，HEAD^^上上存档点
   # HEAD~100：前100个存档点
   git log
   #简洁版展示
   git log --pretty=oneline
   #显示commit id 
   git relog
   #跳转到指定存档点
     #HEAD指针可以是: 1. HEAD^这种类型的 2.指定的commit id
   git reset --hard HEAD指针
   ```

   > 必须记住的第一张图(**图一**)：文件的状态流转图
   >
   > ![image-20200430231034597](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200430231034597.png)
   >
   > 图中存在四个对象：
   >
   > - 工作区 workspace
   > - 版本库 repository
   >   - 暂存区 stage
   >   - 主分支 master
   >
   > 包含的内容上：{stage} >= {master}

###  详说git diff

   ```shell
   # 根据图一
   # 比较工作区workspace与暂存区stage的区别
   git diff
   # 比较版本库主分支master与工作区workspace的区别
   git diff HEAD -- fileName
   ```

   

### 撤销修改

   第6点说到的版本回退是指stage与master一致下（stage为空），将当前maste版本切换回之前的master版本，工作区同步修改。

   撤销修改指的是撤销一个小的改动，涉及workspace/repository/stage/master之间状态的转移。

   ```shell
#工作区修改，暂存区和主分支没有变化 撤销修改
git status #根据提示敲入下一行命令
git restore fileName #撤销工作区的修改（本质是用版本库覆盖工作区）
#工作区修改，提交到暂存区，主分支没有变化 撤销修改
git status
git restore --unstage fileName #撤销暂存区的修改
git restore fileName #撤销工作区的修改
# 修改已经commit 撤销修改(版本回退)
git reset --hard HEAD^
   ```

   

###  文件删除

```shell
# 新增文件并add、commit
git add test.txt
git commit -m "add test.txt"
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    test.txt
#根据提示
  # 如果本地删除是误操作
git restore test.txt
  # 如果确实删除某个文件
git add test.txt # or
git rm test.txt
git commit -m "delete test.txt"
```

###  建立远程库分支与本地分支连接

```shell
# 1.先有本地库，再有远程库
    # 1.1 与远程仓库建立连接
        # 连接远程（remote）添加git@github.com:ShanggguanZpure/learn.git为远程仓库，并命名为origin
    git remote add origin git@github.com:ShangguanZpure/learngit.git
    # 1.2 将本地文件推送到远程库对应分支
        #第一次提交:关联（-u）本地master分支内容到远程仓库（origin）的master分支,并推送（push）本地master分支内容到远程仓库（origin）的master分支
        git push -u origin master
        #第二次及以上提交：本地推送到远程仓库的master
        git push origin master
        # 推送本地指定分支（branch-name）到远程库(origin)指定分支(origin/branch-name)
        git push origin branch-name origin/branch-name #远程分支可以省略，会找对应分支合并
        
# 2.先有远程库，再有本地库
	# 克隆远程库内容
git clone git@github.com:ShangguanZpure/gitskills.git
```

### 分支管理

#### 什么时候建立分支

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

因为创建、合并和删除分支非常快，所以Git鼓励你**使用分支完成某个任务**，**合并后再删掉分支**，这和直接在`master`分支上工作效果是一样的，但过程更安全。

#### 创建与合并分支

```shell
# 查看分支：
git branch
#创建分支：
git branch <name>
#切换分支：
git checkout <name> #或者
git switch <name>
#创建+切换分支：
git checkout -b <name> #或者
git switch -c <name>
#合并某分支到当前分支：
git merge <name>
#删除分支：
git branch -d <name>
#分支合并默认为fast forward,并且删除分支后，合并记录不保留，使用 --no-ff就会按照普通模式合并，并做一次commit提交，可以保留合并记录。举例如下：
	git switch -c dev
	vi readme.txt
	#修改readme.txt文件进行保存
	git add readme.txt
	git commit -m "merge -no-ff test"
	git switch master
	git merge --no-ff -m "merge with dev" dev
	git branch -d dev
	git log --graph --pretty=oneline --abbrrev-commit
```

#### 解决冲突

```shell
# 产生冲突的原因：两个分支修改了同一个地方，且不一致。
git merge feature1 #产生冲突
vi conflictedFileName
#手动修改不一致地方
git add conflictedFileName
git commit -m "conflict fixed"
#删除分支
git branch -d feature1
#查看分支合并图
$ git log --graph --pretty=oneline --abbrev-commit
```

#### Bug分支：修复紧急Bug操作

暂存（stash）现有分支的工作，切换到Bug分支修复（待测试）

```shell
#当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交.
# 1.存放dev分支工作区和暂存区的内容，包含：暂存区未提交的内容（暂存区与主分支区别），工作区未提交到暂存区的内容（工作区与暂存区的区别）workspace - stage - master
git stash
# 2. 确认在哪个分支上修复（假设为master，当然也可以是其他分支）
git switch master
# 3.从master分支新建Bug分支
git switch -c issue-101
# 4.修复Bug
# 5.提交修改
git add readme.txt
git commit -m "fix bug 101"
# 6.切换到主分支，提交修改
git switch master
git merge --no-ff -m "merged bug fix 101" issue-101
# 7.切换回dev分支继续开发
git switch dev
# 8.查看dev分支状态
git status
git stash list #！！！
# 9.恢复之前暂存的内容
git stash apply #拉取
git stash drop  #删除
	#上面两步合成一步
git stash pop
	#你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash
	git stash apply stash@{0}

# 在master分支上修复的bug，想要合并到当前dev分支，可以用git cherry-pick <commit>命令，把bug提交的修改“复制”到当前分支，避免重复劳动。
git cherry-pick 4c805e2 #！！！
```

#### Feature 分支:开发新功能

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D `强行删除。

#### 多人协作

```shell
# 查看远程库的信息
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
# 推送分支
	#推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上  
	#全格式是： git push <remote> local-branch-name remote/branch-name(可以省略最后一项）
	$ git push origin master
	#推送其他分支，比如dev，就改成
	$ git push origin dev
# 抓取分支
	#要在dev分支上开发，就必须创建远程origin的dev分支到本地
	$ git checkout -b dev origin/dev
# 本地分支提交远程分支产生冲突
	#先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送
	$ git pull
    There is no tracking information for the current branch.
    Please specify which branch you want to merge with.
    See git-pull(1) for details.

        git pull <remote> <branch>

    If you wish to set tracking information for this branch you can do so with:

        git branch --set-upstream-to=origin/<branch> dev
    	#git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：
    $ git branch --set-upstream-to=origin/dev dev
	Branch 'dev' set up to track remote branch 'dev' from 'origin'.
	# 解决冲突后再次push
	$ git push origin dev
```

#### commit log管理

merge操作会产生一些merge commit的提交，意义不明且冗余，应该去除。

主要有以下两种情形：

1. 多人在同一个远程分支Feature上协作，需要多次`git pull`别人的提交到本地合并，在合并完之后，还没push之前，使用`git rebase`可以将分支合并的操作整合成一条直线。

   也可以是

   `git pull --rebase` (等价于`git fetch` + `git rebase`)

2. **背景**：

   本地`master`分支在`commit1`处新建分支`feature`，`feature`分支上有提交一次`feature_commit1`，`master`分支自己独立提交了一次`commit2`

   **动作**：

   现在`feature`分支合并`master分支`的`commit2`，方便做下一步开发

   **具体操作**:

   为了使得合并之后`feature`分支的提交记录`git log --graph --pretty=oneline --abbrev-commit`是一条直线，没有分叉，采用:

   `git rebase master`

3. 本地自己独立的一个分支other（实现特定功能）合并到 dev 分支，分支other多次提交过于琐碎，希望合成一个，使用`git merge --squash other`,`git commit -m "Message"`。

>参考：
>
>1.https://www.liaoxuefeng.com/wiki/896043488029600/1216289527823648
>
>2.https://blog.csdn.net/themagickeyjianan/article/details/80333645?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2

#### 标签 tag

TODO

## 先有远程库，再有本地库

### 本地仓库初始化

```shell
# 根据SSH协议，克隆（clone）版本库git@github.com:ShangguanZpure/gitskills.git到当前目录
git clone git@github.com:ShangguanZpure/gitskills.git
# 当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin
# 从远程库clone时，默认情况下，你的小伙伴只能看到本地的master分支
```

### 多人协作

```shell
# 查看远程库的信息
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
# 推送分支
	#推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上  
	#全格式是： git push <remote> local-branch-name remote/branch-name(可以省略最后一项）
	$ git push origin master
	#推送其他分支，比如dev，就改成
	$ git push origin dev
# 抓取分支
	#要在dev分支上开发，就必须创建远程origin的dev分支到本地
	$ git checkout -b dev origin/dev
# 本地分支提交远程分支产生冲突
	#先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送
	$ git pull
    There is no tracking information for the current branch.
    Please specify which branch you want to merge with.
    See git-pull(1) for details.

        git pull <remote> <branch>

    If you wish to set tracking information for this branch you can do so with:

        git branch --set-upstream-to=origin/<branch> dev
    	#git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：
    $ git branch --set-upstream-to=origin/dev dev
	Branch 'dev' set up to track remote branch 'dev' from 'origin'.
	# 解决冲突后再次push
	$ git push origin dev
```

### 小结

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

## 自定义Git

# git的进阶操作

## 记录一次合并分支探索：

一开始的master分支git log:

```shell
$ git log --graph --pretty=oneline --abbrev-commit
* 45edb69 (HEAD -> master, origin/master, origin/feature, feature)revisereadme.txt
* b2b4a71 add featureFile
* 007eea3 branch master commit
```

新建一个 branchOne 分支，并做两次提交：

```shell
* 5847216 (HEAD -> branchOne) branch one commit second
* 6afe53f branch one commit first
* 45edb69 (origin/master, origin/feature, master, feature) revise readme.txt
* b2b4a71 add featureFile
* 007eea3 branch master commit
```

master分支在提交一次：

```shell
* c0ed012 (HEAD -> master) master commit first
* 45edb69 (origin/master, origin/feature, feature) revise readme.txt
* b2b4a71 add featureFile
* 007eea3 branch master commit、
```

新建 branchTwo 分支，并做一次提交：

```shell
* 838a94c (HEAD -> branchTwo) branch two commit first
* c0ed012 (master) master commit first
* 45edb69 (origin/master, origin/feature, feature) revise readme.txt
* b2b4a71 add featureFile
* 007eea3 branch master commit
```

![image-20200514232200124](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200514232200124.png)

```shell
#master合并branchOne
*   d3a3350 (HEAD -> master) Merge branch 'branchOne'
|\
| * 5847216 (origin/branchOne, branchOne) branch one commit second
| * 6afe53f branch one commit first
* | c0ed012 (origin/master) master commit first
|/
* 45edb69 (origin/feature, feature) revise readme.txt
* b2b4a71 add featureFile
* 007eea3 branch master commit

#紧接着branchTwo 合并主分支做下一步的开发
*   a3f215e (HEAD -> branchTwo) Merge branch 'master' into branchTwo
|\
| *   d3a3350 (master) Merge branch 'branchOne'
| |\
| | * 5847216 (origin/branchOne, branchOne) branch one commit second
| | * 6afe53f branch one commit first
* | | 838a94c (origin/branchTwo) branch two commit first
|/ /
* | c0ed012 (origin/master) master commit first
#branchTwo开发后提交一次commit,合并到主分支master
  #no-ff
*   e867107 (HEAD -> master) Merge branch 'branchTwo'
|\
| * 4474480 (branchTwo) branch two commit second
| *   a3f215e Merge branch 'master' into branchTwo
| |\
| |/
|/|
* |   d3a3350 Merge branch 'branchOne'
|\ \
| * | 5847216 (origin/branchOne, branchOne) branch one commit second
| * | 6afe53f branch one commit first
| | * 838a94c (origin/branchTwo) branch two commit first
| |/
|/|
* | c0ed012 (origin/master) master commit first
|/
* 45edb69 (origin/feature, feature) revise readme.txt
* b2b4a71 add featureFile
* 007eea3 branch master commit
```

![image-20200515002445858](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200515002445858.png)



### 总结：

一开始的分支状态

![image-20200514232200124](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200514232200124.png)

合并的顺序为

```shell
#实现branchTwo是在branchOne开发完成后才从主分支master拉出。
# master 
git merge --no-ff branchOne
*   ea91fc4 (HEAD -> master) Merge branch 'branchOne'
|\
| * 5847216 (origin/branchOne, branchOne) branch one commit second
| * 6afe53f branch one commit first
* | c0ed012 master commit first
|/
* 45edb69 (origin/feature, feature) revise readme.txt
* b2b4a71 add featureFile
* 007eea3 branch master commit
# branchTwo 
git checkout branchTwo
git rebase master
	#合并前
* 838a94c (HEAD -> branchTwo, origin/branchTwo) branch two commit first
* c0ed012 master commit first
* 45edb69 (origin/feature, feature) revise readme.txt
* b2b4a71 add featureFile
* 007eea3 branch master commit
	#合并后
* 29755bf (HEAD -> branchTwo) branch two commit first
*   ea91fc4 (master) Merge branch 'branchOne'
|\
| * 5847216 (origin/branchOne, branchOne) branch one commit second
| * 6afe53f branch one commit first
* | c0ed012 master commit first
|/
* 45edb69 (origin/feature, feature) revise readme.txt
* b2b4a71 add featureFile
* 007eea3 branch master commit
#####
#可以看到，branchTwo从原来的分叉点master commit first 移动到了  Merge branch 'branchOne'
#这样就可以认为branchTwo是在branchOne开发完成后才从主分支master拉出。
######
#主分支 master
git merge --no-ff branchTwo
*   93e5997 (HEAD -> master) Merge branch 'branchTwo'
|\
| * 29755bf (branchTwo) branch two commit first
|/
*   ea91fc4 Merge branch 'branchOne'
|\
| * 5847216 (origin/branchOne, branchOne) branch one commit second
| * 6afe53f branch one commit first
* | c0ed012 master commit first
|/
* 45edb69 (origin/feature, feature) revise readme.txt
* b2b4a71 add featureFile
* 007eea3 branch master commit

```

# git使用的注意事项

# 疑问

1、

```shell
$ git reflog
f7d7681 (HEAD -> master) HEAD@{0}: commit (merge): conflict fixed
8957634 HEAD@{1}: commit: & simple
5f311b8 HEAD@{2}: checkout: moving from feature1 to master
c4d3fa9 (feature1) HEAD@{3}: commit: AND simple
5f311b8 HEAD@{4}: checkout: moving from master to feature1
5f311b8 HEAD@{5}: merge dev: Fast-forward
0da8bf7 (origin/master) HEAD@{6}: checkout: moving from dev to master
5f311b8 HEAD@{7}: commit: branch test
0da8bf7 (origin/master) HEAD@{8}: checkout: moving from master to dev
0da8bf7 (origin/master) HEAD@{9}: commit: delete test.txt
09915bb HEAD@{10}: commit: add test.txt
131ea24 HEAD@{11}: reset: moving to HEAD^
7fe3d31 HEAD@{12}: commit: error content commit
131ea24 HEAD@{13}: commit: git tracks changes
d0950e0 HEAD@{14}: commit: understand how stage works
d576e2b HEAD@{15}: reset: moving to d576e2b
d87032b HEAD@{16}: reset: moving to HEAD^
d576e2b HEAD@{17}: commit: add GPL
d87032b HEAD@{18}: commit: add distribuye
951d21f HEAD@{19}: commit (initial): creat a readme file
# 第二列表示什么呢？
表示具体的指针指向的位置。本地master指向“conflict fixed”。
# 远程的master分支（origin/master）指向哪里呢？看到提交过程中多次出现？按理说，没有push操作，远程库应该没有变动才对。

```







