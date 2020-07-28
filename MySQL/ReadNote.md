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

```

