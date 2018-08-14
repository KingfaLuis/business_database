# MYSQL HAVING的用法和训练

### HAVING - 提示

**HAVING** 是过滤被聚合的查询的 “整洁”方式，但是通常采用[子查询](https://community.modeanalytics.com/sql/tutorial/sql-subqueries/)的方式来实现。本质上，只要你想对通过聚合创建的查询中的元素执行 **WHERE** 条件，就需要使用 **HAVING**

- HAVING 总是使用在GROUP BY 语句后面



# 练习：HAVING

经常你会对 **WHERE** 和 **HAVING** 之间的差别感到困惑。关于 **HAVING** 和 **WHERE** 的语句，请选出以下所有正确语句。

- [x] **WHERE** 子集根据逻辑条件对返回的数据进行筛选。
- [x] **WHERE** 出现在 **FROM**，**JOIN** 和 **ON** 条件之后，但是在 **GROUP BY** 之前。
- [x] **HAVING** 出现在 **GROUP BY** 条件之后，但是在 **ORDER BY 条件之前。
- [x] **HAVING** 和 **WHERE** 相似，但是它适合涉及聚合的逻辑语句。

所有这些语句都正确。这些语句确保你知道这些语句在查询中都位于哪个位置，以及为何要使用特定的语句。’ 

### 问题：

1. 有多少位**销售代表**需要管理超过 5 个客户？
2. 有多少个**客户**具有超过 20 个订单？
3. 哪个客户的订单最多？
4. 有多少个客户在所有订单上消费的总额超过了 30,000 美元？
5. 有多少个客户在所有订单上消费的总额不到 1,000 美元？
6. 哪个客户消费的最多？
7. 哪个客户消费的最少？
8. 哪个客户使用 `facebook` 作为与消费者沟通的**渠道**超过 6 次？
9. 哪个客户使用 `facebook` 作为沟通**渠道**的次数最多？
10. 哪个渠道是客户最常用的渠道？

### 解决方案：HAVING

1. 有多少位**销售代表**需要管理超过 5 个客户？

```sql
SELECT s.id, s.name, COUNT(*) num_accounts
FROM accounts a
JOIN sales_reps s
ON s.id = a.sales_rep_id
GROUP BY s.id, s.name
HAVING COUNT(*) > 5
ORDER BY num_accounts;
```
