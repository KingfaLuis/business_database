# MIN和MAX的用法

计算列的最小值和最大值。

注意，**MIN** 和 **MAX** 聚合函数也会忽略 **NULL** 值。

### 提示

从功能上来说，**MIN** 和 **MAX** 与 **COUNT** 相似，它们都可以用在非数字列上。**MIN** 将返回最小的数字、最早的日期或按字母表排序的最之前的非数字值，具体取决于列类型。**MAX** 则正好相反，返回的是最大的数字、最近的日期，或与“Z”最接近（按字母表顺序排列）的非数字值。



# 练习使用MIN，MAX和AVG

### 问题

1. 最早的订单下于何时？
2. 尝试执行和第一个问题一样的查询，但是不使用聚合函数。
3. 最近的 **web_event** 发生在什么时候？
4. 尝试以另一种方式执行上个问题的查询，不使用聚合函数。
5. 算出每个订单在每种纸张上消费的平均 (**AVERAGE**) 金额，以及每个订单针对每种纸张购买的平均数量。最终答案应该有 6 个值，每个纸张类型平均销量对应一个值，以及平均数量对应一个值。
6. 在所有订单上消费的中值 **total_usd** 是多少？



根据**SQL** 表格信息回答。

1.最早的订单下于何时？

```sql
SELECT MIN(occurred_at) 
FROM orders;
```

2.尝试执行和第一个问题一样的查询，但是不使用聚合函数。 

```sql
SELECT occurred_at 
FROM orders 
ORDER BY occurred_at
LIMIT 1;
```

3.最近的 **web_event** 发生在什么时候？ 

```sql
SELECT MAX(occurred_at)
FROM web_events;
```

4.尝试以另一种方式执行上个问题的查询，不使用聚合函数。 

```sql
SELECT occurred_at
FROM web_events
ORDER BY occurred_at DESC
LIMIT 1;
```

5.算出每个订单在每种纸张上消费的平均 (**AVERAGE**) 金额，以及每个订单针对每种纸张购买的平均数量。最终答案应该有 6 个值，每个纸张类型平均销量对应一个值，以及平均数量对应一个值。 

```sql
SELECT AVG(standard_qty) mean_standard, AVG(gloss_qty) mean_gloss, 
           AVG(poster_qty) mean_poster, AVG(standard_amt_usd) mean_standard_usd, 
           AVG(gloss_amt_usd) mean_gloss_usd, AVG(poster_amt_usd) mean_poster_usd
FROM orders;
```

6.如何计算中位数呢？问题：对于所有订单（orders）数据，其total_usd字段的中位数是多少？

```sql
SELECT *
FROM (SELECT total_amt_usd
      FROM orders
      ORDER BY total_amt_usd
      LIMIT 3457) AS Table1
ORDER BY total_amt_usd DESC
LIMIT 2;
```

因为订单一共有6912个，因此我们需要第3456和第3457个订单（按total_amt_usd排序）的total_amt_usd字段的平均值。这样就能得出中位数结果，为**2482.855**。这显然不是一个好办法。如果我们有了新订单，再次计算时就必须修改LIMIT。SQL实际上并不会为我们计算中位数。以上代码使用了一个子查询（SUBQUERY），但你可以使用任何方法找到需要的两个值，然后再求平均即可得到中位数。 