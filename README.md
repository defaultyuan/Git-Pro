# Git-Pro
自己在工作中常用的git命令行！

## <a name="basic">Git基本操作</a>
### 用户信息
当我们要进行Git提交的时候，我们作为提交者要以什么姓名和电子邮件地址进行提交？
```shell
git config --global user.name "DefaultYuan"
git config --global user.email johndoe@example.com
```
### 查看配置信息
```shell
git config --list
```
上面命令配置好的用户信息可以通过**git config --list**命令查看到，该命令到用户名、邮箱、别名、remote地址等等！真是一览无遗！当然也可以到.git目录下的查看config文件

### 从现有的仓库克隆到本地
```shell
git clone https://github.com/DefaultYuan/Git-Pro.git A/B
```
以上命令是将现有的仓库克隆到**A目录下的B文件夹**里面，如果后面没有添加**A/B**就会默认新建一个名为**Git-Pro**文件夹，且将仓库克隆到这个文件夹下面！

### 提交代码到本地仓库
当我们在本地修改了文件，比如修改了**README.md**文件，我们首先查看状态确定哪些文件当前处于什么状态！
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
当前**README.md**文件是有做修改的，该文件还没有添加到stage的，提示我们可以通过**git add <file>**来添加，当然我们也可以通过**git checkout -- <file>**来撤销本次做到修改，建议不要轻易这么干！后续我会讲讲相关撤销操作。

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
继上面的提交操作之后，我们先拉取一下远程仓库的更新信息，有可能同事进行过提交并推送到远程仓库
```shell
git fetch
```
接下来，我们就可以手动**merge**远程仓库的修改了
```shell
git merge
```
我们为了让我们的提交记录比较美观一些的话，在这里我们会用**rebase**去代替**merge**操作
```shell
git rebase
```
或者用**pull**来拉取代码
```shell
git pull
```
**fetch**跟**pull**有啥区别呢？？
**fetch**从远程仓库抓取到本地之后不会自动**merge**操作，但**pull**会自动**merge**

最后我们进行**push**操作
```shell
git push
```
这样我们就可以做到本地仓库和远程仓库的同步了！

## Git进阶
上面讲的是一些[**Git**基本操作](#basic)，也就是默认我们修改的代码合并的时候没有冲突。
那在合并代码时有冲突该怎么办？在开发过程中临时想切换其他分支上去开发怎么办？等等一系列的问题接下来会讲解一下。



