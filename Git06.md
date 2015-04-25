
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











[返回目录](https://github.com/wdyggh/note)
