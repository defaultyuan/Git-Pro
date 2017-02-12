# Git-Pro
自己在工作中常用的git命令行！

## <a name="basic">Git基本操作</a>
## 用户信息
当我们要进行Git提交的时候，我们作为提交者要以什么姓名和电子邮件地址进行提交？
```shell
git config --global user.name "DefaultYuan"
git config --global user.email johndoe@example.com
```
### 查看配置信息
```shell
git config --list
```
上面命令配置好的用户信息可以通过`git config --list`命令查看用户名、邮箱、别名、remote地址等等！真是一览无遗！当然也可以到.git目录下的查看config文件

### 从现有的仓库克隆到本地
```shell
git clone https://github.com/DefaultYuan/Git-Pro.git A/B
```
以上命令是将现有的仓库克隆到**A目录下的B文件夹**里面，如果后面没有添加**A/B**就会默认新建一个名为**Git-Pro**文件夹，且将仓库克隆到这个文件夹下面！

### 提交代码到本地仓库
当我们在本地修改了文件，比如修改了`README.md`文件，我们首先查看状态确定哪些文件当前处于什么状态！
```shell
git status
```
该命令执行完终端输出如下：
```shell
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)

modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```
当前`README.md`文件是有做修改的，该文件还没有添加到stage的，提示我们可以通过**git add <file>**来添加，当然我们也可以通过**git checkout -- <file>**来撤销本次做到修改，建议不要轻易这么干！后续我会讲讲相关撤销操作。

现在我们加入stage
```shell
git add README.md
```
如果我们现在要将本地所有修改的文件进行添加，可以用该命令：
```shell
git add .
```
最后，我们进行提交
```shell
git commit -m "fix 具体哪个功能"
```
如果本地我们改的内容有点多，涉及到修改多个模块的代码，建议多次添加并提交！

### 将本地代码提交到远程仓库
继上面的提交操作之后<br>
我们先拉取一下远程仓库的更新内容
```shell
git fetch
```
接下来，我们就可以手动`merge`远程仓库的修改了
```shell
git merge
```
我们为了让我们的提交记录比较美观一些的话，在这里我们会用`rebase`去代替`merge`操作
```shell
git rebase
```
或者用`pull`来拉取代码
```shell
git pull
```
`fetch`跟`pull`有啥区别呢？<br>
`fetch`从远程仓库抓取到本地之后不会自动`merge`操作，但`pull`会自动`merge`

最后我们进行`push`操作
```shell
git push
```
这样我们就可以做到远程仓库和本地仓库的同步了！
***
## Git进阶
上面讲的是一些[**Git**基本操作](#basic)。<br>
接下来讲讲一些Git进阶小技巧

### 提交技巧
##### 当我们要将`stage`里的修改分多次提交到本地仓库,也就是具体到哪个文件
```shell
git commit DefaultYuan.txt -m "fix 具体功能"
```
##### 如果我们想查看要提交的内容与版本库中的比较，然后进行提交
```shell
git commit -v
```
##### 将工作区和`stage`中的所有修改的文件一次性提交到本地仓库
```shell
git commit -a
```
这样会默认使用 vi 添加描述,我们也可以在`.git/config`的`core`中`editor = vim`指定编辑器<br>
当然也可以使用`-m`选项直接添加提交信息
```shell
git commit -a -m "fix 具体功能"
```
##### 有时候我们需要对上一次提交信息进行修改
```shell
git commit --amend
```
也可以后面添加参数`-m`做到修改
### 查看状态
##### 查看工作区中所有目录下文件的变动
```shell
git status
```
##### 比较工作区与暂存区的改动
```shell
git diff
```
##### 添加参数`--cached`，比较暂存区和版本库之间的区别
```shell
git diff --cached
```
##### 添加参数`HEAD`，比较工作区、暂存区和版本库之间的区别
```shell
git diff HEAD
```
`HEAD`关键字它指的是当前分支的最新提交，相当于一个**指针**,后续讲到新建分支也会提到它
### stash的使用
有时候我们在工作区进行开发并且不想提交的时候，这时我们又想`pull`最新代码；或者又想切到另外一个分支上修改紧急bug的时候<br>
**git stash**可以暂存当前的工作区内容
##### 可以先暂存当前的工作区的内容
```shell
git stash
```
等我们切到另外分支修改完了bug之后，可以切回之前分支
##### 然后恢复之前工作区的内容继续开发
```shell
git stash pop
```
##### 也可以查看`stash`的Git栈信息
```shell
git stash list
```
当我们的`stash`栈列表里面有很多，并且我们想要找到对应的版本号
##### 并且将我们想要的版本号为**stash@{2}**的工作内容取出来
```shell
git stash apply stash@{2}
```
##### 也可以查看版本号为**stash@{2}**的工作内容
```shell
git stash show stash@{2}
```
当我们的`stash`栈列表的工作内容都应用回来的时候
##### 可以将栈清空
```shell
git stash clear
```
### 分支
分支在Git里面是比较廉价的，我们在开发的时候可以在本地仓库创建很多条分支
##### 新建一条名字为dev的分支
```shell
git branch dev
```
这种创建方式会依然停留在当前分支
##### 开发新功能、或者修改bug，我们会创建一条分支并切到新开分支上开发
```shell
git checkout -b dev_fixHot
```
##### 也可以从当前分支新建一个dev分支，指向指定`commit`
```shell
git branch dev f0299e2
```
`f0299e2`是随便写的，具体要以我们自己的`commit`
##### 切换到指定分支
```shell
git checkout dev
```
分支其实也是指向某个`commit`的指针，`HEAD`也是一个指针，它指向当前工作目录下的`commit`<br>
因此`checkout`就是让`HEAD`指向dev分支
##### 删除dev分支
```shell
git branch -d dev
```
##### 强制删除dev分支
```shell
git branch -D dev
```
##### 将本地dev分支与dev_fixHot远程分支建立追踪关系
```shell
git branch --set-upstream dev origin/dev_fixHot
```
##### 添加一个dev远程分支
```shell
git push origin dev:dev
```
##### 删除dev远程分支
```shell
git push origin :dev
```
或者<br>
```shell
git push origin --delete dev
```
##### 选择一个`commit`,合并到当前分支
```shell
git cherry-pick f0299e2
```
### 合并分支`rebase`和`merge`
####在工作中，如果团队开发同一个项目的人数特别多的时候，每个人都在自己的分支上开发<br>
在开发过程中，为了使提交记录直观，方便在一条分支上查看提交记录，可以对分支进行衍合，一般是在做了一些小的修改之后会用它<br>
将修改`commit`到本地分支上之后<br>
当本地分支上有相对应的远程分支，可以直接使用
```shell
git rebase
```
为了方便讲解，`origin/dev_homepageFix`这个远程分支是随便写的分支名<br>
当这次`commit`要衍合到`dev_homepageFix`分支上时<br>
```shell
git rebase origin/dev_homepageFix
```
最后就可以`push`跟远程仓库同步了
####当我们到项目接近尾声的时候，需要将各自的分支都合并到`dev`分支上去<br>
先抓取最新仓库更新
```shell
git fetch
```
然后切到`dev`分支上
```shell
git checkout dev
```
再分别合并各个分支，这里以`dev_homepageFix`分支为例
```shell
git merge origin/dev_homepageFix
```
### 撤销操作

当我们想去整理我的提交，或者是把不该这次提交的修改提交了，我们可以对修改进行撤销，或者对提交进行撤销
##### 恢复暂存区的所有文件到工作区
```shell
git reset HEAD
```
##### 恢复暂存区的指定的某个文件到工作区
```shell
git reset HEAD README.md
```

我们做了一些修改，然后觉得自己做错了，不想提交
##### 可以重置暂存区、工作区，这样就能与上次提交保持一致，建议不要轻易这么做！
```shell
git reset --hard
```

当我们想重新整理提交的时候，比如说将上两次的提交整理到一次提交,我们可以重置到上两次的上次提交`commit`
##### 可以重置到指定的`commit`,并重置了暂存区，但工作区是不变的
```shell
git reset 2126dce
```
这种情况也会引发另外一个问题，当我们本地仓库与远程仓库一致的时候，然后我们重置，就需要用到`push`的`force`来进行强推，这是我们不想操作的，后续我会讲解一下强推的小建议！

##### 新建一个`commit`，用来撤销指定`commit`，后者所有变化都会被前者抵消，并应用到当前分支 <br>
我们也是不要轻易这么去做！
```shell
git revert 2126dce
```
###起别名
当我们有比较常用的命令行用的比较频繁，但是我们又觉得完整把它们敲出来费事
#####可以在 `~/.gitconfig` 文件里面进行设置别名,添加`alias`
```shell
[alias]
ft = fetch
ck = checkout
st = status
br = branch
rb = rebase
lg = log
```
然后试一下`git st`就管用了！

TODO `tag`<br>

[微博:DefaultYuan](http://weibo.com/2792951481)

