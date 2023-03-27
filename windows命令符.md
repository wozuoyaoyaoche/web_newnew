**在命令后面加 /?是命令详情**

# 常规指令

## cd指令

cd指令加 /d可以cd到c盘以外的盘

## dir指令

用于显示目录和文件列表

```shell
dir
dir c:\abc             #指定路径扫描
dir /a:h c:\           #/a是指定类型为hide,作用是查看c盘下隐藏的目录和文件
dir /o:-n c:\          #使用字母逆序查看
```

## md或mkdir

创建目录(文件夹),也可以直接创建多极子目录

```
md a
md a\b\c
```

## rd

删除目录(不能删除文件)

直接使用rd只能删除空目录,有文件的话要加上 /s

```
rd empty_dir   #只能删除空目录
rd dir         #可以删除有文件的文件夹
```

## move

用来移动(重命名)文件,目录

```
move a b\   #把a移到b目录下
move a.txt b.txt #改名
```

## copy

用于复制文件,也可以融合文件

```
copy c:\1.txt d:\abc\   #复制一份文件
copy 1.txt+2.txt 3.txt  #将两个文件中的内容直接融合到新的文件中
```

## xcopy

用于复制目录

## del

用于删除文件

```
del 2.txt
```

# 文本操作指令

## type

用于显示文本文件内容

```
type 1.txt
```



## > 重定向符号

有点类似于管道符,把前面指令的输出结果重定向给后面

```
ipconfig > 1.txt      #把内容写入1.txt,如果没有会新建一个1.txt
```



## findstr

查找字符串,输出匹配的所有行

```
findstr 1.txt
ipconfig | findstr WLAN
```



# 网络相关操作

默认网关:标识与主机直连的路由器的ip地址

## 配置ip地址

```powershell
netsh interface ipv4 set address "接口名" (static可选) ip地址 子网掩码 默认网关
```

```powershell
netsh interface ip set address "接口名" dhcp      #自动获取接口的tcp/ip参数(ip地址,子网掩码,默认网关,dns地址)
```

```powershell
netsh interface dnsserver set address "接口名" (static) dns 地址    #设置dns地址
netsh interface dnsserver add address "接口名" (static) dns 地址 index=2    #添加备用dns
```

## ipconfig

```
ipconfig
ipconfig /all
ipconfig /release   #丢弃ip地址
ipconfig /renew     #重新获取ip地址
ipconfig /flushdns  #丢弃之前获得的dns缓存,之后访问网页会重新访问dns服务器
```

## ping

注意ping命令跳过传输层,直接使用网络层的icmp回送请求和回答报文

```powershell
ping -n 数量 ip地址/域名    #-n指定发包数量
ping -l 字节量 ip地址//域名 #-l设置每个包的大小(默认32)
ping -t ip地址/域名        #一直不停的发包
ping -a ip地址/域名        #返回ip对应真实机的名字(一般在局域网里用)
```

## tracert

跟踪本机到目标ip地址之间经过的(越点)路由器

```
tracert ip/域名
```

## route

用来操作网络路由表

0.0.0.0代表任意网络

```
#打印路由表
route -4 print    #打印ipv4地址
#添加路由条目       #32代表子网掩码,或者如果是112.52.42.0网段,那就写/24.后面的192.168.33.15是网关地址
route add 112.53.42.52/32 192.168.33.15   
#删除路由条目
route delete 112.53.42.52  #后面的是网络目标
```

## netstat

显示端口状态,包括占用他的进程等信息

```
netstat -o #显示占用端口的进程
-a #显示全部(建议选上)
-p 协议名 #只显示特定协议(TCP/UDP等)
-n 以数字形式显示
-r 等同于 route print
```

