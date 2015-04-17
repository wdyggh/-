
#### 查看提交历史

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


















[返回目录](https://github.com/wdyggh/note)
