# 建表和删表

```sql
create database db1;

drop table if exists table1;

create table2(
	id int,
    name varchar(255),
    primary key(id,name) #表示id,name二者联合后一起作为该表的主键
    text varchar(255) not null #表示不能为空
    idnum varchar(255) unique #表示不能重复
    foreign key(cno) references table1(classno)
);
```



# 查询

group by是分组, 一般与count,avg,sum,max等函数联合使用

order by要写在group by后面

联合查询时用union all实现不去重



## 内连接查询

 老语法:

```sql
select u.device_id,
q.question_id,
q.result
from question_practice_detail q,user_profile u
where q.device_id=u.device_id
```

缺点:结构不清晰,where语句很乱

99新语法:

```sql
select u.device_id,
q.question_id,
q.result
from question_practice_detail q
join
user_profile u
on
q.device_id=u.device_id
```

## 内联查询(自己连接自己)

把一张表看成两张表

| id   | ename    | bossid |
| ---- | -------- | ------ |
| 1    | 李员工   | 666    |
| 666  | 王经理   | 888    |
| 888  | 张董事长 | null   |

```sql
select a.ename as '员工名',b.ename as '领导名'
from emp a
join emp b
on a.bossid=b.id;
```

## 外连接查询

如果说内连接像交集,外连接就像并集

left join就是将左表当成主表,捎带着查右表,right join同理

所以如果是left join那么右表只会查出on后面条件匹配到的,而左表哪怕不满足on后面的条件也会被查到

