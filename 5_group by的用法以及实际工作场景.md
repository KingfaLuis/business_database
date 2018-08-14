---
typora-copy-images-to: img
---

# GROUP BY的用法以及实际工作场景

## GROUP BY知识点

主要知识点包括：

- **GROUP BY** 可以用来在数据子集中聚合数据。例如，不同客户、不同区域或不同销售代表分组。
- **SELECT** 语句中的任何一列如果不在聚合函数中，则必须在 **GROUP BY** 条件中。
- **GROUP BY** 始终在 **WHERE** 和 **ORDER BY** 之间。
- **ORDER BY** 有点像电子表格软件中的 **SORT**。

### 问题：

根据以下 **SQL** 表格信息回答问题

1. 哪个**客户**（按照名称）下的订单最早？答案应该包含订单的**客户名称**和**日期**。
2. 算出每个客户的总销售额（单位是**美元**）。答案应该包括两列：每个公司的订单总销售额（单位是**美元**）以及公司**名称**。
3. 最近的 **web_event** 是通过哪个**渠道**发生的，与此 **web_event** 相关的**客户**是哪个？你的查询应该仅返回三个值：**日期**、**渠道**和**客户名称**。
4. 算出 **web_events** 中每种**渠道**的次数。最终表格应该有两列：**渠道**和渠道的使用次数。
5. 与最早的 **web_event** 相关的**主要联系人**是谁？
6. 每个**客户**所下的最小订单是什么（以**总金额（美元）**为准）。答案只需两列：客户**名称**和**总金额（美元）**。从最小金额到最大金额排序。
7. 算出每个区域的**销售代表**人数。最早表格应该包含两列：**区域**和 **sales_reps** 数量。从最少到最多的代表人数排序。

### 解决方案：使用GROUP BY

1.哪个**客户**（按照名称）下的订单最早？答案应该包含订单的**客户名称**和**日期**。 

```sql
SELECT a.name, o.occurred_at
FROM accounts a
JOIN orders o
ON a.id = o.account_id
ORDER BY occurred_at
LIMIT 1;
```

![1530893268865](1530893268865.png)

```sql
name	occurred_at
DISH Network	2013-12-04T04:22:44.000Z
```



2.算出每个客户的总销售额（单位是**美元**）。答案应该包括两列：每个公司的订单总销售额（单位是**美元**）以及公司**名称**。 

```sql
SELECT a.name, SUM(total_amt_usd) total_sales
FROM orders o
JOIN accounts a
ON a.id = o.account_id
GROUP BY a.name;
```

![1530893938611](1530893938611.png)

3.最近的 **web_event** 是通过哪个**渠道**发生的，与此 **web_event** 相关的**客户**是哪个？你的查询应该仅返回三个值：**日期**、**渠道**和**客户名称**。 

```sql
SELECT w.occurred_at, w.channel, a.name
FROM web_events w
JOIN accounts a
ON w.account_id = a.id 
ORDER BY w.occurred_at DESC
LIMIT 1;
```

![1530894838440](1530894838440.png)

4.算出 **web_events** 中每种**渠道**的次数。最终表格应该有两列：**渠道**和渠道的使用次数。 

```sql
SELECT w.channel, COUNT(*)
FROM web_events w
JOIN accounts a
ON a.id = w.account_id
GROUP BY w.channel
```

![1530896269573](1530896269573.png)

5.与最早的 **web_event** 相关的**主要联系人**是谁？ 

```sql
SELECT a.primary_poc
FROM web_events w
JOIN accounts a
ON a.id = w.account_id
ORDER BY w.occurred_at
LIMIT 1;
```

![1530896643887](1530896643887.png)

6.每个**客户**所下的最小订单是什么（以**总金额（美元）**为准）。答案只需两列：客户**名称**和**总金额（美元）**。从最小金额到最大金额排序。 

```sql
SELECT a.name, MIN(total_amt_usd) smallest_order
FROM accounts a
JOIN orders o
ON a.id = o.account_id
GROUP BY a.name
ORDER BY smallest_order;
```

![1530897579700](1530897579700.png)

奇怪的是，很多订单没有美元金额。我们可能需要检查下这些订单。 



7.算出每个区域的**销售代表**人数。最终表格应该包含两列：**区域**和 **sales_reps** 数量。从最少到最多的代表人数排序。 

```sql
SELECT r.name, COUNT(*) num_reps
FROM region r
JOIN sales_reps s
ON r.id = s.region_id
GROUP BY r.name
ORDER BY num_reps;
```

![1530898102356](1530898102356.png)

