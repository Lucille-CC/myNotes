

![](https://github.com/Lucille-CC/pictures/blob/master/git%E4%BB%93%E5%BA%93%E7%A4%BA%E6%84%8F%E5%9B%BE.png?raw=true)


	workspace：工作区
	staging area：暂存区/缓存区
	local repository：版本库或本地仓库
	remote repository：远程仓库

#<font color=red>git工作流程</font>


![](https://github.com/Lucille-CC/pictures/blob/master/git%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B.png?raw=true)

	1. 克隆 Git 资源作为工作目录。
	2. 在克隆的资源上添加或修改文件。
	3. 如果其他人修改了，你可以更新资源。
	4. 在提交前查看修改。
	5. 提交修改。
	6. 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交。

#<font color=red>git基本操作</font>
##<font color=green>创建仓库命令</font>
###git init 
	//用于在工作区目录中创建新的本地仓库
	//需要先 cd dir

###git clone [url]  
	//拷贝一个远程仓库到本地仓库
	//git clone https://github.com/testRepository

##<font color=green>远程操作命令</font>
###git remote
	//远程操作命令
	1. git remote  # 显示当前配置下的所有远程分支
	2. git remote show [url]  # 显示某远程仓库的信息
	3. git remote add [remotename] [url]  #为远程仓库起别名，默认为origin
	4. git remote rm [remotename]  # 删除远程仓库
	5. git remote rename [oldname] [newname]


###git fetch
	//从远程仓库获取代码库，命令执行完后需要执行 git merge 远程分支到你所在的分支，一般直接用git pull 一步完成
	git fetch [alias]  # 告诉 Git去获取它有你没有的数据
	git merge [alias]/[branch]  # 尝试合并内容

###git pull
	//下载远程文件并合并
	git pull <远程主机名> <远程分支名>:<本地分支名>
	1. git pull
       git pull origin
	2. git pull origin master:brantest  # 将远程主机 origin 的 master 分支拉取过来，与本地的 brantest 分支合并
	3. git pull origin master  # 如果远程分支是与当前分支合并，则冒号后面的部分可以省略

###git push
	//上传远程代码并合并
	git push <远程主机名> <本地分支名>:<远程分支名>
	1. git push <远程主机名> <本地分支名>  # 如果本地分支名与远程分支名相同，则可以省略冒号
	2. 

##<font color=green>提交命令</font>
###git add 
	//将工作区该文件添加到暂存区
	1. git add [file1][file2]...    //添加文件
	2. git add [dir]  //添加某一文件夹
	3. git add .    //添加当前目录的所有文件
	
###git commit
	//将暂存区文件添加到本地仓库
	1. git commit -m <message>
	2. git commit [file1] [file2] ... -m <message> //提交指定文件
	3. git commit -am <message>  //-a 参数设置修改文件后不需要执行 git add 命令，直接提交

##<font color=green>修改命令</font>
###git reset
	//回退版本，可以指定退回某一次提交的版本
	参数:
	1. --mixed 默认参数，可省略。用于重置暂存区的文件与上一次的提交(commit)保持一致，工作区文件内容保持不变
		git reset HEAD^            # 回退所有内容到上一个版本  
		git reset HEAD^ hello.php  # 回退 hello.php 文件的版本到上一个版本  
		git reset 052e            # 回退到指定版本
	2. --soft 用于回退到某个版本
		git reset --soft HEAD~3 # 回退上上上一个版本
	 or git reset --soft HEAD^^^
	 or git reset --soft 052e
	3. --hard (慎用)参数撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有信息提交

###git rm
	//命令用于删除暂存区/工作区的文件
	1. git rm <file>     # 从暂存区和工作区中删除
	2. git rm -f <file>  # 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f
	3. git rm --cached <file>  # 仅从暂存区域移除，仍保留在当前工作区中 使用 --cached

###git mv
	//用于移动或重命名一个文件、目录或软连接
	git mv [file] [newfile]

##<font color=green>查看命令</font>
###git status
	//查看在上次从工作区提交到暂存区后是否有对文件进行再次修改
	//可使用 -s 来获得简短的输出结果 git status -s

###git diff
	//比较文件的不同，即比较文件在暂存区和工作区的差异
	1. git diff <file>           # 显示某一文件在暂存区和工作区的区别
	2. git diff --cached <file>
	or git diff --staged <file>  # 显示上一次commit前工作区与当前暂存区的差异
	3. git diff [first-branch]...[second-branch]  # 两次提交之间的差异

###git log
	//查看历史提交记录


###git blame <file>
	//以列表形式查看指定文件的历史修改记录

##<font color=green>仓库配置命令</font>
###git config
	1. git config --list      # 显示当前配置信息
	1. git config --global user.name 'runoob'          # 修改用户名
	2. git config --global user.email test@runoob.com  # 修改邮箱
	注：若去掉'--global'则只对当前仓库有效

#<font color=red>git分支管理</font>
##列出分支
	git branch

##创建分支
	git branch [branchname]

##切换分支
	git checkout [branchname]
		// git checkout -b [branchname]   创建新分支并立即切换到该分支下

##删除分支
	git branch -d [branchname]

##合并分支
	git merge [otherbranch]    # 将任何分支合并到当前分支
	当两个分支在合并前修改了同一个文件，那么这个文件会产生冲突，需要手动修改

#<font color=red>git标签</font>
##给某一版本打标签	
	git tag -a v0.9 	#  当前版本
	git tag -a v0.9 85fc7e7   # 85fc7e7版本