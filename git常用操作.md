# Git操作	- By 尤明
## 基本操作

1. 初始化仓库
	
	- git init
2. 配置作者信息
	- git config --global user.email "youremail@corp.com"
	- git config --global user.name "yourname"
	
3. 添加文件到暂存区
	- git add \<filename\> 
	- git add * (添加所有文件到暂存区)
4. 移除文件
	- git rm \<filename\>
5. 重命名一个文件
	- git mv \<oldfilename\> \<newfilename\>
6. 提交暂存区

	`git commit只会提交暂存区（staged）里面的文件`

	- git commit -m "message"	
7. 查看工作目录的状态
	- git status
8. 查看提交历史记录
	- git log
9. 查看文件改变
	- git diff

---
##撤销操作
1. 撤销加入暂存区的操作
	- git reset HEAD \<file\>
2. 撤销你对原文件的所有修改的操作(要在modify状态相下) 
	- git checkout -- \<file\>
3. 将本地的修改放进回收站
	- git stash
4. 从回收站中恢复本地的修改(会返回到modify的状态)
	- git stash apply
5. 删除tag(可以同时删除多个)
- git tag -d <tagname>.....	
---
## Tag操作
1. 查看tag
	- git tag
2. 创建tag
	- git tag -a v1.0 -m "my version 1.0"
3. 显示tag信息
	- git show v1.0
4. 对之前的提交打tag
	- git tag -a v0.1 -m "version 0.1"


 	
---
## 分支操作
1. 查看分支
	- git branch
2. 创建分支
	- git branch \<branchname\>
3. 删除分支
	- git branch -d \<branchname\>
4. 切换分支
	- git checkout \<branchname\>
5. 合并分支
	- git merge \<branchname\>
6. rebase操作
	- git rebase \<basebranch\> \<newbranch\>
7. HEAD 指当前所在的分支   cat HEAD  查看当前的分支
	*表示当前是哪个分支
8. 解决合并冲突（修改同一个文件会有冲突，不同的文件没有冲突）
   在冲突的文件中直接修改内容（cat file）
	git branch --merged查看已经合并的分支
	git branch --no-merged查看没有合并的分支
9.	git cherry-pick -哈希值（希望只用另一个分支的单个功能）
10.	

---
## 远端仓库操作
1. 克隆一个远端仓库
	- git clone *URL*
	
	当你进行远端操作的时候，初始化不能直接使用init，要用 git init --bara (初始化一个裸仓库)
2. 添加远端仓库
	- git remote add \<name\> \<URL\>
3. 更新远端仓库的分支和数据
	- git fetch \<name\> 
4. 获取并合并远端仓库的分支到当前分支
	- git pull \<reponame\> \<branchname\>
	- eg: `git pull origin master`
5. 上传本地分支和数据到远端仓库
	- git push \<reponame\> \<branchname\>
	- eg: `git push origin master` 
	删除上传到本地的分支和数据
	- git push origin :<branchname>   
	-eg:`git push origin :分支名`
6. 跟踪远端仓库上的分支
	- git checkout --track origin/testbranch
	- git checkout -b test origin/testbranch

---