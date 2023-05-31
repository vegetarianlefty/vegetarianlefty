### git文件添加命令

#### git add 命令介绍

将工作区的文件或目录通过add命令添加到git暂存区,命令如下：

将单个新建文件，添加到暂存区：

```
git add index.html
```

将多个新建文件夹和文件，添加到暂存存：

```
git add images test index.js index.css
```

将文件的新增，修改，删除，添加到暂存区：

```
git add .`或者`git add -all
```

将git管理中跟踪的文件，从新修改过的文件，添加到暂存区：

`git add -u` 说明：新创建的文件，不会被添加到暂存区

重命名暂存区里面的文件名，命令如下：

`git mv old_name new_name` *从新命名后，不需要从新提交到暂存区，直接commit提交就行*

清空暂存区和工作区的变动，使文件还原为当初的样子，命令如下：

`git reset --hard` **⚠️危险命令，不推荐使用**

#### git branch 命令介绍

git 分支查看命令：

```console
git branch        //查看本地分支
git branch -a     //查看本地和remote远程分支
git branch -av    //查看分支提交详情
```

git 分支创建 branchName , 并切换到创建的新分支 branchName 目录下：

```console
git branch branchName    //创建分支
git checkout branchName  //切换分支
##以上命令可以简写
git checkout -b branchName  //创建分支并切换到新创建的分支目录下
```

git 删除分支命令：

```console
git branch -d branchName       //删除分支
git branch -D branchName    //强制删除分支
**区别 使用-d 在删除前Git会判断在该分支上开发的功能是否被merge的其它分支。如果没有，不能删除。如果merge到其它分支，但之后又在其上做了开发，使用-d还是不能删除。-D会强制删除**
```



#### git commit 命令介绍

git 提交命令：

```console
git commit        //把添加的暂存区的文件提交到git
git commit -m 'message'  //把添加的暂存区的文件提交到git，并加入message
```

git 修改最新 commit 的 message 的命令：

```console
git commit --amend     //修改最新commit的message   

hint: Waiting for your editor to close the file... error: There was a problem with the editor 'vi'.
Please supply the message using either -m or -F option.
解决办法：
git config --global core.editor 'vim'
```

git 修改老旧的 commit 的 message 的命令：

```console
git rebase -i xxxxx   //修改老旧commit的message   xxxxx 父节点commitID
```

git 修改老旧的多个 commit 的 message 的命令：

```console
git rebase -i xxxxx   //修改老旧commit的message   xxxxx 父节点commitID
```

git 撤销最近几次的 commit ：

```console
git reset --hard xxxx    //撤销最近几次的 commit   xxxxx 父节点commitID
```

#### git diff 命令介绍

git diff 命令：

```console
暂存区与HEAD比较：git diff --cached

暂存区与工作区比较: git diff

暂存区恢复成HEAD : git reset HEAD

暂存区覆盖工作区修改：git checkout 
```



#### git log命令介绍

查看所有分支的历史

```
git log --all
```

查看图形化的 log 地址

```
git log --all --graph
```

查看单行的简洁历史

```
git log --oneline
```

查看最近的四条简洁历史

```
git log --oneline -n4
```

查看所有分支最近 4 条单行的图形化历史

```
git log --oneline --all -n4 --graph
```

跳转到git log 的帮助文档网页

```
git help --web log
```