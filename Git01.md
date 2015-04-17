###Git 安装
- - -

####Linux

* Fedora  

```bash
yun install git-core
```

* Ubuntu & Debian

```bash
apt-get install git
```

####Mac

* Use MacPorts

```bash
sudo port install git-core +svn +doc +bash_completion +gitweb
```

####Windows

[install](http://code.google.com/p/msysgit)


## Git 初次使用配置

- - - 

####用户信息

* 用户名称
* 电子邮件地址

```bash
$ git config --global user.name "aaa"
$ git config --global user.email aaa@example.com
```

如果用了 –global 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。如果要在某个特定的项目中使用其他名字或者电邮，只要去掉 –global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。

####文本编辑器 ??

设置默认编辑器 Emacs 或者其他的编辑器

```bash
$ git config --global core.editor emacs
```

####差异分析工具??(还没搞懂)
在解决合并冲突时使用哪种差异分析工具。比如要改用Vimdiff:

```bash
$ git config --global merge.tool vimdiff
```

Git 还可以理解 kdiff3，tkdiff，meld，xxdiff，emerge，vimdiff，gvimdiff，ecmerge，和 opendiff 等合并工具的输出信息。
暂时还不会用。

####查看配置信息

可以使用 git config --list命令：

```bash
$ git config --list
user.name=Scott Chacon
user.email=schacon@gmail.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

也可以直接查阅某个环境变量的设定,像这样:

```bash
$ git config user.name
```

####获取帮助

有三种方法：

```bash
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

比如想知道config命令怎样使用：

```bash
$ git help config
```
