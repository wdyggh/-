
### 取得项目的 Git 仓库

- - - 

#### 在工作目录中初始化新仓库  

要对现有的某个项目开始用 Git 管理，只需到此项目所在的目录，执行:

```bash
$ git init
```

初始化后，在当前目录下会出现一个名为 .git 的目录，所有 Git 需要的数据和资源都存放在这个目录中。

如果当前目录下有几个文件想要纳入版本控制，需要先用 git add 命令告诉 Git 开始对这些文件进行跟踪，然后提交:

```bash
$ git add *.c
$ git add README
$ git commit -m 'initial project verson'
```

####从现有仓库克隆

- - -

如果想对某个开源项目出一份力，可以先把该项目的 Git 仓库复制一份出来，这就需要用到 git clone 命令。
克隆仓库的命令格式为 *git clone [url]*

```bash
$ git clone git://github.com/schacon/grit.git
```

这会在当前目录下创建一个名为 “grit” 的目录，其中包含一个 .git 的目录，用于保存下载下来的所有版本记录，然后从中取出最新版本的文件拷贝。如果进入这个新建的 grit 目录，你会看到项目中的所有文件已经在里边了，准备好后续的开发和使用。

如果希望在克隆的时候，自己定义要新建的项目目录名称，可以在上面的命令末尾指定新的名字:

```bash
$ git clone git://github.com/schacon/grit.git mygrit
```

现在新建的目录成了 mygrit，其他的都和上边的一样。


###记录每次更新到仓库

- - -

工作目录下面的所有文件都不外乎这两种状态：**已跟踪**或**未跟踪**。  
> *已跟踪的文件是指本来就被纳入版本控制管理的文件，在上次快照中有它们的记录，工作一段时间后，它们的状态可能是未更新，已修改或者已放入暂存区.而所有其他文件都属于未跟踪文件。它们既没有上次更新时的快照，也不在当前的暂存区域。初次克隆某个仓库时，工作目录中的所有文件都属于已跟踪文件，且状态为未修改。*

在编辑过某些文件之后，Git 将这些文件标为已修改。我们逐步把这些修改过的文件放到暂存区域，直到最后一次性提交所有这些暂存起来的文件，如此重复。  

![adw](http://docs.pythontab.com/github/gitbook/_images/18333fig0201-tn.png)

###检查当前文件状态

可以使用`git status`命令，如果在克隆仓库后立刻执行会看到以下结果：

```bash
$ git status
# On branch master
nothing to commit (working directory clean)
```

这说明你现在的工作目录相当干净。换句话说，当前没有任何跟踪着的文件，也没有任何文件在上次提交后更改过。此外，上面的信息还表明，当前目录下没有出现任何处于未跟踪的新文件，否则 Git 会在这里列出来。最后，该命令还显示了当前所在的分支是 master，这是默认的分支名称，实际是可以修改的，现在先不用考虑。   

#### 跟踪新文件

- - -

使用命令`git add`开始跟踪一个新文件：

```bash
$ git add README
```

此时在运行`git status`，会看到README文件已被跟踪，并处于暂存状态。

```bash
$ git status
# On branch master
#Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#   new file:   README
#
```

只要在 “Changes to be committed” 这行下面的，就说明是已暂存状态。如果此时提交，那么该文件此时此刻的版本将被留存在历史记录中。你可能会想起之前我们使用 git init 后就运行了 git add 命令，开始跟踪当前目录下的文件。在 git add 后面可以指明要跟踪的文件或目录路径。如果是目录的话，就说明要递归跟踪该目录下的所有文件。（译注：其实 git add 的潜台词就是把目标文件快照放入暂存区域，也就是 add file into staged area ，同时未曾跟踪过的文件标记为需要跟踪。这样就好理解后续 add 操作的实际意义了。）


####暂存已修改文件

- - -

现在我们修改下之前已跟踪过的文件 `benchmarks.rb` ，然后再次运行 `git status` 命令，会看到这样的状态报告:

```bash
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#   new file:   README
#
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#
#   modified:   benchmarks.rb
#
```

文件 benchmarks.rb 出现在 “Changes not staged for commit” 这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。要暂存这次更新，需要运行 git add 命令（这是个多功能命令，根据目标文件的状态不同，此命令的效果也不同：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等）。现在让我们运行 git add 将 benchmarks.rb 放到暂存区，然后再看看 git status 的输出:

```bash
$ git add benchmarks.rb
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#   new file:   README
#   modified:   benchmarks.rb
#
```

如果想再添加注释，就必须重新使用 `git add `,不然只提交 添加注释之前的内容。

### *忽略某些文件*

- - -

一般总有一些文件无需纳入Git的管理。比如一些自动生成的文件，日志，编译的临时文件。。。
此时我们可以创建一个`.gitignore`的文件，列出要忽略的文件模式。

```bash
$ cat .gitignore
*.[oa]            #Git 忽略所有以 .o 或 .a 结尾的文件
*~                #Git 忽略所有以波浪符（~）结尾的文件
```

此外，你可能还需要忽略 log，tmp 或者 pid 目录，以及自动生成的文档等等。要养成一开始就设置好 .gitignore 文件的习惯，以免将来误提交这类无用的文件。

文件 .gitignore 的格式规范如下：

* 所有空行或者以注释符号 ＃ 开头的行都会被 Git 忽略。
* 可以使用标准的 glob 模式匹配。
* 匹配模式最后跟反斜杠（/）说明要忽略的是目录。
* 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

*所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。星号（/*）匹配零个或多个任意字符；[abc] 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 [0-9] 表示匹配所有 0 到 9 的数字）。*






[返回目录](https://github.com/wdyggh/note)
