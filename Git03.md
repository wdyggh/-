
### 查看提交历史

- - - 

在项目中运行 `git log` 可以看到以下输出:

```bash
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test code

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit
```

git log 有许多选项可以帮助你搜寻感兴趣的提交，接下来我们介绍些最常用的。  

`$ git log -p -2` -p 选项展开显示每次提交的内容差异，用 -2 则仅显示最近的两次更新  

`$ git log --stat` 比如 –stat，仅显示简要的增改行数统计  

还有的在[这](http://docs.pythontab.com/github/gitbook/Git-Basics/Viewing-the-Commit-History.html)不一一介绍了。

### 撤消操作

- - -

####修改最后一次提交

当提交完了之后发现有几个文件没有提交时，想要撤消刚刚的提交操作，可以使用`--amend`选项重心提交：

```bash
$ git commit --amend
```

此命令将使用当前的暂存区域快照提交。如果刚才提交完没有作任何改动，直接运行此命令的话，相当于有机会重新编辑提交说明，但将要提交的文件快照和之前的一样。

如果刚才提交时忘了暂存某些修改，可以先补上暂存操作，然后再运行 –amend 提交:

```bash
$ git commit -m 'initial commit'
$ git add 要添加的文件名
$ git commit --amend
```

上面的三条命令最终只是产生一个提交，第二个提交命令修正了第一个的提交内容。

####取消已经暂存的文件

- - -

看下面的例子，有两个修改过的文件，我们想要分开提交，但不小心用 git add . 全加到了暂存区域。该如何撤消暂存其中的一个文件呢？其实，git status 的命令输出已经告诉了我们该怎么做:

```bash
$ git add .
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   README.txt
#       modified:   benchmarks.rb
#
```

就在 “Changes to be committed” 下面，括号中有提示，可以使用 git reset HEAD <file>... 的方式取消暂存。好吧，我们来试试取消暂存 benchmarks.rb 文件:

```bash
$ git reset HEAD benchmarks.rb      #取消暂存 benchmarks.rb 文件
benchmarks.rb: locally modified
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   README.txt      #可以看到只有一个文件
#
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   benchmarks.rb   #并未提交
#
```

####取消对文件的修改

- - -

```bash
$ git checkout -- benchmarks.rb
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   README.txt
#
```

这样就能会到之前的状态 就是修改之前的版本。

###远程仓库的使用

####查看当前的远程库

- - -

用 git remote 命令，它会列出每个远程库的简短名字。在克隆完某个项目后，至少可以看到一个名为 origin 的远程库，Git 默认使用这个名字来标识你所克隆的原始仓库     
加上 -v 选项（译注::此为 –verbose 的简写，取首字母），显示对应的克隆地址:  

![Git03_01](http://7xifyp.com1.z0.glb.clouddn.com/Git03_01.png)

####添加远程仓库

- - -

添加一个新的远程仓库，可以指定一个简单的名字，以便将来引用，运行 git remote add [shortname] [url]:

```bash
$ git remote
origin
$ git remote add pb git://github.com/paulboone/ticgit.git
$ git remote -v
origin  git://github.com/schacon/ticgit.git
pb  git://github.com/paulboone/ticgit.git
```

**现在可以用字串 pb 指代对应的仓库地址了**。比如说，要抓取所有 Paul 有的，但本地仓库没有的信息，可以运行 `git fetch pb`:

```bash
$ git fetch pb
remote: Counting objects: 58, done.
remote: Compressing objects: 100% (41/41), done.
remote: Total 44 (delta 24), reused 1 (delta 0)
Unpacking objects: 100% (44/44), done.
From git://github.com/paulboone/ticgit
 * [new branch]      master     -> pb/master
 * [new branch]      ticgit     -> pb/ticgit
 ```
 
现在，Paul 的主干分支（master）已经完全可以在本地访问了，对应的名字是 pb/master，你可以将它合并到自己的某个分支，或者切换到这个分支，看看有些什么有趣的更新
 
####从远程仓库抓取数据

- - - 

可以用下面的命令从远程仓库抓取数据到本地:

```bash
$ git fetch [remote-name]
```

此命令会到远程仓库中拉取所有你本地仓库中还没有的数据。运行完成后，你就可以在本地访问该远程仓库中的所有分支，将其中某个分支合并到本地，或者只是取出某个分支，一探究竟。   

如果是克隆了一个仓库，此命令会自动将远程仓库归于 origin 名下。所以，git fetch origin 会抓取从你上次克隆以来别人上传到此远程仓库中的所有更新（或是上次 fetch 以来别人提交的更新）。**有一点很重要，需要记住，fetch 命令只是将远端的数据拉到本地仓库，并不自动合并到当前工作分支，只有当你确实准备好了，才能手工合并**。    

####推送数据到远程仓库

- - -

*项目进行到一个阶段，要同别人分享目前的成果，可以将本地仓库中的数据推送到远程仓库*。实现这个任务的命令很简单 `git push [remote-name] [branch-name]`。如果要把本地的 master 分支推送到 origin 服务器上（**再次说明下，克隆操作会自动使用默认的 master 和 origin 名字**），可以运行下面的命令:

```bash
$ git push origin master
```

只有在所克隆的服务器上有写权限，或者*同一时刻*没有其他人在推数据，这条命令才会如期完成任务。*如果在你推数据前，已经有其他人推送了若干更新，那你的推送操作就会被驳回*。你必须先把他们的更新抓取到本地，合并到自己的项目中，然后才可以再次推送。

 #### 查看远程仓库信息
 
 - - -
 
 我们可以通过命令` git remote show [remote-name]` 查看某个远程仓库的详细信息，比如要看所克隆的 origin 仓库，可以运行:
 
 ![Git03_02](http://7xifyp.com1.z0.glb.clouddn.com/Git03_02.png)
 
 随着使用 Git 的深入，`git remote show` 给出的信息可能会像这样:
 
 ```bash
 $ git remote show origin
* remote origin
  URL: git@github.com:defunkt/github.git
  Remote branch merged with 'git pull' while on branch issues
    issues
  Remote branch merged with 'git pull' while on branch master
    master
  New remote branches (next fetch will store in remotes/origin)
    caching
  Stale tracking branches (use 'git remote prune')
    libwalker
    walker2
  Tracked remote branches
    acl
    apiv2
    dashboard2
    issues
    master
    postgres
  Local branch pushed with 'git push'
    master:master
 ```
 
#### 远程仓库的删除和重命名

- - -

在新版 Git 中可以用 `git remote rename` 命令修改某个远程仓库在本地的简短名称，比如想把 pb 改成 paul，可以这么运行:
 
```bash
$ git remote rename pb paul
$ git remote
origin
paul
```

**注意，对远程仓库的重命名，也会使对应的分支名称发生变化，原来的 pb/master 分支现在成了 paul/master。**
 
碰到远端仓库服务器迁移，或者原来的克隆镜像不再使用，又或者某个参与者不再贡献代码，那么需要移除对应的远端仓库，可以运行 git remote rm 命令:

```bash
$ git remote rm paul
$ git remote
origin
```
 
 




[返回目录](https://github.com/wdyggh/note)
