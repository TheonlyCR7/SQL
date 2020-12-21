## SQL 函数

**SQL 拥有很多可用于计数和计算的内建函数**



语法

```sql
SELECT function(列) FROM 表
```



函数基本类型

-   Aggregate 函数   面向多个值，返回单一值
-   Scalar 函数     

**注释：**如果在 SELECT 语句的项目列表中的众多其它表达式中使用 SELECT 语句，则这个 SELECT 必须使用 GROUP BY 语句





### SQL AVG

AVG 函数返回数值列的平均值。NULL 值不包括在计算中

```sql
SELECT AVG(column_name) FROM table_name
```



我们拥有下面这个 "Orders" 表：

| O_Id | OrderDate  | OrderPrice | Customer |
| :--- | :--------- | :--------- | :------- |
| 1    | 2008/12/29 | 1000       | Bush     |
| 2    | 2008/11/23 | 1600       | Carter   |
| 3    | 2008/10/05 | 700        | Bush     |
| 4    | 2008/09/28 | 300        | Bush     |
| 5    | 2008/08/06 | 2000       | Adams    |
| 6    | 2008/07/21 | 100        | Carter   |

现在，我们希望计算 "OrderPrice" 字段的平均值。

我们使用如下 SQL 语句：

```sql
SELECT AVG(OrderPrice) AS OrderAverage FROM Orders
```

结果集类似这样：

| OrderAverage |
| :----------- |
| 950          |

### 例子 2

现在，我们希望找到 OrderPrice 值高于 OrderPrice 平均值的客户。

我们使用如下 SQL 语句：

```sql
SELECT Customer 
FROM Orders
WHERE OrderPrice > (SELECT AVG(OrderPrice) FROM Orders)
```

结果集类似这样：

| Customer |
| :------- |
| Bush     |
| Carter   |
| Adams    |





### SQL COUNT()

COUNT() 函数返回匹配指定条件的行数 (**返回一个数值**)  NULL 不计入

```sql
SELECT COUNT(column_name) FROM table_name
```

返回指定列的不同值的数目

```sql
SELECT COUNT(DISTINCT column_name) FROM table_name
```



### SQL GROUP BY 

GROUP BY 语句用于结合合计函数，根据一个或多个列对结果集进行分组

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name  -- 以该列为基础  进行归类
```

**实例  ORDERS表**

| O_Id | OrderDate  | OrderPrice | Customer |
| :--- | :--------- | :--------- | :------- |
| 1    | 2008/12/29 | 1000       | Bush     |
| 2    | 2008/11/23 | 1600       | Carter   |
| 3    | 2008/10/05 | 700        | Bush     |
| 4    | 2008/09/28 | 300        | Bush     |
| 5    | 2008/08/06 | 2000       | Adams    |
| 6    | 2008/07/21 | 100        | Carter   |

查找每个客户的总金额（总订单），使用 GROUP BY 语句对客户进行组合

```sql
SELECT Customer,SUM(OrderPrice) 
FROM Orders
GROUP BY Customer
```

结果为

| Customer | SUM(OrderPrice) |
| :------- | :-------------- |
| Bush     | 2000            |
| Carter   | 1700            |
| Adams    | 2000            |

若不使用 **GROUP BY**   

```sql
SELECT Customer,SUM(OrderPrice) 
FROM Orders
```

结果为

| Customer | SUM(OrderPrice) |
| :------- | :-------------- |
| Bush     | 5700            |
| Carter   | 5700            |
| Bush     | 5700            |
| Bush     | 5700            |
| Adams    | 5700            |
| Carter   | 5700            |

也可以对一个以上的列应用 GROUP BY 语句，就像这样：

```sql
SELECT Customer,OrderDate,SUM(OrderPrice) FROM Orders
GROUP BY Customer,OrderDate
```

