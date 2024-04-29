# SQL 10小时速成笔记 - 第三章（自用版）
在多张表格中检索数据
## 1-Inner Joins 内连接 / JOIN ON
customers 和 orders在不同表格

因为customers的地址、手机会经常更改，如果orders里面也记录了顾客地址等，会每次更改时都要改两张表里的数据，很麻烦


JOIN 有分 INNER JOIN内连接 和 OUTER JOIN外连接

INNER JOIN 或 JOIN // JOIN 默认为内连接INNER JOIN

选择orders表中的订单，但不展示customer_id，而返回顾客全名

```sql
SELECT order_id, first_name, last_name
FROM orders
JOIN customers 
	ON orders.customer_id = customers.customer_id
-- JOIN ON 代表当连接orders表和customers表时
-- 		确保orders表里的顾客id和customers表里的顾客id列一样
```

能不能在内连接的表里SELECT 连接的那个相同项呢？不能

错误❌示范：
```sql
SELECT order_id, customer_id, first_name, last_name
FROM orders
JOIN customers 
	ON orders.customer_id = customers.customer_id
-- Error Code: 1052. Column 'customer_id' in field list is ambiguous
-- 因为orders和customers表里都有customer_id, MySQL无法确定我们想从那张表里选取这列
```
正确✅示范：加上表名称前缀
```sql
SELECT order_id, orders.customer_id, first_name, last_name
FROM orders
JOIN customers 
	ON orders.customer_id = customers.customer_id
-- 当多张表里有一样的列，需增加表名称前缀
-- orders.customer_id 或者 customers.customer_id
```
使用alias让上述代码更简单。简化表格名称，就在FROM JOIN 后面写alias
```sql
SELECT order_id, o.customer_id, first_name, last_name
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
-- 简化表格名称，就在FROM JOIN 后面写alias
-- FROM orders o代表用o作为orders的别名
-- JOIN customers c 代表用c作为customers别名
```
有了别名之后，其他地方**必须**都用别名，原名 MySQL就不认识了
