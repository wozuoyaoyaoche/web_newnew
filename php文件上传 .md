# php文件上传漏洞

#### **<font color='red'>一定注意可能碰见的坑:直接把文本粘贴到bp里很可能乱码,尤其注意" ' 这种符号,需要手动改回来</font>**!!!!!!!!!!!!!!

## 图片上传漏洞

假如让你上传一张图片,可以考虑一句话木马,然后webshell.

<img src="C:\Users\我坐摇摇车\AppData\Roaming\Typora\typora-user-images\image-20221011002352431.png" alt="image-20221011002352431" style="zoom: 50%;" />

在上传时,可以用bp抓包,改格式为允许上传的类型,比如

 image/gif
 image/jpeg
 image/png

如果对文件名也有检测,可以试着把php改为php3,php4,php5,phtml尝试

如果上传时还有文件头验证,可以在文件头添加一行: GIF89a

​         这是一种文件欺骗,骗过头文件检测.

额外提一句,如果还对<?这种php格式有检验,可以改成<script language='php'>语句</script>

的格式绕过

如果对文本里不能包含php这个单词做了屏蔽,可以<?= @eval($_POST["hack"]);?>这样绕过

# .htaccess绕过(buuctf-你传你马呢)

这是一种php配置文件,具体参照[.htaccess文件解析漏洞 - ʚɞ无恙 - 博客园 (cnblogs.com)](https://www.cnblogs.com/ggc-gyx/p/16412236.html)

适用于屏蔽了所有php后缀的时候,原理

##### 上传覆盖.htaccess文件，重写解析规则，将上传的***图片马***以脚本方式解析





# .user.ini绕过(buuctf-checkin)

.user.ini绕过是比.htaccess绕过更通用的手段,如果.htaccess不行可以考虑这个

这个文件可以理解为用户自定义的php.ini文件.我们试图达到的目的就是把它设置成**在执行一个php文件前.先用php方法执行另一个文件**

**所以对应的前提**是在上传的.user.ini同目录下必须可执行的php文件,例如index.php,否则无效.

使用:

例如把脚本写在了evil2.jpg里,就写auto_prepend_file=evil2.jpg

然后在url里打开另一个已经存在的php文件,例如index.php.就发现在打开index.php前先执行了evil2.jpg里的内容,木马利用成功
