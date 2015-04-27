
### 分支的新建与合并

现在让我看一个简单的分支合并与合并的例子，实际工作中大体也会用到这样的工作流程

1. 开发某个网站
2. 为实现某个新的需求，创建一个分支
3. 在这个分支上开展工作

假设此时，接到电话有个严重的问题要修补，可以按照下面方式处理：
1. 返回到原先已发布到生产服务器上的分支
2. 为这次修补建立一个新的分支，并在其中修复问题
3. 通过测试后，回到生产服务器所在的分支，将修补分支合并进来，然后推送到生产服务器上
4. 切换到之前时迁新需求的分支，继续工作

#### 分支的新建与切换

- - -

首先假设在此项目中工作，并提交了几次更新。

![一个简短的提交历史](http://docs.pythontab.com/github/gitbook/_images/18333fig0310-tn.png)

现在要修补问题追踪系统上的#53问题。这里可以把新建的分支取名为 iss53。要新建并切换到该分支，运行`git checkout` 并加上`-b`参数;

```bash
$ git checkout -b iss53
Switched to a new branch "iss53"
```

这相当于执行下面这两条命令:

```bash
$ git branch iss53    #新建分支
$ git checkout iss53  #转到分支
```

命令的执行结果:

![创建了一个新分支的指针](http://docs.pythontab.com/github/gitbook/_images/18333fig0311-tn.png)

接着你开始尝试修复问题，在提交了若干次更新后，iss53 分支的指针也会随着向前推进，因为它就是当前分支（换句话说，当前的 HEAD 指针正指向 iss53）:

```bash
$ vim index.html
$ git commit -a -m 'added a new footer [issue 53]'
```

![iss53 分支随工作进展向前推进](http://docs.pythontab.com/github/gitbook/_images/18333fig0312-tn.png)

现在你就接到了那个网站问题的紧急电话，需要马上修补。有了 Git ，我们就`不需要同时发布这个补丁`和` iss53` 里作出的修改，也不需要在创建和发布该补丁到服务器之前花费大力气来复原这些修改。**唯一需要的仅仅是切换回 master 分支。**

不过在此之前，留心你的暂存区或者工作目录里，那些还没有提交的修改，它会和你即将检出的分支产生冲突从而阻止 Git 为你切换分支。切换分支的时候最好保持一个清洁的工作区域。稍后会介绍几个绕过这种问题的办法（分别叫做 stashing 和 commit amending）。**目前已经提交了所有的修改，所以接下来可以正常转换到 master 分支:**  

```bash
$ git checkout master
Switched to branch "master"
```

此时工作目录中的**内容和你在解决问题 #53 之前一模一样，**你可以集中精力进行紧急修补。这一点值得牢记::Git 会把工作目录的内容恢复为检出某分支时它所指向的那个提交对象的快照。它会自动添加、删除和修改文件以确保目录的内容和你当时提交时完全一样。

接下来，你得进行紧急修补。我们**创建一个紧急修补分支 hotfix 来开展工作，直到搞定**:

```bash
$ git checkout -b 'hotfix'
Switched to a new branch "hotfix"
$ vim index.html
$ git commit -a -m 'fixed the broken email address'
[hotfix]: created 3a0874c: "fixed the broken email address"
 1 files changed, 0 insertions(+), 1 deletions(-)
```

hotfix 分支是从 master 分支所在点分化出来的   
![hotfix 分支是从 master 分支所在点分化出来的](http://docs.pythontab.com/github/gitbook/_images/18333fig0313-tn.png)

有必要作些测试，确保修补是成功的，然后回到 master 分支并把它合并进来，然后发布到生产服务器。用 `git merge` 命令来进行合并:

```bash
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast forward
 README |    1 -
 1 files changed, 0 insertions(+), 1 deletions(-)
```

##### master \> hotfix 

请注意，合并时出现了“Fast forward”的提示。由于当前 master 分支所在的提交对象是要并入的 hotfix 分支的直接上游，**Git 只需把 master 分支指针直接右移**。换句话说，如果顺着一个分支走下去可以到达另一个分支的话，那么 Git 在合并两者时，**只会简单地把指针右移，因为这种单线的历史分支不存在任何需要解决的分歧**，所以这种合并过程可以称为**快进（Fast forward）。**    
现在最新的修改已经在当前 master 分支所指向的提交对象中了，可以部署到生产服务器上去了。

合并之后，master 分支和 hotfix 分支指向同一位置。    

![合并之后，master 分支和 hotfix 分支指向同一位置。](http://docs.pythontab.com/github/gitbook/_images/18333fig0314-tn.png)

#####回到iss53 继续工作

```bash
$ git checkout iss53
Switched to branch "iss53"
$ vim index.html
$ git commit -a -m 'finished the new footer[issue53]'
[iss53]: created ad82d7a: "finished the new footer [issue 53]"
 1 files changed, 1 insertions(+), 0 deletions(-)
```

下图可以看到 iss53 分支可以不受影响继续推进。

![iss53 分支可以不受影响继续推进。](http://docs.pythontab.com/github/gitbook/_images/18333fig0315-tn.png)

不用担心之前 hotfix 分支的修改内容尚未包含到 iss53 中来。如果确实需要纳入此次修补，可以用 git merge master 把 master 分支合并到 iss53；或者等 iss53 完成之后，再将 iss53 分支中的更新并入 master。

####分支的合并

在问题 #53 相关的工作完成之后，可以合并回 master 分支。实际操作同前面合并 hotfix 分支差不多，只需回到 master 分支，运行 git merge 命令指定要合并进来的分支:

```bash
$ git checkout master
$ git merge iss53
Merge made by recursive.
 README |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
```
请注意，这次合并操作的底层实现，并不同于之前 hotfix 的并入方式。因为这次你的开发历史是从更早的地方开始分叉的。由于当前 master 分支所指向的提交对象（C4）并不是 iss53 分支的直接祖先，Git 不得不进行一些额外处理。就此例而言，Git 会用两个分支的末端（C4 和 C5）以及它们的共同祖先（C2）进行一次简单的三方合并计算用红框标出了 Git 用于合并的三个提交对象:

Git 为分支合并自动识别出最佳的同源合并点

![Git 为分支合并自动识别出最佳的同源合并点](http://git-scm.com/figures/18333fig0316-tn.png)









[返回目录](https://github.com/wdyggh/note)
