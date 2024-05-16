---
title: mysql
date: 2023-09-21 13:16:13
tags:
typora-root-url: ./mysql
hidden: true
---

## Mysql

### 创建数据库

```mysql
/*用于创建数据库*/
CREATE DATABASE 数据库名字;

/*用于删除数据库*/
drop database 数据库名;


```

### 数据类型

**数值类型**

| 类型         | 大小                                     | 范围（有符号）                                               | 范围（无符号）                                               | 用途            |
| :----------- | :--------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :-------------- |
| TINYINT      | 1 Bytes                                  | (-128，127)                                                  | (0，255)                                                     | 小整数值        |
| SMALLINT     | 2 Bytes                                  | (-32 768，32 767)                                            | (0，65 535)                                                  | 大整数值        |
| MEDIUMINT    | 3 Bytes                                  | (-8 388 608，8 388 607)                                      | (0，16 777 215)                                              | 大整数值        |
| INT或INTEGER | 4 Bytes                                  | (-2 147 483 648，2 147 483 647)                              | (0，4 294 967 295)                                           | 大整数值        |
| BIGINT       | 8 Bytes                                  | (-9,223,372,036,854,775,808，9 223 372 036 854 775 807)      | (0，18 446 744 073 709 551 615)                              | 极大整数值      |
| FLOAT        | 4 Bytes                                  | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度 浮点数值 |
| DOUBLE       | 8 Bytes                                  | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
| DECIMAL      | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 | 依赖于M和D的值                                               | 依赖于M和D的值                                               | 小数值          |

**日期和时间**

| 类型      | 大小 ( bytes) | 范围                                                         | 格式                | 用途                     |
| :-------- | :------------ | :----------------------------------------------------------- | :------------------ | :----------------------- |
| DATE      | 3             | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3             | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1             | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8             | '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'               | YYYY-MM-DD hh:mm:ss | 混合日期和时间值         |
| TIMESTAMP | 4             | '1970-01-01 00:00:01' UTC 到 '2038-01-19 03:14:07' UTC结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYY-MM-DD hh:mm:ss | 混合日期和时间值，时间戳 |

**字符串**

| 类型       | 大小                  | 用途                            |
| :--------- | :-------------------- | :------------------------------ |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| VARCHAR    | 0-65535 bytes         | 变长字符串                      |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535 bytes        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

### 创建数据表

```mysql
/*创建数据表*/
CREATE TABLE table_name (column_name column_type);

/*runoob_tbl 创建名称*/
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
    /*表中的键值*/
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

### 删除数据表

```mysql
/*删除表通用*/
DROP TABLE table_name ;
```

### 插入数据

```mysql
INSERT INTO table_name ( field1,field2,...fieldN) VALUES (value1,value2,...valueN);
```

### 查询数据

```mysql
SELECT column_name,column_name
FROM table_name
[WHERE Clause]
[LIMIT N][ OFFSET M]
```

- 查询语句中你可以使用一个或者多个表，表之间使用逗号(,)分割，并使用WHERE语句来设定查询条件。
- SELECT 命令可以读取一条或者多条记录。
- 你可以使用星号（*）来代替其他字段，SELECT语句会返回表的所有字段数据
- 你可以使用 WHERE 语句来包含任何条件。
- 你可以使用 LIMIT 属性来设定返回的记录数。
- 你可以通过OFFSET指定SELECT语句开始查询的数据偏移量。默认情况下偏移量为0。

## MYSQL语法

### WHEAR

```mysql
SELECT field1, field2,...fieldN FROM table_name1, table_name2...
[WHERE condition1 [AND [OR]] condition2.....
```

- 查询语句中你可以使用一个或者多个表，表之间使用逗号**、**分割，并使用WHERE语句来设定查询条件。
- 你可以在 WHERE 子句中指定任何条件。
- 你可以使用 AND 或 OR 指定一个或多个条件。
- WHERE 子句也可以运用于 SQL 的 DELETE 或 UPDATE 命令。
- WHERE子句相当于程序语言中的if条件，根据`MySQL`表中的字段值来读取指定的数据。

### MySQL更新更新

```mysql
UPDATE table_name SET field1 = new-value1, field2 = new-value2
[WHERE 子句]
```

- 您可以同时更新一个或多个字段。
- 你可以在 WHERE 子句中指定任何条件。
- 您可以在一个单独的表中同时更新数据。
- 当你需要更新数据表中指定行的数据时 WHERE 子句是非常有用的。

> UPDATE 就是更新表的数据，名字啊，数据之类的

```mysql
mysql> UPDATE runoob_tbl SET runoob_title='学习 C++' WHERE runoob_id=3;
Query OK, 1 rows affected (0.01 sec)
 
mysql> SELECT * from runoob_tbl WHERE runoob_id=3;
+-----------+--------------+---------------+-----------------+
| runoob_id | runoob_title | runoob_author | submission_date |
+-----------+--------------+---------------+-----------------+
| 3         | 学习 C++   | RUNOOB.COM    | 2016-05-06      |
+-----------+--------------+---------------+-----------------+
1 rows in set (0.01 sec)
```

这个实例演示了如何使用MySQL数据库中的`UPDATE`语句来更新表中的数据。

首先，执行了以下`UPDATE`语句：

```mysql
UPDATE runoob_tbl SET runoob_title='学习 C++' WHERE runoob_id=3;
```

这个语句的含义是：将名为 `runoob_tbl` 的表中 `runoob_id` 等于 3 的行的 `runoob_title` 字段的值更新为 '学习 C++'。在这个例子中，具体的更新操作针对 `runoob_id` 为 3 的那一行。执行这个`UPDATE`语句后，你会看到输出中有一行信息：

```mysql
Query OK, 1 rows affected (0.01 sec)
```

这表示成功执行了更新操作，并影响了1行数据（因为只有一个 `runoob_id` 为 3 的行被更新）。

然后，执行了以下`SELECT`语句：

```mysql
SELECT * from runoob_tbl WHERE runoob_id=3;
```

这个`SELECT`语句用于检查更新后的数据。它选择了 `runoob_tbl` 表中 `runoob_id` 等于 3 的行，并返回了结果：

```diff
+-----------+--------------+---------------+-----------------+
| runoob_id | runoob_title | runoob_author | submission_date |
+-----------+--------------+---------------+-----------------+
| 3         | 学习 C++   | RUNOOB.COM    | 2016-05-06      |
+-----------+--------------+---------------+-----------------+
```

这表明名为 `runoob_tbl` 的表中 `runoob_id` 为 3 的行的 `runoob_title` 字段的值已经成功更新为 `学习 C++`。

### MySQL DELETE 语句

```mysql
DELETE FROM table_name [WHERE Clause]
```

- 如果没有指定 WHERE 子句，`MySQL` 表中的所有记录将被删除。
- 你可以在 WHERE 子句中指定任何条件
- 您可以在单个表中一次性删除记录。

### MySQL LIKE 子句

SQL LIKE 子句中使用百分号 **%**字符来表示任意字符，类似于UNIX或正则表达式中的星号 *****。如果没有使用百分号 **%**, LIKE 子句与等号 **=** 的效果是一样的。

```mysql
SELECT field1, field2,...fieldN 
FROM table_name
WHERE field1 LIKE condition1 [AND [OR]] filed2 = 'somevalue'
```

- 你可以在 WHERE 子句中指定任何条件。
- 你可以在 WHERE 子句中使用LIKE子句。
- 你可以使用LIKE子句代替等号 **=**。
- LIKE 通常与 **%** 一同使用，类似于一个元字符的搜索。
- 你可以使用 AND 或者 OR 指定一个或多个条件。
- 你可以在 DELETE 或 UPDATE 命令中使用 WHERE...LIKE 子句来指定条件。

```mysql
mysql> use RUNOOB;
Database changed
mysql> SELECT * from runoob_tbl  WHERE runoob_author LIKE '%COM';
+-----------+---------------+---------------+-----------------+
| runoob_id | runoob_title  | runoob_author | submission_date |
+-----------+---------------+---------------+-----------------+
| 3         | 学习 Java   | RUNOOB.COM    | 2015-05-01      |
| 4         | 学习 Python | RUNOOB.COM    | 2016-03-06      |
+-----------+---------------+---------------+-----------------+
2 rows in set (0.01 sec)
```

这个`MySQL`查询的作用是从名为 `RUNOOB` 的数据库中的 `runoob_tbl` 表中检索数据，并筛选出满足特定条件的行。具体来说，它使用了`LIKE`操作符来查找 `runoob_author` 字段中以 'COM' 结尾的值的行。

- `use RUNOOB;`: 这个命令用于选择数据库，将当前的数据库上下文切换到名为 `RUNOOB` 的数据库。
- `SELECT * from runoob_tbl`: 这是实际的查询部分，它选择了名为 `runoob_tbl` 的表中的所有字段（使用`*`通配符表示所有字段）。
- `WHERE runoob_author LIKE '%COM'`: 这是查询的筛选条件部分。它使用`LIKE`操作符来匹配 `runoob_author` 字段的值，查找以 'COM' 结尾的文本。`%`是通配符，表示匹配任意字符或字符序列。

查询结果包括两行数据，这两行数据的 `runoob_author` 字段的值都以 'COM' 结尾。在这个特定示例中，这些行可能表示在 `runoob_tbl` 表中具有 "COM" 结尾作者的相关信息。

### MySQL UNION 操作符*

```mysql
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions]
UNION [ALL | DISTINCT]
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions];
```

- **expression1, expression2, ... expression_n**: 要检索的列。
- **tables:** 要检索的数据表。
- **WHERE conditions:** 可选， 检索条件。
- **DISTINCT:** 可选，删除结果集中重复的数据。默认情况下 UNION 操作符已经删除了重复数据，所以 DISTINCT 修饰符对结果没啥影响。
- **ALL:** 可选，返回所有结果集，包含重复数据。

### 演示数据库

我们将使用` RUNOOB `样本数据库。下面是选自 `"Websites" `表的数据：

```
mysql> SELECT * FROM Websites;
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

下面是 `"apps" APP` 的数据：

```
mysql> SELECT * FROM apps;
+----+------------+-------------------------+---------+
| id | app_name   | url                     | country |
+----+------------+-------------------------+---------+
|  1 | QQ APP     | http://im.qq.com/       | CN      |
|  2 | 微博 APP | http://weibo.com/       | CN      |
|  3 | 淘宝 APP | https://www.taobao.com/ | CN      |
+----+------------+-------------------------+---------+
3 rows in set (0.00 sec)
```

### mysql排序

```mysql
SELECT field1, field2,...fieldN FROM table_name1, table_name2...
ORDER BY field1 [ASC [DESC][默认 ASC]], [field2...] [ASC [DESC][默认 ASC]]
```

- 你可以使用任何字段来作为排序的条件，从而返回排序后的查询结果。
- 你可以设定多个字段来排序。
- 你可以使用 ASC 或 DESC 关键字来设置查询结果是按升序或降序排列。 默认情况下，它是按升序排列。
- 你可以添加 WHERE...LIKE 子句来设置条件。









## 数据库设计
