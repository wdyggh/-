
### Git 分支

**几乎每一种版本控制系统都以某种形式支持分支。**使用分支意味着你可以从开发主线上分离开来，然后在不影响主线的同时继续工作。**有人把 Git 的分支模型称为“必杀技特性”，**而正是因为它，将 Git 从版本控制系统家族里区分出来。Git 有何特别之处呢？Git 的分支可谓是难以置信的**轻量级**，它的新建操作几乎可以在瞬间完成，并且在不同分支间切换起来也差不多一样快。和许多其他版本控制系统不同，**Git 鼓励在工作流程中频繁使用分支与合并**，哪怕一天之内进行许多次都没有关系。理解分支的概念并熟练运用后，你才会意识到为什么 Git 是一个如此强大而独特的工具，并从此真正改变你的开发方式。

*我也暂时还没理解，前几章我也没好好看，这章开始要好好看了。*

#### 何谓分支

- - -

Git 是如何储存数据的。Git 保存的**不是文件差异或者变化量，而只是一系列文件快照。**   
Git提交时，会保存一个提交(commit)对象，该对象包含一个指向暂存内容快照的指针，包含了本次提交的作者等相关附属信息，包含零个或多个指向该提交对象的父对象指针：首次提交是没有直接祖先的，普通提交有一个祖先，由两个或多个分支合并产生的提交则有多个祖先。   

假设在工作目录中有三个文件，将它们暂存后提交。暂存操作会对每一个文件计算校验和（即第一章中提到的 SHA-1 哈希字串），然后把当前版本的文件快照保存到 Git 仓库中（Git 使用 blob 类型的对象存储这些快照），并将校验和加入暂存区域:


```bash
$ git add README test.rb LICENSE
$ git commit -m 'initial commit of my project'
```

> 1. git commit 新建一个提交对象前，Git 会先计算每一个子目录（本例中就是项目根目录）的校验和   
> 2. 在 Git 仓库中将这些目录保存为树（tree）对象
> 3. Git 创建的提交对象，除了包含相关提交信息以外，还包含着指向这个树对象（项目根目录）的指针,在将来需要的时候，重现此次快照的内容了。


下图中  
绿色框内 记录着目录树内容及其中各个文件对应 blob 对象索引的 tree 对象。    
紫色框内 指向 tree 对象（根目录）的索引和其他提交信息元数据的 commit 对象。     
红色框内 三个表示文件快照内容的 blob 对象。    

![单个提交对象在仓库中的数据结构](http://docs.pythontab.com/github/gitbook/_images/18333fig0301-tn.png)     

作些修改后再次提交，那么这次的提交对象会包含一个指向上次提交对象的指针（译注：即下图中的 parent 对象）。两次提交后，仓库历史会变成下图的样子:

![多个提交对象之间的链接关系](http://docs.pythontab.com/github/gitbook/_images/18333fig0302-tn.png)

现在来谈分支。Git 中的分支，其实**本质上仅仅是个指向 commit 对象的可变指针。**Git 会使用 **master** 作为分支的默认名字。在若干次提交后，你其实已经有了一个指向最后一次提交对象的 master 分支，它在每次提交的时候都会自动向前移动。

![ 分支其实就是从某个提交对象往回看的历史](http://docs.pythontab.com/github/gitbook/_images/18333fig0303-tn.png)

那么，Git 又是如何创建一个新的分支的呢？答案很简单，创建一个新的分支指针。比如新建一个 testing 分支，可以使用 `git branch` 命令:

```bash
$ git branch testing
```

这会在当前 commit 对象上新建一个分支指针。如下图：

![多个分支指向提交数据的历史](http://docs.pythontab.com/github/gitbook/_images/18333fig0304-tn.png)

那么，Git 是如何知道你当前在哪个分支上工作的呢？其实答案也很简单，它保存着一个名为 `HEAD` 的特别指针。请注意它和你熟知的许多其他版本控制系统（比如 Subversion 或 CVS）里的 HEAD 概念大不相同。在 Git 中，**它是一个指向你正在工作中的本地分支的指针（译注：将 HEAD 想象为当前分支的别名）**。运行 `git branch` 命令，**仅仅是建立了一个新的分支，但不会自动切换到这个分支中去，所以在这个例子中，我们依然还在 master 分支里工作。**

![HEAD 指向当前所在的分支](http://docs.pythontab.com/github/gitbook/_images/18333fig0305-tn.png)

要切换到其他分支，可以执行 `git checkout` 命令。我们现在转换到新建的 testing 分支:

```bash
$ git checkout testing  #这样就能切换到 testing 分支
```

这样 HEAD 就指向了 testing 分支

![HEAD 在你转换分支时指向新的分支](http://docs.pythontab.com/github/gitbook/_images/18333fig0306-tn.png)

这样的实现方式会给我们带来什么好处呢？好吧，现在不妨再提交一次:

```bash
$ vim test.rb
$ git commit -a -m 'made a change'
```

下图展示了提交后的结果。

![每次提交后 HEAD 随着分支一起向前移动](http://docs.pythontab.com/github/gitbook/_images/18333fig0307-tn.png)

现在 testing 分支向前移动了一格，而 master 分支仍然指向原先 `git checkout` 时所在的 commit 对象。现在我们回到 master 分支看看:

```bash
$ git checkout master
```

这条命令做了两件事。它把     

> 1. HEAD 指针移回到 master 分支     
> 2. 把工作目录中的文件换成了 master分支所指向的快照内容  

也就是说，现在开始所做的改动，将始于本项目中一个较老的版本。它的主要作用是将 testing 分支里作出的修改暂时取消，这样你就可以向另一个方向进行开发。

![HEAD 在一次 checkout 之后移动到了另一个分支](http://docs.pythontab.com/github/gitbook/_images/18333fig0308-tn.png)








[返回目录](https://github.com/wdyggh/note)
