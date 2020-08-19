# chap1 了解Mysql
## 1. 主键
其值能够唯一区分表中每个行

# chap3 使用MySQL
## 1. SHOW
```MYSQL
SHOW databases; /*返回可用数据库的列表*/
USE  ***db;       /*使用某个数据*/
SHOW tables;    /*返回当前选择的数据库里所有可用的表*/
SHOW COLUMNS FROM ***tables; /* 显示表中的所有列 */
```
## 2.SELECT
```MYSQL
SELECT Xcolumn FROM Xtable;  /* 从Xtable中检索Xcolumn */
SELECT Xcolumn, Ycolumn, Zcolumn FROM Xtable; /* 从Xtable中检索X, Y, Z column */
SELECT * FROM Xtable; /* 从Xtable中检索所有的列 */
SELECT DISTINCT Xcolumn FROM Xtable; /* 返回唯一不同的Xcolumn行，不会重复显示 */
SELECT xcolumn FROM Xtable LIMIT 5; /* 最多只返回5行 */
SELECT xcolumn FROM Xtable LIMIT 5, 5; /* 返回从第5行开始的5行， param1: 开始的位置， param2:要检索的行数 */
SELECT Xtable.Xcolumn FROM Xtable; /* 完全限定的表名 */
```

## 3.ORDER BY
```MYSQL
SELECT Xcolumn FROM Xtables ORDER BY Xcolumn; /* 按照Xcolumn顺序排序选取的Xcolumn列 */
SELECT Xcolumn, Ycolumn, Zcolumn FROM Xtables ORDER BY Xcolumn, Ycolumn; /* 按照Xcolumn, Ycolumn 排序选中的所有column */
SELECT Xcolumn FROM Xtables ORDER BY Xcolumn DESC; /* 按照Xcolumn降序排序选取的Xcolumn列， DESC只应用到用在其前面的列名。默认是升序排序 */
```

## 4. WHERE
```MYSQL
SELECT Xcolumn FROM Xtable WHERE Xcolumn = XValue 
SELECT Xcolumn FROM Xtable WHERE Xcolumn BETWEEN XValue AND YValue   /* 选取值在X,Y之间的记录 */
SELECT Xcolumn FROM Xtable WHERE Ycolumn IS NULL;  /* 选取为空的记录 */

```
|  操作符   | 说明  |
|  ----  | ----  |
| =  | 等于 |
| <>  | 不等于 |
| ！=  | 不等于 |
| <  | 小于 |
| <=  | 小于等于 |
| >  | 大于 |
| >=  | 大于等于 |
| BETWEEN  | 在指定的两个值之间 |

## 5. AND OR IN NOT
```MYSQL
SELECT Xcolumn FROM Xtable WHERE Xcolumn = XValue  AND Ycolumn < YValue
SELECT Xcolumn FROM Xtable WHERE Xcolumn = XValue OR Xcolumn = XValue  /* 如果混合使用AND OR，AND的优先级更高一些 */
SELECT Xcolumn FROM Xtable WHERE Xcolumn IN (XValue, YValue) ORDER BY YColumn /* 选取在XValue和YValue的记录并且排序 */
SELECT Xcolumn FROM Xtable WHERE Xcolumn NOT IN (XValue, YValue) ORDER BY YColumn  /* 选取记录不在在XValue和YValue的记录并且排序 */
```
## 6 通配符
在搜索子句中使用通配符必须要使用LIKE操作符
### 6.1 `%`
`%`可以匹配0个或者多个字符
```MYSQL
SELECT Xcolumn FROM Xtable WHERE Xcolumn LIKE 'jet%' /* 搜索所有以jet开头的Xcolumn记录 */
SELECT Xcolumn FROM Xtable WHERE Xcolumn LIKE '%jet%' /* 搜索所有包含jet的Xcolumn记录 */
SELECT Xcolumn FROM Xtable WHERE Xcolumn LIKE 'j%et'
```
### 6.2 `_`
`_`通配符的使用方式和`%`一样，但是`_`只能匹配单个字符而不是多个字符

## 7. 正则表达式
```MYSQL
SELECT Xcolumn FROM Xtable WHERE Xcolumn REGEXP '1000' /* 与文本正文1000匹配的一个正则表达式 */
SELECT Xcolumn FROM Xtable WHERE Xcolumn REGEXP '.000' /* .是正则表达式中特殊字符，表示匹配任意一个字符。*/
SELECT Xcolumn FROM Xtable WHERE Xcolumn REGEXP '1000 | 2000' /* 与文本正文1000或者2000匹配的一个正则表达式 */
SELECT Xcolumn FROM Xtable WHERE Xcolumn REGEXP '[abc]' /* []内定义一组字符，表示包含其中的任意一个就可以，类似于or的汉语a|b|c */
SELECT Xcolumn FROM Xtable WHERE Xcolumn REGEXP '[0-9]' /* 匹配0~9内的所有数字，只要包含其中一个就可以 */
SELECT Xcolumn FROM Xtable WHERE Xcolumn REGEXP  '\\.'  /* 匹配含.的所有内容，对于特殊字符，需要加上\\进行转义 */
```
`REGEXP`之后的内容为正则表达式
 -- 匹配字符类
 -- 匹配多个实例
 -- 定位符

## 8.创建计算字段
### 8.1 拼接字段
`concat`拼接串，把多个串链接起来形成一个较长的串。
```MYSQL
SELECT Concat(Xcolumn, '(', Ycolumn, ')')  /* 最后输出Xcolumn(Ycolumn),但是新的列没有名字 */
SELECT Concat(Xcolumn, '(', Ycolumn, ')') AS Xtitle  /* 最后输出Xcolumn(Ycolumn),并且新的列的名字是Xtitle */
```

## 9.使用数据处理函数
### 9.1 用于处理字符串的文本函数（删除，填充，转换大小写）
```MYSQL
SELECT Xcolumn, Upper(Ycolumn) AS Xtitle FROM Xtable /* 选取Xcolumn， Ycolumn， 将Ycolumn转换成大写，并且重命名为Xtitle */
```
常用的文本处理函数：
|  函数   | 说明  |
|  ----  | ----  |
| Left() | 返回串左边的字符 |
| Length() | 返回串的长度 |
| Locate() | 找出串的一个子串 |
| Lower() | 将串转换为小写 |
| LTrim() | 去掉串左边的空格 |
| Right() | 返回串右边的字符 |
| RTrim() | 去掉串右边的空格 |
| Soundex() | 返回串的SOUNDEX的值 |
| SubString() | 返回子串的字符 |
| Upper() | 将串转换为大写 |

### 9.2 在数值数据上进行算术操作（返回绝对值，进行代数运算）
|  函数   | 说明  |
|  ----  | ----  |
|Abs()| 返回一个数的绝对值|
|Cos()| 返回一个角度的余弦|
|Exp()| 返回一个数的指数值|
|Mod()| 返回除操作的余数|
|Pi()| 返回圆周率|
|Rand()| 返回一个随机数|
|Sin()| 返回一个角度的正弦|
|Sqrt()| 返回一个数的平方根|
|Tan()| 返回一个角度的正切|

### 9.3 处理日期和时间值并从这些值中提取特定成分
|  函数   | 说明  |
|  ----  | ----  |
|AddDate()| 增加一个日期（天、周等）|
|AddTime()| 增加一个时间（时、分等）|
|CurDate() |返回当前日期|
|CurTime() |返回当前时间|
|Date() |返回日期时间的日期部分|
|DateDiff() |计算两个日期之差|
|Date_Add() |高度灵活的日期运算函数|
|Date_Format() |返回一个格式化的日期或时间串|
|Day() |返回一个日期的天数部分|
|DayOfWeek() |对于一个日期，返回对应的星期几|
|Hour() |返回一个时间的小时部分|
|Minute()| 返回一个时间的分钟部分|
|Month() |返回一个日期的月份部分|
|Now() |返回当前日期和时间|
|Second()| 返回一个时间的秒部分|
|Time() |返回一个日期时间的时间部分|
|Year() |返回一个日期的年份部分|

### 9.4 返回DBMS的相关信息的系统函数

## 10. 汇总函数
|  函数   | 说明  |
|  ----  | ----  |
|AVG()| 返回某列的平均值|
|COUNT()| 返回某列的行数|
|MAX()| 返回某列的最大值|
|MIN()| 返回某列的最小值|
|SUM()| 返回某列值之和|

- `AVG()`
通过对表中的行数计数并计算特定列的值
```MYSQL
SELECT AVG(Xcolumn) AS XTitle From XTables; /* 返回XTables中的XColumn的平均值，并且重命名为XTitle */
SELECT AVG(Xcolumn) AS XTitle From XTables WHERE Ycolumn = YValue;
SELECT COUNT(*) FROM XTables; /* 统计XTables中所有column数，不管表列中包含的，一般而言，count(*)不可以再仅仅只是搭配普通的column组放在select里使用，普通的column需要加上GROUP BY或者聚合函数才可以，也就是 SELECT XColumn, COUNT(*) FROM XTables; 这是不合法的语句*/
SELECT COUNT(Xcolumn) FROM XTables; /*统计XTables中所有记录中包含Xcolumn的数目，*/
SELECT MAX(Xcolumn)  AS Xtitle FROM XTables; /* 统计XTables中Xcolumn列中的最大值， MAX函数忽略值为NULL的行 */
SELECT AVG(DISTINCT Xcolumn) AS XTitle From XTables; /* 返回XTables中的XColumn不重复的值的平均值，并且重命名为XTitle */
SELECT COUNT(*) AS XTitle,
       MIN(XColumn) AS YTitle,
       MAX(XColumn) AS ZTitle,
       AVG(XColumn) AS TTitle
FROM XTables;   /*可以在一条语句中使用多个聚集函数*/
```

## 11. 分组数据
### 11.1 GROUP BY
```MYSQL
SELECT XColumn, COUNT(*) AS XTitle FROM XTable GROUP BY XColumn; /* 对每个XColumn值进行分组，计数，计数结果的列重命名为XTitle */
```
- GROUP BY子句可以包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。
- 如果在GROUP BY子句中嵌套了分组，数据将在最后规定的分组上进行汇总。换句话说，在建立分组时，指定的所有列都一起计算（所以不能从个别的列取回数据）。
- GROUP BY子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）。如果在SELECT中使用表达式，则必须在GROUP BY子句中指定相同的表达式。不能使用别名。
- 除聚集计算语句外，SELECT语句中的每个列都必须在GROUP BY子句中给出。
- 如果分组列中具有NULL值，则NULL将作为一个分组返回。如果列中有多行NULL值，它们将分为一组。
- GROUP BY子句必须出现在WHERE子句之后，ORDER BY子句之前。

### 11.2 HAVING
HAVING和WHERE的子句相似，但是WHERE过滤行，HAVING过滤分组
```MYSQL
SELECT XColumn, COUNT(*) AS XTitle FROM XTable GROUP BY XColumn HAVING COUNT(*) >= 2 /* 过滤COUNT(*) >= 2的那些分组 */
```
## 12. 联结表
关系数据库：各表通过某些列的关系相互关联。  
主键：其值能够唯一区分表中每个行  
外键：外键为某个表中的一列，它包含另一个表中的主键值，定义了两个表之间的关系  
### 12.1 联结
在数据库创建的时候没有可以指示MYSQL如何对各个表进行联结的东西，这种联结是程序员必须要清楚的内容。由没有联结关系的表关系返回的结果为笛卡尔积：第一个表中的行数乘以第二个表中的列数。  
- 等值也称为内部联结，就是通过两张表中的某两个列的数值相等进行联结。内部联结方式的SQL语句有两种：
```MYSQL
SELECT XColumn, YColumn, ZColumn FROM Xtable, Ytable WHERE Xtable.xcolumn = Ytable.xcolumn ORDER BY Xcolumn, Ycolumn; /* 可以使用AND联结多个=号限定条件 */
SELECT XColumn, YColumn, ZColumn FROM Xtable INNER JOIN Ytable ON Xtable.xcolumn = Ytable.xcolumn ORDER BY Xcolumn, Ycolumn; /* 作用同上一条语句，但是推荐使用这种 */
```
### 12.2 创建高级联结
可以使用`AS`为列创建别名，并返回到客户机，但是也可以使用`AS`为表创建别名，创建的别名不返回客户机，但是可以使用在后续的语句中，用于减少拼写的难度
```MYSQL
SELECT Xcolumn, Ycolumn FROM Xtable AS X, Ytable AS Y WHERE X.column1 = Y.column1
```
- 自染联结：
- 外部联结：
- 使用带凝聚函数的联结：
## 13. 组合查询
多数SQL查询只包含从一个或者多个表中返回数据的单条SELECT语句。MYSQL也允许执行多个查询（多条select语句），并将结果作为单个查询结果集返回。这些组合查询查询同行成为并或者复合查询。   
需要使用组合查询的两种情况
- 在单个查询中从不同的表返回类似结构的数据
- 对单个表执行多个查询，按单个查询返回数据
`UNION`使用规则：  
- UNION 必须由两条或者两条以上的SELECT语句组成，语句之间使用关键字UNION分隔；
- UNION 中的每个查询必须包含相同的列，表达式或者聚集函数
- 列数据类型必须兼容  
在使用UNION的时候，会自动消除重复的行。如果不想消除重复的行就可以选择使用UNION ALL

## 14. 全文本搜索


## 15. 插入数据
`Insert`
### 15.1 插入完整的行
Insert语句一般没有输出。
```MYSQL
INSERT INTO XTables VALUES(Value1, Value2, Value3...);
INSERT INTO XTables(Column1, Column2, Column3...)  VALUES(Value1, Value2, Value3...);//推荐使用这种方式，即使表结构发生变化，这条语句也可以正确的工作
```
### 15.2 插入行的一部分
可以使用上面的方式二，只要表允许某些行在插入数值时缺省并且给出默认值，就可以使用上述的语句插入行的一部分
### 15.3 插入多行
```MYSQL
INSERT INTO XTables(Column1, Column2, Column3...)  VALUES(Value1, Value2, Value3...),(Value21, Value22, Value23...)

```
### 15.4 插入某些查询的结果
```MYSQL
INSERT INTO XTables(Column1, Column2, Column3...)  SELECT (Column1, Column2, Column3...) FROM YTables

```
## 16. 更新和删除数据
### 16.1 更新`UPDATE`
基本的UPDATE语句由三个部分组成：1.要更新的表；2.列名和它们的新值；3.确定要更新的过滤条件；例如：
```MYSQL
UPDATE XTables SET XColumn = 'Xvalue', YColumn = 'YValue' WHERE ZColumn = 'ZValue'
```
### 16.2 删除`DELETE`
```MYSQL
DELETE FROM XTables WHERE XColumn = `XValue`
```
如果UPDATE或者DELETE后面没有WHERE子句，将会影响表中的所有行。

## 17. 创建和操纵表
`CREATE TABLE`之后必须给出：新表的名字；表列的名字和定义（用逗号分隔）
```MYSQL
CREATE TABLE XTables
(
Column1  int       NOT NULL AUTO_INCREMENT,
Column2  char(50)  NOT NULL,
Column3  char(50)  NULL,
Column4  int       NOT NULL DEFAULT 1,
PRIMARY KEY(Column1)
)ENGINE=InnoDB;

```
- NULL值就是没有值或者缺值。允许NULL值的列也允许在插入行的时候不给出该列的值。不允许NULL值的列在插入或者更新的时候，该列必须有值
- 如果需要创建多个列组成主键，应该以逗号分隔的列表给出各列名，`PRIMARY KEY(Column1, Column2)`.主键可以在创建表的时候定义，也可以在创建表之后定义
- 每个表只允许一个AUTO_INCREMENT列，而且它必须被索引
- 给改列增加一个DEFAULT修饰词，表示该列的默认值
- 如果省略最后选择引擎的语句表示使用默认的引擎

`ALTER TABLE`之后必须要给出要更改的表名；所做更改的列表
```MYSQL
ALTER TABLE XTable ADD Column1 CHAR(20);//增加一个列
ALTER TABLE XTable DROP COLUMN Column1;//删除刚刚增加的列
DROP TABLE XTable; //删除整张表
RENAME TABLE XTabel TO YTable; //重命名整张表

```
外键？？？

## 18. 使用视图
视图是虚拟的表，与包含数据的表不一样，视图只包含使用时动态检索数据的查询。  
视图的作用：
- 重用SQL语句
- 简化复杂的SQL操作，在编写查询之后，可以方便的重用它而不必知道它的基本查询细节
- 使用表的组成部分而不是整个表
- 保护数据。可以给用户授予表的特定部分的访问权限而不是整个表的访问权限
- 更改数据格式和表示。视图可返回与底层表的表示和格式不同的数据  
使用视图：
- 视图使用CREATE VIEW来创建
- 使用SHOW CREATE VIEW viewname来查看视图的语句
- 用DROP删除视图，其语法为DROP VIEW viewname
- 更新视图时，可以先用DROP 再使用CREATE
```MYSQL
CREATE VIEW XView AS SELECT column1, column2, column3 FROM table1, table2, table3 WHERE table1.column_x = table2.column_x AND table2.column_y = table3.column.y;//XView相当于后面这一大串SQL语句

```
## 19.使用存储过程
存储过程简单来说就是为以后使用而保存的一条或者多条MYSQL语句的集合。可将其视为批文件，不过它们的作用不仅限于批处理。MYSQL称存储过程的执行为调用。因此MYSQL执行存储过程的语句为CALL。创建存储过程使用CREATE PROCEDURE
```MYSQL
//创建存储过程
CERATE PROCEDURE productpricing()
BEGIN
 SELECT Avg(Columnx) AS name_x FROM Table_x;
END
//调用存储过程
CALL productpricing();
//删除存储过程
DROP PROCEDURE productpricing();
//带参数
//关键字：OUT: 从存储过程返回一个值给使用者
//        IN: 传入一个值给存储过程
//        INOUT
//通过INTO将值写入相关变量
CERATE PROCEDURE productpricing(OUT p1 DECIMAL(8, 2), OUT p2 DECIMAL(8, 2), OUT p3 DECIMAL(8, 2))
BEGIN
 SELCET MIN(Column1) INTO p1 FROM Table_1;
 SELECT Max(Column2) INTO p2 FROM Table_2;
 SELECT Avg(Columnx) INTO p3 FROM Table_x;
END
//带变量的调用,所有MYSQL的变量都必须以@开头
CALL productpricing(@p1, @p2, @p3);
```
DECLARE: 定义变量，指定变量名和数据类型

## 20. 使用游标






