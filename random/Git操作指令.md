# Git操作命令
### 创建仓库 和 获取远程仓库内容
- git init
- git fetch <主机名> <分支名>

### 提交文件
- git add filename
- git add ...
- git commit -m "message" <br>
  '-m' 参数是必须的，如果不想携带任何信息就提交（commit）本次更改，则需要另外的参数，但不鼓励这样做。如果直接 git commit 易导致错误，且无法正确提交

### 查看操作日志
- git log
- git log --pretty=oneline // 把每条commit日志合并为一条
- git reflog // 查看全部操作的历史记录及其对应的commit id， 通常用于在当前版本是回退版本的情况下，回到未来版本
- git log --graph --pretty=oneline --abbrev-commit // 打印日至并显示图标

### 重回历史版本 和 恢复未来
注：head指针指向的版本为［当前版本］， head之后的为［历史版本］，head之前的为［未来版本］

- git reset --hard HEAD^ // 回到上1个版本，HEAD^^ 回到上两个版本，HEAD~100 回到上100个版本
- git reset --hard "commit id" // 这个id如果比当前版本新，则回到未来版本

### 比较文件
-  git diff HEAD -- filename // 比较版本库的当前版本和工作区文件的差别

### 撤销修改
- git checkout -- file // 撤销发生在工作区的修改
- git reset HEAD file // 撤销发生在缓存区的修改
注：如果撤销发生在版本库中的修改，请使用版本回退

### 删除文件步骤
- rm filename // 删除工作区文件
- git rm filename // 将删除文件操作，提交到暂存区
- git commit -m "message" // commit 删除文件操作到版本库

如果不小心误删了文件，可以参考撤销修改的办法恢复。前提是版本库里有这个文件。

### 远程仓库的推送和关联
- git remote add origin https://github.com/dylan2wang/tutorial // 将本地仓库和远程仓库进行关联，origin是远程仓库的默认名，其后面跟的参数是远程仓库的网址，github上，具体到某个仓库的网址
- 第一次推送时，使用：git push -u origin master 命令，-u 会使得在推送之余，将本地仓库和远程仓库关联起来
- 以后的推送：git push origin master 

说明：origin表示远程主机，上面命令的含义是，推送到远程名为origin主机的master分支上。

### 克隆远程仓库
- git clone "远程仓库的地址"

注：github支持多种协议，https， ssh等，所以远程仓库的地址也会有多种形式，例如：https://github.com/dylan2wang/tutorial 和 git@github.com:michaelliao/gitskills.git等

### 分支的基本操作
- git branch // 查看分支
- git branch name // 创建分支
- git checkout name // 切换分支
- git checkout -b name // 创建并切换分支
- git merge name // 合并某分支到当前分支
- git merge --no-ff -m "merge with no-ff" name // 以非fast方式合并分支，--abbrev-commit 使得commit id以简短值方式显示
- git branch -d name // 删除分支
- git branch -D name // 强行删除分支
- git branch -a // 查看全部分支，包括远程和本地
- git branch -r // 查看全部远程分支

创建并以 fast-forward 方式合并一个分支的一般步骤：

1. 创建分支，newbranch，并切换到新分支下
2. 在工作区进行修改，然后提交（影响的是newbranch）
3. 将head切换到master分支
4. 合并master和newbranch
5. 删除newbranch

### 创建临时分支
应用场景：当前正在dev分支上开发， 但还未开发完成，此时临时有一个bug需要修复。可以暂时将dev分支的工作现场，通过stash功能储藏起来，等修复好bug后继续工作

1. git stash // 将当前dev分支贮藏起来
2. git checkout master //在哪个分支上要修复bug，就在哪个分支上创建临时分支，假定在master分支上需要修复bug
3. git checkout -b issue-101 // 假设要创建代号为101的bug，创建该分支
4. git add file // 修复bug后，提交
5. git commit -m "fix bug 101"
6. git checkout master
7. git merge --no-ff -m "merge bug fix 101" issue-101
8. git branch -d issue-101 // 删除临时分支
9. git checkout dev // bug 修复的工作结束后，回到dev分支
10. git stash pop // 恢复之前dev的工作现场，并把stash相应内容删除

注：

- git stash apply // 也可以恢复现场，但stash内容并不删除
- git stash list // 可以查看全部现场快照
- git stash apply stash@{0} // 可以恢复指定现场，针对进行多次stash的情况

### 多人协作的基本模型

假设在dev分支上

- 首先，可以试图通过 git push origin dev 推送自己的修改；
- 如果推送失败，则是因为远程分支比你本地的版本新，需要先用git pull试图合并；
- 如果合并有冲突，则解决冲突，并在本地提交；
- 没有冲突或解决掉冲突后，再用 git push origin dev 推送就能成功
- 如果 git pull 提示 “no tracking information”，则说明本地分支和远程分支的链接关系没有建立，用命令 git branch --set-upstream dev origin/dev 就可解决；

其它命令：

- 在本地创建和远程分支对应的分支：git checkout -b branch-name origin/branch-name，本地和远程的分支名最好一致

### 打标签

name 是标签名

- git tag name // 新建一个标签，默认是当前最新提交的commit
- git tag -a name -m "message" // 新建一个带说明信息的标签
- git tag -a name -m "message" commitId // 可以给指定commitid的内容打标签
- git show name // 显示标签信息
- git tag -s name -m "message" // 创建用PGP签名的标签，ps：这个比较高级，有些防伪权限的意味，暂时用不到
- git tag -d name // 删除本地标签
- git push origin name // 推送一个本地标签
- git push origin --tags //  推送全部未推送过的本地标签
- git push origin --delete tag name // 删除一个远程标签

注：如果删除一个未推送的本地标签，直接执行命令删除即可，如果删除一个已经推送的标签，先删除本地，再执行远程删除命令

### 其它临时记录
- 假设远程目前只有master一个分支，本地有master和dev两个分支，则在dev分支下，执行 push dev origin dev 操作后，会自动在远程创建dev分支并推送成功，ps：不清楚这样操作是否符合规范
- 远程dev和master的分支，应该是在github网站上进行，还是统一由本地推送？
- 在本地，master去merge dev时，产生了冲突，进行手工修复后，发现只更改了master下的版本，切换到dev版本后，还是没有变化，那以后再次合并时不会又要产生冲突了？