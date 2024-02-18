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

## DBMS分类

- 层次性数据库（Hierarchical DB）
- 关系型数据库（Relational）
  最广泛使用的数据库

  形式为行列二维表，类似 excel，包括：

    - SQL（Structured Query Language，结构化查询语言）

    - 关系数据库管理系统（Relational Database Management System，RDBMS）
- 面向对象数据库（Object Oriented Database, OODB）
- XML 数据库（XML Database, XMLDB）
- 键值存储系统（Key- Value Store, KVS）

## RDBMS常见系统结构
Client/Server

Client使用SQL语句请求数据读写

Server即RDBMS是管理数据库，相应语句返回请求的数据

## 什么是SQL
SQL（发音为字母S-Q-L 或sequel）是Structured Query Language（结构化查询语言）的缩写。SQL 是一种专门用来与数据库沟通的语言。

### SQL分类
1. DDL（Data Definition Language，数据定义语言）

    CREATE： 创建数据库和表等对象

    DROP： 删除数据库和表等对象

    ALTER： 修改数据库和表等对象的结构

2. DML（Data Manipulation Language，数据操纵语言）**占绝大多数**

    SELECT：查询表中的数据

    INSERT：向表中插入新数据

    UPDATE：更新表中的数据

    DELETE：删除表中的数据

3. DCL（Data Control Language，数据控制语言）

    COMMIT： 确认对数据库中的数据进行的变更

    ROLLBACK： 取消对数据库中的数据进行的变更

    GRANT： 赋予用户操作权限

    REVOKE： 取消用户的操作权限

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

## 数据库/表的操作
``` SQL
#创建数据库 shop
CREATE DATABASE shop ;

# 创建表 Product
CREATE TABLE Product
(
    # 列名 数据类型 该列所需约束
    product_id CHAR(4) NOT NULL,
    product_name VARCHAR(100) NOT NULL,
    product_type VARCHAR(32) NOT NULL,
    sale_price INTEGER ,
    purchase_price INTEGER ,
    regist_date DATE ,
    PRIMARY KEY (product_id) # 设置主键约束
);

# 删除表
DROP TABLE Product

# 表的更新
ALTER TABLE Product ADD COLUMN amount INTEGER;
ALTER TABLE Product DROP COLUMN amount;
RENAME TABLE Product to PRODUCTS;

```
## 数据类型指定

1. `INTEGER` 整数
2. `CHAR` 定长字符串
   在括号中指定长度

   在达不到最大长度时用半角空格补足

   字符串区分大小写
3. `VARCHAR` 可变长度字符串
    不需要补齐最大长度
4. `DATE` 日期

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

### WHERE来选择记录
``` SQL
SELECT prod_name
FROM Products
WHERE <condition clause> #必须紧跟在FROM语句之后
```
### 运算符
\+ - \* /
\> < >= <= = <>
`NOT` `AND` `OR`应当与比较运算语句搭配使用
- NULL和任何数做加减乘除都会是NULL，也没办法使用比较运算符，必要时可以用`IS NULL` `IS NOT NULL`


# CH3 聚合与排序
## 对表进行聚合查询
所谓聚合，就是将多行汇总为一行。
### 常用聚合函数
- COUNT：计算表中的记录数（行数）

- SUM：计算表中数值列中数据的合计值

- AVG：计算表中数值列中数据的平均值

- MAX：求出表中任意列中数据的最大值

- MIN：求出表中任意列中数据的最小值

### 注意NULL
- COUNT(*)会返回包括NULL在内的所有行数，而COUNT(\<name>)则返回该列不包含NULL的行数
- 其他聚合函数都会忽略NULL
### 聚合函数删除重复值（DISTINCT）
```SQL
# 输出prod_type去重后有几行
SELECT COUNT(DISTINCT prod_type)
FROM product;

SELECT DISTINCT COUNT(prod_type)
FROM product;
# 不同于上面先去重再聚合，这样把DISTINCT写在外面会先聚合再去重

```

## 对表进行分组 GROUPBY
```SQL

SELECT DISTINCT prod_type, COUNT(*)
FROM product
GROUP BY prod_type
# 会返回每一种商品的数据行数
```
### GROUP BY 和 WHERE并用的时候的执行顺序
FROM→ WHERE→ GROUP BY→ SELECT

### 常见错误1 —— SELECT语句中书写了多余的列
不能把聚合键之外的列名写在SELECT语句中
### 常见错误2 —— 在 GROUP BY 子句中写了在 SELECT 里指定的列的别名（也就是as后面的列名）
跟执行顺序有关，最后执行SELECT
### 常见错误3 —— 在WHERE子句中使用聚合函数
**只有SELECT, ORDER BY 和HAVING子句中可以使用聚合函数**

## 为聚合结果指定条件 HAVING
HAVING子句：为分组指定条件
WHERE子句：指定数据行的条件
```SQL
-- WHERE 子句
SELECT *
FROM "Product"
WHERE "product_type" = '体育'

-- HAVING 子句：分组行数为2行的组
SELECT "product_type", COUNT(*)
FROM "Product"
GROUP BY "product_type"
HAVING COUNT(*) = 2

-- WHERE + HAVING 子句
SELECT "product_type", COUNT(*)
FROM "Product"
GROUP BY "product_type"
WHERE "product_price" > 10
HAVING COUNT(*) = 2
```
使用HAVING可以理解为对GROUP BY后得到的结果作为起点进行约束
## 对查询结果进行排序 ORDER BY
`asc` ascendent 升序（省略默认）

`desc` descendent 降序（不可省略）

可以使用多个排序键，按照先后顺序参考排序

**执行顺序 FROM→ WHERE→ GROUP BY→ HAVING→ SELECT→ ORDER BY**

