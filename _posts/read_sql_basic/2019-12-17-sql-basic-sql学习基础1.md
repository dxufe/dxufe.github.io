---
title: "sql基础学习1"
subtitle: "「sql」- basic"
layout: post
author: "Dxufe"
header-style: text
tags:
  - sql-basic
---


##### 1.引号
	SQL 使用单引号来环绕文本值（大部分数据库系统也接受双引号）。如果是数值，请不要使用引号。
	SELECT  FROM Persons WHERE Year1965
	
##### 2.where and or
	1.AND 和 OR 可在 WHERE 子语句中把两个或多个条件结合起来。
	2.也可以把 AND 和 OR 结合起来（使用圆括号来组成复杂的表达式）
```
    SELECT  FROM Persons WHERE (FirstName='Thomas' OR FirstName='William')
AND LastName='Carter'’
```

##### 3.order by
    1.以字母顺序升序 
    SELECT Company, OrderNumber FROM Orders ORDER BY Company
    2.字母升序数字升序，多个条件
	SELECT Company, OrderNumber FROM Orders ORDER BY Company, OrderNumber
    3.以字母降序
	SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC
    4.字母降序，数字升序。
	SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC,OrderNumber ASC
	
##### 4.insert into
    1.向表格中插入行的行
	INSERT INTO 表名称 VALUES (值1, 值2,....)
    2.指定所要插入数据的列
	INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
    3.从其他表select再插入，就不用values
    insert into actor_name select last_name,first_name from actor

##### 5.update
    1.更新某一行中的某一列：
    UPDATE 表名称 SET 列1名称 = 新值，列2名称=新值 WHERE 列名称 = 某值
	
##### 6.delete
	1.删除某行：DELETE FROM 表名称 WHERE 列名称 = 值
	2.删除所有行，保留表结构、属性和索引：DELETE FROM table_name
