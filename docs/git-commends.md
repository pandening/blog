版本控制系统git使用总结
=========================

首先，为了避免每次提交都需要输入用户名和密码，配置一个全局的用户信息：
```
    git config --global user.name "pandening"
    git config --global user.email "1425124481@qq.com"
```

-> 如何初始化一个git仓库?
```
    cd dir
    git init
```

-> 如何克隆或者复制一个项目?
```
    git clone [url] 
    example: git clone https://github.com/pandening/storm-ml.git
```

-> 如何添加新的文件到仓库?
```
   git add [file]
   git add 命令会将更改添加到缓存中，需要使用命令:
   git commit -m [ comment ] 来提交才能有效
   git status命令可以查看文件的状态
```

-> git diff
-> git diff --cached
-> git diff HEAD
```
如果没有其他参数，git diff 会以规范化的 diff 格式（一个补丁）显示自从你上次提交快照之后尚未缓存的所有更改.
 + --cached :查看已经缓存的改动 
 + HEAD:查看所有改动，包括没有缓存的改动
```

-> 如何取消缓存呢？
```
  git reset HEAD --file:这样file就会被取消缓存，这时候提交就不会提交file了
  git rm file:这个命令不仅仅取消缓存，而且会将内容剔除
  git mv file-from file-to:移动缓存
```

-> 分支

```
   git branck [branch name]:创建分支
   git checkout [branch name]:切换分支
   git branch:可以罗列出所有的分支
   git checkout -b [branch name]:切换分支，如果没有该分支则先创建，然后切换进入
   git branch -d [branch name]:删除分支
   
   git merge [branch name]:将该分支的内容合并到当前的分支上
   
   git log:显示当前分支中的提交历史
   git log --oneline:紧凑显示
   git log --oneline --graph:开启拓扑图
   
   git push [alias] [branch]:将branch分支推送到远端alias的branch分支
```

常用命令详述
-------------------
```
=========================================
|查看、添加、提交、删除、找回，重置修改文件 |
=========================================

git help <command> # 显示command的help

git show # 显示某次提交的内容 git show $id

git co -- <file> # 抛弃工作区修改

git co . # 抛弃工作区修改

git add <file> # 将工作文件修改提交到本地暂存区

git add . # 将所有修改过的工作文件提交暂存区

git rm <file> # 从版本库中删除文件

git rm <file> --cached # 从版本库中删除文件，但不删除文件

git reset <file> # 从暂存区恢复到工作文件

git reset -- . # 从暂存区恢复到工作文件

git reset --hard # 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改

git ci --amend # 修改最后一次提交记录

git revert <$id> # 恢复某次提交的状态，恢复动作本身也创建次提交对象

git revert HEAD # 恢复最后一次提交的状态

===========================
|       查看文件diff       |
===========================

git diff <file> # 比较当前文件和暂存区文件差异 git diff

git diff <id1><id1><id2> # 比较两次提交之间的差异

git diff <branch1>..<branch2> # 在两个分支之间比较

git diff --staged # 比较暂存区和版本库差异

git diff --cached # 比较暂存区和版本库差异

git diff --stat # 仅仅比较统计信息

================
|  查看提交记录 |
================

git log git log <file> # 查看该文件每次提交记录

git log -p <file> # 查看每次详细修改内容的diff

git log -p -2 # 查看最近两次详细修改内容的diff

git log --stat #查看提交统计信息

==================
| 分支合并和rebase|
==================

git co <branch> # 切换到某个分支

git co -b <new_branch> # 创建新的分支，并且切换过去

git co -b <new_branch> <branch> # 基于branch创建新的new_branch

git co $id # 把某次历史提交记录checkout出来，但无分支信息，切换到其他分支会自动删除

git co $id -b <new_branch> # 把某次历史提交记录checkout出来，创建成一个分支

git merge <branch> # 将branch分支合并到当前分支

git merge origin/master --no-ff # 不要Fast-Foward合并，这样可以生成merge提交

git rebase master <branch> # 将master rebase到branch，相当于： git co <branch> && git rebase master && git co master && git merge <branch>

=========================================
| Git补丁管理(方便在多台机器上开发同步时用) |
==========================================

git diff > ../sync.patch # 生成补丁

git apply ../sync.patch # 打补丁

git apply --check ../sync.patch #测试补丁能否成功

==============
| Git暂存管理 |
==============

git stash # 暂存

git stash list # 列所有stash

git stash apply # 恢复暂存的内容

git stash drop # 删除暂存区

===================
| Git远程分支管理  |
===================

git pull # 抓取远程仓库所有分支更新并合并到本地

git pull --no-ff # 抓取远程仓库所有分支更新并合并到本地，不要快进合并

git fetch origin # 抓取远程仓库更新

git merge origin/master # 将远程主分支合并到本地当前分支

git co --track origin/branch # 跟踪某个远程分支创建相应的本地分支

git co -b <local_branch> origin/<remote_branch> # 基于远程分支创建本地分支，功能同上

git push # push所有分支

git push origin master # 将本地主分支推到远程主分支

git push -u origin master # 将本地主分支推到远程(如无远程主分支则创建，用于初始化远程仓库)

git push origin <local_branch> # 创建远程分支， origin是远程仓库名

git push origin <local_branch>:<remote_branch> # 创建远程分支

git push origin :<remote_branch> #先删除本地分支(git br -d <branch>)，然后再push删除远程分支

======================
|  Git远程仓库管理    |
======================

git remote -v # 查看远程服务器地址和仓库名称

git remote show origin # 查看远程服务器仓库状态

git remote add origin git@ github:robbin/robbin_site.git # 添加远程仓库地址

git remote set-url origin git@ github.com:robbin/robbin_site.git # 设置远程仓库地址(用于修改远程仓库地址)

git remote rm <repository> # 删除远程仓库

===================
|  创建远程仓库    |
===================

git remote add origin git@ github.com:username/repository.git # 设置远程仓库地址

git push -u origin master # 客户端首次提交

git push -u origin develop # 首次将本地develop分支提交到远程develop分支，并且track

git remote set-head origin master # 设置远程仓库的HEAD指向master分支

```

参考:</br>
[http://www.ihref.com/read-16369.html]</br>
[http://blog.bcmeng.com/post/git.html]</br>
