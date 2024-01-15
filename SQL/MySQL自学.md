MySQL自学

十天打卡

# MySQL概述

## MySQL启动和客户端连接

`net start mysql80`启动

`net stop mysql80`停止、

MySQL自带命令行启动

或`mysql -u root -p`

## 数据模型

客户端通过sql语句操作DBMS，DBMS软件创建和操作多个数据库，每个数据库存储多个表

## 关系型数据库 RDBMS

建立在关系模型基础上，有多张相互连接的二维表组成的数据库





# SQL

## SQL通用语法

分号结尾每一行

空格缩进没有限制

不区分大小写 关键字最好大写

单行注释 -- 或#  多行注释/* */

## SQL分类

### DDL数据定义语言

    #查询数据库
    show databases;
    #创建数据库
    create database if not exists [数据库名];
    #删除
    drop database [数据库名];
    #使用
    use [数据库名];
    #查询当前所在的数据库
    select database();
    
    #表操作
    show tables;
    #创建表
    create table tb_user(
        -> id int comment 'id',
        -> name varchar(50),
        -> age int,
        -> gender varchar(1)
        -> ) comment 'user';
    #查询表
    desc tb_user;
    #查询表的建表语句
    show create table tb_user;
    
    #修改表
    #插入字段名
    alter table emp add nickname varchar(20);
    #修改字段名和字段类型
    alter table emp change nickname username varchar(30) comment '用户名';
    #修改数据类型
    alter table emp modify username char(20);
    #删除字段
    alter table emp drop username;
    #修改表名
    alter table emp rename to employee;
    #删除表
    drop table [if exists] employee;
    truncate table employee; #删除原表并重新创建该表



数据类型



### DML数据操作语言

#### 添加数据

    #给指定字段添加数据
    insert into 
    #给全部字段添加数据
    
    #批量添加数据
    



# 基础语句

运行原理顺序：from-\>where-\>group by-\>having-\>order by-\>limit

- select & from
    - select 字段名 
    - from 表名
    - 示例《world》

    SELECT name, continent, population FROM world #每个字段前面都放空格
    
    SELECT name as 国家, continent as 大洲, population as 人口 FROM world #只改变查询出的字段名为别名 也可以省略as
    SELECT name 国家, continent 大洲, population 人口 FROM world
    
    #查询全部
    SELECT * FROM world
    
    #查询去重
    SELECT continent FROM world #显示的大洲名有重复
    SELECT distinct continent FROM world #显示的大洲名每个只显示一次
    #distinct紧跟SELECT，并且如果是多个字段，必须多个字段每一项都一样的才会去重
    
    #可以计算加减乘除，还可以使用系统内置的函数
    SELECT name, gdp, population, gdp/population 人均gdp, continent FROM world

- where 数据筛选
    - = \> \< \>= \<= \<\> !=
    - between...and... 是大于等于且小于等于 
    - and or not
    - *like + 通配符  模糊匹配*
        - % 任意字符出现任意次数
        - \_ 任意字符出现一次

    SELECT name, gdp FROM world where name='China'
    
    SELECT name, population FROM world where name in ('China', 'Sweden')
    
    SELECT name, population FROM world where gdp between 25000000 and 300000000
    
    SELECT name, population FROM world
    where name like '_ar%' #得到的结果是Barbados、Marshall Islands、Paraguay
    
    SELECT name, population FROM world
    where (name like '%a%a%a%' and area >= 600000) or ( population > 1300000000)
    



`select name, population/1000000 'population in millions' from world where continent = 'South America'`



- order by 排序——后加多个字段，依次作为排序依据
    - desc 降序
    - asc 升序（默认）
    - `select winner, yr, subject from nobel where winner like 'Sir%' order by yr desc, winner asc`这里就是先按照年份降序，再按照winner名字升序
    - 配合limit起到限制查询的作用
        - limit 3 则只显示前3行
        - limit x, n 则显示第x-1到第x-1+n行



    #把化学和物理的放在最后
    SELECT winner, subject
      FROM nobel
     WHERE yr=1984
     ORDER BY  subject IN ('physics','chemistry'), subject,winner
    # subject IN ('chemistry','physics')是一个布尔型数值，为0或1，所以表中物理和化学奖项的该值为1，按照该数值升序，则自然排在最后 



- 聚合函数
    - sum() 
    - count(字段)返回该字段的非空值有多少行
    - count(\*)返回该字段的所全部行数
- group by 分组聚合



    select continent, count(name) from world group by continent
    #依据continent字段分区，然后去重分组，然后将分区内的多行数据聚合计算成一行数据，ex count计算每个分区内name的个数



- having *限定分组聚合后*的查询行必须满足的条件，这点与where不同，where的对象为分组聚合前

    #查询总人口数量至少为1亿的大洲
    select continent from world
    group by continent
    having sum(population)>100000000



# *常见函数*

- round(x,y) 对x四舍五入，小数点后y位
- avg()计算平均值
- concat(a,b,...) 连接字符串
- replace(a,b,c) 将字符串a中的所有b替换为c
- left(s,n) right(s,n)返回字符串s中的最左（右）的n个字符
- substring(s,n,len) 返回字符串s从第n个起的长度为n的字符串，len可选参数，如果长度不限制就不写
- length(s)返回字符串的长度
- substring\_index(s,'/',n)表示的是以‘/’符号来切割字符串s并取出其中第n个‘/’前面的内容
    - 嵌套使用：`SUBSTRING_INDEX(SUBSTRING_INDEX(profile,',',-2),',',1)`对于180cm,75kg,27,male数据可以取出年龄27
- cast转换数据类型
- date\_add偏移时间戳
- datediff算出偏移的时间天数
- date\_format

    select day(date) as day, count(question_id) question_cnt
    from question_practice_detail
    where  year(date)=2021  and  month(date)=8
    group by date



- 条件函数
    - case when
    - if（）

    #case when
    select device_id, gender, 
    (case when age<20  then  "20岁以下"  
    when (age>=20  and age<=24) then  "20-24岁"
    when age>=25  then  "25岁及以上"
    else  "其他" end) as age_cut
    from user_profile
    
    #if
    select  if(age>=25,'25岁及以上','25岁以下') age_cut, count(device_id) number
    from user_profile
    group by age_cut



# 窗口函数

- 标准语法
    - over([partition by 字段名][order by 字段名 asc|desc])分区内排序，order by不会对表格产生影响 
    - 处理的是基于having后但还没有order by的表格来进行分区然后排序，不会影响原表的顺序
    - 对比排序方法的异同 对于99 99 90 89
        - rank()  1,1,3,4
        - dense\_rank() 1,1,2,3
        - row\_number() 1,2,3,4
- 偏移分析函数
    - lag(confirmed,1) over(partition by name order by whn)分区排序后向上偏移一行，就是取上一行
    - lead(...)就是向下偏移

    SELECT name, date_format(whn,'%Y-%m-%d') date, confirmed, 
    lag(confirmed,1) over(partition by name order by whn) 之前的确诊人数, 
    (confirmed-lag(confirmed,1) over(partition by name order by whn)) 当日确诊
    from covid
    where name in ('France','Germany') and month(whn)=1
    order by whn



# 表连接

- 基础语法
    - 完全连接——就是将两表的所有数据中匹配到的相同字段对应的数值的所有可能的排列组合全部列出来
    - 内连接
        - select
        - from表1 inner join 表2 on 表1.字段名=表2.字段名
        - inner可省略，join默认内连接
        - 只保留两边都没有null的数据
    - 左连接——保留左边的所有值，剔除左表所有的null值
    - 右连接——保留右边的所有值，剔除右表所有的null值
    - 可以将多张表连接起来，只需要多家join on 的语法，join几个就可以添加几张表

    select university, difficult_level, round((count(qpd.question_id)/count(distinct(qpd.device_id))),4)
    from user_profile u right 
    join question_practice_detail qpd
    on u.device_id = qpd.device_id
    join question_detail qd 
    on qpd.question_id = qd.question_id
    group by university, difficult_level 





# 子查询

相当于查询的嵌套

    #查询世界上比欧洲gdp最高的国家gdp还高的国家有哪些
    select name
    from world
    where gdp is not null
    and gdp>(
      select max(gdp) from world
      where continent='Europe'
    )
    



# 表的嵌套

    SELECT party, constituency
      FROM 
    (select firstName,lastName,constituency,party,
    rank()over(partition by constituency order by votes desc) as rc from ge
    where yr=2017
    and constituency in ('S14000021','S14000022','S14000023','S14000024','S14000025','S14000026')
    ) as format #子查询生成的表必须要取一个别名！！！！！
    where format.rc=1



# 联合查询 union

- 将两个select语句的结果作为整体显示出来，并且会按照select的顺序进行排序（会去冲）
- union all 与union几乎相同，但对结果不去重

    select device_id, gender, age, gpa
    from user_profile
    where university="山东大学"
    union all
    select device_id, gender, age, gpa
    from user_profile
    where gender="male"



# 





select name,continent,population

from world

where continent in

(

select continent

from (select name, continent, population, 

rank()over(partition by continent order by population desc) as rk

from world) as temp

where temp.rk=1 and population\<= 25000000

)



select name, date*format(whn,'%Y-%m-%d') date, *

newcure,

rank()over(partition by name order by newcure desc) rk

from

(

SELECT name, date*format(whn,'%Y-%m-%d') date, *

lag(recovered,1) over(partition by name order by whn) recovered, 

(recovered-lag(recovered,1) over(partition by name order by whn)) newcure

from covid

where name in ('Italy','Germany')

) as temp

order by rk

