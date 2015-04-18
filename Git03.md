
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

![Git03_01](http://7xifyp.com1.z0.glb.clouddn.com/Git03_01.png)









[返回目录](https://github.com/wdyggh/note)
