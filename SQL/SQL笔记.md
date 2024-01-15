借鉴别人的学习笔记：https://www.cnblogs.com/xjnotxj/p/12372807.html#2294870506

# CH1 DBMS与SQL
## DBMS是什么？
数据库管理系统 (Database Management System, DBMS) 是用来管理数据库的计算机系统。

## 为什么使用DBMS
问：为什么不用 文本文件 或者 excel（+VBA）?

答：DBMS 的好处有：

多人共享

海量存储

可编程

容灾机制
……

## 什么是SQL
SQL（发音为字母S-Q-L 或sequel）是Structured Query Language（结构化查询语言）的缩写。SQL 是一种专门用来与数据库沟通的语言。




# CH2 SQL 基础
## 数据库
### 列
表中的一个字段。所有表都是由一个或多个列组成的。
### 数据类型
允许什么类型的数据。每个表列都有相应的数据类型，它限制（或允许）该列中存储的数据。
**在磁盘优化中有重要作用，需特别关注**
**数据类型及其名称是SQL 不兼容的一个主要原因**
### 行
表中的一个记录
### 表
表是一种结构化的文件，可用来存储某种特定类型的数据。
一个数据库中的表名唯一
### 主键
表中一列或几列唯一标识表的一行的字段
* 应该总是定义主键；
* 任意两行都不具有相同的主键值；
* 每一行都必须具有一个主键值（主键列不允许空值NULL）；
* 主键列中的值不允许修改或更新；
* 主键值不能重用（如果某行从表中删除，它的主键不能赋给以后的新行）。

## Select语句
* SQL语句不区分大小写
* 可以空格 也可以分行 最好最后加上分号
  
``` SQL
SELECT prod_id id, prod_name name, prod_price price
FROM Products;
```

### 去重返回Distinct
直接放在列名前面，不能部分使用
`SELECT DISTINCT vend_id, prod_price`就会把这两列的所有组合中相同的去掉
### 限制结果
```SQL
-- 只返回前五行数据，各数据库软件中该操作的语句不同
-- MySQL
SELECT prod_name
FROM Products
LIMIT 5;
```

```SQL
-- 返回偏移5行后的5 行数据
-- MySQL
SELECT prod_name
FROM Products
LIMIT 5 OFFSET 5;
```