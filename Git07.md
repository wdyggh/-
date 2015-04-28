
### 分支的管理

到目前为止，你已经学会了如何`创建、合并和删除分支`。除此之外，我们还需要学习如何`管理分支`，在日后的常规工作中会经常用到下面介绍的管理命令。

- - - 

#### git branch     

git branch 命令不仅仅能创建和删除分支，如果不加任何参数，它会给出当前所有分支的清单:

```bash
$ git branch
  iss53
* master    #当前所在的分支，提交更新的话，master分支将随着开发进度前移
  testing
```

![git branch](http://7xifyp.com1.z0.glb.clouddn.com/gitbranch.jpg)

若要查看各个分支最后一个提交对象的信息，可以尝试一下 `git branch -v`:    

```bash
$ git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
```

![git branch -v](http://7xifyp.com1.z0.glb.clouddn.com/gitbranch-v.jpg)



`git branch --merged` *查看已经与当前分支合并的分支*     
`git branch --no-merged` *查看尚未合并的分支*      
`git branch -d testing` *删除testing分支 *
`git branch -D testing` *强制删除testing分支 *



[返回目录](https://github.com/wdyggh/note)
