#进驻gitHub前的准备工作
4/11/2017 8:44:28 PM 

----------

git官网：[https://git-scm.com/](https://git-scm.com/)  
#### 在系统上安装git
可使用源代码安装或者是下载客户端进行安装，目测在windows系统上使用客户端安装比较方便，甚至可以使用git for windows软件进行版本控制。  
#### 初始化仓库  
- 新建一个git仓库目录：mkdir directory  
- 进入目录初始化：cd directory --> git init  
#### 常用的git命令  
- **git add** 将目录内修改的文件添加到修改区，可指定某个文件 git add directory，也可以是使用全局git add .  
- **git status** 查看仓库内文件的状态，可以哪些文件做了修改，以及是否存放进修改区以待提交  
- **git diff** 查看文件内修改内容与原先的差异，二进制文件无法进行查看，静态文本文件最好使用utf-8编码格式进行存储  
- **git commit -m "some description"** 提交修改区的文件，同时-m后的描述是对本次更改操作的一个描述  
- **git log** 查看仓库提交的日志文件，加上参数 --pretty=oneline 可以进行简单的查看  
- **git reset** 回退版本， 上一个版本以 HEAD^ 表示，上上个版本以 HEAD^^ 表示，更早的版本采用 HEAD~100 表示。也可以使用在存储过程中产生的版本号的前几位来进行回退，git会自动查找并进行回退。eg: git reset --hard HEAD^ 回退到上个版本  
- **git reflog** 记录每一次命令  
- **git checkout -- file** 让修改文件撤回到暂存区或者是最后一次提交的版本，如果自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态  
- **git reset HEAD file** 把暂存区的修改撤销掉  
- **rm file** 可以删除工作区中的指定文件，误删可以使用git checkout -- file 进行回退。如果真的确认要从项目中删除指定文件，则可以使用git rm file 同时上传仓库git commit  
#### 创建一个远程仓库的SSH key 
在git bash命令行里面执行 ssh-keygen -t rsa -C "youremail@example.com"，则可以在系统用户目录里看到所生成的id_rsa.pub公钥文件，将文件里的内容添加到github网站上的个人用户中，则有权进行仓库上传
#### 创建一个github仓库
* 在github上创建一个空repositories
* 在git bash上运行github给的提示：git remote add origin git@github.com:BendMoly/gitTest.git（上传例子）
* 将本地的目录推送到服务器仓库中：git push -u origin master（第一次推送加参数-u，可以将上传的分支进行关联，以后就可以简化命令）
#### 克隆一个远程仓库
git clone git@github.com:BendMoly/gitTest.git（克隆仓库时需确定仓库有readme文件）
#### 使用分支
* 查看分支：git branch
* 创建分支：git branch <name>
* 切换分支：git checkout <name>
* 创建并切换分支：git checkout -b <name>
* 合并某分支到当前分支：git merge <name>
* 删除分支：git branch -d <name>
#### git冲突
当git上的分支在文件上有冲突时，在冲突文件上git会用>>>>>>>,======,<<<<<<进行标出，在修改完冲突部分的内容再保存到暂存区并提交，这样就能合并，可以使用带参数的git log进行查看git log --graph --pretty=oneline --abbrev-commit
#### 暂时保存工作区
对于没有完成的工作，突然需要去其他分支进行开发时，可以通过使用git stash来进行暂存，而后回到当前分支时，可先查看stash内的暂存列表git stash list，之后使用两种办法进行恢复：  

- 使用git stash apply / git stash drop，这种方式为先恢复后删除暂存区   
- 使用git stash pop，这种方式会直接恢复并删除暂存区
#### 删除
对于没有合并的分支，如果要进行强制删除，需要使用大写D来进行删除：git branch -D <name>
#### 多人协作
* 查看远程库信息，使用git remote -v
* 本地新建的分支如果不推送到远程，对其他人不可见
* 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程上最新的提交
* 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name 本地及远程的分支最好一致
* 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name
* 如果抓取后文件有冲突，需优先解决冲突
#### 打标签
* git tag <name> 默认为HEAD，也可以指定一个commit进行打标签，如 git tab <name> commit-id
* git tag -a <name> -m "blablablabla..." 指定标签信息
* git tag -s <tagname> -m "blablablabla..." 用PGP签名标签
* git tag查看所有标签
* git push origin <tagname> 推送一个本地标签
* git push origin --tags 推送本地所有未推送过的标签
* git tag -d <tagname> 删除一个本地标签
* git push origin :ref/tags/<tagname> 删除一个远程标签  // git push origin --delete tag v0.9
* git show v1.0 可以查看版本1.0的commit-id，然后返回到此版本就可以进行操作
#### 忽略特殊文件
* 创建一个特殊文件.gitignore，具体忽略规则 https://github.com/github/gitignore
* 创建完后提交到git
* git add -f node_modules 将文件强制添加到git
* git check-ignore -v node_modules 检查哪条命令限制了文件的提交

#### 预告
目前才开发vue版本的博客的同时，也在对公司的产品进行优化，会对所应用的知识点进行分享，同时也希望各位能多多指教。  
**下一期：** gulp构建过程中项目针对性gulpfile的使用


