参考:https://blog.csdn.net/wulanlin/article/details/122409259

# Git简介

### 简介

官方给出的解释是：Git是一个开源的分布式版本控制系统 ,我们简单的理解为Git 是一个内容寻址文件系统，也就是说Git 的核心部分是键值对数据库。 当我们向 Git 仓库中插入任意类型的内容(开发者们在其中做的版本信息修改之类的操作)，它会返回一个唯一的键，通过该键可以在任意时刻再次取回该内容

------

Git是一个可以实现有效控制应用版本的系统，但是在一旦在代码发布的时候，在配置不当的情况下，可能会将“.git”文件直接部署到线上环境，这就造成了git泄露问题。那么，一旦攻击者或者黑客发现这个问题之后，就可能利用其获取网站的源码、数据库等重要资源信息，进而造成严重的危害。

### 

### 结构

.git目录：使用git init初始化git仓库的时候，生成的隐藏目录，git会将所有的文件，目录，提交等转化为git对象，压缩存储在这个文件夹当中。

COMMIT_EDITMSG：保存最新的commit message，Git系统不会用到这个文件，用户一个参考文件

config：Git仓库的配置文件

description：仓库的描述信息，主要给gitweb等git托管系统使用

HEAD：这个文件包含了一个档期分支（branch）的引用，通过这个文件Git可以得到下一次commit的parent

hooks：这个目录存放一些shell脚本，可以设置特定的git命令后触发相应的脚本；在搭建gitweb系统或其他

git托管系统会经常用到hook script(钩子脚本)

index：这个文件就是我们前面提到的暂存区（stage），是一个二进制文件

info：包含仓库的一些信息

logs：保存所有更新的引用记录

objects：所有的Git对象都会存放在这个目录中，对象的SHA1哈希值的前两位是文件夹名称，后38位作为对象文件名

refs：这个目录一般包括三个子文件夹，heads、remotes和tags，heads中的文件标识了项目中的各个分支指向的当前commit

ORIG_HEAD：HEAD指针的前一个状态



# Git泄露

## 利用方法

先dirsearch扫描出目录存在/.git,然后githack拉取文件到本地



