# Git学习笔记

## 1、学习目标

Git的学习目标：

- 了解Git基本概念
- 概述Git工作流程
- 使用Git常用命令
- 熟悉Git代码托管服务
- 使用IDEA操作Git



## 2、Git与版本控制工具

### 2.1、Git应用场景

1. 数据备份

   实现项目、数据备份功能。

2. 代码还原

   项目的版本控制，可以通过备份的项目版本实现代码还原功能。

3. 协同开发

   一个大的项目在实际开发中，往往会多人协作开发，通过Git可以实现多人同步开发同一项目。

4. 追溯问题代码的编写人和编写时间

   可通过各个开发人员上传的各个版本的代码，来找到有问题的代码，并且可以根据上传的信息定位到是谁在什么时候编写的代码，在发生重大问题时方便追责问责。

### 2.2、版本控制器的方式

1. 集中式版本控制工具 

   集中式版本控制工具，版本库是集中存放在中央服务器的，每个人中央服务器下载代码，必须联街局域网或互联网才能下载。个人修改后然后提交到中央版本库。常见的有SVN和CVS。
   存在问题：如果服务器宕机或者网络中断，会导致代码丢失，因此现在弃用了。

2. 分布式版本控制工具

   分布式版本控制系统没有中央服务器，取而代之的是远程仓库，并且每个人的电脑上都是一个完整的版本库（本地仓库），这样工作的时候，无需要联网了，因为版本库就在你自己的电脑。这样即使远程仓库丢失，也能从每台电脑上进行数据恢复。最常用的就是Git。

## 3、Git安装与常用命令

### 3.1、Git简介

Git的优势：

1. 速度
2. 简单的设计
3. 对非线性开发模式的强力支持（即多人同时开发）
4. 完全分布式
5. 有能力高效管理超大规模项目



- Git工作流程图

![Image 01](https://github.com/PcLyrer/study/blob/main/Image/01.png?raw=true)



常用命令：

1. clone(克隆)：从远程仓库中克隆代码到本地仓库
2. checkout(检出)：从本地仓库检出一个仓库分支然后进行修订
3. add(添加)：在提交前先将代码提交到暂存区
4. commit(提交)：提交到本地仓库。本地仓库中保存修改的各个历史版本。
5. fetch(抓取)：从远程库，抓取到本地库，不进行任何的合并动作，一般操作比较少
6. pull(拉取)：从远程库拉到本地库，自动合并(merge)，然后放到工作区，相当于fetch+merge
7. push(推送)：修改完成后，将代码推送到远程仓库

Git中常用的Linux命令：

````
ls/ll	查看当前目录（ll查看隐藏文件）
cat		查看文件内容
touch	创建文件
vi		vi编辑器
````



### 3.2、Git安装与配置

本机安装地址：D:\program files (x86)\Git\Git

版本：Git-2.36.1

- Git工具
  1. Git GUI Here：Git提供的图像化界面工具
  2. Git Bash：Git提供的命令行工具

- Git基本配置

  `````
  配置用户名
  git config --global user.name "xxx" 
  配置邮箱
  git config --global user.email "xxx@xx.com" 
  查看用户名
  git config --global user.name
  查看邮箱
  git config --global user.email
  `````

- 为常用的指令配置别名（简化指令）

1. 打开用户目录(user目录)，创建 .bashrc文件

   ````
   使用指令：
   touch ~/.bashrc
   或者手动创建
   ````

2. 往 .bashrc文件中输入以下内容

   ````
   # 用于输出git提交日志
   alias git-log='git log --pretty=oneline --all --graph --abbrev-commit'
   # 用于输出当前目录所有文件及基本信息
   alias ll='ls -al'
   ````

3. 打开GitBash，执行source ~/.bashrc，使文件生效

### 3.3、建立本地仓库

使用Git对代码进行版本控制，首先需要获取本地仓库，以下是创建本地仓库的步骤：

1. 创建本地仓库文件夹，路径如下：E:\Download\GitLocalRepository
2. 进入该文件夹
3. 在这个文件夹中打开Git bash
4. 执行git init 命令
5. 会在该目录下生成.git 隐藏的文件

### 3.4、基础操作指令

Git工作目录下对于文件的修改(增加、删除、更新)会存在几个状态，这些修改的状态会随着我们执行Git的命令而发生变化。

![Image 02](https://github.com/PcLyrer/study/blob/main/Image/02.png?raw=true)

首先掌握Git使用命令在这些状态的转换：

````
1. git status	(查看文件当前的状态)
2. git add		(工作区 --> 暂存区)
3. git commit   (暂存区 --> 本地仓库)
4. git log		(查看提交到仓库的文件，或称为日志)
````

1. 查看修改的状态（status）

   作用：查看的修改的状态（暂存区、工作区）

   命令形式：git status

2. 添加工作区到暂存区（add）

   作用：添加工作区一个或多个文件的修改到暂存区

   命令形式：git add 文件名|通配符

   git add . 将所有修改加入暂存区

3. 提交暂存区到本地仓库（commit）

   作用：提交暂存区内容到本地仓库的当前分支

   命令形式：git commit -m "注释"

4. 查看提交日志（log）

   作用：查看提交记录

   命令形式：git log [option]

   ````
   option：
   --all 				显示所有分支
   --pretty=oneline 	 将提交信息显示为一行
   --abbrev-commmit	 使得输出的commitid更简短
   --graph 			以图的形式显示
   
   实例：
   # 以一行查看
   git log --pretty=oneline 
   # 在上面的基础上以简短的注释信息查看所有提交
   git log --pretty=oneline --abbrev-commit --all 
   # 在上面的基础上以图的形式进行查看
   git log --pretty=oneline --abbrev-commit --all --graph
   
   
   git log --pretty=oneline --all --graph --abbrev-commit
   等价于 git-log（因为前面在.bashrc中设置了别名）
   alias gti-log='git log --pretty=oneline --all --graph --abbrev-commit'
   ````

5. 版本回退

   作用：版本切换

   命令形式：git reset --hard commitID
   - commitID 可以使用 git-log 或者 git log 查看

   如何查看已经删除的记录
   - git reflog
   - 这个命令可以看到已经删除的提交记录

6. 将文件添加至忽略列表

   针对一些文件我们不希望使用Git管理，也不希望出现在未跟踪文件列表，这时可以创建.gitignore的文件（固定文件名），然后进入该文件，列出要忽略的文件模式，如下：

   ````
   /mtk/		# 过滤整个文件夹
   *.zip		# 过滤所有后缀为.zip的文件
   /mtl/a.txt	 # 过滤某个具体文件
   ！index.txt	# 不过滤某个文件
   ````

   

- 实例

  ````
  1. 初始化仓库（首先创建本地文件夹，在这个文件夹中进入git bash）
  git init
  
  2. 将新建文件加入到仓库
  # 创建文件
  touch file01.txt
  # 从工作区添加至暂存区
  git add .
  git add file01.txt
  # 从暂存区添加至仓库，并添加提交说明
  git commit -m "add file01"
  # 查看提交记录
  git log
  # 以精简的方式查看提交记录
  git-log
  
  3. 修改文件后加入到仓库
  # 更新文件内容
  vi file01.txt
  # 添加至暂存区
  git add .
  # 添加至仓库，并添加提交说明
  git commit -m "update file01"
  # 以精简的方式查看提交记录
  git-log
  
  4. 版本回退
  # 回退到 4889c4b 这一版本，commitID可以从git log或者git reflog进行查看
  git reset --hard 4889c4b
  
  
  5. 添加至忽略文件
  # 创建 file.a 文件
  touch file.a
  # 创建忽略文件夹
  touch .gitignore
  # 查看此时状态，file.a和.gitignore两个文件的状态是 Untracked
  git status
  # 编辑忽略文件夹
  vi .gitignore
  	# 在忽略文件夹中，使用通配符忽略所有以.a后缀结尾的文件夹
  	*.a
  # 查看此时状态，只有.gitignore这个文件的状态是 Untracked
  git status
  ````

### 3.5、分支

几乎所有的版本控制系统都以某种形式支持分支。使用分支意味着你可以把你的工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

1. 查看当前分支

   命令：git branch

2. 创建本地分支

   命令：git branch 分支名

3. 查看分支的信息

   命令：git-log

4. 切换分支（checkout）
   
   命令：git checkout 分支名
   
   还可以直接切换到一个不存在的分支（创建并切换）
   
   命令：git checkout -b 分支名
   
5. 合并分支（merge）

   一个分支上的提交可以合并到另一个分支

   命令：git merge 分支名 （将分支合并到当前所在分支上，一般是将其他合并到master上）
   分为快进模式和分支合并模式

6. 删除分支
   不能删除当前分支，只能删除其他分支
   命令：git branch -d b1 删除分支时，需要做各种检查

   命令：git barnch -D b1 不作任何检查，强制删除（慎用）
   强制删除的使用场景：当分支上的内容没有merge到master上时，使用-d删除不了，必须使用-D才能删除

7. 解决冲突

   当两个分支同时修改一个文件，例如修改统一文件的同一行，合并时会产生冲突，因为不知道使用哪个分支上的内容，此时需要手动处理冲突，即：

   1. 处理文件中冲突的地方
   2. 将解决完的冲突加入暂存区
   3. 提交到仓库

   ````
   # 在master分支上修改file01.txt并提交
   vi file01.txt
   	count=1
   git add .
   git commit -m 'update file01 count=1'
   
   # 进入到slave分支
   git checkout -b slave
   # 在slave分支上修改file01.txt
   vi file01.txt
   	count=1
   git add .
   git commit -m 'update file01 count=2'
   
   # 进入master分支
   git checkout master
   # 将slave分支合并到 master分支上
   git merge slave 
   # 此时合并会出现冲突，file01.txt文件内容如下所示：
   <<<<<< HEAD
   count=2
   =======
   count=1
   >>>>>>> slave
   
   # 然后使用修改 file01.txt内容为下
   vi file01.txt
   	count=5
   git add .
   git commit #不添加描述
   ````

8. 分支在开发中的使用原则和规范

   - master（生产）分支

     线上分支，主分支，中小规模项目作为线上运行的应用对应的分支

   
   - develop（开发）分支
   
     是从master创建的分支，一般作为开发部门的主要开发分支，如果没有其他并行开发不同期上线要求，都可以在此版本进行开发，阶段开发完成后，需要是合并到master分支,准备上线。
   

   - feature/xxxx分支

     从develop创建的分支，一般是同期并行开发，但不同期上线时创建的分支，分支上的研发任务完成后合并到develop分支。

   
   - hotfix/xxxx分支
   
     从master派生的分支，一般作为线上bug修复使用，修复完成后需要合并到master、test、develop分支
   
   
   - 提示：一般master和develop分支是不删除的，是固定保留的，创建的其他分支，当合并到develop上，可以删除这个分支



- 实例：

  `````
  # 创建分支
  git branch dev01
  
  # 切换到dev01分支
  git checkout dev01
  
  # [dev01]创建文件并提交至仓库
  touch file03.txt
  git add .
  git commit -m 'add file03.txt on dev01'
  
  # 切换至master分支
  git checkout master
  # 将dev01分支合并到master分支上
  git merge dev01
  
  # 删除dev01分支
  git branch -d dev01
  `````



## 4、GitHub线上仓库使用

### 4.1、基于HTTP协议管理GitHub仓库

1. 在GitHub上创建study仓库

2. 在本地创建同名仓库

   路径：E:\Download\GitHubLocalRepository\study

3. 使用 clone 指令克隆线上仓库到本地
   命令：git clone 线上仓库地址

   ````
   $ git clone https://github.com/PcLyrer/study.git
   Cloning into 'study'...
   warning: You appear to have cloned an empty repository.
   ````

   此时，能够将GitHub的仓库克隆到本地

4. 在本地仓库上做对应的操作（提交暂存区、提交本地仓库、提交线上仓库、拉取线上仓库）  
   提交暂存区：git add  
   提交本地仓库：git commit  
   提交到线上仓库：git push  
   拉取到本地仓库：git pull     

- 实例

  ````
  touch README.md
  vi README.md
  	该文件是线上远程仓库的读我文件，初始化时创建。
  git add .
  git commit -m 'add Readme file'
  # 提交到线上仓库
  git push 
  # 首次往线上仓库提交，出现403错误，原因：不是任何人都能在线上仓库提交内容，需要进行配置，建立权限(操作如下)
  ````

- 首次连接进行签权

  1. 在GitHub中申请令牌Setting->Developer settings->Personal access tokens

  2. 勾选权限：

     - 要使用token从命令行访问仓库，请选择repo

     - 要使用token从命令行删除仓库，请选择delete_repo
     - 其他根据需要进行勾选

  3. 保存令牌
  4. 在本地仓库进行如下修改：

  ````
  # 修改config配置文件
  vi .git/config
  
  # 内容如下,在url[token]处添加令牌
  [core]
          repositoryformatversion = 0
          filemode = false
          bare = false
          logallrefupdates = true
          symlinks = false
          ignorecase = true
  [remote "origin"]
          url = https://[token]@github.com/PcLyrer/study.git
          fetch = +refs/heads/*:refs/remotes/origin/*
  [branch "main"]
          remote = origin
          merge = refs/heads/main
  ````



- 再次提交到线上仓库，提交成功，如图：

  ![Image 03](https://github.com/PcLyrer/study/blob/main/Image/03.png?raw=true)    

  

  并且在线上仓库上也能看到READEME.md文件，如图：  

  ![Image 04](https://github.com/PcLyrer/study/blob/main/Image/04.png?raw=true)    

  

  - 提示：关键字fetal显示错误

- 拉取线上的文件，首先在线上仓库中创建OnlineFile.txt文件，并添加内容，然后拉取到本地：

  ````
  #将远程仓库拉取到本地的命令
  git pull 
  ````

  出现以下提示表示拉取成功：      

  ![Image 05](https://github.com/PcLyrer/study/blob/main/Image/05.png?raw=true)

  

  并且本地仓库中也出现了该文件：    

  ![Image 06](https://github.com/PcLyrer/study/blob/main/Image/06.png?raw=true) 

  

- 提示
  每天工作的第一件事就是git pull 拉取线上最新的版本；每天下班的最后一件事情就是git push 将开发好的版本上传至线上仓库。

### 4.2、基于SSH协议管理GitHub仓库

1. 创建公私钥对文件

   ssh-keygen -t rsa -C "github邮箱"

   生成的公钥文件保存在C：/Users/Administrator/.ssh/id_rsa

2. 打开id_rsa.pub文件，复制内容

3. 将公钥文件内容上传至GitHub的setting—>SSH and GPG keys中

4. 执行后续操作

   ````
   以SSH的方式克隆文件
   git clone git@github.com:PcLyrer/study.git
   ...后续操作一致与使用http协议一致...
   ````

5. 冲突解决

   前面介绍到合并两个对同一文件同一行内容进行修改的分支时会出现冲突，需要手动解决，首先git push时会不成功，显示有冲突，然后此时git pull，git会自动将冲突内容合并，我们拉取文件内容到本地，手动保留需要内容，解决冲突，然后重新push到远程仓库。



## 5、在IDEA中使用Git

### 5.1、从本地仓库push至远程仓库

1. 在IDEA中配置Git

   setting->version control->git

2. 使用IDEA打开某个项目，新建.gitignore文件，忽略.idea以及其他文件
3. 点击VCS（version control）->import into Version Control（引入版本控制）->Create Git Repositroy->选择项目所在文件
4. 点击Git：的 √ ，全选，进行add项目文件操作，点击commit直接提交至本地仓库
5. 可以在最下端的Version Control窗口的log中查看提交记录
6. 将本地仓库内容push到远程仓库：VCS->Git->push->Define remote->添加远程仓库的SSH->OK->push（提交至远程仓库成功）
7. 修改文件后，重复上述操作，更新版本，此时由于连接有远程仓库，commit后直接push

- 实例：
  将本地发反射章节的代码上传至GitHub上的Hello_World仓库，如图所示：   
  ![Image 07](https://github.com/PcLyrer/study/blob/main/Image/07.png?raw=true)

### 5.2、将远程仓库clone至本地

1. VCS->Get from Version Control
2. paste远程仓库SSH
3. 选择Directory
4. Clone
5. 存在冲突时也是按照之前的步骤进行解决

### 5.3、创建分支

1. 分支相关操作可在以下完成：   
   ![Image text](https://github.com/PcLyrer/study/blob/main/Image/08.png?raw=true)

   

- 最后IDEA集成GitBash作为Terminal设置，注意设置的是bin目录下的bash.exe，而不是git-bash.exe，这样就可以使用命令行来进行控制Git了。

  ![Image text](https://github.com/PcLyrer/study/blob/main/Image/09.png?raw=true)



- 最后需要注意的几点：
  1. 切换分支前先提交本地的修改
  2. 代码及时提交，提交过了就不会丢
  3. 遇到任何问题都不要删除文件目录，删除后就真的找不到了