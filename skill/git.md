title: "git常用操作"

date: 2018-08-16 20:47:20 +0800

update: 2018-08-16 20:47:20 +0800

author: me

tags: 

- 技术笔记

------

##### Clone（下载）项目

git clone project-url  #按照gerrit上的提示操作即可

##### 查看状态

git status   #最常用的操作，会给出明确的提示

##### 查看分支（tag）

git br      #查看本地分支，当前分支颜色是绿色，前面有个 *
git br -a   #查看所有分支，其中的远程分支可用户切换分支使用

##### 切换分支

git co branch-name  #branch-name指已经存在的分支名称

##### 新建分支

git br branch-name    #从当前分支的当前commit新建一个分支
git co branch-name    #切换到新建的分支，切换的时候会携带未提交的修改

##### 拉取远程修改

git pull    #会拉去包括tag在内的信息

##### 提交修改

git add . –A    #将当前目录下的所有更改都添加到缓存区
git ci          #将缓存区的更改提交到本地库

##### 打标签

git tag tag-name            #在当前commit上打一个tag
git tag tag-name commitId   #在commitId上打一个tag

##### 推送修改

git push          #将当前分支的更改推送到远程分支
git push –tags    #将当前分支及所有新打的tag推送到远程服务器

##### 合并分支

git co master   #将当前分支切换到master
git merge dev   #将dev分支的修改切换到master

##### 删除远程分支或者tag

git push origin –delete branch-name     #删除远程分支，需要gerrit权限
git push origin --delete tag tag-name   #删除远程tag，需要gerrit权限

#### 重写上一次注释

#如果提交后立即发现注释写错了，即只需要重写上一次的注释，在这些如下命令；
git ci –amend

##### 重写前几次注释

慎用git reset操作，尤其是reset已经推送到远程服务器上的commit。
如果发现之前的几次提交注释写错了，找到写错注释的前一个commitId，假设为abcdefg，执行
git reset –-soft abcdefg    #类似将abcdefg之后的git ci命令取消，并且将git add命令合并得到的结果。然后执行
git ci                      #和正常提交一样写注释。

##### 取消发布（慎用！配合操作！）

场景：1.0.0已经发布，计划功能F1，F2，F3，以及不过B1，B2需要在1.1.0发布，
分支F1，F2，F3，B1，B2都已经合到dev上了，并且已经发布了1.1.0-b1，1.1.0-b2两个beta版。
此时F1功能因种种原因需要取消，此时需要执行如下操作：
git co dev           #切换到dev分支。
git tag -d 1.1.0-b1  #删除本地tag 1.1.0-b1
git tag -d 1.1.0-b2  #删除本地tag 1.1.0-b2
git reset –hard 1.0.0  #将所有合并撤销，恢复dev到1.0.0的状态。
git merge F2   #重新合并F2
git merge F3   #重新合并F3
git merge B1   #重新合并B1
git merge B2   #重新合并B2
git tag 1.1.0-b1 #重新打tag 1.1.0-b1
git push origin –delete tag 1.1.0-b1 #删除远程分支1.1.0-b1
git push origin –delete tag 1.1.0-b2 #删除远程分支1.1.0-b2
git push -f origin dev #使用本地dev分支强制更新远程dev分支
git push -tags   #推送本地tag到远程服务器