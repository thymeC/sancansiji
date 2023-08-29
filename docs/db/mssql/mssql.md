# SQL - Structured Query Language

Reference:
- [learn.microsoft sql](https://learn.microsoft.com/en-us/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?view=sql-server-ver16)(got more detail, like date functions)
- https://www.w3schools.com/sql/sql_intro.asp
- [SQL必知必会] [本 福达]

## SQL Introduction
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

## Some of The Most Important SQL Commands

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
#### 常用的文本处理函数：
- LEFT, RIGHT, SUBSTRING
- LTRIM, RTRIM, TRIM
- LOWER, UPPER
- LENGTH

#### 常用的日期和时间处理函数：
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


#### 聚集函数
- AVG, COUNT, MAX, MIN, SUM

### 子查询
```sql
select cust_id 
from orders 
where order_num in (select order_num from orderitems where prod_id = 'RGAN01');

select cust_name, 
    cust_state, 
    (select count(*) from orders where orders.cust_id = customers.cust_id) as orders
from customers order by cust_name;
```

### 联结表

![img.png](join.png)

### 组合查询
union

主要有两种情况需要使用组合查询：
- 在一个查询中从不同的表返回类似结构的数据
- 对一个表执行多个查询， 按单个查询返回的结果组合成一个结果集
任何具有多个where子句的查询都可以重写为使用union的查询

union自动去除了重复的行，如果想保留重复的行，使用union all

```sql


```sql
select cust_name, cust_contact, cust_email
from customers
where cust_state in ('IL', 'IN', 'MI')
union
select cust_name, cust_contact, cust_email
from customers
where cust_name = 'Fun4All';
```

```sql
select cust_name, cust_contact, cust_email
from customers
where cust_state in ('IL', 'IN', 'MI') or cust_name = 'Fun4All';
```
这个例子中，union可能比where子句复杂些，但对于较复杂的过滤条件，或者从多个表中检索数据，使用union可能更简单


### 插入数据

插入有几种方式
- 插入完整的行
- 插入行的一部分
- 插入某些查询的结果


```sql
-- 这里值要按顺序填充，如果和列对应不上，不太安全
insert into customers
values ('1000000006', 'Toy Land', '123 Any Street', 'New York', 'NY', '11111', 'USA', NULL, NULL);

insert into customers(cust_id, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country, cust_contact, cust_email)
values ('1000000006', 'Toy Land', '123 Any Street', 'New York', 'NY', '11111', 'USA', NULL, NULL);

-- 插入行的一部分
insert into customers(cust_id, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country)
values ('1000000006', 'Toy Land', '123 Any Street', 'New York', 'NY', '11111', 'USA');

-- 插入检索出的数据
insert into customers(cust_id, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country)
select cust_id, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country
from custnew;

-- 从一个表复制到另一个表
-- create xxx select xxx
create table custcopy as select * from customers;
select * into custcopy from custcomers;
```

### 更新和删除数据

```sql
-- update 不要忘记where条件
update customers
set cust_email = 'kim@163.com',
    cust_contact = 'Kim Kim'
where cust_id = '1000000006';

-- delete 不要忘记where条件
delete from customers
where cust_id = '1000000006';
```

使用update和delete的规则
- 除非必要，否则都带上where
- select先测试，然后再update或delete
- 使用强制实施引用完整性的数据库，可以防止删除与其他表关联的行


## 创建和操纵表

```sql
-- create table products
create table products
(
    prod_id char(10) NOT NULL,
    vend_id char(10) NOT NULL,
    prod_name char(255) NOT NULL,
    prod_price decimal(8,2) NOT NULL,
    prod_desc varchar(1000) NULL
);

-- 指定默认值 with default
create table orders
(
    order_num int NOT NULL,
    order_date datetime NOT NULL DEFAULT GETDATE(),
    cust_id char(10) NOT NULL
);

-- update table
ALTER TABLE Vendors
ADD vend_phone CHAR(20);

ALTER TABLE Vendors
DROP COLUMN vend_phone;

-- delete table
drop table vendors;

-- rename table
SQL Server 用sp_rename存储过程
```

### 存储过程

存储过程，即把过程（一段sql statement）存储在数据库中，可以在需要的时候调用
```sql
CREATE PROCEDURE procedure_name
AS
sql_statement
GO;
```

```sql
-- 执行存储过程
EXEC procedure_name;
```

```sql
CREATE PROCEDURE SelectAllCustomers 
    @City nvarchar(30)
AS
    SELECT * FROM Customers WHERE City = @City
GO;

EXEC SelectAllCustomers @City = 'London';

CREATE PROCEDURE SelectAllCustomers 
    @City nvarchar(30), 
    @PostalCode nvarchar(10)
AS
    SELECT * FROM Customers WHERE City = @City AND PostalCode = @PostalCode
GO;

EXEC SelectAllCustomers @City = 'London', @PostalCode = 'WA1 1DP';

USE AdventureWorks2022;  
GO  
CREATE PROCEDURE HumanResources.uspGetEmployeesTest2   
    @LastName nvarchar(50),   
    @FirstName nvarchar(50)   
AS   

    SET NOCOUNT ON;  
    SELECT FirstName, LastName, Department  
    FROM HumanResources.vEmployeeDepartmentHistory  
    WHERE FirstName = @FirstName AND LastName = @LastName  
    AND EndDate IS NULL;  
GO

EXECUTE HumanResources.uspGetEmployeesTest2 N'Ackerman', N'Pilar';  
-- Or  
EXEC HumanResources.uspGetEmployeesTest2 @LastName = N'Ackerman', @FirstName = N'Pilar';  
GO  
-- Or  
EXECUTE HumanResources.uspGetEmployeesTest2 @FirstName = N'Pilar', @LastName = N'Ackerman';  
GO

CREATE PROCEDURE UpdateEmployee @EmpID int, @Name nchar(50),
				@Department nchar(50), @Age int, @Salary real,
				@Message nchar(30) output
AS
BEGIN
SET NOCOUNT ON
IF EXISTS(SELECT * FROM [DBO].[Employees] WHERE [Employee ID]=@EmpID)
	BEGIN
	SET @Message='Row Updated'
	UPDATE Employees
	SET [Name]= @Name,
	Department=@Department,
	Age=@Age,
	Salary=@Salary
	WHERE [Employee ID]=@EmpID
	END
ELSE
	BEGIN
	INSERT INTO Employees([Employee ID],[Name], [Department], [Age], [Salary])
	VALUES(@EmpID, @Name, @Department, @Age, @Salary)
	SET @Message='Row Inserted'
	END
END

```