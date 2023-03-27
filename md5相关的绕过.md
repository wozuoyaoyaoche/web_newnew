# https://blog.csdn.net/wangyuxiang946/article/details/119845182

例题:buuctf-easy_md5



## 1.0e科学计数法绕过

适用于弱类型的md5比较,md5加密后前面可能出现0e,两个0e在弱比较时会被识别成0,从而绕过

例:md5('QNKCDZO') == md5(240610708)

### 形如md5(str)==str的,0e215962017可以实现,加密后还是0e开头,弱类型比较通过



## 2.数组绕过

可以用强类型比较,md5在加密数组时会报错但会继续进行,值为null,两个数组就是null===null,从而绕过

例:md5(a[]=1) === md5(b[]=1)

## 3.sql注入中的md5

#### 形如md5($str,true)

原理:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210319200310579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MzQ5ODYxNg==,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/2582d89bcc4c459b9138eac95a1b76bd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rKh5LqL5bCx6YCb5Y2a5a6i,size_20,color_FFFFFF,t_70,g_se,x_16)

根据上面原理,举个例子,假如有如下sql语句:

select * from 'admin' where password=md5($pass,true)

我们另pass=ffifdyop

则此时在经过md5函数后sql语句变为了:

select * from 'admin' where password=''or'6]!r,b'  (开头和结尾两个单引号是md5后加上去的)

随后'6]!r,b'被识别为6,在bool中相当于true,所以永真,实现绕过





