# MySQL基础
网易云课堂 :[视频地址](https://study.163.com/course/courseLearn.htm?courseId=1005932016#/learn/video?lessonId=1053337036&courseId=1005932016)

## 一、存储引擎
### 1、概述
不同的存储引擎性能是不一样的。MySQL中的数据用各种不同的技术存储在文件或内存中。
这些技术使用不同的存储机制、索引技巧、锁定水平并提供不同的功能和能力，从而改善应用的整体水平。
### 2分类
#### 2.1、MYISAM
* 不支持事务，外键。
* 访问速度块
* 适用于对事务完整性没有要求或者以SELECT、INSERT为主的应用
* 每个MYISAM在磁盘上存储成3个文件，拓展名为.fm（存储表定义）MYD(存储数据) MYI(存储索引)
#### 2.2、INNODB
* 事务安全(提交、回滚、崩溃恢复能力)
* 对比MYISAM，写的处理效率要差一些并且占用磁盘存储空间大以用来保留索引和数据。
#### 2.3、MEMORY
* 使用存在内存中的内容来创建表。
* 每个MEMORY表实际对应一个磁盘文件。格式是.fm
* 表的访问非常快，因为它的数据是放在内存中的，并且默认使用HASH索引，但是一旦服务器关闭，表中的数据就会丢失，表还会继续存在。
---

## 二、SQL
### 1、分类
* DDL：数据定义原：用来定义数据库对象：创建库、表、列等
* DML：数据操作语言：用来操作数据库表中的数据
* DQL：数据查询语言：用来查询数据
* DCL：数据控制语言：用来定义访问权限和安全级别
### 2、数据类型
#### 2.1、数值型
类型        | 大小                        | 范围（有符号）      | 	范围（无符号）| 用途
---:|:---:|:---:|:---:|:---
TINYINT    | 1字节                       | -128~127         | 0-255         | 小整数值
SMALLINT   | 2字节                       | -32768~32767     | 0-65535       | 大整数类值
MEDIUMINT  | 3字节                       | -8388608~8388607 | 无符号0-1600万  | 大整数类值
INT/INTEGER| 4字节                       | -21亿~21亿        |0-41亿         | 大整数类值
BIGINT     | 8字节                       | -                | -             | 极大整数类值
FLOAT      | 4字节                       | -                | -             | 单精度 浮点数值
DOUBLE     | 8字节                       | -                | -             | 双精度 浮点数值 
DECIMAL    | M>D,M+2/M<D,D+2            | -                | -             | 小数值
* 备注： 
  * DOUBLE（5,2）表示最多5位数，2位是小数 即 最大值(999.99)
#### 2.2、字符串
类型 | 大小 | 用途
---:|:---:|:---
​CHAR         | 0~255字节                                  | 定长字符串
​VARCHAR      | 0~255字节                                  | 变长字符串
TINYBLOB     | 0~255字节                                  | 不超过 255 个字符的二进制字符串
TINYTEXT     | 0~255字节                                  | 短文本字符串
BLOB         | 0-65535字节                                | 二进制形式的长文本数据
​TEXT         | 0-65535字节                                | 长文本数据
​MEDIUMBLOB   | 0-16777215字节                             | 二进制形式的中等长度文本数据
​MEDIUMTEXT   | 0-16777215字节                             | 中等长度文本数据
​LOGNGBLOB    | 0-4294967295字节                           | 二进制形式的极大文本数据
​LONGTEXT     | 0-4294967295字节                           | 极大文本数据
​VARBINARY(M) | 允许长度0-M个字节的定长字节符串，值的长度+1个字节 | 定长字符串
​BINARY(M)    | M允许长度0-M个字节的定长字节符串               | 定长字符串
#### 2.3、日期和时间类型
类型 | 大小 | 格式 | 用途 |范围
---:|:---:|:---:|:---:|:---
DATE      | 3字节 | YYYY-MM-DD          | 日期值                | 1000-01-01~9999-12-31
TIME      | 3字节 | HH:MM:SS            | 时间或者持续时间        | -
YEAR      | 1字节 | YYYY                | 年份值                | 1901-2155
DATETIME  | 8字节 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值        | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59
TIMESTAMP | 4字节 | YYYYMMDDHHMMSS      | 混合日期和时间值，时间戳 | 1970-01-01 00:00:00 ~ 北京时间 2038-01-19 11:14:07/格林尼治时间：2038-01-19 03:14:07
### 3、DDL
````
# 创建表
CREATE TABLE 表名(列名 列的类型[约束],列名2 列的类型[约束])
demo：
CREATE TABLE student_test(
id INT(4) NOT NULL auto_increment,
age VARCHAR(10),
PRIMARY KEY (id)
)ENGINE = InnoDB CHARACTER SET = utf8;
# 添加一列 
ALTER TABLE 表名 ADD 列名 列的类型[约束]；
# 修改一个表字段类型 
ALTER TABLE 表名 MODIFY 字段名 数据类型
# 修改表的字符集为GBK 
ALTER TABLE 表名 CHARACTER SET 字符集名称
# 修改表的列名 
ALTER TABLE 表名　CHANGE　原始列名　新列名　数据类型
# 删除表的其中一列 
ALTER TABLE 表名 DROP 字段名
# 查看表字段信息 
DESC 表名
# 查看表的创建细节 
SHOW CREATE TABLE 表名
# 修改表名 
RENAME TABLE 原始表名 TO 新表名
# 删除表 
DROP TABLE 表名
````
### 4、DML
````
# 插入操作 
INSERT INTO 表名 (列1,列2,列3) VALUE(值1,值2,值3)
# 批量插入 
INSERT INTO 表名 (列1,列2,列3) VALUES(值1,值2,值3),(值1,值2,值3)
# 修改 
UPDATE 表名 SET 字段名 = 字段值+1，字段2 = 修改后的值 WHERE 条件
# 删除 
DELETE FROM 表名 [WHERE 列名=值]
TRUNCATE TABLE 表名 （不能加WHERE条件，删除表后重建表，从0开始）
````
### 5、条件查询
* In、Between和Exists子句,NOT IN 结果集中不能有NULL，否则查询结果为NULL
````
# = 等于  
# != 不等于 
# <>大于小于 
# <= >= 小于等于 大于等于
#  值 BETWEEN..值1..AND. 值2.;值在什么范围
# IN 
# IS NULL  IS NOT NULL
# AND 与
# OR  或
# NOT 非

# 查询性别非男的学生记录
SELECT * FROM SQL50Question.Student s WHERE s.Ssex != '男'
# 错误写法
SELECT * FROM SQL50Question.Student s WHERE s.Ssex NOT '男'
````
### 6、模糊查询
````
# _任意一个字母
# %任意0-n个字母

# 查询由5个字母组成的人
SELECT * FROM Student s WHERE s.Sname LIKE '_____';
````
### 7、去     重和排序
````
# 去重 DISTINCT
# 别名 AS
# ORDER BY 字段1 规则，字段2 规则；先按字段1排序，如果相同按字段2排序 
# 升序ASC 降序 DESC，可以使用聚合函数
````
### 8、聚合函数
````
# COUNT(*) 
# COUNT(字段) 不包含NULL值
# SUM() 计算指定列的数值和，如果指定列类型不是数值类型，那么计算结果为0
# AVG() 计算指定列的平均值，如果指定列类型不是数值类型，那么计算结果为0
# MAX() 计算指定列的最大值，如果指定列类型是字符串类型，那么使用字符串排序运算
# MIN() 计算指定列的最小值，如果指定列类型是字符串类型，那么使用字符串排序运算
````
### 9、分组查询
* 将查询的记过按照一个或多个字段进行分组，字段值相同的为一组
````
# GROUP BY 后面的字段 一般要在SELECT 后出现
# 与聚合函数连用 聚合函数是对分组以后的数据进行计算

# GROUP BY + GROUP_CONCAT()
SELECT *,GROUP_CONCAT(s.Sname) FROM SQL50Question.Student s GROUP BY s.Ssex

# GROUP BY 和 DISTINCT 区别
# 一个是将数据分组数据还在，一个是将数据去掉数据丢失
````    
### 10、查询语句书写顺序
````
# 书写顺序
SELECT->FROM- >WHERE->GROUP BY->HAVING->ORDER BY ->LIMIT
# 执行顺序
FROM ->WHERE->GROUP BY ->HAVING ->SELECT ->ORDER BY ->LIMIT
````        
### 11、分页查询
````
# int x = 1 -- 当前页
# int y = 3 --每页多少条数据
# 公式：LIMIT (x-1)*y，y
````
### 12、合并结果集
````
# UNION、UNION_ALL
# UNION:合并时去掉重复数据
# UNION_ALL：合并时不去掉重复数据
# 注意: 被合并的两个结果:列数,类型必须相同

# 格式
SELECT * FROM 表1 UNION SELECT * FROM 表2
SELECT * FROM 表2 UNION ALL SELECT * FROM 表2 

# demo 
CREATE TABLE A (
name VARCHAR(10),
score INT
)
CREATE TABLE B(
name VARCHAR(10),
score INT
)

INSERT INTO SQL50Question.A VALUES('a',10),('b',20),('c',30);
INSERT INTO SQL50Question.B VALUES('a',10),('b',20),('d',40);
INSERT INTO SQL50Question.B VALUE('c',30)

SELECT * FROM SQL50Question.A UNION ALL SELECT * FROM SQL50Question.B
SELECT * FROM SQL50Question.A UNION  SELECT * FROM SQL50Question.B

结果:
a	10
b	20
c	30
a	10
b	20
d	40
c	30
--------------------------------------
a	10
b	20
c	30
d	40
````
---

## 三、数据完整性介绍
### 3.1、数据完整性
* 概念
    * 保证用户输入到数据库的数据是正确的
* 如何保证数据完整性
    * 添加约束(约束的是插入修改删除,不约束查询)
* 分类
    * 实体完整性、域完整性、引用完整性（参照完整性）
### 3.2、实体完整性
* 实体 表中的一行记录叫实体 
* 行级约束：标识每一行数据不重复
* 约束类型：主键约束(PRIMARY KEY )、唯一约束(UNIQUE)、自动增长列(AUTO_INCREMENT)
* 主键约束(PRIMARY KEY )：每个表都要有一个主键，数据唯一，不能为NULL，可以是多个字段作为主键（即联合主键）
* 唯一约束(UNIQUE)：数据唯一，可以为NULL
### 3.3、域完整性
* 域 代表当前单元格
* 数据类型、非空约束(NOT NULL)、默认值约束(DEFAULT)
### 3.4 参照完整性
* 是指表与表之间的一种对应关系。
* 通常情况下通过设置量表之间的主键、外键关系，或者编写两表的触发器来实现。
* 有对应参照完整性的两张表，在对他们进行数据插入、更新、删除的过程中，系统都会将被修改的表格与另一张对应表格进行对照，从而阻止一些不正确的数据的操作。
* 特点
    * 数据库主键和外键类型要一致
    * 两个表必须都是INNODB引擎
    * 设置参照完整性后，外键当中的内值，必须得是主键当中的内容、一个表设置当中的字段为主键，设置主键的表为主表
    * 创建表时，设置外键，设置外键的为子表。
* 创建外键
````
# 格式
CONSTRAINT 外键名 FOREIGN KEY (设置哪个字段为外键) REFERENCES 主表名(哪个主键)

ALTER TABLE 表名 ADD CONSTAINT 外键名 FOREIGN KEY （设置哪个字段为外键）REFERENCES 主表名(哪个主键)

# 或 创建表的时候添加 外键依赖
# 货 使用可视化工具
# 先删外表，再删主表
````
### 3.5、表之间的关系
* 表之间关系：一对一、一对多关系、多对多关系。创建关系表，给关系表的两个字段添加外键
* 拆分表是为了避免大量冗余数据的出现
--- 

## 4、笛卡尔集现象
````
# 一条SQL查询两个表会出现笛卡尔集现象
# 假设 集合A={a,b}集合B={0,1,2},则两个集合的笛卡尔积为(a,0)(a,1)(a,2)(b,0)(b,1)(b,2)

SELECT * FROM A,B 会出现笛卡尔集现象
SELECT * FROM A a ;
SELECT * FROM B b;

SELECT b.name AS bn,b.score AS bs,a.name AS an,a.score AS ass FROM A a,B b;

结果:
a	10
b	20
c	30
----------------------
a	10
b	20
c	30
d	40
-----------------------
a	10	a	10
a	10	b	20
a	10	c	30
b	20	a	10
b	20	b	20
b	20	c	30
c	30	a	10
c	30	b	20
c	30	c	30
d	40	a	10
d	40	b	20
d	40	c	30

# 去笛卡尔集
# 主表种的数据参照子表中的数据 或 主键和外键保持一致
# 原理:逐行判断.相等的留下,不相等的全不要

SELECT b.name AS bn,b.score AS bs,a.name AS an,a.score AS ass FROM A a,B b WHEREa.name = b.name
````
---

## 5、连接查询
* 根据连接方式分为:内连接/外连接/自然连接
### 5.1、内连接:
````
# 等值连接（INNER JOIN ）
# 非等值连接 ON后面的不再是等于号
# 自连接

# on 只写主外键的字段
# 与多表联查约束主键外键一样,只是写法改变 

SELECT * FROM 表名 别名 INNER JOIN 表2 别名 on 表1.字段 = 表2.字段
SELECT * FROM 表名 别名,表2 别名 WHERE 表1.字段 = 表2.字段  --99查询法

# 多表联查（三个表以上）
SELECT * FROM 表1 别名,表2 别名,表3 别名 WHERE 表1.字段 =表2.字段 AND 表2.字段 = 表3.字段 --99查询法
SELECT * FROM 表1 别名 INNER JOIN 表2 别名 ON 表1.字段 = 表2.字段 INNER JOIN 表3 别名 ON 表2.字段 = 表3.字段

# 自连接
# 情形：表中是两条数据，需要合并成一条数据
# 同一张表，自己连接自己 起别名
SELECT * FROM 表1 别名1，表1 别名2 WHERE 表1.字段 1 = 表2.字段1 AND 表1.字段1 = 值
```` 
### 5.2、外连接
````
# 左连接:两表满足条件相同的数据查出来,并且左表中有不同数据,将左表中的数据也查询出来
SELECT * FROM 表名 别名 LEFT OUTER JOIN 表2 别名 on 表1.字段 = 表2.字段 WHERE....
# 右连接:两表满足条件相同的数据查出来,并且右表中有不同数据,将右表中的数据也查询出来
SELECT * FROM 表名 别名 RIGHT OUTER JOIN 表2 别名 on 表1.字段 = 表2.字段 WHERE....
# 第二个表没数据的为空值
SELECT b.name AS bn,b.score AS bs,a.name AS an,a.score AS ass FROM A a LEFT OUTER JOIN B b on a.name = b.name
结果:
a	10	a	10
b	20	b	20
c	30	c	30
		e	50
````
### 5.3、自然连接（NATURAL JOIN）
````
# 要求
# 两张表中的列名和类型完全一致的列作为条件，会去除相同的列，
# 如果有两个相同列，则会把第二个列也作为查询条件
SELECT * FROM 表1 别名 NATURAL JOIN 表2 别名
```` 
--- 

## 6、子查询：
````
# 什么是子查询：
# 一个SELECT语句中包含另一个完整的SELECT语句，或者两个以上SELECT，那么就是子查询语句了
# 子查询出现位置
# WHERE后：把SELECT查询出的结果当作另一个SELECT的条件值 即：SELECT * FROM 表 WHERE 字段 = ( SELECT...查询)
# FROM后，把查询出的结果当作一个新表 即：SELECT * FROM  ( SELECT...查询)
````
---

## 7、常用函数简介
* 可以在SELECT语句中，也可以用UPDATE、DELETE语句中
* 分类：字符串函数、数值函数、日期和时间函数、流程函数、其它函数
### 7.1、字符串函数
````
# CONCAT(s1,s2...sn) 将传入的字符连接成一个字符串 任何字符串和null进行连接的结果都是NULL
SELECT CONCAT('aaa','bbb','ccc')

# insert(str,x,y,instr) 将字符串str从x位置开始，y个字符长度的子串替换成为指定的字符(instr) 从1开始
SELECT INSERT('bbbbb',2,3,'***')
# 如果y = instr的长度 (***),则正常替换 b***b
# 如果y < instr的长度(**),则剩余用空代替->b**b
# 如果y > instr的长度(****),则按instr的长度 ->b****b

# LOWER(Str)/UPPER(str )转换大小写

# LEFT(str,x)/RIGHT(str,x) 分别返回字符串最左边的x个字符和最右边的x个字符，如果第二个参数为null，那么不返回任何字符

# LPAD(str,n,pad)/RPAD(str,n,pad)用字符串pad对str最左边或最右边进行填充，直到长度为n个字符长度 从1开始
SELECT LPAD('my',4','1234')  ->12my

# LTRIM(str)/LTRIM(str)去掉字符串当中最左侧和最右侧的空格

# TRIM(str) 去掉两头的空格

# REPEAT(str,x) 返回str重复x次的结果

# REPLACE(str,a,b) 用字符串b替换字符串str中所有出现的字符串a

# SUBSTRING(str,x,y) 截取
````
### 7.2、数值函数
````
# ABS(x)绝对值
# CEIL(x) 向上取整 1.1 ->2
# FLOOR(x) 向下取整 1.9 ->1
# ROUND(x) 四舍五入 1.1->1
# MOD(x,y)返回x/y的模 取余数
# RAND()随机0-1的值
````
### 7.3、日期时间函数
````
# CURDATE() 只有日期
SELECT CURDATE()
结果：2020-10-13

# CURTIME()
SELECT CURTIME()
结果：20:40:17

# NOW()
SELECT NOW()
结果：2020-10-13 20:41:08

# UNIX_TIMESTAMP 转换成UNIX时间戳 
# FROM_UNIXTIME(unixtime) 将UNIX时间戳转换成为Date类型
# WEEK(DATE) 一年中的第几周
SELECT WEEK(NOW())
结果:41

# YEAR(DATE)
# HOUR(TIME) 几点
# MINUTE(TIME) 几分
# DATE_FORMAT(date,fmt) 按字符串fmt格式化date
# DATE_ADD(date,interval expr type ) 向前推,,天  可以是周 月 年 天  写- 是向前推
# DATE_SUB(date,INTERVAL expr type)函数从日期减去指定的时间间隔。
# LAST_DAY(date) 月份最后一天
SELECT DATE_ADD(NOW(),interval 31 DAY)

# DATEDIFF(date1,date2) 两日期相差天数
````
### 7.4、流程函数和其它函数
````
# IF(value,t,f) 如果value是真，返回t否则返回false
IF(2>3,'true','false')

# IFNULL(value1,value2)如果value1不为空，返回value1否则返回value2
CASE WHEN THEN END 
CASE WHEN 2>3 THEN ‘对’ELSE‘错’ END
CASE WHEN 2 THEN '2',WHEN 3 THEN '3'END


# FOMRAT(N,D,locale);
# N是要格式化的数字。
# D是要舍入的小数位数。
# locale是一个可选参数，用于确定千个分隔符和分隔符之间的分组。如果省略locale操作符，MySQL将默认使用en_US 还有de_DE语言

# 其它函数
# DATEBASE() 返回当前数据库名称
# VERSION() 返回当前数据版本
# USER() 返回当前用户
# PASSWORD(STR) 对STR进行加密
# MD5() 返回STR的MD5值

# CAST函数语法规则是：Cast(字段名 as 转换的类型 )，其中类型可以为：
# CHAR[(N)] 字符型
# DATE 日期型
# DATETIME 日期和时间型
# DECIMAL float型
# SIGNED int
# TIME 时间型
````
---

## 8、事务
* 不可分割的操作，每条SQL都是一个事务，事务只对DML语句有效，对DQL无效
### 8.1、事务的ACID
* A:原子性(Atomicy)：事务包含的所有操作要么都成功，要么都失败
* C:一致性(Consistency)：一个事务执行前和执行后的状态要一致 例如：商品出库时，仓库商品数量-1yong'hu购物车商品数量+1
* I:隔离性(Isolation)：事务在操作过程中无法进行其他操作。
* D:持久性(Durability)：事务在操作完成后入库无法在对该数据修改。
### 8.2、事务的提交
* 一个事务开始 `START TRANSACTION` 到结束 `COMMIT` 或者 `ROLLBACK` 以后需要重新开始
````
CREATE TABLE zs_account(
name VARCHAR(30),
money DECIMAL
);
-- 
CREATE TABLE li_account(
name VARCHAR(30),
money DECIMAL
);

START TRANSACTION;
UPDATE show_data.zs_account za SET za.money = za.money-2000;
UPDATE show_data.li_account la SET la.money = la.money+2000;
COMMIT;

# 要开一个新窗口实验 否则数据为更新过的数据
````
* ROLLBACK 当遇到突发情况撤销语句
### 8.3、事务的并发操作
* 事务并发出现的问题：脏读、不可重复读、幻读
* 隔离级别
    * READ UNCOMMITTED 读未提交一个事务可以读取另一个事务未提交的数据
    * READ COMMIT 读已提交 一个事务要等另一个事务提交以后才能读取数据
    * REPEATABLE READ 可重复读 （默认）
    * SERIALIZABLE 串行化 
* 脏读
    * 查询到事务提交之前的数据
    * 解决：隔离级别设置成 `READ COMMITTED` 读提交，能解决脏读问题
* 不可重复读 
    * 一个事务范围内两个相同的查询却返回了不同的数据
    * 解决：隔离级别设置成 `REPEATABLE READ`
* 幻读
    * 在一个事务范围内，显示了不同的数据
    * 解决：隔离级别设置成 `SERIALIZABLE` , 但是数据库性能会降低，一般不适用。
````
# 查看事务级别
SELECT @@global.tx_isolation, @@tx_isolation;
# 设置事务的隔离级别
# 全局 (关机后消失)
SET global transaction isolation level read commited；
````     
---

## 9、权限管理
### 9.1、什么是权限
限制一个用户可以做什么，MySQL中可以设置全局权限、指定数据库权限、指定表权限、指定字段权限
### 9.2、有哪些权限
````
# create  创建数据库、表或者索引的权限
# drop 删除数据库的权限
# alter 更改表、字段、索引的权限
# delete 删除数据的权限
# index 索引的权限
# insert 插入权限
# select 查询的权限
# update 更新的权限
# create view 创建视图的权限
# execute 执行存储过程的权限
````
### 9.3、操作
````
# 创建用户
create user `用户名`@`localhost` identified by '密码'
create user `用户名`@`ip地址` identified by '密码'
# 删除用户
drop user `用户名`@`ip地址`

# 分配权限 格式
GRANT 权限(columns) ON '数据库对象' TO 用户 IDENTIFIED BY '密码' WITH GRANT OPTION;

# 创建一个超级管理员mylk,密码为1234，拥有所有权限，并能继续授予权限
# *.* 所有数据库中的所有表
# with GRANT option 继续给其他用户授权
GRANT ALL privileges on *.* TO mylk@localhost identified by '1234556' with GRANT OPTION;
flush privileges;

# 创建对指定数据库的权限
GRANT ALL privileges on 数据库名.* TO mylk@localhost identified by '1234556';
flush privileges;

# 创建一个gxq用户只能对stu表进行crud操作
GRANT insert,select,update,delete on 数据库名.stu TO mylk@localhost identified by '1234556';
flush privileges;

# 查看权限
show grant
# 查看指定用户权限
show grant for 用户名@地址

# 删除权限 
REVOKE 权限 ON 数据库对象 FROM 用户
````
----

## 10、视图
### 10.1、什么是视图
虚拟表，其内容由定义查询
### 10.2、特点
* 不存储具体数据，基本表数据发生改变，视图也会随着改变
* 可进行增删改查操作
### 10.3、作用
* 安全性 将用户和视图进行权限绑定，
* 查询性能提高
* 提高了数据独立性
### 10.4 创建视图参数
* 参数： ALGORITHM / WITH CHECK OPTION / LOCAL和CASCADED
* ALGORITHM（算法）
    * merge：处理方式 替换式，可以进行更新真实表中的数据
    * temptable：具化式，数据放在临时表中，不可以进行更新操作
    * UNDEFINED：没有定义ALGORITHM参数，mysql 更倾向于选择替换式。
* WITH CHECK OPTION 
    * 更新数据时不能插入或更新不符合视图限制条件的记录
* LOCAL和CASCADED
    * 可选参数，决定了检查测试的范围，默认值为CASCADED
* 机制：替换式/具化式
    * 替代式与具化式区别
        * 替代式：操作试图时，视图名直接被视图定义给替换掉
        * SELECT * FROM (SELECT * FROM emp) t
        * 具化式 放入内存后操作视图
        * (SELECT * FROM emp) t 
        * select * FROM t 
### 10.5、操作
````
# 创建视图 格式
CREATE [ALGORITHM] = {UNDEFINED|MERGE|TEMPTABLE} VIEW 视图名（ *_view） AS (结果集) [WITH [CASCADED|LOCAL] CHECK OPTION];

# 修改视图内容
CREATE OR REPLACE VIEW 视图名 AS (结果集);

# 删除视图
DROP VIEW 视图名称;
````
### 10.6、视图不可更新部分
````
# 聚合函数
# DISTINCT关键字
# GROUP BY子句
# HAVING子句
# UNION运算符
# FROM子句中包含多个表
# SELECT语句中引用了不可更新视图，是要视图当中的数据不是来自于基表，就不能直接修改
````
---

## 11、存储过程
### 11.1、什么是存储过程
* 一组由名字的可编程的函数，是为了完成特定功能的SQL语句集，保存在数据的数据字典中。
* 将重复性很高的操作，封装并简化SQL调用，批量处理，统一接口，确保数据安全。
### 11.2、创建和调用
````
# 分隔符 
# 将标准的分隔符;改为$$ 
DELIMITER $$ 

# 创建
DELIMITER $$ 
CREATE PROCEDUER show_emp()
BEGIN 
SELECT * FROM EMP;
END $$ 
DELIMITER ;

# 调用
CALL show_emp();

# 查看存储过程
SHOW PROCEDUER STATUS;
# 查看指定数据库中的存储过程
SHOW PROCEDUER STATUS WHERE db = '数据库名'
# 查看指定存储过程源代码
SHOW CREATE PROCEDUER 存储过程名称

# 删除存储过程
DROP PROCEDUER 存储过程名称

# 声明变量
DECLARE 宣称
DECLARE 名称(可以多个eg name1,name2) 类型 DEFAULT 默认值

# 修改变量
SET 变量名 = 值 
# 使用SELECT INTO语句将查询的结果分配给一个变量
INTO 变量名
  
demo:
DELIMITER   $$
CREATE PROCEDUER test() 
BEGIN

DECLARE res varchar(255) default '';
DECLARE x,y int defalut '0';
＃ 声明变量并赋值
DECLARE avg DOUBLE default 0;
# 将查询的结果赋值给了变量 avgRes 
DECLARE avg(salary) into avgRes from emp;

END $$
DILIMITER ;
````
### 11.3、存储过程参数
* 类型：`IN` `out` `INOUT`
````
demo @in 
# 根据传入的名称，获取对应的信息
DELIMITER $$
CREATE PROCEDUER getName(in name varchar(255))
BEGIN
# 根据传入的名称查询
SELECT * FROM EMP where ename = name;
END $$
DELIMITER ;

CALL getName('李白');
---

demo @out
# 入参名称，返回结果集
DELIMITER $$
CREATE PROCEDUER getSarlary(in n varchar(255), out sarlary int)
BEGIN
SELECT `sarlary` into sarlary FROM `emp` WHERE ename = n;
END $$
DELIMITER ;

CALL getSarlary('鲁班', @sy);
# 查看返回结果 
# DUAL 虚拟表, 可以省略
SELECT @sy from DUAL;
--- 

demo @inout 
DELIMITER $$;
CREATE PROCEDURE test(inout num int, in inc int)
BEGIN
SET num = num + inc;
END $$
DELIMITER ;

# 定义变量 @ 代表地址的传递
set @num1 = 20;
CALL test(@num1, 10);
SELECT @num1;
````
### 11.4、存储过程语句
`IF语句` `CASE语句` `循环语句`
`````
# IF语句 格式
# statements 声明；陈述 
IF expression 
THEN statements;
ELSE else-statements;
END IF;

# CASE语句 格式
# command 命令
CASE case_expression
WHEN when_expression_1 THEN commands
WHEN when_expression_2 THEN commands
..
ELSE commands
END CASE;

# 循环语句 格式

# WHILE 循环
WHILE expression 
DO statements 
ENd WHILE;

# REPEAT 循环
REPEAT 
statement
UNTIL expression
END REPEAT;
`````

## 11.5、创建千万条数据
````
# 创建一张表
CREATE TABLE emp (id int, name varchar(50), age int);

# 随机生成一个指定个数的字符串
DELIMITER $$
CREATE FUNCTION rand_str(n int) returns varchar(255)
BEGIN
-- 声明一个str 52字母
DECLARE str  varchar(100) default 'abcd....z';
-- 记录当前是第几个
DECLARE i int default 0;
-- 生成结果
DECLARE res_str varchar(255) DEFALUT '';
while i < n 
DO 
SET res_str = CONCAT(res_str, SUBSTR(str, FLOOR(1 + RAND() * 52), 1));
SET i = i + 1;
END WHILE;
-- 返回字符串
return res_str;
END$$
DELIMITER ;

# 测试结果
SELECT rand_str(5);

## 存储过程 
DELIMITER $$
CREATE PROCEDURE insert_emp(in startNum int, in max_num int)
BEGIN 
-- 记录当前是第几条数据
DECLARE i int default 0;
-- 默认情况下自动提交SQL 
SET autocommit = 0;

REPEAT
SET i = i + 1;
-- 插入数据
INSERT INTO EMP VALUE(startNum + i, rand_str(5), FLOOR(10 + RAND()*30));
UNTIL i = max_num;
END REPEAT;
END $$
-- 整体提交所有SQL

commit;
DELIMITER ;

CALL insert_emp(0, 99999999)
````

---

## 12、自定义函数
`CREATE FUNCTION rand_str(n int) returns varchar(255)`
````
# 随机生成一个指定个数的字符串
DELIMITER $$
CREATE FUNCTION rand_str(n int) returns varchar(255)
BEGIN
-- 声明一个str 52字母
DECLARE str  varchar(100) default 'abcd....z';
-- 记录当前是第几个
DECLARE i int default 0;
-- 生成结果
DECLARE res_str varchar(255) DEFALUT '';

while i < n 
DO 
SET res_str = CONCAT(res_str, SUBSTR(str, FLOOR(1 + RAND() * 52), 1));
SET i = i + 1;
END WHILE;
-- 返回字符串
return res_str;
END$$
DELIMITER ;

SELECT rand_str(5);
````
---