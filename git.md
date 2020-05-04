### 一、创建Git	

#### 	1. 创建目录

  ```
  $ mkdir learngit
  $ cd learngit
  $ pwd (可以查看目录)
  /Users/12345/learngit
  ```

#### 	2. 把目录变成git可以管理的仓库

  ```
  $ git init
  ```

#### 	3. 在目录下编写文件

#### 	4. 添加文件

  ```
  $ git add 文件名
  $ git commit -m"注释解释"
  ```

#### 	5. 查看工作区状态

  ```
  $ git status
  //可以为'clean'或者'改变未提交'以及'改变已添加未提交'
  ```

#### 	6. 查看修改

  ```
  $ git diff 文件名
  ```

#### 	7. 查看提交日志（可加--pretty=oneline参数简化）

  ```
  $ git log
  ```



### 二、版本回退

#### 	

#### 	1. 版本回退

  ```
  $ git reset --hard HEAD^(一百次就是HEAD~100)
  ```

#### 	2. 回到未来

  ```
  $ git reset --hard 版本号
  Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD从指向你想要的版本
  版本号可以用git reflog查看
  ```

#### 	3. 工作区和暂存区

    工作区：文件夹目录
    版本库：工作区的隐藏目录.git。包含了stage的暂存区，自动创建的master分支以及master的一个指针HEAD。
    因此，git add是把文件添加到暂存区stage上，git commit就是把暂存区的内容一起提交到当前分支。

#### 	4. Git的特点

    Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

#### 	5. 查看工作区和版本库里面最新版本的区别

    $ git diff HEAD -- 文件名

#### 	6. 撤销修改

    *丢弃工作区的修改, git checkout -- file
    *丢弃添加到暂存区的修改，git reset HEAD <file>, 然后 git checkout -- file
    *撤销本次提交操作， 使用版本回退（没有推送到远程库）

#### 	7. 删除文件

    对于提交的文件
    工作区删除：直接删除文件，用$ rm <file>
    版本库删除：$ git rm <file>, 并且git commit
    误删恢复：$ git checkout --<file>（实质为用版本库里的版本替换工作区的版本，无论工作区是修改或者删除都可以“一键还原”）
    tip：未被添加到版本库就删除的文件无法恢复。
    tip2：先手动删除文件，然后使用 git rm <file> 和 git add <file>效果是一样的

###  

### 二、 远程仓库



#### 	1.	添加远程库Q-S-yue（分布式版本系统）
	首先在github上new repo
	在本地仓库运行git remote add origin git@github.com:Q-S-yue/learngit.git命令，关联远程库
	git push -u origin master，第一次推送
	git push origin master, 推送最新提交

#### 	2. 	从远程库克隆
	首先在github上准备远程库
	命令git clone git@github.com:Q-S-yue/filename.git克隆一个本地库。
	git支持多种协议，包括https，但是ssh协议速度最快。



### 三、 分支管理



#### 	1. 创建和合并分支
	$ git checkout -b 分支名(令人迷惑，毕竟checkout还有撤销修改的作用)
	等于 $git branch  +$git checkout dev
	$ git switch -c 分支名
#### 	2. 切换到已有分支
	$ git switch 分支名
	$ git checkout 分支名
#### 	3. 查看当前分支
	git branch
#### 	4. 合并指定分支到当前分支
	git merge dev
#### 	5. 删除分支
	git branch -d 分支名(提交合并之后的分支)
	git brach -D 分支名（强行删除未合并的）
#### 	6. 解决冲突
	当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
	解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
	用git log --graph命令可以看到分支合并图。
#### 	7. Fast forward模式
	分支合并时，默认使用Fast forword模式，但删除分支后会丢掉分支信息。
	可用--no-ff的方式禁用fast forword模式，命令为$ git merge --no-ff -m "备注" 分支名。合并后的历史有分支，能看出来曾经做过合并。
#### 	8. bug分支处理
	$ git stash ,储藏当前工作现场，用status查看干净就可以啦，然后去相应的分支改bug，改完后合并并删除分支bug，可以用git stash list 查看刚才的工作现场。最后git stash pop 相当于git stash apply恢复加上git stash drop。这个时候你再git stash list就看不到内容了。（恢复的时候可以恢复指定的stash，$ git stash apply stash@{0} ）
	cherry-pick命令，复制一个特定的提交到当前分支
#### 	9. Feature分支（新功能）处理
	开发新功能，新建分支。如果要丢弃一个没有被合并过的分支，git brach -D 分支名 强行删除
####	10. 多人协作
	*查看远程库信息：git remote/git remote -v（更详细）
	*推送分支： git push 远程仓库名（一般是origin） 分支名
	*抓取分支： git pull
	*多人协作的工作模式通常是这样：
		首先，可以试图用git push origin <branch-name>推送自己的修改；
		如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
		如果合并有冲突，则解决冲突，并在本地提交；
		没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！
		如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。
####	11. Rebase
	*rebase操作可以把本地未push的分叉提交历史整理成直线；
	*rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

### 四、标签管理

#### 1. 标签管理
	标签是版本库的快照，是指向某个commit的指针（跟分支很像，但是分支可以移动，标签不能移动），创建和删除标签都是瞬间完成的。

#### 2. 创建标签
	创建：$ git tag <name>
	查看所有标签：$ git tag
	默认标签是打在最新提交的commit上的。如果想在以前的提交打上标签，方法是找到历史提交的commit id： $ git log --pretty=oneline --abbrev-commit，然后打上标签： $git tag <name> id(如f52c633)
	创建带有说明的标签：$ git tag -a v0.1 -m "version 0.1 released(版本v0.1发布)" 1094adb
	查看具体标签信息：$ git show <tagname>，可以看到文字说明

#### 3. 操作标签
	删除标签（本地）：git tag -d v0.1
	推送远程：git push origin <tagname>
	推送所有标签：git push origin --tags
	删除标签（远程）：先本地删除，再远程删除git push origin :refs/tags/tagname

### 五、使用github

#### 1.复制github上的项目：
	点击Fork在自己账号下克隆一个仓库，然后从自己账号下clone。在GitHub上，可以任意Fork开源仓库。自己拥有Fork后的仓库的读写权限。可以推送pull request给官方仓库来贡献代码。


​	
### 六、使用码云
	git remote add origin git@gitee.com:liaoxuefeng/learngit.git
	本地库可以同时与多个远程库互相同步，但不能重复命名远程库，如果重复可用git remote rm origin删除不想要的那一个或者换个名字?
	查看远程库信息：git remote -v

### 七、自定义Git
	git config --global color.ui true，设置颜色

#### 1.忽略特殊文件
	·在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore
	·忽略文件的原则是：
		忽略操作系统自动生成的文件，比如缩略图等；
		忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过	另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如	Java编译产生的.class文件；
		忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
	·强制添加被gitignore忽略的文件，git add -f 文件名
	·检查（添加不上某个文件）gitignore的错误：git check-ignore -v 文件名
	·gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！

#### 2.配置别名
	$ git config --global alias.别名 原名
	如，以后st就表示status，用co表示checkout，ci表示commit，br表示branch。
	$ git config --global alias.unstage 'reset HEAD'：把命令git reset HEAD 文件名（把暂存区的修改撤销掉）配置一个unstage别名。
	$ git config --global alias.last 'log -1'：配置一个git last，让其显示最后一次提交信息：
	$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit":好像可以配置颜色
$ git config --global alias.unstage 'reset HEAD'
	PS：--global参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。不加则只是对当前仓库有用。
	PS：当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中，当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中。
	

### 八、搭建git服务器