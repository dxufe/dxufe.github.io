---
title: "sql基础学习2"
subtitle: "「sql」- basic"
layout: post
author: "Dxufe"
header-style: text
tags:
  - sql-basic
---
##### 1.sql like
   1."%" 可用于定义通配符
    SELECT * FROM Persons WHERE City LIKE '%lon%'
   
   2.使用 NOT 关键字
    SELECT * FROM PersonsWHERE City NOT LIKE '%lon%'
##### 2.sql通配符
  1.%替代一个或多个字符
  
  2._仅替代一个字符
        第三个字符为特， select *from file_related where prospectusName like '__特%';
##### 3.sql in
   1.IN 操作符允许我们在 WHERE 子句中规定多个值。SELECT * FROM Persons WHERE LastName IN ('Adams','Carter')
##### 4 betwween and
   
   1.操作符 BETWEEN ... AND 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。select * from file_related where prospectusName BETWEEN '万代服装股份有限公司' AND '三只松鼠股份有限公司'  返回按数值顺序 文本顺序或日期顺序排列的介于中间的值，是否包括两端需看sql版本。
  
  2.NOT BETWEEN ...AND...
  select * from file_related where prospectusName Not BETWEEN '万代服装股份有限公司' AND '三只松鼠股份有限公司'
##### 5 SQL Alias(指定别名)
1.为列名指定别名
    SELECT LastName AS Family, FirstName AS Name FROM Persons

2.为表指定别名
    select a.name from ffff as a
##### 6 sql join
SQL join 用于根据两个或多个表中的列之间的关系，从这些表中查询数据。A JOIN B ON的形式

1.#JOIN: 如果表中有至少一个匹配，则返回行，与INNER JOIN 相同，没有匹配则不会列出

2.#LEFT JOIN:  关键字会从左表 (table_name1) 那里返回所有的行，即使在右表 (table_name2) 中没有匹配的行。

3.#RIGHT JOIN: 关键字会右表 (table_name2) 那里返回所有的行，即使在左表 (table_name1) 中没有匹配的行。

4#FULL JOIN： 关键字会从左表和右表那里返回所有的行。如果 左表 中的行在右表 中没有匹配，或者如果右表中的行在左表中没有匹配，这些行同样会列出。
```
#引用两个表：
SELECT
	file_related.submit_date,
	file_related.company_name,
	issuer_information.name_of_legal_representative 
FROM
	file_related,
	issuer_information 
WHERE
	file_related.prospectus_md5 = issuer_information.prospectus_md5
    
#使用join或inner join
SELECT
	a.submit_date,
	a.company_name,
	bbbb.name_of_legal_representative 
FROM
	file_related AS a
	RIGHT JOIN issuer_information AS bbbb ON a.prospectus_md5 = bbbb.prospectus_md5

```

##### 7 mysql不支持 full join
MySQL不支持FULL JOIN,下面是替代方法
select * from A left join B on A.id = B.id 
union
select *
from A right join B on A.id = B.id
```


SELECT
	a.submit_date,
	a.company_name,
	bbbb.name_of_legal_representative 
FROM
	file_related AS a
	RIGHT JOIN issuer_information AS bbbb ON a.prospectus_md5 = bbbb.prospectus_md5 UNION 
SELECT
	a.submit_date,
	a.company_name,
	bbbb.name_of_legal_representative 
FROM
	file_related AS a
	LEFT JOIN issuer_information AS bbbb ON a.prospectus_md5 = bbbb.prospectus_md5)
```

8 SQL UNION
1.<u>UNION 操作符用于合并两个或多个 SELECT 语句的结果集。请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。</u>
2.另外，UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。
3. UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL
```
SELECT E_Name FROM Employees_China
UNION
SELECT E_Name FROM Employees_USA

SQL Statement 1
UNION ALL
SQL Statement 2
```



   
