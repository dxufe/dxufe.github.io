---
title: "sql基础学习3"
subtitle: "「sql」- basic"
layout: post
author: "Dxufe"
header-style: text
tags:
  - sql-basic
---
##### 1.sql-select into,备份数据，前提是备份表已经存在。

1. select * into tablename-backup from origintablename  整表备份
2. select a,b from tablenmae-backup from origintablename where c='' 备份满足条件的列
3. select TA.a,TB.b into TC from TA inner join TB on TA.id=TB.id    备份join起来的数据
4. select * into tablename-backup in 'aaa.MDB' from origintablename  跨库拷贝
##### 2.create table

1. CREATE TABLE 表名称
(列名称1 数据类型,
列名称2 数据类型,
列名称3 数据类型,
....)

1. 数据类型：varchar(size)  容纳可变长度的字符串（可容纳字母、数字以及特殊的字符）。在括号中规定字符串的最大长度。
2.  char(size) 容纳固定长度的字符串
3.  integer(size) int(size) smallint(size) tinyint(size)  ：仅容纳整数。在括号内规定数字的最大位数。
4.  decimal(size,d) numeric(size,d)：容纳带有小数的数字。"size" 规定数字的最大位数。"d" 规定小数点右侧的最大位数。
5.  date(yyyymmdd)，容纳日期。
##### 3.sql约束
约束用于限制加入表的数据的类型。可以在创建表时规定约束（通过 CREATE TABLE 语句）。主要探讨以下几种约束：NOT NULL UNIQUE PRIMARY KEY FOREIGN KEY CHECK DEFAULT

1. NOT NULL 
    强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。
    
```
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```
2. UNIQUE

    UNIQUE 约束唯一标识数据库表中的每条记录。UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证。PRIMARY KEY 拥有自动定义的 UNIQUE 约束。请注意，每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束。
    MYSQL在某一列创建unique约束
```
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
UNIQUE (Id_P)
)
```
 命名并增加多个列的unique约束:CONSTRAINT uc_PersonID UNIQUE (Id_P,LastName)
 表已存在，添加unique约束:ADD UNIQUE (Id_P)

3. PRIMARY KEY

    PRIMARY KEY 约束唯一标识数据库表中的每条记录。主键必须包含唯一的值。主键列不能包含 NULL 值。每个表都应该有一个主键，并且每个表只能有一个
 
A. 
  ```
CREATE TABLE Persons
(Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
PRIMARY KEY (Id_P))
```

B. 
 ```
ALTER TABLE Persons
ADD PRIMARY KEY (Id_P)`
```

C. 
```
ALTER TABLE Persons
DROP PRIMARY KEY`
```

4. FOREIGN KEY

      <u>  FOREIGN KEY 约束用于预防破坏表之间连接的动作。FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。</u>
```
CREATE TABLE Orders
(
Id_O int NOT NULL,
OrderNo int NOT NULL,
Id_P int,
PRIMARY KEY (Id_O),
FOREIGN KEY (Id_P) REFERENCES Persons(Id_P))
```
```
ALTER TABLE Orders
ADD FOREIGN KEY (Id_P)
REFERENCES Persons(Id_P)
```
注意：
<u>要设置外键的字段不能为主键；改建所参考的字段必须为主键；两个字段必须具有相同的数据类型和约束</u>

5. CHECK

**单个约束**:
```
 CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CHECK (Id_P>0)
)
```

**多个约束**：
```
    CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT chk_Person CHECK (Id_P>0 AND City='Sandnes')
)
```

**表已存在增加约束：**
```
    ALTER TABLE Persons 
    ADD CHECK (Id_P>0)
#ADD CONSTRAINT chk_Person CHECK (Id_P>0 AND City='Sandnes')
```

6. DEFAULT

    DEFAULT 约束用于向列中插入默认值。如果没有规定其他的值，那么会将默认值添加到所有的新记录。

**默认值**：
```
    CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255) DEFAULT 'Sandnes'
)
```

**插入系统值：**
getdate()系统日期
```
CREATE TABLE Orders
(
Id_O int NOT NULL,
OrderNo int NOT NULL,
Id_P int,
OrderDate date DEFAULT GETDATE()
)
```

**表已存在 设定默认值：**
```
ALTER TABLE Persons
ALTER City SET DEFAULT 'SANDNES'
```

**表已存在 删除默认值：**
```
    ALTER TABLE Persons
ALTER City DROP DEFAULT
```
#
####  4 .create index
可以在表中创建索引，以便更加快速高效地查询数据。用户无法看到索引，它们只能被用来加速搜索/查询。注释：<u>更新一个包含索引的表需要比更新一个没有索引的表更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。</u>

在表上创建一个唯一的索引。唯一的索引意味着两个行不能拥有相同的索引值。:
    CREATE UNIQUE INDEX index_name
    ON table_name (column_name)
#### 5.drop index, table, database

1. drop index
mysql:
    ALTER TABLE table_name 
    DROP INDEX index_name
1. drop table
    DROP TABLE 语句用于删除表（表的结构、属性以及索引也会被删除）
    <u>DROP TABLE 表名称</u>

1. drop database
    DROP DATABASE 语句用于删除数据库
    DROP DATABASE 数据库名称

1. trundate table
    TRUNCATE TABLE 命令（仅仅删除表格中的数据）
    <u> TRUNCATE TABLE 表名称</u>
    不删除表的情况下删除所有的行。这意味着表的结构、属性和索引都是完整的：
    <u>DELETE FROM table_name</u>
#### 6.Alter 已存在的表中删除修改增加列

1. 添加列：
    ALTER TABLE table_name
ADD COLUMN column_name datatype

1. 删除列：
    ALTER TABLE table_name 
DROP COLUMN column_name

1. 修改数据类型：
    ALTER TABLE table_name
ALTER COLUMN column_name datatype
SQL Server / MS Access：
ALTER TABLE table_name
ALTER COLUMN column_name datatype
My SQL / Oracle：
ALTER TABLE table_name
MODIFY COLUMN column_name datatype



#### 7.AUTO INCREMENT
MYSQL:
```

CREATE TABLE Persons
(
P_Id int NOT NULL AUTO_INCREMENT,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
PRIMARY KEY (P_Id)
)
```
 AUTO_INCREMENT 的开始值是 1，每条新记录递增 1。要让 AUTO_INCREMENT 序列以其他的值起始，请使用下列 SQL 语法：
  ALTER TABLE Persons     AUTO_INCREMENT=100
