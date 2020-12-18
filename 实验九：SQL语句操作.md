**实验目的：**

1.  掌握SQL基本的单表和多表查询。
2.  掌握SQL语句的条件查询。
3.  掌握SQL语句的分组查询和排序查询。
4.  掌握简单的谓词关键字的使用。

**实验环境：**

1.  硬件要求：微处理器Intel奔腾4，内存512M以上。
2.  运行环境：Windows 7/8/10。
3.  语言环境：SQL SERVER 2008及以上版本。

**实验内容与要求：
**

```sql
1.打开SSMS（集成管理器），新建一个“书籍管理”的数据库，并将素材的中的ACCESS数据导入到该数据库。在C盘建立一个sql的文件夹，下面的sql查询语句均放入该文件夹。


-- 2.新建查询，查询计算机类的图书的书籍名称、定价和出版社名称。将查询语句保存为sj1.sql。
-- sj1.sql
USE 书籍管理
SELECT 书籍名称, 定价, 出版社名称
FROM tBook
WHERE 类别='计算机'


-- 3.新建查询，查询价格在15元到25元之间的电子工业出版社的图书的书籍号、名称、定价，并分别用书籍编码、书名和价格做为显示标题。将查询语句保存为sj2.sql。
-- sj2.sql
USE 书籍管理
SELECT 书籍名称, 书籍名称, 定价
FROM tBook
WHERE 出版社名称='电子工业出版社' AND 定价 > 15 AND 定价 < 25

-- 4.新建查询，查询女雇员的雇员号、姓名和年龄。将查询语句保存为sj3.sql。
-- sj3.sql
USE 书籍管理
SELECT 雇员号, 姓名, 2020 - YEAR(出生日期) AS 年龄
FROM tEmployee
WHERE 性别 = '女'

-- 5.新建查询，查询哪些雇员手头有订单，显示其雇员姓名（去除重复记录）。将查询语句保存为sj4.sql。
-- sj4.sql
USE 书籍管理
SELECT DISTINCT 姓名      -- DISTINCT  去重
FROM tEmployee, tOrder
WHERE tEmployee.雇员号 = tOrder.雇员号
GROUP BY tEmployee.雇员号, 姓名
HAVING COUNT(订单号) > 0

-- 6.新建查询，查询2季度订购的订单号、雇员姓名和职务。将查询语句保存为sj5.sql。
-- sj5.sql
USE 书籍管理
SELECT 订单号, 姓名 AS 雇员姓名, 职务
FROM tEmployee, tOrder
WHERE tEmployee.雇员号 = tOrder.雇员号 AND MONTH(订购日期) in (4,5,6)
-- GROUP BY 订单号, 姓名, 职务   可加可不加


-- 7.新建查询，查询原理类的图书（书名包含原理）的订单明细号、书名、定价和售出价格。将查询语句保存为sj6.sql。
-- sj6.sql
USE 书籍管理
SELECT 订单明细号, 书籍名称 AS 书名, 定价, 售出单价 AS 售出价格
FROM tDetail, tBook
WHERE tDetail.书籍号 = tBook.书籍号 AND 书籍名称 LIKE '%原理%'
-- 注意表之间的连接   % 表示任意数量的任意字符

-- 8.新建查询，查询经理经手的雇员号、订单号、书籍号和订购日期，以雇员号升序排列，雇员号相同，以订单号降序排列。将查询语句保存为sj7.sql。
-- sj7.sql
USE 书籍管理
SELECT tEmployee.雇员号, tOrder.订单号, 书籍号, 订购日期
FROM tEmployee, tOrder, tDetail
WHERE tEmployee.雇员号 = tOrder.雇员号 AND tOrder.订单号 = tDetail.订单号
AND tEmployee.职务 = '经理'
ORDER BY tEmployee.雇员号 ASC , tOrder.订单号 DESC
-- DESC 降序

-- 9.新建查询，查询每笔订单明细的明细号、书籍名、总价。将查询语句保存为sj8.sql。
-- sj8.sql
USE 书籍管理
SELECT 订单明细号, 书籍名称, 数量 * 售出单价 AS 总价
FROM tBook, tDetail
WHERE tBook.书籍号 = tDetail.书籍号


-- 10.新建查询，查询工业出版社每类图书的的平均价格，显示类别名和平均价格。将查询语句保存为sj9.sql。
-- sj9.sql
USE 书籍管理
SELECT 类别 AS 类别名, AVG(定价) AS 平均价格
FROM tBook
-- WHERE 出版社名称 like '%工业出版社'
GROUP BY 类别


-- 11.新建查询，查询每个雇员经手的的订单数量，显示雇员号和订单数量。将查询语句保存为sj10.sql。
-- sj10.sql
USE 书籍管理
SELECT  雇员号, COUNT(tOrder.订单号) AS 订单数量    
FROM tDetail, tOrder
WHERE tDetail.订单号 = tOrder.订单号
GROUP BY tOrder.雇员号

-- 错误
USE 书籍管理
SELECT  雇员号, 数量 AS 订单数量    
FROM tDetail, tOrder
WHERE tDetail.订单号 = tOrder.订单号
GROUP BY tOrder.雇员号, 数量
-- 此时 相同雇员号的订单并不会合并


-- 12.新建查询，查询最贵书籍的前三名，显示书名、作者、单价和出版社。将查询语句保存为sj11.sql。
-- sj11.sql
USE 书籍管理
SELECT top 3 定价 AS 单价, 书籍名称 AS 书名, 作者名 AS 作者,  出版社名称 AS 出版社
FROM tBook
ORDER BY 定价 DESC  -- 先将表格降序排序，然后  top 3 取前三个

-- 13.新建查询，查询下订单数最多的客户号和订单数。将查询语句保存为sj12.sql。
-- sj12.sql
USE 书籍管理
SELECT TOP 1 COUNT(订单号) AS 订单数, 客户号
FROM tOrder
GROUP BY 客户号
ORDER BY  订单数 DESC  -- 降序


-- 14.新建查询，查询哪几笔订单的购买的书籍种类大于3，显示订单号,客户号和书籍种类数。将查询语句保存为sj13.sql。
-- sj13.sql 


15.新建查询，查询按出生日期月份的升序显示名称为2个字雇员的姓名、性别、职称和出生日期的月份。将查询语句保存为sj14.sql。

16.新建查询，查询每笔订单的订单号、客户号、雇员名称、书籍名称和总价,并按订单号升序排列。将查询语句保存为sj15.sql。
```

最后将文件夹sql压缩上传。