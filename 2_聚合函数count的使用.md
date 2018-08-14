# 聚合函数 - COUNT

### 计算表格中的行数

试着手动数数每个表格的行数。以下是计算 **accounts** 表格中的行数示例：

```sql
SELECT COUNT(*)
FROM accounts;
```

![1530820794355](C:\Users\ADMINI~1\AppData\Local\Temp\1530820794355.png)

我们可以选择一列来放置聚合函数：

```sql
SELECT COUNT(accounts.id)
FROM accounts;
```

![1530820757607](C:\Users\ADMINI~1\AppData\Local\Temp\1530820757607.png)

上述两个语句是对等的，但是并不一直这样。

