# SQL - Structured Query Language

Reference:
- [learn.microsoft sql](https://learn.microsoft.com/en-us/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?view=sql-server-ver16)(got more detail, like date functions)
- https://www.w3schools.com/sql/sql_intro.asp
- [SQL必知必会] [本 福达]

## SQL
SQL lets you access and manipulate databases

SQL became a standard of the American National Standards Institute (ANSI) in 1986, and of the International Organization for Standardization 
Although ANSI/ISO standard, there are different versions of the SQL language. However, to be compliant with the ANSI standard, they all support at least the major commands (such as **SELECT, UPDATE, DELETE, INSERT, WHERE**) in a similar manner.

RDBMS stands for Relational Database Management System.

## 数据库基础元素

- database
- table
- schema: 模式， 模式可以用来描述数据库中特定的表，也可以用来描述整个数据库（和其中表的关系）
- 列
- 数据类型，数据类型及其名称是SQL不兼容的一个主要原因
- 行, a record
- 主键，唯一，不为空，不允许被修改，删除后不能被重用

## Syntax
- SQL keywords are NOT case-sensitive
- Semicolon after SQL statements

### Some of The Most Important SQL Commands

```sql
SELECT - extracts data from a database
UPDATE - updates data in a database
DELETE - deletes data from a database
INSERT INTO - inserts new data into a database
CREATE DATABASE - creates a new database
ALTER DATABASE - modifies a database
CREATE TABLE - creates a new table
ALTER TABLE - modifies a table
DROP TABLE - deletes a table
CREATE INDEX - creates an index (search key)
DROP INDEX - deletes an index
```

### SQL select examples
select 子句顺序:
1. SELECT
2. FROM
3. WHERE
4. GROUP BY
5. HAVING
6. ORDER BY

```sql
SELECT * FROM Customers;
SELECT CustomerName, City FROM Customers;
-- distinct必须放列名前面， 不能部分使用distinct
SELECT DISTINCT column1, column2 FROM table_name;

SELECT * FROM Customers WHERE Country = 'Spain' AND CustomerName LIKE 'G%';
SELECT * FROM Products WHERE Price BETWEEN 50 AND 60;
SELECT * FROM Customers WHERE City IN ('Paris','London');
-- 复杂条件用not可以使查询变得简单
SELECT * FROM Customers WHERE NOT City IN ('Paris','London');
SELECT * FROM Customers WHERE City is NULL;
SELECT * FROM Customers WHERE City is NOT NULL;
-- %aaa%, %aaa, aaa%, a%b, a%b%, _pple, _a%, a_%, a__%, 
-- []匹配字符集， [abc]表示a或b或c， [^abc]表示非a非b非c
-- 使用通配符搜索会慢一些，其他的方式可以用的就不用通配符
SELECT * FROM Customers WHERE City LIKE 's%';


/*limit
Oracle, where rownum <= 5  
MySQL, limit 5  
DB2, fetch first 5 rows only */
SELECT TOP 5 prod_name FROM Products;

SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;

SELECT * FROM Customers
ORDER BY Country ASC, CustomerName DESC;

-- 拼接字段
SELECT vendor_name+ '(' + vendor_country + ')' FROM Vendors order by vendor_name;
-- RTRIM, LTRIM, TRIM
SELECT RTRIM(vendor_name) + '(' + RTRIM(vendor_country) + ')' FROM Vendors order by vendor_name;

-- group by
/* group by 可以包含多个列
group by 子句必须是检索列或有效的表达式，不能是聚集函数
group by 必须是where之后，order by之前
*/
SELECT vend_id, COUNT(*) as num_prods FROM Products GROUP BY vend_id;

-- having过滤分组
-- where 在分组前过滤行， having 在分组后过滤组
select cust_id, count(*) as orders from Orders group by cust_id having count(*) >= 2;
select vend_id, count(*) as num_prods from Products where prod_price >= 4 group by vend_id having count(*) >= 2;
select order_num, count(*) as items from OrderItems group by order_num having count(*) >= 2 order by items desc;
```
![img.png](where%20condition.png)
![img.png](wildcard.png)

### 使用函数
大多数SQL实现支持以下类型的函数：
- 用于处理文本字符串的函数，如删除或填充值，转换值为大写或小写
- 用于处理日期和时间值的函数， 如从日期中提取部分值， 返回日期之差，检查日期有效性
- 用于数值数据的函数， 如返回绝对值，舍入值，代数运算
- 用于生成格式化的内容的函数， 如格式化数字或日期，货币格式化


```sql
select vend_name, upper(vend_name) as vend_name_upcase from Vendors order by vend_name;

```
常用的文本处理函数：
- LEFT, RIGHT, SUBSTRING
- LTRIM, RTRIM, TRIM
- LOWER, UPPER
- LENGTH

常用的日期和时间处理函数：
- DATEPART, DATEDIFF, DATEADD
- GETDATE, GETUTCDATE

![img.png](date%20type.png)
for more, [learn.microsoft date](https://learn.microsoft.com/en-us/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?view=sql-server-ver16)

```sql
select order_num, order_date, DATEPART(yy, order_date) as order_year, DATEPART(mm, order_date) as order_month from Orders;

DATEDIFF ( datepart , startdate , enddate )
SELECT DATEDIFF(year,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(quarter,     '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(month,       '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(day,         '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(week,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
DATEADD (datepart , number , date )
SELECT DATEADD(month, 1, '20060830');
SELECT DATEADD(month, 1, '2006-08-31');

```
![img.png](datepart.png)


聚集函数
- AVG, COUNT, MAX, MIN, SUM


