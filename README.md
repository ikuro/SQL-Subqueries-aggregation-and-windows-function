# SQL-Subqueries-aggregation-and-windows-function
## SQL queries to show various features in SQL

### Subquery with aggregation
``` SQL
SELECT r.name, COUNT(o.total) total_orders
FROM sales_reps s
JOIN accounts a
ON a.sales_rep_id = s.id
JOIN orders o
ON o.account_id = a.id
JOIN region r
ON r.id = s.region_id
GROUP BY r.name
HAVING SUM(o.total_amt_usd) = (
      SELECT MAX(total_amt)
      FROM (SELECT r.name region_name, SUM(o.total_amt_usd) total_amt
              FROM sales_reps s
              JOIN accounts a
              ON a.sales_rep_id = s.id
              JOIN orders o
              ON o.account_id = a.id
              JOIN region r
              ON r.id = s.region_id
              GROUP BY r.name) sub);
```
### Windows function
``` SQL

  Select occurred_at,Date_trunc('month', occurred_at) as month, standard_amt_usd,
  sum(standard_amt_usd) over(order by occurred_at) as Full_Running_Totals,
  sum(standard_amt_usd) over(PARTITION BY Date_trunc('month', occurred_at) order by occurred_at) as Monthly_Running_Totals
  from orders;
```
