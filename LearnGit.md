###Git

git主要概念：

在本地和远端进行版本管理。有四个空间概念：工作目录(workspace)，暂存区(index)，本地仓库(local repository)，远程仓库(remote repository)

![](http://blog.osteele.com/images/2008/git-transport.png)

[Git简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
默认工作目录？

####工作流

本地仓库由 git 维护的三棵“树”组成。第一个是你的**工作目录**，它持有实际文件；第二个是**暂存区（Index）**，它像个缓存区域，临时保存你的改动；最后是**HEAD**，它指向你最后一次提交的结果。
![](http://rogerdudler.github.io/git-guide/img/trees.png)

####创建

**需要进入目标目录进行操作**

创建新仓库：``git init``

创建一个本地仓库的克隆版本：``git clone /path/to/repository ``

克隆远端服务器上的仓库： ``git clone username@host:/path/to/repository ``

####查询
``git status``

* staged:Files are ready to be committed.
* unstaged:Files with changes that have not been prepared to be commited.
* untracked:Files aren't tracked by Git yet. This usually indicates a newly created file.
* deleted:File has been deleted and is waiting to be removed from Git.

####添加

添加到暂存区（让git开始跟踪更改）：``git add <filename>`` 或 ``git add *``

add all:You can also type ``git add -A`` . where the dot stands for the current directory, so everything in and beneath it is added. The -A ensures even file deletions are included.

git reset:
You can use ``git reset <filename>`` to remove a file or files from the staging area.

####提交

Staging Area:A place where we can group files together before we "commit" them to Git.

Commit
A "commit" is a snapshot of our repository. This way if we ever need to look back at the changes we've made (or if someone else does), we will see a nice timeline of all changes.

提交改动(到head，但还没到远程服务器)：``git commit -m "代码提交信息"``

提交到远程仓库：``git push origin master`` 可以把 master 换成你想要推送的任何分支。
如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：
``git remote add origin <server>``这样就将改动推送到所添加的服务器上去了。

**commit和push的区别？commit本地，push服务器**

The push command tells Git where to put our commits when we're ready

The name of our remote is **origin** and the default local branch name is **master**.
The -u tells Git to remember the parameters, so that next time we can simply run git push and Git will know what to do. Go ahead and push it!

``git push -u origin master``

####pull

We can check for changes on our GitHub repository and pull down any new changes by running:

``git pull origin master``
 

####分支

分支是用来将特性开发绝缘开来的。在创建仓库的时候，master 是“默认的”分支。在其他分支上进行开发，完成后再将它们合并到主分支上。
![](http://rogerdudler.github.io/git-guide/img/branches.png)

另开分支

When developers are working on a feature or bug they'll often create a copy (aka. branch) of their code they can make separate commits to. Then when they're done they can merge this branch back into their main master branch.

切换分支

You can switch branches using the ``git checkout <branch>`` command.

ou can use:
``git checkout -b new_branch``
to checkout and create a branch at the same time. This is the same thing as doing:
``git branch new_branch``
``git checkout new_branch``

####比对 diff
``git diff``

####reset和checkout
``git reset`` did a great job of unstaging
Files can be changed back to how they were at the last commit by using the command: ``git checkout -- <target>``


####remove
``git rm`` command which will not only remove the actual files from disk, but will also stage the removal of the files


####Commiting Branch Changes

Now that you've removed all the cats you'll need to commit your changes.

Feel free to run git status to check the changes you're about to commit.

``git commit -m "Remove all the cats"``

####merge
``git merge``

####clean
``git branch -d <branch name>``