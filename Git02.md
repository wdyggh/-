
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

如果想对某个开源项目出一份力，可以先把该项目的 Git 仓库复制一份出来，这就需要用到 git clone 命令。
克隆仓库的命令格式为 * git clone [url] *

```bash
$ git clone git://github.com/schacon/grit.git
```





[返回目录](https://github.com/wdyggh/note)
