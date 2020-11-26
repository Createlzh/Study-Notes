# Git简明教程

[TOC]

___

## 基本操作

### git基本介绍

* Git本地结构：

  ```ascii
   _______
  | 本地库 |：存储每个历史版本信息
  |_______|
  
     /|\   git commit
  	|
   _______
  | 暂存区 |：打算提交但还没有提交，作为临时存储
  |_______|
  
     /|\   git add
  	|
   _______
  | 工作区 |：写代码
  |_______|
  ```

  

* 本地库和远程库的两种交互方式：

  1. 团队内部协作

     ![团队内部协作](https://github.com/Createlzh/Study-Notes/tree/main/pics/local.png)

     

  2. 跨团队协作

     ![跨团队协作](https://github.com/Createlzh/Study-Notes/tree/main/pics/remote.png)



* 托管中心种类
  1. 局域网下：搭建**GitLab**服务器
  2. 外网下：使用**Github**或**Gitee**



____

### 初始化本地仓库

> 在git中的命令与linux一致
>
> * 查看**git**版本：`git --version`
> * git bash中清屏操作：`clear`

* 设置签名：

  * 设置用户名

    ```shell
    git config --global user.name "liaozh"
    ```

  * 设置邮箱

    ```shell
    git config --global user.email "zhihui.liao1@gmail.com"
    ```

* 本地仓库的初始化操作:<font color=red>`git init`</font>

  > 利用`cd`进入相应的目录，或者目录下右键`git bash here`

  ```shell
  $ git init
  Initialized empty Git repository ain D:/GitRepo/.git/
  ```



___

### add和commit

* 将文件提交到暂存区：<font color=red>`git add <file>`</font>

  ```shell
  git add Demo.txt
  ```

  > 可以一次性add多个文件
  >
  > ```shell
  > git add file1.txt file2.txt
  > ```
  >
  > 使用<font color=red>`-i`</font>参数可以以交互方式，一步一步添加文件
  >
  > ```shell
  > git add -i
  > ```
  >
  > 使用<font color=red>`-p`</font>参数是一种更精确的提交方式，允许只提交部分修改的内容
  >
  > ```shell
  > git add -p
  > ```
  >
  > 使用<font color=red>`.`</font>参数直接跟踪所有改动过的文件（需要是tracked文件，untracked文件不算）
  >
  > ```shell
  > git add .
  > ```

* 将暂存区的内容提交到本地库：<font color=red>`git commit [-m "修改注释"]`</font>

  ```shell
  git commit -m "这是我提交的第一个文件Demo.txt"
  ```

* 单独提交暂存区的单个文件到本地库，需要在最后加上文件名：<font color=red>`git commit [-m "修改注释"] <filename>`</font>

  ```shell
  git commit -m "单独提交一个文件Test2.txt" Test2.txt
  ```

* 要修改上一次提交的说明，使用<font color=red>`--amend`</font>参数，文本编辑器会自动打开

  ```shell
  git commit -amend
  ```

* 注意：
  1. 不放在本地库中的文件，git不进行管理
  2. 放在本地仓库中的文件，git也不管理，必须通过`add`和`commit`命令操作才可以提交到本地库



___

### mv

* 可以直接在操作系统中移动文件，git能够自动识别并给这个文件打上`move`标识

* 也可以直接使用<font color=red>`git mv <file> <dir/newfilename>`</font>

  ```shell
  git mv originalfile.txt newsubdir/newfile.txt
  ```



____

### rm

* 使用<font color=red>`git rm <file>`</font>将一个文件删除：

  ```shell
  git rm filetoremove.txt
  ```

* 使用<font color=red>`git rm --cached <file>`</font>来停止跟踪一个文件，但不删除文件：

  ```shell
  git rm --cached untrackedButNotRemoved.txt
  ```



___

## 查看

### status

* <font color=red>`git status`</font>：查看当前目录下文件的状态，新添加的、被修改的、未跟踪的等

  ```shell
  git status
  ```



### diff

* <font color=red>`git diff`</font>：查看当前工作树和暂存区之间的差别

  ```shell
  git diff
  ```

* <font color=red>`git diff --staged`</font>：查看暂存区与最新一次提交之间的区别

* <font color=red>`git diff -cached [<commit>]`</font>：查看暂存区与指定提交版本的不同，版本可缺省（为HEAD）

  > `HEAD`指向当前分支中最新一次提交的指针

* <font color=red>`git diff <commit>`</font>：查看工作区与指定提交版本的不同

  * <font color=red>`git diff HEAD`</font>：查看工作区与当前分支最新一次提交的区别



### log

* <font color=red>`git log`</font>：查看以往仓库中提交的日志，包含`完整索引`+`修改者和修改时间`+`修改注释`

  ```shell
  git log
  ```

* 使用<font color=red>`--reverse`</font>参数来逆向显示所有日志

  ```shell
  git log --reverse
  ```

* 使用<font color=red>`--author`</font>参数来查看指定用户提交的日志

  ```shell
  git log --author=Liaozh
  ```

* 使用<font color=red>`-数字`</font>查看最近指定次数的提交

  ```shell
  git log -3
  ```

* 使用<font color=red>`--since`，`--until`，`--before`，`--after`</font>指定部分时间

  ```shell
  git log --since=yesterday
  git log --oneline --before={3.weeks.ago} --after={2010-04-18} --no-merges
  ```

* 当历史日志过多的时候，查看日志时会有分页的效果

  1. 下一页：`空格`

  2. 上一页：`b`

  3. 到尾页了会显示`END`

  4. 退出：`q`

* 可以使用<font color=red>`git log --graph`</font>命令来查看**分支合并图**

  ```shell
  git log --graph
  ```

* 日志过多时，使用以下命令可以查看简洁的日志

  1. ```shell
     git log --pretty=oneline
     ```

     `完整索引`+`修改注释`

  2. ```shell
     git log --oneline
     ```

     `不完整索引`+`修改注释`

  

### reflog

* <font color=red>`git reflog`</font>：比`git log --oneline`多了信息`HEAD@{数字}`

  > 数字的含义：指针回退到这个历史版本需要走多少步

  ```shell
  git reflog
  ```

  `不完整索引`+`修改注释`+`HEAD@{数字}`

  

### blame

* <font color=red>`git blame <file>`</font>：帮助发现文件中的每一行是在什么时间以及为什么被加上或更改的

  > 以列表方式查看指定文件的提交历史

  ```shell
  git blame demo.txt
  ```

  

___

## 撤销操作

### reset

* 如果想要忽略所有还没有提交的操作，并且返回到最后一次提交后的状态，可以使用<font color=red>`git reset`</font>命令

  ```shell
  git reset [<filename>] //如果只对单个文件操作要加上文件名
  ```

  > 文件的修改还在，但是add过的文件不在暂存区了：即只是把暂存区的文件挪回到了工作区

* 使用<font color=red>`--hard`</font>参数将所有未提交或者未被放入暂存区的文件进行回滚

  > 即回滚暂存区和工作区，文件的修改也不在了

  ```shell
  git reset --hard
  ```

* 可以使用<font color=red>`HEAD^`</font>来向前回退版本

  > `HEAD`后面跟几个`^`就是往回退几个版本，回退100个版本也可以用`HEAD~100`来代替

  ```shell
  git reset --hard HEAD^
  ```

* 也使用索引可以回滚到指定版本

  > 索引可以使用`git reflog`或者`git log`查看

  ```shell
  git reset --hard <索引>
  ```



### hard,mixed,soft参数

* <font color=red>`git reset --hard`</font>：本地库指针移动的同时，重置暂存区，重置工作区

  > 小心这个命令，会把当前仓库下所有未commit过的内容都删掉..

* <font color=red>`git reset --mixed`</font>：本地库指针移动的同时，重置暂存区，工作区不动

* <font color=red>`git reset --soft`</font>：本地库的指针移动时，暂存区和工作区都不动

* 如果在后面加上索引，那么会移动指针同时，也会恢复相应区域的内容



### checkout

* 如果只希望回滚单独某个文件，使用<font color=red>`git checkout -- <file>`</font>

  ```shell
  git checkout -- modifiedfile.txt
  ```

  > `--`代表转义

* 两种简单情况：

  1. `modifiedfile.txt`自修改之后还没有放到暂存区，那么撤销修改就回到和版本库一模一样的状态
  2. `modifiedfile.txt`已经添加到了暂存区，并且又做了修改，撤销修改就回到添加到暂存区之后的状态

  > 总之就是让这个文件回到最近一次`git commit`或`git add`时的状态

* 一个工作区文件在修改之后，`commit`到了暂存区，想要丢弃修改要进行两步：
  1. 首先利用`git reset [<filename>]`将文件从暂存区退回到工作区
  2. 然后利用`git checkout -- <file>`将文件修改撤销
* 一个工作区文件修改之后，还提交到了版本库时，想要撤销就要利用版本回退



### --转义

* `--`代表无论如何，将其之后的argument视为一个文件名`<filename>`

* 如果你要删除的文件名字前带有`-`，就可以执行`git checkout -- -file`



### revert

* 使用<font color=red>`git revert <commit>`</font>来撤销指定提交

  ```shell
  git revert 4552879
  ```

  

___

## 分支

> 参考csdn博客[《实际项目中如何使用Git做分支管理》](https://blog.csdn.net/ShuSheng0007/article/details/80791849)
>
> 教程来自runoob.com的[《Git分支管理》](https://www.runoob.com/git/git-branch.html)



### 创建切换和删除分支

* 列出现有分支<font color=red>`git branch`</font>：

  ```shell
  git branch
  ```

  > 没有任何参数时，`git branch`会列出你在本地的分支

* 创建分支<font color=red>`git branch <branchname>`</font>：

  ```shell
  git branch branch01
  ```

* 切换分支命令<font color=red>`git checkout <branchname>`</font>：

  ```shell
  git checkout branch01
  ```

  * 使用<font color=red>`-b`</font>参数创建新分支并立即切换到该分支下：

    ```shell
    git checkout -b branch02
    ```

* 删除分支<font color=red>`git branch -d <branchname>`</font>：

  ```shell
  git branch -d branch01
  ```



### 合并分支与合并冲突

* 合并任意分支到**当前分支**<font color=red>`git merge <branchname>`</font>：

  ```shell
  git merge branch02
  ```

  > 合并之后就可以把原来的分支`branch02`用`-d`删掉了

* 如果出现了**合并冲突**，无冲突的文件会合并完成，而存在冲突的文件需要手动修改：

  > 期间Git会一直提示`MERGING`，直到第4步完成

  1. 使用`vim <filename>`或者`cat <filename>`查看冲突的文件

     > Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容

  2. 对冲突的文件修改，留下想要留下的部分

  3. 将修改后的冲突的文件`git add`

  4. 进行一次`git commit`



### 待补充内容

> 参见廖雪峰Git教程[《分支管理策略》](https://www.liaoxuefeng.com/wiki/896043488029600/900005860592480)

1. 分支管理策略
2. bug分支
3. Feature分支
4. 多人协作
5. Rebase
6. 标签管理
7. …



___

## 远程仓库Github

