# NULL 和聚合的用法

```sql
select *
from accounts
where id >1000 and id < 1800

/*查询哪些联系人为空*/
select *
from accounts
where primary_poc is null
```

注意语法中是is null 和is not null来判断判断，而不是用=null。null不是一个数值，而是一种数据特性。