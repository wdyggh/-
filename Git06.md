
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

===

回头写在开头的小结：    

* 大概会用到的命令：    
>$ git checkout -b iss53  #新建iss53分支 -b转到iss53分支    
>$ git checkout master    #转到master分支    
>$ git merge hotfix       #在当前分支下，把hotfix分支合并进来     
>$ git branch -d hotfix   #删除hotfix分支     


===

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

- - -

在问题 #53 相关的工作完成之后，可以合并回 master 分支。实际操作同前面合并 hotfix 分支差不多，只需回到 master 分支，运行 git merge 命令指定要合并进来的分支:

```bash
$ git checkout master
$ git merge iss53
Merge made by recursive.
 README |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
```
请注意，这次合并操作的底层实现，并不同于之前 hotfix 的并入方式。因为这次你的开发历史是从更早的地方开始分叉的。由于当前 master 分支所指向的提交对象（C4）并不是 iss53 分支的直接祖先，Git 不得不进行一些额外处理。就此例而言，Git 会用两个分支的末端（C4 和 C5）以及它们的共同祖先（C2）进行一次简单的三方合并计算用红框标出了 Git 用于合并的三个提交对象:

* C2:共同祖先   
* C5:iss53 分支末端    
* C4:master分支末端    

Git 为分支合并自动识别出最佳的同源合并点

![Git 为分支合并自动识别出最佳的同源合并点](http://git-scm.com/figures/18333fig0316-tn.png)

- - -

这次，Git 没有简单地把分支指针右移，而是对三方合并后的结果重新做一个新的快照，并自动创建一个指向它的提交对象（C6）（见图 3-17）。这个提交对象比较特殊，它有两个祖先（C4 和 C5）。

值得一提的是 Git 可以自己裁决哪个共同祖先才是最佳合并基础；这和 CVS 或 Subversion（1.5 以后的版本）不同，它们需要开发者手工指定合并基础。所以此特性让 Git 的合并操作比其他系统都要简单不少。

Git 自动创建了一个包含了合并结果的提交对象

![Git 自动创建了一个包含了合并结果的提交对象](http://docs.pythontab.com/github/gitbook/_images/18333fig0317-tn.png)

既然之前的工作成果已经合并到 master 了，那么 iss53 也就没用了。你可以就此删除它，并在问题追踪系统里关闭该问题。

```bash
$ git branch -d iss53   #删除iss53分支
```

####遇到冲突时的分支合并

有时候合并操作并不会如此顺利。如果在**不同的分支中都修改了同一个文件的同一部分**，Git 就无法干净地把两者合到一起（译注::逻辑上说，这种问题只能**由人来裁决**。）。如果你在解决问题 #53 的过程中修改了 hotfix 中修改的部分，将得到类似下面的结果:

```bash
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

Git 作了合并，但没有提交，它会停下来等你解决冲突。要看看哪些文件在合并时发生冲突，可以用 `git status` 查阅:

``` bash
[master*]$ git status
index.html: needs merge
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#   unmerged:   index.html   #未解决冲突的文件都会以未合并（unmerged）的状态列出
#
```

任何包含未解决冲突的文件都会以未合并（unmerged）的状态列出。Git 会在有冲突的文件里加入标准的冲突解决标记，可以通过它们来手工定位并解决这些冲突。可以看到此文件包含类似下面这样的部分:

```html
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
  please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```

可以看到======= 隔开的**上半部分，是 HEAD（即 master 分支，在运行 merge 命令时所切换到的分支）中的内容，下半部分是在 iss53 分支中的内容**。解决冲突的办法无非是**二者选其一或者由你亲自整合到一起**。比如你可以通过把这段内容替换为下面这样来解决:

```html
<div id="footer">
 please contact us at email.support@github.com
</div>
```

这个解决方案各采纳了两个分支中的一部分内容，而且我还删除了 <<<<<<<，======= 和 >>>>>>> 这些行。在解决了所有文件里的所有冲突后，运行 `git add` 将把它们标记为已解决状态（译注::实际上就是来一次快照保存到暂存区域。）。因为一旦暂存，就表示冲突已经解决。如果你想用一个有图形界面的工具来解决这些问题，不妨运行 `git mergetool`，它会调用一个可视化的合并工具并引导你解决所有冲突:

```bash
$ git mergetool
merge tool candidates: kdiff3 tkdiff xxdiff meld gvimdiff opendiff emerge vimdiff
Merging the files: index.html

Normal merge conflict for 'index.html':
  {local}: modified
  {remote}: modified
Hit return to start merge resolution tool (opendiff):
```

如果不想用**默认的合并工具**（Git 为我默认选择了 *opendiff*，因为我在 Mac 上运行了该命令），你可以在上方”merge tool candidates”里找到可用的合并工具列表，输入你想用的工具名。我们将在第七章讨论怎样改变环境中的默认值。

退出合并工具以后，Git 会询问你合并是否成功。如果回答是，它会为你把相关文件暂存起来，以表明状态为已解决。  

再运行一次 git status 来确认所有冲突都已解决:   

```bash
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#   modified:   index.html
#
```

如果觉得满意了，并且确认所有冲突都已解决，也就是进入了暂存区，就可以用 git commit 来完成这次合并提交。提交的记录差不多是这样:

```bash
Merge branch 'iss53'

Conflicts:
  index.html
#
# It looks like you may be committing a MERGE.
# If this is not correct, please remove the file
# .git/MERGE_HEAD
# and try again.
#
```



[返回目录](https://github.com/wdyggh/note)
