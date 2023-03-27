# 1.版本控制

## 1.1 有如下功能:

- 实现跨区域多人开发
- 追踪和记载一个或多个文件的历史记录
- 组织和保护源代码和文档
- 统计工作量
- 并行开发,提高工作效率
- 追踪记录整个软件的开发过程
- 减轻开发人员的负担,节省时间,降低人为错误

**简单地说就是用于管理多人协同开发项目的技术**

------

<font color='orange'>顺便提一句,不光有git,还可以用svn等</font>

------



## 1.2 版本控制分类

### 一.本地版本控制

记录文件每次更新,可以对每个版本做个快照,适合个人用

### 二.集中版本控制

所有版本数据保存在服务器上,协同开发者从服务器上同步更新或上传自己的修改

**弊端**:所有数据都在服务器上,必须联网以使用全部功能,且若服务器损坏,数据会丢失,可以时常备份来避免

**代表软件:svn**

### 三.分布式版本控制

每个人拥有全部代码(可能有程序员跑路的安全隐患)

**好处**:所有版本信息仓库全部同步到本地每个用户,这样可以在本地查看所有版本历史,可以离线在本地提交,联网时再push到服务器或其他用户即可;只要有一个用户的设备没问题就可以恢复所有数据

**弊端**:增加了本地储存空间



# 2.git基本理论(核心)

## 2.1 工作区域

Git本地有三个工作区域：工作目录（Working Directory）、暂存区(Stage/Index)、资源库(Repository或Git Directory)。如果在加上远程的git仓库(Remote Directory)就可以分为四个工作区域。文件在这四个区域之间的转换关系如下：

![image-20230109170712901](C:\Users\我坐摇摇车\AppData\Roaming\Typora\typora-user-images\image-20230109170712901.png)





- Workspace：工作区，就是你平时存放项目代码的地方
- Index / Stage：暂存区，用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表信息
- Repository：仓库区（或本地仓库），就是安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本
- Remote：远程仓库，托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换

## 2.2 工作流程

git的工作流程一般是这样的：

１、在工作目录中添加、修改文件；    

２、将需要进行版本管理的文件放入暂存区域；   (git add .)

３、将暂存区域的文件提交到git仓库。                  (git commit)

因此，git管理的文件有三种状态：已修改（modified）,已暂存（staged）,已提交(committed)

![image-20230327224732138](C:\Users\wozuoyaoyaoche\AppData\Roaming\Typora\typora-user-images\image-20230327224732138.png)

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7Ksu8UlITwMlbX3kMGtZ9p09iaOhl0dACfLrMwNbDzucGQ30s3HnsiaczfcR6dC9OehicuwibKuHjRlzg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



# 3.文件操作

## 3.1 文件4种状态

- Untracked: 未跟踪, 此文件在文件夹中, 但并没有加入到git库, 不参与版本控制. 通过git add 状态变为Staged.
- Unmodify: 文件已经入库, 未修改, 即版本库中的文件快照内容与文件夹中完全一致. 这种类型的文件有两种去处, 如果它被修改, 而变为Modified. 如果使用git rm移出版本库, 则成为Untracked文件
- Modified: 文件已修改, 仅仅是修改, 并没有进行其他的操作. 这个文件也有两个去处, 通过git add可进入暂存staged状态, 使用git  checkout 则丢弃修改过, 返回到unmodify状态, 这个git checkout即从库中取出文件, 覆盖当前修改 !
- Staged: 暂存状态. 执行git commit则将修改同步到库中, 这时库中的文件和本地文件又变为一致, 文件为Unmodify状态. 执行git reset HEAD filename取消暂存, 文件状态为Modified



## 3.2 常用指令

### 一.git配置

- #查看所有config

  git config -l

- #查看系统config

  git config --system --list

  　　

- #查看当前用户（global）配置

  git config --global  --list

### 二.设置用户名和邮箱(必要)

git config --global user.name "kuangshen"  #名称
git config --global user.email 24736743@qq.com   #邮箱

### 三.克隆仓库和初始化自己的仓库

git clone url

git init

### 四.查看文件状态

git status [filename]    #若不指定filename就是全部文件

### 五.提交文件

- #添加文件到暂存区

​     	git add [filename/.]   #add后面跟filename是指定文件,跟.是全部文件

- #提交暂存区内容到仓库

​		 git commit -m "这次提交的说明"  #m是message的意思,后面的可以理解为注释,告知这次更新了什么



## 3.3忽略文件

有时一些文件不需要提交到git,可以用.gitignore文件来定义规则

在主目录下建立".gitignore"文件，此文件有如下规则：

1. 忽略文件中的空行或以井号（#）开始的行将会被忽略。
2. 可以使用Linux通配符。例如：星号（*）代表任意多个字符，问号（？）代表一个字符，方括号（[abc]）代表可选字符范围，大括号（{string1,string2,...}）代表可选的字符串等。
3. 如果名称的最前面有一个感叹号（!），表示例外规则，将不被忽略。
4. 如果名称的最前面是一个路径分隔符（/），表示要忽略的文件在此目录下，而子目录中的文件不忽略。
5. 如果名称的最后面是一个路径分隔符（/），表示要忽略的是此目录下该名称的子目录，而非文件（默认文件或目录都忽略）。



```
#为注释
*.txt        #忽略所有 
.txt结尾的文件,这样的话上传就不会被选中！
!lib.txt     #但lib.txt除外
/temp        #仅忽略项目根目录下的TODO文件,不包括其它目录temp
build/       #忽略build/目录下的所有文件
doc/*.txt    #会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```

