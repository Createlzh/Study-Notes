# SQL教程

[TOC]

___

## CH1.数据库和SQL

### 1.1 数据库

* 数据库：Database

* 数据库管理系统：DBMS


##### 数据库基本类型

> 数据的保存形式

1. 层次数据库：`HDB`

   > 古老，现在很少使用

2. 关系数据库：`RDB`

   > 行和列组成的二维表，SQL（Structured Query Language）管理数据
   >
   > 对应的关系数据库管理系统RDBMS（Management System）

3. 面向对象数据库：`OODB`

   > 把数据以及对数据的操作集合起来，以对象为单位进行管理

4. XML数据库：`XMLDB`

   > 对XML形式的大量数据进行高速处理

5. 键值存储系统：`KVS`

   > 单纯用来保存查询所使用的`主键Key`和`值Value`的组合的数据库



### 1.2 数据库的结构

##### RDBMS的系统结构

* RDBMS最常见的系统结构是`客户端/服务器类型（C/S类型）`

```ascii
      -----SQL语句------>
客户端                    服务器RDBMS <----> 数据库
      <----请求的数据-----
```

* 通过网络可以实现多个客户端访问同一个数据库

```ascii
客户端1-----┐   ┌-----SQL语句------>
客户端2-----网络-                  服务器RDBMS <----> 数据库
客户端3-----┘   └<----请求的数据-----
```

##### 表的结构

* 一个数据库中可以存储多个表
* 根据SQL语句的内容返回的数据必须同样是二维表的形式
* 表的列称为`字段`，表中的行称为`记录`，一个单元格中只能输入一个数据
* 关系数据库必须以行为单位进行数据的读写



### 1.3 SQL概要

##### SQL语句及其种类

* 数据定义语言：DDL（Data Definition Language）

  > 创建或删除存储数据用的数据库以及数据库中的表等对象

  * `CREATE`
  * `DROP`
  * `ALTER`

* 数据操纵语言：DML（Data Manipulation Language）

  > 查询或者变更表中的记录

  * `SELECT`
  * `INSERT`
  * `UPDATE`
  * `DELETE`

* 数据控制语言：DCL（Data Control Language）

  > 确认或者取消对数据库中的数据进行的变更

  * `COMMIT`
  * `ROLLBACK`
  * `GRANT`
  * `REVOKE`

##### SQL的基本书写规则

* 以分号`;`结尾

* 不区分关键字大小写

  > 关键字大写
  >
  > 表名的首字母大写
  >
  > 其余（列名）小写

* 表中的具体数据是区分大小写的

* 常数的书写方式是固定的

  > 常数：字符串、日期、数字等
  >
  > 字符串使用单引号`'abc'`
  >
  > 日期格式有多种，同样需要用单引号`'2010-01-26'或'10/01/26'`等
  >
  > 数字不需使用任何标识，直接书写即可

* 单词需要用半角空格或者换行来分隔

##### 换行符

* SQL使用换行符或半角空格来分隔单词，在任何位置分割均可，原则上以子句为单位进行换行

  ```sql
  SELECT
  *
  FROM
  Product
  ;
  --无错但是看不清楚
  ```

* 插入空行（无任何字符的行）会造成执行错误

  ```sql
  SELECT *
  
   FROM Product;
  --会出错
  ```

##### 注释

* 单行注释`--`
* 多行注释`/*`开头，`*/`结尾



### 1.4 表的创建

##### 数据库的创建

```sql
CREATE DATABASE <数据库名称>；
CREATE DATABASE shop;
```

##### 表的创建

```sql
CREATE TABLE <表名>
(<列名1> <数据类型> <该列所需约束>,
 <列名2> <数据类型> <该列所需约束>,
 <列名3> <数据类型> <该列所需约束>,
 ...
 <列名n> <数据类型> <该列所需约束>,
 <该表的约束1>, <该表的约束2>, ...);
 
CREATE TABLE Product
 (product_id      CHAR(4)      NOT NULL,
  product_name    VARCHAR(4)   NOT NULL,
  product_type    VARCHAR(100) NOT NULL,
  sale_price      INTEGER      ,
  purchase_price  INTEGER      ,
  regist_date     DATE         ,
  PRIMARY KEY (product_id)
 );
```

##### 命名规则

* 以半角英文字母开头
* 半角数字
* 下划线
* 同一数据库中不能出现相同名称的两个表
* 同一个列中不能出现相同名称的两个列

##### 数据类型

* `INTEGER`：整数类型

  > `INT`：默认值0，非空
  >
  > `INTEGER`：默认值为NULL

* `CHAR`：定长字符串类型

  > `CHAR(10)`可指定存储的字符串的长度，超出的部分无法输入，不足的部分会以半角空格补足
  >
  > 如`abcdef`会变为`abcdef    (4个半角空格)`

* `VARCHAR`：可变长字符串类型

  > `VARCHAR(100)`指定最大长度为100，不足的部分不会补足

* `DATE`：日期型

##### 约束的设置

* `NOT NULL`：数据必须非空
* `PRIMARY KEY`：主键



### 1.5 表的删除和更新

##### 表的删除

```sql
DROP TABLE <表名>;
DROP TABLE Product;
```

* 删除的表无法恢复

##### 表定义的更新

* 添加列

  ```sql
  ALTER TABLE <表名> ADD COLUMN <列的定义>;
  ALTER TABLE Product ADD COLUMN product_name_pinyin VARCHAR(100);
  
  -- Oracle和SQL Server中不需要写COLUMN
  -- Oracle可用括号添加多列:
  -- ALTER TABLE <表名> ADD (<列1的定义>, <列2的定义>, ...);
  ALTER TABLE Product ADD (product_name_pinyin VARCHAR2(100));
  ```

* 删除列

  ```sql
  ALTER TABLE <表名> DROP COLUMN <列名>;
  ALTER TABLE Product DROP COLUMN product_name_pinyin;
  
  -- Oracle中不需要写COLUMN
  -- Oracle中可用括号删除多列
  -- -- ALTER TABLE <表名> DROP （<列1的定义>, <列2的定义>, ...);
  ALTER TABLE Product DROP (product_name_pinyin);
  ```

* 表的更新和表的删除一样，执行后无法恢复

##### 向表中插入数据

```sql
-- DML:插入数据
BEGIN TRANSACTION; --开始插入行的指令语句
INSERT INTO Product VALUES ('0001', 'T恤衫', '衣服',1000, 500, '2009-09-20');
INSERT INTO Product VALUES ('0002', '打孔器', '文具',500, 320, '2009-09-11');
INSERT INTO Product VALUES ('0003', '运动T恤', '衣服',4000, 280, NULL);
INSERT INTO Product VALUES ('0004', '菜刀', '厨房用具',3000, 2800, '2009-09-20');
INSERT INTO Product VALUES ('0005', '高压锅', '厨房用具',6800, 5000, '2009-01-15');
INSERT INTO Product VALUES ('0006', '叉子', '厨房用具',500, NULL, '2009-09-20');
INSERT INTO Product VALUES ('0007', '擦菜板', '厨房用具',880, 790, '2009-04-28');
INSERT INTO Product VALUES ('0008', '圆珠笔', '文具',100, NULL, '2009-11-11');
COMMIT; --确定插入行的指令语句

--MySQL中'BEGIN TRANSACTION'改为'START TRANSACTION'
--Oracle和DB2中无需使用该语句
```

##### 表名的修改

* 由于标准SQL没有`RENAME`，因此各个数据库的语法不同

```Sql
-- Oracle/PostgreSQL
ALTER TABLE Poduct RENAME TO Product;
--DB2
RENAME TABLE Poduct TO Product;
--SQL Server
sp_rename 'Poduct', 'Prodcut';
--MySQL
RENAME TABLE poduct to Product;
```





## CH2.查询基础

### 2.1 SELECT语句基础

##### 列的查询

```sql
SELECT <列名>,..
 FROM <表名>;
```

##### 查询所有列

```sql
SELECT *
 FROM <表名>;
```

* 使用`*`星号的话，无法设定列的显示顺序，这时会按照`CREATE TABLE`语句的定义对列进行排序

##### 为列设定别名

* SQL语句使用`AS`关键字为列设定别名

  ```SQL
  SELECT product_id   AS id,
         product_name AS name,
         purchase_price AS "价格"
    FROM Product;
  ```

* 别名使用中文时需要用双引号

##### 常数的查询

* SELECT子句不仅可以书写列名，还可以书写常数

  > 字符串常数、数字常数、日期常数，除数字以外需要用单引号`''`

  ```sql
  SELECT '商品' AS string,
          38   AS number,
          '2009-02-24' AS date,
          product_id
     FROM Product;
  ```

  | string | number | date       | product_id |
  | :----- | :----- | :--------- | :--------- |
  | 商品   | 38     | 2009-02-24 | 0001       |
  | 商品   | 38     | 2009-02-24 | 0002       |

##### 从结果中删除重复行

* 在SELECT子句中使用`DISTINCT`实现

  ```sql
  SELECT DISTINCT product_type
   FROM Product;
  ```

* 使用`DISTINCT`时，NULL也被视为一类数据，会在输出结果中保留下来

* 在多列中使用`DISTINCT`，会将多列数据组合，只合并多列数据均一致的数据

##### 使用WHERE语句选择记录

```sql
SELECT <列名>,
  FROM <表名>,
 WHERE <条件表达式>;
--例：
SELECT product_name, product_type
  FROM Product
 WHERE product_type = '衣服';
```

* 首先通过`WHERE`子句查询符合指定条件的记录
* 然后再从中选取`SELECT`语句指定的列
* `WHERE`子句必须跟在`FROM`之后

##### 不包含FROM的特殊情况

* 一般情况下SELECT子句是由SELECT子句和FROM子句组成的

* 特殊情况下FROM并不是必不可少的，只是用SELECT进行计算也是可以的

  ```sql
  --SQL Server/PostgreSQL/MySQL
  SELECT (100+200)*3 AS calculation;
  ```

* 但存在像Oracle这样不允许省略SELECT语句中FROM子句的RDBMS



### 2.2 算术运算符和比较运算符

##### 算术运算符

* SQL语句中能够使用计算表达式

  ```sql
  SELECT product_name,
         sale_price,
         sale_price * 2 AS sale_price_x2
    FROM Product;
  ```

* 主要运算符：

  * `+`
  * `-`
  * `*`
  * `/`

* SELECT子句中可以使用常数或者表达式

##### 运算符的NULL

* 所有包含NULL的计算，结果都是NULL，即使NULL除以0时这一原则也适用

  > 如5+NULL, 10-NULL, 4/NULL, 1*NULL, NULL/9, NULL/0

  > 有时希望NULL像0一样，得到NULL+5=5的结果，将在6.1中介绍

##### 比较运算符

* 比较运算符：
  * `=`
  * `<>`：不等于
  * `>=`
  * `>`
  * `<=`
  * `<`

* 在WHERE子句中使用比较运算符可以组合不同的条件表达式

  ```sql
  SELECT product_name, product_type, sale_price
    FROM Product
   WHERE sale_price>=100;
  ```

* 条件表达式中可以使用计算表达式

  ```sql
  SELECT product_name, product_type, sale_price
    FROM Product
   WHERE sale_price - purchase_price >= 500;
  ```

##### 对字符串使用不等号

* 字符串类型的数据原则上按照字典顺序进行排序，不能与数字的大小顺序混淆

* 该规则对定长字符串和可变长字符串都适用

  ```sql
  '1'
  '10'
  '2'
  '222'
  '3'
  '3-1'
  -- 因此有如下：
  '1-3'<'2'
  '222'<'3'
  ```

##### 比较运算符的NULL

* 作为查询条件的列中有NULL值时，使用`pruchase_price = 2800`和`pruchase_price <> 2800`的查询结果均不包含值为`NULL`的行

* 使用`pruchase_price = NULL`也不正确

* 查询NULL的正确做法是使用`IS NULL`

* 与之对应，选取不是NULL的记录时，使用`IS NOT NULL`

  ```sql
  SELECT *
    FROM Product
   WHere purchase_price IS NOT NULL;
  ```



### 2.3 逻辑运算符

##### NOT运算符

* NOT无法单独使用，需要结合其他查询条件使用

* NOT相较于`<>`不等于运算符范围更广，结果会包括NULL值

  > 参见[比较运算符的NULL](#比较运算符的NULL)

  ```sql
  SELECT *
    FROM Product
   WHERE NOT sale_price >=1000;
  ```

  > sale_price小于1000以及值为NULL的记录都会被输出

* NOT逻辑直观性更差，不能滥用

##### AND运算符和OR运算符

* AND两侧查询条件都成立时整个查询条件才成立

* OR两侧查询条件有一个成立时整个查询条件都成立

  ```sql
  SELECT *
    FROM Product
   WHERE product_type = '文具'
     AND sale_price >= 500;
  ```

##### 通过括号强化处理

* AND运算符优先于OR运算符

  ```sql
  SELECT *
    FROM Product
   WHERE product_type = '文具'
     AND regist_date = '2009-09-11'
      OR regist_date = '2009-09-20';
  /* 等价于
  WHERE (product_type = '文具' AND regist_date = '2009-09-11')
     OR regist_date = '2009-09-20';
  */
  ```

* 想要优先执行OR运算符时，需要使用半角括号

  ```sql
  SELECT *
    FROM Product
   WHERE product_type = '文具'
     AND (   regist_date = '2009-09-11'
          OR regist_date = '2009-09-20');
  ```

##### 逻辑运算符与真值

* 逻辑运算符`NOT`，`AND`，`OR`实际上是对`真值`进行操作
* 使用AND的逻辑运算称为`逻辑积`，使用OR的逻辑运算称为`逻辑和`
* 真值表

##### 含有NULL时的真值

* SQL中真值存在特殊的第三种值：`不确定UNKNOWN`

  > 参见[比较运算符的NULL](#比较运算符的NULL)
  >
  > 即无论`pruchase_price = 2800`和`pruchase_price <> 2800`的查询结果均不包含值为`NULL`的行

* 三值逻辑的真值表

  ```ascii
  P     Q     (P AND Q) (P OR Q)
  1     1     1          1
  1     0     0          1
  1     NULL  NULL       1
  0     1     0          1
  0     0     0          0
  0     NULL  0          NULL
  NULL  1     NULL       1
  NULL  0     0          NULL
  NULL  NULL  NULL       NULL
  ```

  > 1 > NULL > 0
  >
  > AND取大值，OR取小值

  

## CH3.聚合查询

### 3.1 对表进行聚合查询

##### 聚合函数

* 聚合函数：将多行汇总为一行
  * `COUNT`：计算表中的记录数
  * `SUM`
  * `AVG`
  * `MAX`
  * `MIN`

##### COUNT

```sql
SELECT COUNT(*)
  FROM Product;
```

##### 计算NULL以外的行数

```sql
SELECT COUNT(purchase_price)
  FROM Product;
```

* 将包含NULL的列作为参数时，`COUNT(*)`和`COUNT(<列名>)`的结果不相同

  ```sql
  --Nulltable是一个三行一列，且值均为NULL的空表
  SELECT 	COUNT(*), COUNT(COL_1)
    FROM Nulltable;
  --执行结果
  count count
      3     0
  ```

* `COUNT(*)`会得到包含NULL的数据行数，而`COUNT(<列名>)`不会得到包含NULL的数据行数

* 该特性为`COUNT`函数特有，其他函数不能使用`*`星号作为参数

* 事实上除了`COUNT(*)`以外，聚合函数会将NULL排除在外，不参与统计

##### SUM

```sql
SELECT SUM(sale_price)
  FROM Product;
```

> 四则运算如果存在NULL，结果一定是NULL；但是所有聚合函数（除了COUNT以外）都会无视NULL，即NULL根本不参与求和运算。

##### AVG

```sql
SELECT AVG(purchase_price)
  FROM Product;
```

> 由于总共八行数据，而purchase_price中有两行为NULL，因此求平均的分母为6而不是8

##### MAX和MIN

* 语法同`SUM`，以列为参数
* `MAX/MIN`与`SUM/AVG`不同：
  * `SUM/AVG`只能对数值类型的列使用
  * `MAX/MIN`原则上可以适用于任何数据类型的列

##### 使用聚合函数删除重复值

```sql
SELECT COUNT(DISTINCT product_type)
  FROM Product;
```

> 表中有3种商品共8行，为了计算商品种类的数量，需要以`DISTINCT`关键字作为参数

* DISTINCT必须写在COUNT括号中，否则就会先计算出行数（8），再删除重复数据
* 所有聚合函数均可以使用DISTINCT

```sql
SELECT SUM(DISTINCT sale_price)
  FROM Product;
```



### 3.2 对表进行分组

##### GROUP BY子句

```sql
SELECT <列名1>, <列名2>, <列名3>,...
  FROM <表名>
GROUP BY <列名1>, ...

--例：
SELECT product_type, COUNT(*)
  FROM Product
GROUP BY product_type;
```

* GROUP BY必须写在FROM语句之后，如果有WHERE语句的话必须在WHERE语句之后

  > SELECT
  >
  > FROM
  >
  > WHERE
  >
  > GROUP BY

##### 聚合键中包含NULL

> 聚合键：GROUP BY子句中指定的列名

* 聚合键中存在NULL值时，会将NULL作为一组特定数据

  ```sql
  SELECT purchase_price, COUNT(*)
    FROM Product
  GROUP BY purchase_price;
  ```

  | purchase_price | COUNT(*) |
  | :------------- | :------- |
  | 500            | 1        |
  | 320            | 1        |
  | 280            | 1        |
  | 2800           | 1        |
  | 5000           | 1        |
  | NULL           | 2        |
  | 790            | 1        |

##### WHERE语句GROUP BY

* 使用WHERE语句进行汇总处理时，会先根据WHERE子句指定的条件进行过滤，然后再进行汇总处理

  ```sql
  SELECT purchase_price, COUNT(*)
  FROM Product
  WHERE product_type = '衣服'
  GROUP BY purchase_price;
  ```

  > WHERE子句过滤结果：
  >
  > | product_id | product_name | product_type | sale_price | purchase_price | regist_date |
  > | :--------- | :----------- | :----------- | :--------- | :------------- | :---------- |
  > | 0001       | T恤衫        | 衣服         | 1000       | 500            | 2009-09-20  |
  > | 0003       | 运动T恤      | 衣服         | 4000       | 280            | NULL        |
  >
  > GROUP BY汇总结果：
  >
  > | purchase_price | COUNT(*) |
  > | :------------- | :------- |
  > | 500            | 1        |
  > | 280            | 1        |

* 即执行顺序为

  1. `FROM`
  2. `WHERE`
  3. `GROUP BY`
  4. `SELECT`

##### 聚合函数与GROUP BY子句有关的常见错误

1. SELECT子句中书写了多余的列

   * 使用聚合函数时，SELECT子句只能存在以下三种元素：
     1. 常数（整数，字符串，日期）
     2. 聚合函数
     3. GROUP BY子句中指定的列名（即聚合键）

   ```sql
   SELECT product_name, purchase_price, COUNT(*)
   FROM Product
   GROUP BY purchase_price;
   -- 由于product_name既非常数也不是聚合函数，也不是聚合键，因此会出错
   -- 事实上，由于聚合键和商品名并不是一一对应的，因此无法知道查询结果中product_name该填入什么
   ```

2. GROUP BY子句中写了列的别名

   * SELECT子句可以使用AS关键字来指定别名，但是再GROUP BY子句中不能使用别名

     ```sql
     SELECT product_type AS pt, COUNT(*)
     FROM Product
     GROUP BY pt;
     -- 出错，GROUP BY子句不能使用别名
     ```

   * 这是由于SQL语句在DBMS中执行顺序造成的

     > 上一小节中提到，SELECT子句在GROUP BY字句之后执行，因此SELECT子句中定义的别名，DBMG在执行GROUP BY时还不知道
     >
     > 不过在PostpreSQL中不会出错，但并不通用

3. GROUP BY子句结果的排序

   * GROUP BY子句结果的显示是无序的
   * 要按照某种特定顺序排序的话，需要在SELECT子句中进行指定，见第四章

4. 在WHERE子句中使用聚合函数

   * 只有在SELECT、HAVING、ORDER BY子句中能够使用COUNT等聚合函数

   ```sql
   SELECT product_type, COUNT(*)
   FROM Product
   GROUP BY product_type;
   ```

   | product_type | COUNT(*) |
   | :----------- | :------- |
   | 衣服         | 2        |
   | 文具         | 2        |
   | 厨房用具     | 4        |

   > 如果要选出正好包含两行数据的组，初学者往往会尝试在WHERE子句中使用聚合函数：
   >
   > ```sql
   > SELECT product_type, COUNT(*)
   > FROM Product
   > WHERE COUNT(*) = 2
   > GROUP BY product_type;
   > -- 出错，WHERE子句中不能使用聚合函数
   > ```

##### DISTINCT与GROUP BY

* distinct简单来说就是用来去重的，而group by的设计目的则是用来聚合统计的
* 两者在能够实现的功能上有些相同之处，但应该仔细区分，因为用错场景的话效率相差可以倍计
* 单纯的去重操作使用distinct，速度是快于group by的



### 3.3 为聚合结果指定条件

##### HAVING子句

* WHERE子句只能指定行的条件，不能指定组的条件

* HAVING子句可以指定组的条件

  ```sql
  SELECT <列名1>, <列名2>...
  FROM <表名>
  GROUP BY <列名1>, <列名2>...
  HAVING <分组结果对应的条件>
  
  -- 例：
  SELECT product_type, AVG(sale_price)
  FROM Product
  GROUP BY product_type
  HAVING AVG(sale_price) >= 2500;
  ```

* HAVING子句必须写在GROUP BY字句之后，且在DBMS内部执行顺序也在GROUP BY之后

* 即执行顺序为

  1. `FROM`
  2. `WHERE`
  3. `GROUP BY`
  4. `HAVING`
  5. `SELECT`

##### HAVING子句的构成要素

* 与包含GROUP BY子句时的SELECT子句一样，HAVING子句能够使用的三种要素：

  * 常数（整数，字符串，日期）
  * 聚合函数
  * GROUP BY子句中指定的列名（即聚合键）

  ```SQL
  -- 正确示例
  SELECT product_type, AVG(sale_price)
  FROM Product
  GROUP BY product_type
  HAVING COUNT(*) = 2;
  
  -- 错误示例
  SELECT product_type, AVG(sale_price)
  FROM Product
  GROUP BY product_type
  HAVING product_name = '圆珠笔';
  ```

##### 能写在HAVING与WHERE中的条件

* 聚合键所对应的条件，既能写在HAVING中，也能写在WHERE中

  ```sql
  SELECT product_type, AVG(sale_price)
  FROM Product
  GROUP BY product_type
  HAVING product_type = '衣服';
  
  SELECT product_type, AVG(sale_price)
  FROM Product
  WHERE product_type = '衣服'
  GROUP BY product_type;
  ```

* 但是应该尽量写在WHERE子句中：

  * WHERE子句：指定行所对应的条件
  * HAVING子句：指定组所对应的条件
  * 由于WHERE在排序之前就对数据进行了过滤，大大减少了对数据排序的次数，因此处理速度更快
  * 由于可以对WHERE子句指定条件所对应的列建立索引，也可以大幅提高处理速度



### 3.4 对查询结果进行排序

##### ORDER BY子句

* 在SELECT语句末尾添加ORDER BY子句能明确指定排列顺序

* 默认为升序

  ```sql
  SELECT <列名>,..
   FROM <表名>
   ORDER BY <排序基准列1>, <排序基准列2>, ...;
  
  -- 例：
  SELECT product_id, sale_price
    FROM Product
   ORDER BY sale_price;
  
  -- 但是我测试时发现在如下代码中结果未能正常排序：
  SELECT *
    FROM Product
   ORDER BY sale_price;
  ```

##### 指定升序或降序

* 未指定时，ORDER BY默认使用升序进行排列

* 使用DESC指定降序，ASC指定升序

  ```sql
  SELECT product_id, sale_price
    FROM Product
   ORDER BY sale_price DESC;
  ```

* ASC和DESC是以列为单位指定的，因此可以同时指定一个列为升序，指定其他列为降序

##### 指定多个排序键

```sql
SELECT product_id, sale_price
  FROM Product
 ORDER BY sale_price, product_id;
```

* 规则是优先使用左侧的键

##### NULL的顺序

* 由于NULL和数字不能进行排序，也不能与字符串和日期比较大小，因此含有NULL的列作为排序键时，NULL会在结果开头或者末尾汇总显示
* 究竟在开头或末尾，并无特殊规定，某些DBMS可以自行指定

##### 在排序键中使用别名

* GROUP BY中不能使用SELECT子句中定义的别名
* 但是ORDER BY中可以使用，这是由SQL语句在DBMS内部执行顺序决定的：
  1. `FROM`
  2. `WHERE`
  3. `GROUP BY`
  4. `HAVING`
  5. `SELECT`
  6. `ORDER BY`

##### ORDER BY子句中可以使用的列

* ORDER BY子句中可以使用存在于表中，但是不包含在SELECT子句之中的列

  ```sql
  SELECT product_name, sale_price
  FROM Product
  ORDER BY product_id;
  ```

* 还可以使用聚合函数

  ```sql
  SELECT product_type, COUNT(*)
  FROM Product
  GROUP BY product_type
  ORDER BY COUNT(*);
  ```

##### 不要使用列编号

* 列编号：SELECT子句中的列按照从左到右的序号

  ```sql
  SELECT product_id, product_name, sale_price, purchase_price
  FROM Product
  ORDER BY 3 DESC, 1;
  ```

* 代码阅读困难且该功能将会被删除



## CH4.数据更新

### 4.1 数据的插入

##### INSERT

```SQL
INSERT INTO <表名> (列1,列2,列3,...) VALUES (值1,值2,值3,...)
-- 例
INSERT INTO ProductIns (product_id, product_name, product_type, sale_price, purchase_price, regist_date) VALUES ('0001', 'T恤衫', '衣服', 1000, 500, '2009-09-20')
```

* 原则上执行一次INSERT会插入一行数据，但部分DBMS支持一次插入多行数据
* 但是多行插入很难在出错时找出发生错误的地方，因此不推荐

##### 列清单的省略

* 对表进行全列INSERT时，可以省略表名后的列清单，此时VALUES子句的值会默认按照从左到右的顺序依次赋值

  ```sql
  INSERT INTO <表名> VALUES (值1,值2,值3,...)
  -- 例
  INSERT INTO ProductIns VALUES ('0001', 'T恤衫', '衣服', 1000, 500, '2009-09-20')
  ```

##### 插入NULL

* INSERT语句想要给某一列赋值NULL时，可以直接在VALUES子句的值清单中写入NULL
* 但是想要插入NULL的列不能设置NOT NULL约束

##### 插入默认值

* 在CREATE TABLE语句中设置`DEFAULT`约束来设定默认值

* `DEFAULT <默认值>`的形式可以指定默认值

  ```sql
  CREATE TABLE ProductIns
  (product_id   CHAR(4)   NOT NULL,
   sale_price   INTEGER   DEFAULT 0,
   PRIMARY KEY (product_id));
  ```

* 如果在创建表的同时设定了默认值，就可以在INSERT语句中自动为列赋值

  * 显式

    ```sql
    INSERT INTO ProductIns (product_id, sale_price) VALUES ('0007', DEFAULT);
    ```

    > 更直观，建议使用

  * 隐式

    ```sql
    INSERT INTO ProductIns (product_id) VALUES ('0007');
    ```

* 如果省略了没有设定默认值的列，该列的值就会被设定为NULL

  > 此时如果该列有NOT NULL约束，INSERT语句就会出错

##### 从其他表中复制数据

* 要插入数据，除了使用VALUES子句指定具体的数据之外，还可以从其他表中复制数据

* `INSERT...SELECT`语句

  ```sql
  INSERT INTO <新表名> (<要复制的列1>, <要复制的列2>, <要复制的列3>, ...)
  SELECT <要复制的列1>, <要复制的列2>, <要复制的列3>, ...
    FROM <旧表名>
  ```

* `INSERT...SELECT`中的SELECT语句，可以使用之前学到的各种子句，包括WHERE和GROUP BY等

  ```sql
  INSERT INTO ProductType (product_tye, sum_sale_price, sum_purchase_price)
  SELECT product_type, SUM(sale_price), SUM(purchase_price)
    FROM Product
  GROUP BY Product_type;
  ```



### 4.2 数据的删除

##### DROP TABLE与DELETE

* DROP TABLE可以将表完全删除
* DELETE会留下表，而删除表中的数据
* 两种方法删除后，想要恢复数据都十分困难，因此要慎重删除

##### DELETE

```sql
DELETE FROM <表名>;
--例
DELETE FROM Product;
```

* 该基本语法会删除表中的所有数据行，但是保留了数据表
* 删除对象不是表也不是列，而是数据行，因此无法只删除部分列的数据

##### 指定删除对象的DELETE

* 搜索型DELETE

```SQL
DELETE FROM <表名>
 WHERE <条件>;
--例
DELETE FROM Product
 WHERE sale_price > 500;
```

* DELETE语句只能使用WHERE子句，不能使用GROUP BY，HAVING和ORDER BY三类语句

##### 删除和舍弃

* 部分数据库产品有一种`TRUNCATE`语句，只能删除表中的全部数据

  ```sql
  TRUNCATE <表名>;
  ```

* 由于不能具体控制删除对象，所以处理速度比DELETE快得多



### 4.3 数据的更新

##### UPDATE

* 更改表中某列的所有数据

  ```sql
  UPDATE <表名>
     SET <列名> = <表达式>;
  --例
  UPDATE Product
     SET regist_date = '2009-10-11';
  ```

  > 原本为NULL的数据行的值也被更新了

##### 指定条件的UPDATE

* 搜索型UPDATE

  ```sql
  UPDATE <表名>
     SET <列名> = <表达式>
   WHERE <条件>;
  --例
  UPDATE Product
     SET sale_price = sale_price * 10
   WHERE product_type = '厨房用具';
  ```

##### 使用NULL进行更新

* `NULL清空`：将赋值表达式右边的值直接写为NULL即可
* 但只有未设置NOT NULL约束和主键约束的列才可以清空

##### 多列更新

* 如果想要在一条UPDATE语句中更新多列数据

  * 方法一

    ```sql
    UPDATE Product 
       SET sale_price = sale_price * 2,
           purchase_price = purchase_price / 2
     WHERE product_type = '厨房用具';
    ```

    > 逗号分隔，可以在所有DBMS中使用，因此通常采用方法一

  * 方法二

    ```sql
    UPDATE Product 
       SET (sale_price, purchase_price) = (sale_price * 2, purchase_price / 2)
     WHERE product_type = '厨房用具';
    ```

    > 可以在PostgreSQL和DB2中使用



### 4.4 事务

##### 事务

* 事务即需要在同一个处理单元中执行的一系列更新处理的集合

##### 创建事务

```ascii
事务开始语句；
   DML语句1；
   DML语句2；
   DML语句3；
   DML语句4；
   ...
事务结束语句（COMMIThuozhe ROLLBACK); 
```

* 使用事务开始语句和事务结束语句，将一系列DML语句（INSERT/UPDATE/DELETE）括起来，就实现了一个事务处理

* 标准SQL中并没有定义事务开始语句，而是由各个DBMS自己定义的

  ```sql
  --SQL Server / PostgreSQL
  BEGIN TRANSACTION
  --MySQL
  START TRANSACTION
  --Oracle / DB2
  无
  ```

* 事物的结束需要用户明确地给出指示，结束事务的指令有两种：

  1. COMMIT：提交，之后就无法恢复到事务开始之前的状态了
  2. ROLLBACK：回滚，取消事务包含的全部更新处理的结束指令，数据库恢复到事务开始之前的状态

##### 事务处理何时开始

* 几乎所有的数据库产品都无需事务开始指令，大部分情况下事务在数据库连接建立时就悄悄开始了

  > 如Oracle在数据库建立连接之后，第一条SQL语句执行时事务就开始了

* 不使用指令而实悄悄开始事务的情况下，区分各个事务有两种模式：

  1. 每条SQL语句就是一个事务（自动提交模式）

     > 在自动提交模式下，一旦执行了DELETE操作，即使回滚也无法恢复了

  2. 指导用户执行COMMIT或者ROLLBACK为止算一个事务

  > SQL Sever / PostgreSQL / MySQL默认使用1，Oracle默认使用2

##### ACID特性

* 原子性（Atomicity）：指事务结束时，包含的更新处理要么全部执行（COMMIT）要么全部不执行（ROLLBACK），不会出现执行了一部分的情况
* 一致性（Consistency）：事务中包含的处理要满足数据库提前设置的约束，如NOT NULL和主键约束等
* 隔离性（Isolation）：不同事务之间互不干扰，不会互相嵌套，即一个事务进行的更改在其结束并提交之前，对其他事务是不可见的
* 持久性（Durability）：事务结束后，DBMS能够保证该时间点的数据状态会被保存，即使系统故障也能恢复



## CH5.复杂查询

### 5.1 视图

##### 视图和表

* 视图和表的区别在于是否存储了实际的数据
  * 表中保存的是实际的数据
  * 而视图本身并不存储数据，视图中保存的是从表中取出数据所使用的SELECT语句
  * 从视图中读取数据时，视图会在内部执行该SELECT语句并创建出一张临时表
* 视图的优点：
  * 无需保存数据因此可以节省存储设备的容量
  * 可以将频繁使用的SELECT语句保存成视图，特别是在进行汇总以及复杂查询条件导致SELECT语句非常庞大时，使用视图可以大大提高效率
  * 由于视图本质上就是SELECT语句，因此**视图中的数据会随着原表的变化而自动更新**

##### 创建视图的方法

* 创建视图需要使用`CREATE VIEW`语句

  ```sql
  CREATE VIEW <视图名称>(<视图列名1>, <视图列名2>, ...)
  AS
  <SELECT语句>
  ```

* SELECT语句需要书写在AS关键字之后，SELECT语句中列的排列顺序与视图中列的顺序对应

  > 定义视图时可以使用任何SELECT语句，除了ORDER BY子句

* 视图中列的名字在视图名称后的列表中自定义

  ```sql
  CREATE VIEW ProductSum (product_type, cnt_product)
  AS
  SELECT product_type, COUNT(*)
    FROM Product
   GROUP BY product_type;
  ```

##### 视图的使用

* 视图和表一样，可以书写在SELECT语句的FROM子句之中

  ```SQL
  SELECT produc_type, cnt_product
    FROM ProductSum;
  ```

##### 视图的查询

* 在FROM子句中以视图为对象进行查询，有两个步骤：

  1. 执行定义视图的SELECT语句（即视图自身保存的SELECT语句）
  2. 根据得到的结果，在执行FROM子句中使用视图的SELECT语句（即外层新的查询语句）

* 即对视图的查询通常需要执行2条以上的SELECT语句

  > 2条以上，因为可能出现以视图为基础创建视图的`多重视图`

* 对多数DBMS而言，多重视图会降低SQL的性能，因此初学者应使用单一视图

##### 视图的限制

1. 定义视图时不能使用ORDER BY子句

   > 视图和表一样，数据行本身是没有顺序的，尽管一些DBMS在定义视图时可以使用ORDER BY，但是这并不是通用的语法

2. 对视图进行更新

   > INSERT、DELETE、UPDATE这类更新语句

   * 标准SQL中规定，如果定义视图的SELECT语句满足某些条件，那么这个视图就可以更新
   * 代表性条件：
     1. SELECT子句中未使用DISTINCT
     2. FROM子句中只有一张表
     3. 未使用GROUP BY子句
     4. 未使用HAVING子句
     5. 等等
   * 实例参见《SQL基础教程2nd》P155-157

##### 删除视图

```sql
DROP VIEW 视图名称(<视图列名1>, <视图列名2>, ...);
--例
DROP VIEW ProductSum;
```

> 在PostgreSQL中，如果删除以视图为基础创建的多重试图，由于存在关联视图，会发生错误。此时需要用CASCADE选项删除关联视图：
>
> ```sql
> DROP VIEW ProductSum CASCADE;
> ```



### 5.2 子查询

##### 子查询和视图

* 视图：保存读取数据的SELECT语句来为用户提供便利

* 子查询：将定义视图的SELECT语句直接用在FROM子句中

  ```sql
  --视图
  CREATE VIEW ProductSum (product_type, cnt_product)
  AS
  SELECT product_type, COUNT(*)
    FROM Product
   GROUP BY product_type;
  
  SELECT produc_type, cnt_product
    FROM ProductSum;
  
  --子查询
  SELECT product_type, cnt_product
    FROM (SELECT product_type, COUNT(*) AS cnt_product
            FROM Product
           GROUP BY product_type) AS ProductSum;      --Oracle：去掉AS
  ```

  > 在Oracle的FROM子句中，最后的`AS ProductSum`需要去掉`AS`，否则会出错

* `AS ProductSum`就是子查询的名称，但是该名称是一次性的，并不会像视图那样保存在存储介质之中，而是在SELECT语句执行之后就消失了

* SELECT语句包含嵌套的结构，首先会执行FROM子句中的SELECT语句，然后才会执行外层的SELECT语句

  > 使用子查询的SQL语句会从子查询开始执行，即子查询会作为内层查询首先执行

* 子查询的层数可以一直嵌套：

  ```sql
  SELECT product_type, cnt_product
    FROM (SELECT *
            FROM (SELECT product_type, COUNT(*) AS cnt_product
                    FROM Product
                   GROUP BY product_type) AS ProductSum  --Oracle：去掉AS
           WHERE cnt_product = 4) AS ProductSum2;        --Oracle：去掉AS
  ```

  > 同上，在Oracle中，`AS ProductSum`需要去掉`AS`

* 随着子查询嵌套，可读性变差且SQL性能变差，因此需要避免多层嵌套

##### 子查询的名称

* 原则上子查询必须设定名称
* 为子查询设定名称时需要使用AS关键字，Oracle中例外

##### 标量子查询

* 标量子查询必须且只能返回1行1列的结果，即返回一个单一值

* WHERE子句中使用标量子查询

  ```sql
  --WHERE子句中不能使用聚合函数
  SELECT product_id, product_name, sale_price
    FROM Product
   WHERE sale_price > AVG(sale_price);
  --出错，因为不能使用聚合函数
  
  --使用标量子查询
  SELECT product_id, product_name, sale_price
    FROM Product
   WHERE sale_price > (SELECT AVG(sale_price)
                         FROM Product);
  ```

* 标量子查询能够在任何可以使用单一值的位置都可以使用

  > 即能够使用常数或者列名的地方，无论是SELECT、GROUP BY、HAVING、ORDER BY，几乎所有地方都可以使用

* 在SELECT语句中书写常数时可以使用

  ```sql
  SELECT product_id,
         product_name,
         (SELECT AVG(sale_price)
            FROM Product) AS avg_price --这里的不设定别名的话会直接把整个SELECT子句作为列名
    FROM Product;
  ```

* 在HAVING子句中使用标量子查询

  ```sql
  SELECT product_type, AVG(sale_price)
    FROM Product
   GROUP BY product_type
   HAVING AVG(sale_price) > (SELECT AVG(sale_price)
                               FROM Product);
  ```

##### 标量子查询的注意事项

* 子查询如果返回了多行结果，就不是标量子查询，而是普通子查询了
* 普通子查询就不能用在`=`或`<>`等单一输入值的运算符中，也不能用在SELECT子句中



### 5.3 关联子查询

##### 普通子查询与关联子查询

* 为了选取各个商品种类中高于该商品种类平均销售单价的商品

  ```sql
  --按商品种类求平均价格
  SELECT AVG(sale_price)
    FROM Product
   GROUP BY product_type;
  
  --如果用普通子查询的方式
  SELECT product_type, product_name, sale_price
    FROM Product
   WHERE sale_price > (SELECT AVG(sale_price)
                         FROM Product
                        GROUP BY product_type); --出错
  ```

  > 普通子查询会返回三行结果，并不是标量子查询
  >
  > 而在WHERE子句中使用子查询时，必须是标量子查询

  ```sql
  --正确的做法是关联子查询
  SELECT product_type, product_name, sale_price
    FROM Product AS P1  --（1）
   WHERE sale_price > (SELECT AVG(sale_price)
                         FROM Product AS P2
                        WHERE P1.product_type = P2.product_type  --（2）
                        GROUP BY product_type);
  ```

  > 关键是：在子查询中添加的WHERE子句的条件
  >
  > 由于比较对象是同一张表`Product`，为了区分，分别使用了`P1`和`P2`作为别名

  > Oracle中设定别名要去掉`AS`

* 在使用关联子查询时，需要在表所对应的列名之前加上表的别名，以`<表名>.<列名>`的形似表述

##### 结合条件需写在子查询中

* 关联条件不能写在子查询之外的外层查询中

  ```sql
  SELECT product_type, product_name, sale_price
    FROM Product AS P1
   WHERE P1.product_type = P2.product_type --错误
     AND sale_price > (SELECT AVG(sale_price)
                         FROM Product AS P2
                        GROUP BY product_type);
  ```

* 关联名称有`作用域`，子查询内部设定的关联名称`P2`只能在子查询内部使用

  > SQL优先执行内层子查询再外层查询，因此子查询结束时只会留下执行结果，作为抽出源的P2表已经不存在了（P2这个名称不存在了）



## CH6.函数、谓词、CASE表达式

### 6.1 各种各样的函数

##### 函数的种类

* 函数可以大致分为：

  * 算术函数：数值计算
  * 字符串函数：进行字符串操作
  * 日期函数：进行日期操作
  * 转换函数：转换数据类型和值
  * 聚合函数：进行数据聚合


##### 算术函数

* 四则运算：`+`，`-`，`*`，`/**`**

* **`NUMERIC`数据类型**：`NUMERIC(全体位数, 小数位数)`的形式指定

* `ABS`：绝对值

  ```sql
  SELECT m, ABS(m)
    FROM Sample;
  ```

  * 当ABS函数的参数是NULL时，返回的结果也是NULL
  * 绝大部分函数对NULL的返回值也是NULL

* `MOD`：求余

  ```sql
  SELECT n, p, MOD(n, p) AS mod_col
    FROM Sample;
  ```

  > 除了SQL Server（以`%`求余数）以外的主流DBMS都支持MOD函数

* `ROUND`：四舍五入

  ```sql
  ROUND(对象数值, 保留小数的位数)
  SELECT m, n, 
         ROUND(m, n) AS round_col
    FROM Sample;
  -- 例如，ROUND(5.55, NULL)=NULL
          ROUND(5.55, 1)=5.6
          ROUND(2.270, 2)=2.27
  ```

##### 字符串函数

* `||`：拼接两个字符串

  ```sql
  SELECT str1, str2,
         str1 || str2 AS str_concat
    FROM SapmleStr;
  
  SELECT str1, str2, str3
         str1 || str2 ||str3 AS str_concat
    FROM SapmleStr
   WHERE str1 = '山田';
  ```

  > 同样，如果子字符串中有NULL，拼接的结果也是NULL
  >
  > ||函数在SQL Server（+或者CONCAT）和MySql（CONCAT）中无法使用

* `LENGTH`：字符串长度

  ```sql
  LENGTH(字符串)
  SELECT str1,
         LENGTH AS len_str
    FROM SapmleStr;
  ```

  > 在SQL Server（LEN）中无法使用

* `LOWER`：小写转换

* `UPPER`：大写转换

  > 只适用于英文字母，不适用于英文字母以外的场合

  ```sql
  LOWER(字符串)
  UPPER(字符串)
  
  SELECT str1, str2
         LOWER(str1) AS low_str,
         UPPER(str2) AS up_str
    FROM SampleStr
   WHERE str1 IN ('ABC', 'aBC', 'abc', '山田');
  ```

* `REPLACE`：字符串替换

  ```sql
  REPlACE(对象字符串, 替换前的子字符串, 替换后的子字符串)
  
  SELECT str1, str2, str3
         REPLACE(str1, str2, str3) AS rep_str
    FROM SampleStr;
  
  REPLACE(ABC太郎, 太郎, 是我) = ABC是我
  REPLACE(micmic, i, I) = mIcmIc
  ```

* `SUBSTRING`：字符串的截取

  ```sql
  SUBSTRING(对象字符串 FROM 截取的起始位置 FOR 截取的字数)
  
  SELECT str1,
         SUBSTRING(str1 FROM 3 FOR 2) AS sub_str
    FROM SampleStr;
  
  SUBSTRING(NULL FROM 3 FOR 2) = NULL
  SUBSTRING('ABC' FROM 3 FOR 2) = 'C'
  SUBSTRING('abcdef' FROM 3 FOR 2) = 'cd'
  ```

  > SQL Server：
  >
  > ```sql
  > SUBSTRING(对象字符串, 截取的起始位置, 截取的字数)
  > ```
  >
  > Oracle和DB2：
  >
  > ```sql
  > SUBSTR(对象字符串, 截取的起始位置, 截取的字数)
  > ```

##### 日期函数

* `CURRENT_DATE`：返回当前日期

  > PostgreSQL和MySQL

  ```sql
  SELECT CURRENT_DATE;
  ```

  > SQL Server:
  >
  > ```sql
  > SELECT CAST(CURRENT_TIMESTAMP AS DATE) AS CUR_DATE;
  > ```
  >
  > Oracle：
  >
  > ```sql
  > SELECT CURRENT_DATE FROM dual; --需要在FROM子句中指定临时表（DUAL）
  > ```
  >
  > DB2：
  >
  > ```sql
  > SELECT CURRENT DATE FROM SYSIBM.SYSDUMMY1; --需要指定临时表SYSIBM.SYSDUMMY1
  > ```

* `CURRENT_TIME`：返回函数执行的时间

  > PostgreSQL和MySQL

  ```sql
  SELECT CURRENT_TIME;
  ```

  > SQL Server:
  >
  > ```sql
  > SELECT CAST(CURRENT_TIMESTAMP AS TIME) AS CUR_TIME;
  > ```
  >
  > Oracle：
  >
  > ```sql
  > SELECT CURRENT_TIMESTAMP FROM dual; --需要在FROM子句中指定临时表（DUAL），结果中包含日期
  > ```
  >
  > DB2：
  >
  > ```sql
  > SELECT CURRENT TIME FROM SYSIBM.SYSDUMMY1; --需要指定临时表SYSIBM.SYSDUMMY1
  > ```

* `CURRENT_TIMESTAMP`：取得当前日期和时间

  > SQL Sever和PostgreSQL以及MySQL

  ```sql
  SELECT CURRENT_TIMESTAMP;
  ```

  > Oracle：
  >
  > ```sql
  > SELECT CURRENT_TIMESTAMP FROM dual; --需要在FROM子句中指定临时表（DUAL）
  > ```
  >
  > DB2：
  >
  > ```sql
  > SELECT CURRENT TIMESTAMP FROM SYSIBM.SYSDUMMY1; --需要指定临时表SYSIBM.SYSDUMMY1
  > ```

* `EXTRACT`：截取日期元素，从日期数据中提取特定部分

  ```SQL
  EXTRACT(日期元素 FROM 日期)
  
  --PostgreSQL MySQL
  SELECT CURRENT_TIMESTAMP,
         EXTRACT(YEAR FROM CURRENT_TIMESTAMP) AS year,
         EXTRACT(MONTH FROM CURRENT_TIMESTAMP) AS month,
         EXTRACT(DAY FROM CURRENT_TIMESTAMP) AS day,
         EXTRACT(HOUR FROM CURRENT_TIMESTAMP) AS hour,
         EXTRACT(MINUTE FROM CURRENT_TIMESTAMP) AS minute,
         EXTRACT(SECOND FROM CURRENT_TIMESTAMP) AS second;
  --SQL Server
  SELECT CURRENT_TIMESTAMP,
         DATEPART(YEAR FROM CURRENT_TIMESTAMP) AS year;
  --Oracle
  SELECT CURRENT_TIMESTAMP,
         EXTRACT(YEAR FROM CURRENT_TIMESTAMP) AS year
    FROM DUAL;
  --DB2
  SELECT CURRENT TIMESTAMP,
         EXTRACT(YEAR FROM CURRENT_TIMESTAMP) AS year
    FROM SYSIBM.SYSDUMMY1;
  ```

##### 转换函数

* 转换包括类型转换`cast`以及值的转换

* `CAST`：类型转换

  ```sql
  CAST(转换前的值 AS 想要转换的数据类型)
  ```

  > 避免插入数据与表中数据类型不匹配，或者运算时数据类型不一致，或避免自动类型转换导致效率低下，需要事前进行数据类型转换。

  ```sql
  --SQL Server + PostgreSQL
  SELECT CAST('0001' AS INTEGER) AS int_col;
  --MySQL
  SELECT CAST('0001' AS SIGNED INTEGER) AS int_col;
  --Oracle
  SELECT CAST('0001' AS INTEGER) AS int_col FROM DUAL;
  --DB2
  SELECT CAST('0001' AS INTEGER) AS int_col FROM SYSIBM.SYSDUMMY1;
  /*执行结果：
  int_col
        1 */
  ```

* `COALESCE`：将NULL值转换为其他值

  ```sql
  COALESCE(数据1, 数据2, 数据3 ...)
  ```

  > COALESCE函数返回可变参数中左侧开始第一个不是NULL的值

  ```sql
  --SQL Server + PostgreSQL MySQL
  SELECT COALESCE(NULL, 1) AS col_1,
         COALESCE(NULL, 'test', NULL) AS col_2;
  --Oracle
  SELECT COALESCE(NULL, 1) AS col_1,
         COALESCE(NULL, 'test', NULL) AS col_2
    FROM DUAL;
  --DB2
  SELECT COALESCE(NULL, 1) AS col_1,
         COALESCE(NULL, 'test', NULL) AS col_2
    FROM SYSIBM.SYSDUMMY1;
  /*执行结果:
  col_1   col_2
      1    test  */
  ```

  * 因此如果想要将列中的NULL值替换， 可以如下：

    ```sql
    SELECT COALESCE(str2, 'NULL') FROM SampleStr;
    --所有空值都会被替换为字符串'NULL'
    ```



### 6.2 谓词

##### 谓词predicate

* LIKE
* BETWEEN
* IS NULL, IS NOT NULL
* IN
* EXISTS

##### LIKE谓词

* LIKE：字符串的部分一致查询

  * `%`：0字符以上的任意字符串
  * `_`：代表任意一个字符

  ```sql
  SELECE * FROM Sample WHERE strcol LIKE 'ddd%'; --前方一致
  SELECE * FROM Sample WHERE strcol LIKE '%ddd%'; --中间一致
  SELECE * FROM Sample WHERE strcol LIKE '%ddd'; --后方一致
  SELECE * FROM Sample WHERE strcol LIKE 'ddd__'; --以ddd开头的5位字符串
  ```
  
* 同理，可以使用NOT LIKE来排除特定结果

##### BETWEEN谓词

* BETWEEN：范围查询

  ```sql
  SELECT product_name, sale_price
    FROM Product
   WHERE sale_price BETWEEN 100 and 1000; --包括100和1000
  ```

  > 如果不想要结果中包含临界值100和1000，就必须使用<和>

##### IS NULL和IS NOT NULL

* 判断是否为空

##### IN谓词

* IN：OR的简便用法

  ```	sql
  SELECT product_name, sale_price
    FROM Product
   WHERE sale_price = 100
      OR sale_price = 500
      OR sale_price = 1000;
  
  SELECT product_name, sale_price
    FROM Product
   WHERE sale_price IN (100, 500, 1000);
  ```

  * 同理，**希望选出不包括的，可以使用`NOT IN`**
  * **IN和NOT IN无法选取NULL数据**，需要用IS NULL或者IS NOT NULL来判断

##### 使用子查询作为IN谓词的参数

* 相较于其他谓词，`IN`可以使用子查询作为其参数

  > 子查询本身是SQL内部生成的表，因此也可以说能够将表和视图作为`IN`的参数

  ```sql
  --例：挑选出店铺id为00C的店出售的商品名和售价
  SELECT product_name, sale_price
    FROM Product
   WHERE product_id IN (SELECT product_id
                          FROM ShopProduct
                         WHERE shop_id = '000C');
  ```

##### NOT IN和子查询

* 语法同上，NOT IN和IN一样可以使用子查询作为参数

##### EXISTS谓词

* EXISTS：判断满足某种条件的记录是否存在

  ```sql
  --例：选取“大版店在售商品的销售单价”
  --查找满足“店铺id为000C”且“Product表与ShopProduct表中商品编号相同”的记录
  SELECT product_id, sale_price
  FROM Product
  WHERE EXISTS (SELECT *
                  FROM ShopProduct
                 WHERE ShopProduct.shop_id = '000C'
                   AND Product.product_id = ShopProduct.product_id);
  ```

  > 子查询中`SELECT *`：由于EXISTS只关心记录是否存在，因此具体返回哪些列都没有关系

* NOT EXISTS

  ```sql
  --例：选取“大版店在售之外的商品的销售单价”
  --排除掉满足“店铺id为000C”且“Product表与ShopProduct表中商品编号相同”的记录
  SELECT product_id, sale_price
  FROM Product
  WHERE NOT EXISTS (SELECT *
                  FROM ShopProduct
                 WHERE ShopProduct.shop_id = '000C'
                   AND Product.product_id = ShopProduct.product_id);
  ```



### 6.3 CASE表达式

##### CASE表达式语法

```sql
CASE WHEN <求值表达式> THEN <表达式>
     WHEN <求值表达式> THEN <表达式>
     WHEN <求值表达式> THEN <表达式>
     ....
     ELSE <表达式>
END
--<求值表达式>中可以使用包括 =, !=, LIKE, BETWEEN等谓词编写的表达式
```

* 如果`<求值表达式>`中的结果为TRUE，则返回THEN子句中的表达式，CASE表达式执行结束
* 如果`<求值表达式>`中的结果不为真，则跳转到下一条WHEN子句的求值中
* 如果到最后的WHEN子句返回结果都不为真，那么返回ELSE中的表达式，执行中止

##### CASE表达式使用

```sql
/*要Product表实现如下结果：
A：衣服
B：办公用品
C：厨房用具*/
SELECT Product_name,
       CASE WHEN product_type = '衣服'
            THEN 'A：' || product_type
            WHEN product_type = '办公用品'
            THEN 'B：' || product_type
            WHEN product_type = '厨房用具'
            THEN 'C：' || product_type
            ELSE NULL
       END AS abc_product_type
  FROM Product;
```

* ELSE子句指定了WHEN子句都不被满足的情况时应该返回的值
* ELSE子句如果省略不写，则会默认为`ELSE NULL`，但最好还是显示写出
* END不该遗漏

##### CASE表达式的书写位置

* CASE表达式可以实现将SELECT输出的行转换成列：

  ```sql
  SELECT product_type
         SUM(sale_price) AS sum_price
    FROM Product
   GROUP BY product_type;
  /* 输出结果为多行：
  product_type sum_price
  衣服               5000
  办公用品             600
  厨房用具           11180 */
  
  SELECT SUM (CASE WHEN product_type = '衣服'
                   THEN sale_price ELSE 0 END) AS sum_price_clothes,
         SUM (CASE WHEN product_type = '厨房用具'
                   THEN sale_price ELSE 0 END) AS sum_price_kitchen,
         SUM (CASE WHEN product_type = '办公用品'
                   THEN sale_price ELSE 0 END) AS sum_price_office,
    FROM Product;
  
  /* 输出结果为多行：
  sum_price_clothes sum_price_kitchen sum_price_office
  5000              600               11180 
  */
  ```

##### 简单CASE表达式

* 将想要的<求值表达式>书写一次后，就无需在之后的WHEN子句中重复书写了

* 但是当想要在WHEN子句中指定不同列时，简单CASE表达式就无能为力

  ```sql
  /*要Product表实现如下结果：
  A：衣服
  B：办公用品
  C：厨房用具*/
  SELECT Product_name,
         CASE product_type
              WHEN '衣服'
              THEN 'A：' || product_type
              WHEN '办公用品'
              THEN 'B：' || product_type
              WHEN '厨房用具'
              THEN 'C：' || product_type
              ELSE NULL
         END AS abc_product_type
    FROM Product;
  ```

##### 特定的CASE表达式

* 部分DBMS提供特有的CASE表达式的简化函数

  > Oracle中的DECODE，MySQL中的IF等

* 但相较于CASE表达式没有什么优势，尽量不要使用



## 集合运算

### 7.1 表的加减法



