### 更新操作

> 插入   修改    删除

#### 插入

格式为

```sql
INSERT INTO 表名
VALUES(属性值1, 属性值2, 属性值3……);
-- 或者
INSERT INTO 表名(属性值1, 属性值2, 属性值3……)
VALUES(属性值1, 属性值2, 属性值3……);
```

若表有4个属性，却只给了3个属性值

则数据库自动给其附空值NULL

**插入子查询结果**

```sql
INSERT INTO 表名
子查询;
-- 即
INSERT INTO 表名
SELECT ……
FROM ……
WHERE ……
……

SELCET * INTO ts1 FROM ts
```



#### 修改数据

格式

```SQL
UPDATE 表名
SET 列名 = 表达式
WHERE 条件

--加入子查询
UPDATE 表名
SET 列名 = 表达式
WHERE 属性 IN 
	(SELECT 属性
     FROM 表
     WHERE 属性=值)
```



#### 删除数据

```sql
DELECT 
FROM 表
WHERE 条件  -- 删除所有符合条件的数据

-- 删除表中数据 使表变为空表
DELECT 
FROM 表

-- 带子查询的 删除语句
DELECT 
FROM 表
WHERE 属性 IN
	(SELECT 属性
     FROM 表
     WHERE 属性=值)
```







**实验目的：**

1.  掌握SQL插入、更新数据更新操作。
2.  掌握掌握生成表操作。
3.  掌握删除记录操作。
4.  掌握各种综合数据更新操作。

**实验环境：**

1.  硬件要求：微处理器Intel奔腾4，内存512M以上。
2.  运行环境：Windows 7/8/10。
3.  语言环境：SQL SERVER 2008及以上版本。

**实验内容与要求：**

```SQL
1.将素材文件下载到本地，并将其还原到SQL SERVER库中，在C盘建立gxsql的文件夹，下列操作生成的sql文件均存放到该文件夹中。

-- 2.将财会系读者借书记录存入一个新表ckjy中，保存字段为借书证号、姓名、书名、借书时间，查询保存为gx1.sql。
-- gx1.sql
USE 图书管理
SELECT dz.借书证号,姓名,书名,借书日期 AS 借书时间 INTO ckjy 
FROM dz,jy,ts
WHERE dz.借书证号 = jy.借书证号 AND jy.总编号 = ts.总编号 
AND 单位='财会系'
-- 存在则修改，不存在则创建后修改

-- 3.将记录（'123','沈小霞','大学英语','2009-10-15'),（'125','张自强','线性代数','2011-4-15')插入到ckjy表中，查询保存为gx2.sql。
-- gx2.sql
USE 图书管理
INSERT INTO ckjy (借书证号,姓名,书名,借书时间)
VALUES ('123','沈小霞','大学英语','2009-10-15'),
		('125','张自强','线性代数','2011-4-15')
		

-- 4.根据ts表的结构用命令建立一个新表ts1,，查询保存为gx3.sql。
-- gx3.sql
USE 图书管理
SELCET * INTO ts1 FROM ts    -- 创建修改

-- 5.将数据库类的图书插入到表ts1中，数据库类图书包含书“数据库”和“Fox”两个关键字，查询保存为gx4.sql。
--gx4.sql
USE 图书管理
INSERT INTO ts1(书名)
SELECT 书名
FROM ckjy
WHERE 书名 LIKE '数据库' AND 书名 LIKE 'Fox'


-- 6.将dz表中的年龄字段利用出生日期字段计算出并填充到各个记录中,，查询保存为gx5.sql。
-- gx5.sql

-- 错解
USE 图书管理
UPDATE dz 
SET 年龄 = GETDATE() - YEAR(出生日期)
-- 报错
-- 不允许从数据类型 datetime 到 smallint 的隐式转换。请使用 CONVERT 函数来运行此查询。

-- 正解
USE 图书管理
UPDATE dz 
SET 年龄 = YEAR(GETDATE()) - YEAR(出生日期)


-- 7.将ts表中所有科学出版社的图书的价格设置成原来价格的八折,，查询保存为gx6.sql。
-- gx6.sql
-- 正解
use 图书管理
go
update ts set 单价=单价*0.8
where 书名 in (select 书名 from ts where 出版单位='科学出版社')

-- 我的
USE 图书管理
UPDATE ts
SET 单价 = 单价 * 0.7
WHERE 单价 IN
	(SELECT 单价
	 FROM ts
	 WHERE 出版单位 = '科学出版社')

-- 8.将所有的高级职称（包含“教授”连个字）、姓名为2个字的读者 插入到新表gjdz表中，查询保存为gx7.sql。
-- gx7.sql
USE 图书管理
SELECT * INTO gjdz
FROM dz
WHERE 职称 LIKE '%教授%' AND 姓名 LIKE '__'  -- 表达式中有通配符时，连接符号为 LIKE

-- 9.删除dz表中家住3楼或3楼以下的读者，查询保存为gx8.sql。
--gx8.sql
USE 图书管理
delete 
from dz 
where SUBSTRING(地址,4,1)<=3

-- 10.删除借阅表中关于计算机基础的借阅信息，查询保存为gx9.sql。
-- gx9.sql
USE 图书管理
DELETE
FROM ts
WHERE 书名 = '%计算机基础%'

-- 11.删除gjdz表，查询保存为gx10.sql。
-- gx10.sql
USE 图书管理
DELETE
FROM gjdz

```

将gxsql文件夹压缩上传。