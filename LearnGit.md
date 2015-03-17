###Git

Git是一套版本管理系统。看到“版本管理”，一大部分盆友已经转身想走，在你握着门把手准备开门走人时，请最后听我说一句：人人都需要版本管理。

试过半夜写汇报ppt吗？'汇报ppt'→'汇报ppt1'→'汇报ppt11'→'汇报ppt2015-03-17'→'汇报ppt2015-03-17新'→'汇报ppt2015-03-17新1'……无休止的命名斗争，这就是自然而然的版本管理，只不过，没有好的工具，所以显得一团mess。

无论学生党还是设计师（改20个版本后终于顺利用回第1版），无论公众号运营还是音乐人，都持续产出着自己的“半成品/作品”。99.999%的作品都不可能一气呵成，比如这篇笔记的第一个commit版本，简直惨不忍睹。如果有版本管理意识，以及高效、方便的工具，生活也许可以简单许多，更不要说天有不测风云的停电忘保存、脑残删备份等等好事等着我们。

来吧，fork有用有趣的东西，git你应该在意的东西，日拱一卒，打造我们的作品。


####一、git主要概念

git实现的是在本地和远端进行版本管理。


####1.工作空间

四个空间概念：工作目录(workspace)，暂存区(index)，本地仓库(local repository)，远程仓库(remote repository)

想象一下，我们开一个包子店~ 

- 首先，得有一张**大桌子**用来和面、擀皮儿、包馅等等，这张桌子相当于workspace，随你折腾的地方，工作主要都在这里进行。

- 然后，包好的包子们会放到一个**蒸笼**里，等待被蒸，这个蒸笼就是index暂存区。蒸笼用来放我们想保留的成品或半成品，至于选哪些卖出去，这是以后考虑的事情。

- 下一步，蒸包子。蒸好的包子已经可以吃了，但是我们还是得先把它们从蒸笼拿出来放在**盘子**里。盘子就类似本地仓库local repository，里面都是等待出货的好东西。当然，你也可以在最后一刻把看不顺眼的包子扔掉，或者自己吃掉。

- 最后一步就是把包子送到**货架/客人的桌上**。公之于众的货架，就是远程仓库remote repository，丑媳妇终于见公婆啦。

![](http://blog.osteele.com/images/2008/git-transport.png)

####2.Head & branch

git系统的实质更像是一棵大树，树干（就是Head啦）是最后一次提交的成果。在树干上，你可以开无数的分支（就是branch啦）胡弄，弄乱了也不怕，大不了剪掉再开一个，树干不受任何影响。

用技术性语言描述，分支用来将特性开发绝缘开来。在创建仓库的时候，master 是“默认的”分支。在其他分支上进行开发，完成后再将它们合并到主分支上。
![](http://rogerdudler.github.io/git-guide/img/branches.png)


####3.工作流：add & commit & push

* 把包子从桌子挪到蒸笼，叫add——汇报ppt初稿写成；
* 把包子从蒸笼挪到盘子，叫commit——汇报ppt完稿存到u盘/网盘什么的；
* 把包子从盘子挪到货架，叫push——汇报ppt发送到boss邮箱。

![](http://rogerdudler.github.io/git-guide/img/trees.png)



###二、配置
1.工作目录
2.本地仓库
3.远程仓库

###三、常用命令

####1.创建

**需要进入目标目录进行操作**

创建新仓库：``git init``

创建一个本地仓库的克隆版本：``git clone /path/to/repository ``

克隆远端服务器上的仓库： ``git clone username@host:/path/to/repository ``

####2.查询
``git status``

* staged:Files are ready to be committed.
* unstaged:Files with changes that have not been prepared to be commited.
* untracked:Files aren't tracked by Git yet. This usually indicates a newly created file.
* deleted:File has been deleted and is waiting to be removed from Git.

####3.添加/add

添加到暂存区（让git开始跟踪更改）：``git add <filename>`` 或 ``git add *``

add all:You can also type ``git add -A`` . where the dot stands for the current directory, so everything in and beneath it is added. The -A ensures even file deletions are included.

git reset:
You can use ``git reset <filename>`` to remove a file or files from the staging area.

####4.提交/commit

Staging Area:A place where we can group files together before we "commit" them to Git.

Commit
A "commit" is a snapshot of our repository. This way if we ever need to look back at the changes we've made (or if someone else does), we will see a nice timeline of all changes.

提交改动(到head，但还没到远程服务器)：``git commit -m "代码提交信息"``

提交到远程仓库：``git push origin master`` 可以把 master 换成你想要推送的任何分支。
如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：
``git remote add origin <server>``这样就将改动推送到所添加的服务器上去了。

####5.推送/push

The push command tells Git where to put our commits when we're ready

The name of our remote is **origin** and the default local branch name is **master**.
The -u tells Git to remember the parameters, so that next time we can simply run git push and Git will know what to do. Go ahead and push it!

``git push -u origin master``

####6.pull

We can check for changes on our GitHub repository and pull down any new changes by running:

``git pull origin master``
 

####7.分支


另开分支

When developers are working on a feature or bug they'll often create a copy (aka. branch) of their code they can make separate commits to. Then when they're done they can merge this branch back into their main master branch.

切换分支

You can switch branches using the ``git checkout <branch>`` command.

ou can use:
``git checkout -b new_branch``
to checkout and create a branch at the same time. This is the same thing as doing:
``git branch new_branch``
``git checkout new_branch``

####8.比对 diff
``git diff``

####9.reset和checkout
``git reset`` did a great job of unstaging
Files can be changed back to how they were at the last commit by using the command: ``git checkout -- <target>``


####10.merge
``git merge``

####11.remove & clean

``git rm`` command which will not only remove the actual files from disk, but will also stage the removal of the files

``git branch -d <branch name>``

###Ref
[Git简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)