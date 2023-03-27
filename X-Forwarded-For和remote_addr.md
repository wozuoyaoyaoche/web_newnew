参考:https://blog.csdn.net/shaobingj126/article/details/6794388

xff参考:https://www.runoob.com/w3cnote/http-x-forwarded-for.html

# X-Forwarded-For

经过谁代理,就在这个字段写入上个代理的ip地址,随着经过的代理越来越多,这个字段越来越长

<font color='red'>注意:最后经过的代理不会被写入,而是被写到remote_address</font>

<font color='orange'>例:</font>**真实地址为ip0,代理1为ip1,代理2为ip2,代理3为ip3**

**经过代理1时,代理1把ip0写入,表示自己为真实地址转发**

**经过代理2时,代理2把ip1写入,表示自己为代理1转发,以此类推**