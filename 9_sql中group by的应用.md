# GROUP BY 第二部分

主要知识点：

- 你可以同时按照多列**分组**，正如此处所显示的那样。这样经常可以在大量不同的细分中更好地获得聚合结果。
- **ORDER BY** 条件中列出的列顺序有区别。你是从左到右让列排序。

### GROUP BY - 专家提示

- **GROUP BY** 条件中的列名称顺序并不重要，结果还是一样的。如果运行相同的查询并颠倒 **GROUP BY**条件中列名称的顺序，可以看到结果是一样的。
- 和 **ORDER BY** 一样，你可以在 **GROUP BY** 条件中用数字替换列名称。仅当你对大量的列分组时，或者其他原因导致 GROUP BY 条件中的文字过长时，才建议这么做。
- 提醒下，任何不在聚合函数中的列必须显示 GROUP BY 语句。如果忘记了，可能会遇到错误。但是，即使查询可行，你也可能不会喜欢最后的结果！

# 练习：GROUP BY（第二部分）

### 问题：

根据以下 **SQL** 表格信息回答以下问题。如果你遇到问题或想要对比检查你的答案，可以在下一页面的顶部找到我的答案。

1. 对于每个客户，确定他们在订单中购买的每种纸张的平均数额。结果应该有四列：客户**名称**一列，每种纸张类型的平均数额一列。
2. 对于每个客户，确定在每个订单中针对每个纸张类型的平均消费数额。结果应该有四列：客户**名称**一列，每种纸张类型的平均消费数额一列。
3. 确定在 **web_events** 表格中每个**销售代表**使用特定**渠道**的次数。最终表格应该有三列：**销售代表的名称**、**渠道**和发生次数。按照最高的发生次数在最上面对表格排序。
4. 确定在 **web_events** 表格中针对每个**地区**特定**渠道**的使用次数。最终表格应该有三列：**区域名称**、**渠道**和发生次数。按照最高的发生次数在最上面对表格排序。

### 解决方案：GROUP BY

1. 对于每个客户，确定他们在订单中购买的每种纸张的平均数额。结果应该有四列：客户**名称**一列，每种纸张类型的平均数额一列。

```
SELECT a.name, AVG(o.standard_qty) avg_stand, AVG(gloss_qty) avg_gloss, AVG(poster_qty) avg_post
FROM accounts a
JOIN orders o
ON a.id = o.account_id
GROUP BY a.name;
```

![1530899594518](C:\Users\ADMINI~1\AppData\Local\Temp\1530899594518.png)

2.对于每个客户，确定在每个订单中针对每个纸张类型的平均消费数额。结果应该有四列：客户**名称**一列，每种纸张类型的平均消费数额一列。 

```sql
SELECT a.name, AVG(o.standard_amt_usd) avg_stand, AVG(gloss_amt_usd) avg_gloss, AVG(poster_amt_usd) avg_post
FROM accounts a
JOIN orders o
ON a.id = o.account_id
GROUP BY a.name;
```

![1530899824158](C:\Users\ADMINI~1\AppData\Local\Temp\1530899824158.png)

第四列被挡住看不到

3.确定在 **web_events** 表格中每个**销售代表**使用特定**渠道**的次数。最终表格应该有三列：**销售代表的名称**、**渠道**和发生次数。按照最高的发生次数在最上面对表格排序。 

```sql
SELECT s.name, w.channel, COUNT(*) num_events
FROM accounts a
JOIN web_events w
ON a.id = w.account_id
JOIN sales_reps s
ON s.id = a.sales_rep_id
GROUP BY s.name, w.channel
ORDER BY num_events DESC;
```

![1530901921666](C:\Users\ADMINI~1\AppData\Local\Temp\1530901921666.png)

4.确定在 **web_events** 表格中针对每个**地区**特定**渠道**的使用次数。最终表格应该有三列：**区域名称**、**渠道**和发生次数。按照最高的发生次数在最上面对表格排序。 

```sql
SELECT r.name, w.channel, COUNT(*) num_events
FROM accounts a
JOIN web_events w
ON a.id = w.account_id
JOIN sales_reps s
ON s.id = a.sales_rep_id
JOIN region r
ON r.id = s.region_id
GROUP BY r.name, w.channel #对查询结果进行多表
```

![1530902455675](C:\Users\ADMINI~1\AppData\Local\Temp\1530902455675.png)



[1]: http://www.runoob.com/sql/sql-groupby.html

