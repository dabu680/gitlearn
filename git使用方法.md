创建版本库

$ mkdir learngit
$ cd learngit

git init  #在当前目录下(最好是空目录)初始化一个空仓库
Initialized empty Git repository in /Users/michael/learngit/.git/


可以自由的在仓库中创建文件

```

xhhydeMac-mini:gitconfig xhhy$ echo "git test" > gittest.txt
xhhydeMac-mini:gitconfig xhhy$ ls
README.md	gittest.txt	readme.txt

```


# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"


查看文件状态

xhhydeMac-mini:gitconfig xhhy$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	gittest.txt

no changes added to commit (use "git add" and/or "git commit -a")

#readme.txt表示修改的内容未add到暂存区
#gittest.txt表示新的文件还没有add到暂存区,因此是Untracked




添加文件到暂存区


xhhydeMac-mini:gitconfig xhhy$ git add gittest.txt 
xhhydeMac-mini:gitconfig xhhy$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   gittest.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

#add了gittest.txt到暂存区


退回修改,退回readme.txt文件中所有已修改单尚未add的内容

xhhydeMac-mini:gitconfig xhhy$ cat readme.txt 
asd
asd
asd
a test line

a master line
xhhydeMac-mini:gitconfig xhhy$ git checkout -- readme.txt 
xhhydeMac-mini:gitconfig xhhy$ cat readme.txt 
asd
asd
asd
a test line

<<<<<<< HEAD
a master line
=======
a master line
>>>>>>> test1


提交到版本库


xhhydeMac-mini:gitconfig xhhy$ git commit -m "git test"
[master dcfd923] git test
 1 file changed, 1 insertion(+)
 create mode 100644 gittest.txt
xhhydeMac-mini:gitconfig xhhy$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

#将gittest.txt文件提交到版本库里面,-m表示提交时候的备注信息,


# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...



# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]


查看修改文件的差异


xhhydeMac-mini:gitconfig xhhy$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index cbbfdbb..b8536f3 100644
--- a/readme.txt
+++ b/readme.txt
@@ -3,8 +3,5 @@ asd
 asd
 a test line
 
-<<<<<<< HEAD
 a master line
-=======
-a master line
->>>>>>> test1
+a new  line


#'-'号表示修改的内容比版本库中(本地未修改的)文件缺少的内容
#'+'号表示修改的内容比版本库中(本地未修改的)文件增加的内容



HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令 git reset --hard commit_id。

穿梭前，用 git log 可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用 git reflog 查看命令历史，以便确定要回到未来的哪个版本。


xhhydeMac-mini:gitconfig xhhy$ git log --oneline
dcfd923 (HEAD -> master) git test
771aedc (origin/master, origin/dev, origin/HEAD, dev) conflict fixed
dc1322f a master line
bf9c1b6 (test1) two test line
7eed604 Merge branch 'test'
e250f70 test line
a50c58a master line
8f94b43 test line
8dcea61 Initial commit

xhhydeMac-mini:gitconfig xhhy$ git reset --hard a50c58a
HEAD is now at a50c58a master line
xhhydeMac-mini:gitconfig xhhy$ git status
On branch master
Your branch is behind 'origin/master' by 6 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean

xhhydeMac-mini:gitconfig xhhy$ cat readme.txt 
asd
asd
asd
a master line

xhhydeMac-mini:gitconfig xhhy$ git diff readme.txt 
xhhydeMac-mini:gitconfig xhhy$ git reset --hard e250f70
HEAD is now at e250f70 test line
xhhydeMac-mini:gitconfig xhhy$ cat readme.txt 
asd
asd
asd
a test line

xhhydeMac-mini:gitconfig xhhy$ git status
On branch master
Your branch is behind 'origin/master' by 5 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
xhhydeMac-mini:gitconfig xhhy$ git diff readme.txt


版本修改退回到步骤

1.修改了未add用 git checkout -- filename来退回到版本库

2.add到暂存区用 git reset HEAD filename 先退回到工作区,再执行 git checkout -- filename来退回到版本库

3.commit到版本库的,就先用git log查看commit id 在执行git reset --hard commit id 来切换版本库.

这些都是在未提交到远程仓库的操作.



文件删除方法


1.新增文件未add到直接本地 rm 命令


2.add到暂存区的就先执行 git rm --cache filename 删除暂存区文件在 rm本地文件

3.若版本库里有文件,本地误删了可以执行 git checkout -- filename 来恢复文件,只是本地修改为提交到内容将会丢失.

4.如果你想彻底把版本库的删除掉，先 git rm，再 git commit 就ok了

xhhydeMac-mini:gitconfig xhhy$ git rm git.txt 
rm 'git.txt'
xhhydeMac-mini:gitconfig xhhy$ git commit -m "remove git"
[master d4c85f0] remove git
 1 file changed, 2 deletions(-)
 delete mode 100644 git.txt



远程仓库链接用关联

git使用ssh链接github的方法:


ssh不行是因为你没有设置shs秘钥
1：生成秘钥：ssh-keygen -t rsa -C "github用户名"

(这里不要设置密码，直接按回车就可以，以后更新就不需要密码)

2：id_rsa 这个文件是你的私钥、id_rsa.pub是你的公共必要，执行 cat id_rsa.pub,把里面的内容复制到github配置ssh

3：添加私秘钥到ssh: ssh-add id_rsa（如果添加失败可以先执行命令ssh-agent bash,然后再次添加私秘钥。）

4: 用 ssh -T git@github.com 判断是否绑定成功。如果返回successfully 表示成功



clone仓库到本地


登陆GitHub，创建一个新的仓库

勾选 Initialize this repository with a README，这样GitHub会自动为我们创建一个README.md文件。创建完毕后，可以看到README.md文件：

git clone git@github.com:github用户名/仓库名.git



关联远程仓库


git remote add origin git@github.com:github用户名/关联的仓库.git

#origin为远程默认仓库名,可以修改


取消关联

git remote rm origin

把本地库的所有内容推送到远程库上


git push -u origin master

#-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。



远程同步


# 下载远程仓库的所有变动
$ git fetch [remote]

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all




创建于合并分支


Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。

一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：

git-br-initial

每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长

当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上

从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：


xhhydeMac-mini:~ xhhy$ cd gitconfig/
xhhydeMac-mini:gitconfig xhhy$ git checkout -b dev    #-b表示创建分支并进入该分支
Switched to a new branch 'dev'
xhhydeMac-mini:gitconfig xhhy$ git branch
* dev
  master

# *好表示当前所在的分支


添加文件做测试

xhhydeMac-mini:gitconfig xhhy$ echo "dev test file" > readme.txt
xhhydeMac-mini:gitconfig xhhy$ ls
README.md	readme.txt



提交到dev分支


xhhydeMac-mini:gitconfig xhhy$ git add readme.txt 
xhhydeMac-mini:gitconfig xhhy$ git commit -m "branch test"
[dev 53aaabe] branch test
 1 file changed, 1 insertion(+)
 create mode 100644 readme.txt

切换到master分支查看


xhhydeMac-mini:gitconfig xhhy$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
xhhydeMac-mini:gitconfig xhhy$ ls
README.md


# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]



合并分支

git merge命令用于合并指定分支到当前分支。


xhhydeMac-mini:gitconfig xhhy$ git merge dev
Updating 8dcea61..53aaabe
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 readme.txt
xhhydeMac-mini:gitconfig xhhy$ ls
README.md	readme.txt
xhhydeMac-mini:gitconfig xhhy$ cat readme.txt 
dev test file


冲突解决

在合并分支的时候如果两个分支中的某个语句,有差异时合并就会有冲突,查看提交文件可以看到冲突部分:

Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1

Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，我们修改如下后保存


Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick AND simple.


再提交

$ git add readme.txt 
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed


rebase


rebase操作可以把本地未push的分叉提交历史整理成直线；

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。  



tag打标签


git tag v.1

#v0.1 是tagname

git tag 显示标签

git tag < commit> id  给对应commit id的版本打标签名为tagname


git show <tagname>  查看标签信息

git tag -d <tagname> 删除本地标签


git push origin <tagname>  推送某个标签到远程


git push origin --tags 可以推送全部未推送过的本地标签；

删除远程标签

git tag -d <tagname>

git push origin :refs/tags/<tagname>



# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 新建一个tag在指定commit
$ git tag [tag] [commit]

# 删除本地tag
$ git tag -d [tag]

# 删除远程tag
$ git push origin :refs/tags/[tagName]

# 查看tag信息
$ git show [tag]

# 提交指定tag
$ git push [remote] [tag]

# 提交所有tag
$ git push [remote] --tags

# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]




# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop 恢复改变并删除快照




# 删除远程仓库的所有记录(包括提交记录,防止项目泄密)

1. 新建另一个远程仓库，命名为B； 
2. 将现有的本地代码提交到远程仓库B； 
3. 删除现有的远程仓库A； 
4. 将远程仓库B命名为A；

