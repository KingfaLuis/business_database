# count和null 的使用

count是用来统计不为null的行数，为null的列，如果统计单列，只统计不为null的行数。

例如判断联系人是否为空的例子

```sql
select count(*)
from accounts

select count(accounts.primary_poc)
from accounts
where primary_poc is null

select count(accounts.primary_poc)
from accounts
where primary_poc is not null
```

注意：**COUNT** 不会考虑具有 **NULL** 值的行。因此，可以用来快速判断哪些行缺少数据。