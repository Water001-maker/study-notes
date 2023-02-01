---
typora-copy-images-to: mysql
typora-root-url: mysql
---

# 1-11数据库基础

## 1.数据库操作

### 1.1结构化查询语句分类

| 名称                  | 解释                                         | 命令                    |
| --------------------- | -------------------------------------------- | ----------------------- |
| DDL（数据库定义语句） | 定义和管理数据对象，如数据库，数据表等       | CREATE、DROP、ALTER     |
| DML（数据库操作语句） | 用于操作数据库对象中所包含的数据             | INSERT、UPDATE、DELETE  |
| DQL（数据库查询语句） | 用于查询数据库数据                           | SELECT                  |
| DCL（数据库控制语句） | 用于管理数据库的语句，包括管理权限及数据更改 | GRANT、COMMIT、ROLLBACK |

### 1.2数据库操作

> 命令行操作数据库

创建数据库 :  create database [if not exists] 数据库名;

删除数据库 : drop database [if exists] 数据库名;

查看数据库 : show databases;

使用数据库 : use 数据库名;

> 对比工具操作数据库

**学习方法：**

- 对照SQLyog工具自动生成的语句学习
- 固定语法中的单词需要记忆
- ![image-20230121103256669](/../mysql/image-20230121103256669.png)

### 1.3创建数据表

属于DDL的一种，语法 :

```
create table [if not exists] `表名`(
   '字段名1' 列类型 [属性][索引][注释],
   '字段名2' 列类型 [属性][索引][注释],
  #...
   '字段名n' 列类型 [属性][索引][注释]
)[表类型][表字符集][注释];
```

**说明 :** 反引号用于区别MySQL保留字与普通字符而引入的 (键盘esc下面的键)。

### 1.4数据值和列类型

列类型 : 规定数据库中该列存放的数据类型 

> 数值类型

![image-20230121103532473](/../mysql/image-20230121103532473.png)

> 字符串类型

![image-20230121103543576](/../mysql/image-20230121103543576.png)

> 日期和时间型数值类型

![image-20230121103801345](/../mysql/image-20230121103801345.png)

> NULL值

- 理解为 "没有值" 或 "未知值"
- 不要用NULL进行算术运算 , 结果仍为NULL

### 1.5数据字段属性

**UnSigned**

- 无符号的
- 声明该数据列不允许负数 。

**0填充ZEROFILL**

- 0填充的
- 不足位数的用0来填充 , 如int(3),5则为005

**自增  Auto_InCrement**

- 自动增长的 , 每添加一条数据 , 自动在上一个记录数上加 1(默认)
- 通常用于设置**主键** , 且为整数类型
- 可定义起始值和步长
- - 当前表设置步长(AUTO_INCREMENT=100) : 只影响当前表
  - SET @@auto_increment_increment=5 ; 影响所有使用自增的表(全局)

 **非空  NULL 和 NOT NULL**

- 默认为NULL , 即没有插入该列的数值
- 如果设置为NOT NULL , 则该列必须有值

**默认  DEFAULT**

- 默认的
- 用于设置默认值
- 例如,性别字段,默认为"男" , 否则为 "女" ; 若无指定该列的值 , 则默认值为"男"的值

```mysql
-- 目标 : 创建一个school数据库
-- 创建学生表(列,字段)
-- 学号int 登录密码varchar(20) 姓名,性别varchar(2),出生日期(datatime),家庭住址,email
-- 创建表之前 , 一定要先选择数据库

CREATE TABLE IF NOT EXISTS `student` (
`id` int(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
`name` varchar(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
`pwd` varchar(20) NOT NULL DEFAULT '123456' COMMENT '密码',
`sex` varchar(2) NOT NULL DEFAULT '男' COMMENT '性别',
`birthday` datetime DEFAULT NULL COMMENT '生日',
`address` varchar(100) DEFAULT NULL COMMENT '地址',
`email` varchar(50) DEFAULT NULL COMMENT '邮箱',
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

#### **常用命令**

```mysql
-- 查看数据库的定义
SHOW CREATE DATABASE school;
-- 查看数据表的定义
SHOW CREATE TABLE student;
-- 显示表结构
DESC student;  -- 设置严格检查模式(不能容错了)SET sql_mode='STRICT_TRANS_TABLES';
```

### 1.6数据表的类型

> 设置数据表的类型

```mysql
CREATE TABLE 表名(
   -- 省略一些代码
   -- Mysql注释
   -- 1. # 单行注释
   -- 2. /*...*/ 多行注释
)ENGINE = MyISAM (or InnoDB)

-- 查看mysql所支持的引擎类型 (表类型)
SHOW ENGINES;
```

MySQL的数据表的类型 : **MyISAM** , **InnoDB** , HEAP , BOB , CSV等...

常见的 MyISAM 与 InnoDB 类型：

| 名称       | MyISAM | InnoDB       |
| ---------- | ------ | ------------ |
| 事务处理   | 不支持 | 支持         |
| 数据行锁定 | 不支持 | 支持         |
| 外键约束   | 不支持 | 支持         |
| 全文索引   | 支持   | 不支持       |
| 表空间大小 | 较小   | 较大，约两倍 |

经验 ( 适用场合 )  :

- 适用 MyISAM : 节约空间及相应速度
- 适用 InnoDB : 安全性 , 事务处理及多用户操作数据表

> 数据表的存储位置

- MySQL数据表以文件方式存放在磁盘中

- - 包括表文件 , 数据文件 , 以及数据库的选项文件
  - 位置 : Mysql安装目录\data\下存放数据表 . 目录名对应数据库名 , 该目录下文件名对应数据表 .

- 注意 :

- - \* . frm -- 表结构定义文件

  - \* . MYD -- 数据文件 ( data )

  - \* . MYI -- 索引文件 ( index )

  - InnoDB类型数据表只有一个 *.frm文件 , 以及上一级目录的ibdatal文件

  - MyISAM类型数据表对应三个文件 :

    ![image-20230121105145119](/../mysql/image-20230121105145119.png)

> 设置数据表字符集

我们可为数据库,数据表,数据列设定不同的字符集，设定方法 :

- 创建时通过命令来设置 , 如 : CREATE TABLE 表名()CHARSET = utf8;
- 如无设定 , 则根据MySQL数据库配置文件 my.ini 中的参数设定

### 1.7修改数据库

> 修改表 ( ALTER TABLE )

修改表名 :ALTER TABLE 旧表名 RENAME AS 新表名

添加字段 : ALTER TABLE 表名 ADD 字段名 列属性[属性]

修改字段 :

- ALTER TABLE 表名 MODIFY 字段名 列类型[属性]
- ALTER TABLE 表名 CHANGE 旧字段名 新字段名 列属性[属性]

删除字段 :  ALTER TABLE 表名 DROP 字段名

> 删除数据表

语法：DROP TABLE [IF EXISTS] 表名

- IF EXISTS为可选 , 判断是否存在该数据表
- 如删除不存在的数据表会抛出错误

```
--修改表名: ALTER TABLE 旧表名RENAME AS新表名
ALTER TABLE teacher RENAME AS teacher1
--增加表的字段: ALTER TABLE 表名ADD字段名列属性
ALTER TABLE teacher1 ADD age INT(11)

--修改表的字段 (重命名，修改约束!)
-- ALTER TABLE 表名MODIFY字段名列属性[] 
ALTER TABLE teacher1 MODIFY age VARCHAR(11) -- 修改约東
-- ALTER TABLE 表名CHANGE 旧名字 新名字 列属性[]
ALTER TABLE teacher1 CHANGE age age1 INT(1) -- 字段重名名

--删除表的字段: ALTER TABLE 表名DROP字段名
ALTER TABLE teacher1 DROP age1

--删除表(如果表存在再删除)
DROP TABLE IF EXISTS teacher1
```

所有的创建和删除操作尽量加上判断 IF EXISTS，以免报错

> 其他

1. 可用反引号（`）为标识符（库名、表名、字段名、索引、别名）包裹，以避免与关键字重名！中文也可以作为标识符！

2. 每个库目录存在一个保存当前数据库的选项文件db.opt。

3. 注释：
    单行注释 # 注释内容
    多行注释 /* 注释内容 */
    单行注释 -- 注释内容       (标准SQL注释风格，要求双破折号后加一空格符（空格、TAB、换行等）)
   
4. 模式通配符：
    _   任意单个字符
    %   任意多个字符，甚至包括零字符
    单引号需要进行转义 \'
   
5. CMD命令行内的语句结束符可以为 ";", "\G", "\g"，仅影响显示结果。其他地方还是用分号结束。delimiter 可修改当前对话的语句结束符。

6. SQL对大小写不敏感 （关键字）

7. 清除已有语句：\c

## 2.外键—尽量避免使用

### 2.1外键

#### 外键概念

如果公共关键字在一个关系中是主关键字，那么这个公共关键字被称为另一个关系的外键。由此可见，外键表示了两个关系之间的相关联系。以另一个关系的外键作主关键字的表被称为**主表**，具有此外键的表被称为主表的**从表**。

在实际操作中，将一个表的值放入第二个表来表示关联，所使用的值是第一个表的主键值(在必要时可包括复合主键值)。此时，第二个表中保存这些值的属性称为外键(**foreign key**)。

#### **外键作用**

保持数据**一致性**，**完整性**，主要目的是控制存储在外键表中的数据**约束**。使两张表形成关联，外键只能引用外表中的列的值或使用空值。

#### 创建外键

建表时指定外键约束

```mysql
-- 创建外键的方式一 : 创建子表同时创建外键

-- 年级表 (id\年级名称)
CREATE TABLE `grade` (
`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级ID',
`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
PRIMARY KEY (`gradeid`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

-- 学生信息表 (学号,姓名,性别,年级,手机,地址,出生日期,邮箱,身份证号)
CREATE TABLE `student` (
`studentno` INT(4) NOT NULL COMMENT '学号',
`studentname` VARCHAR(20) NOT NULL DEFAULT '匿名' COMMENT '姓名',
`sex` TINYINT(1) DEFAULT '1' COMMENT '性别',
`gradeid` INT(10) DEFAULT NULL COMMENT '年级',
`phoneNum` VARCHAR(50) NOT NULL COMMENT '手机',
`address` VARCHAR(255) DEFAULT NULL COMMENT '地址',
`borndate` DATETIME DEFAULT NULL COMMENT '生日',
`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
`idCard` VARCHAR(18) DEFAULT NULL COMMENT '身份证号',
PRIMARY KEY (`studentno`),--声明主键
--添加外键
KEY `FK_gradeid` (`gradeid`),
CONSTRAINT `FK_gradeid` 
FOREIGN KEY (`gradeid`) 
REFERENCES `grade` (`gradeid`)
) ENGINE=INNODB DEFAULT CHARSET=utf8
```

建表后修改

```mysql
-- 创建外键方式二 : 创建子表完毕后,修改子表添加外键
ALTER TABLE `student`
ADD CONSTRAINT `FK_gradeid` 
FOREIGN KEY (`gradeid`) 
REFERENCES `grade` (`gradeid`);

/*
ALTER TABLE 表 
ADD CONSTRAINT 约束名 
FOREIGN KEY(作为外健的列) 
REFERENCES 那个表(哪个字段)
*/
```

#### 删除外键

操作：删除 grade 表，发现报错

![image-20230121110612439](/../mysql/image-20230121110612439.png)

**注意** : 删除具有主外键关系的表时 , 要先删子表 , 后删主表

```mysql
-- 删除外键
ALTER TABLE student DROP FOREIGN KEY FK_gradeid;
-- 发现执行完上面的,索引还在,所以还要删除索引
-- 注:这个索引是建立外键的时候默认生成的
ALTER TABLE student DROP INDEX FK_gradeid;


--通过sq1语句删除外键
alter table 表名 drop foreign key 外键名称;
--通过sq1语句添加外键以及级联操作
alter table 表名 add constraint 外键名称(fk_主表_从表)
	foreign key (外键字段名称)
	references 主表 (主键字段)
	on update cascade  --级联修改
	on delete cascade  --级联删除
```

#### 最佳实践

数据库就是单纯的表，用来存储数据，只有行和列（）数据和字段

我们想使用多张表的数据，想使用外键（程序去实现）

有外键时，删除和更新时，还需要考虑外键约束，会导致开发时非常繁琐，测试数据极不方便

### 2.2DML语言

**数据库意义** ： 数据存储、数据管理

**管理数据库数据方法：**

- 通过SQLyog等管理工具管理数据库数据
- 通过**DML语句**管理数据库数据

**DML语言**  ：数据操作语言

- 用于操作数据库对象中所包含的数据
- 包括 :
- - INSERT (添加数据语句)
  - UPDATE (更新数据语句)
  - DELETE (删除数据语句)

### 2.3添加数据

> INSERT命令

**语法：**

```
INSERT INTO 表名[(字段1,字段2,字段3,...)] VALUES('值1','值2','值3')
```

**注意 :** 

- 字段或值之间用英文逗号隔开 .
- ' 字段1,字段2...' 该部分可省略 , 但添加的值务必与表结构,数据列,顺序相对应,且数量一致 .
- 可同时插入多条数据 , values 后用英文逗号隔开 .

```
-- 使用语句如何增加语句?
-- 语法 : INSERT INTO 表名[(字段1,字段2,字段3,...)] VALUES('值1','值2','值3')
INSERT INTO grade(gradename) VALUES ('大一');


-- 主键自增,那能否省略呢?
INSERT INTO grade VALUES ('大二');
-- 查询:INSERT INTO grade VALUE ('大二')错误代码：1136
Column count doesn`t match value count at row 1


-- 结论:'字段1,字段2...'该部分可省略 , 但添加的值务必与表结构,数据列,顺序相对应,且数量一致.
-- 一次插入多条数据
INSERT INTO grade(gradename) VALUES ('大三'),('大四');
```

**练习题目** 

自己使用INSERT语句为课程表subject添加数据 . 使用到外键.

### 2.4修改数据

> update命令

语法：

```
UPDATE 表名 SET column_name=value [,column_name2=value2,...] [WHERE condition];
```

**注意 :** 

- column_name 为要更改的数据列
- value 为修改后的数据 , 可以为变量 , 具体指 , 表达式或者嵌套的SELECT结果
- condition 为筛选条件 , 如不指定则修改该表的所有列数据

> where条件子句

可以简单的理解为 : 有条件地从表中筛选数据

| 运算符   | 含义           | 范围             | 结果  |
| -------- | -------------- | ---------------- | ----- |
| =        | 等于           | 5=6              | false |
| <> 或 != | 不等于         | 5!=6             | true  |
| >        | 大于           | 5>6              | false |
| <        | 小于           | 5<6              | true  |
| >=       | 大于等于       | 5>=6             | false |
| <=       | 小于等于       | 5<=6             | true  |
| BETWEEN  | 在某个范围之间 | BETWEEN 5 AND 10 |       |
| AND      | 并且           | 5 > 1 AND 1 > 2  | false |
| OR       | 或             | 5 > 1 OR 1 > 2   | true  |

测试：

```mysql
-- 修改年级信息
UPDATE grade SET gradename = '高中' WHERE gradeid = 1;
```

### 2.5删除数据

> DELETE命令

语法：

```mysql
DELETE FROM 表名 [WHERE condition];
```

注意：condition为筛选条件 , 如不指定则删除该表的所有列数据

```mysql
-- 删除最后一个数据
DELETE FROM grade WHERE gradeid = 5
```

> TRUNCATE命令

作用：用于完全清空表数据 , 但表结构 , 索引 , 约束等不变 ;

语法：

```mysql
TRUNCATE [TABLE] table_name;

-- 清空年级表
TRUNCATE grade
```

**注意：区别于DELETE命令**

- 相同 : 都能删除数据 , 不删除表结构 , 但TRUNCATE速度更快
- 不同 :
- - 使用TRUNCATE TABLE 重新设置AUTO_INCREMENT计数器
  - 使用TRUNCATE TABLE不会对事务有影响 （事务后面会说）
  - 使用TRUNCATE TABLE会把表删除后，建立一个空表；DELETE是清空原本表中的数据

## 3.DQL语言，使用DQL查询数据

**DQL( Data Query Language 数据查询语言 )**

- 查询数据库数据 , 如**SELECT**语句
- 简单的单表查询或多表的复杂查询和嵌套查询
- 是数据库语言中最核心,最重要的语句
- 使用频率最高的语句

> SELECT语法

```mysql
SELECT [ALL | DISTINCT]
{* | table.* | [table.field1[as alias1][,table.field2[as alias2]][,...]]}
FROM table_name [as table_alias]
  [left | right | inner join table_name2]  -- 联合查询
  [WHERE ...]  -- 指定结果需满足的条件
  [GROUP BY ...]  -- 指定结果按照哪几个字段来分组
  [HAVING]  -- 过滤分组的记录必须满足的次要条件
  [ORDER BY ...]  -- 指定查询记录按一个或多个条件排序
  [LIMIT {[offset,]row_count | row_countOFFSET offset}];
   -- 指定查询的记录从哪条至哪条
```

**注意 : [ ] 括号代表可选的 , { }括号代表必选得**

### 3.1指定查询字段

```mysql
-- 查询表中所有的数据列结果 , 采用 **" \* "** 符号; 但是效率低，不推荐 .

-- 查询所有学生信息
SELECT * FROM student;

-- 查询指定列(学号 , 姓名)
SELECT studentno,studentname FROM student;
```

> AS 子句作为别名

作用：

- 可给数据列取一个新别名
- 可给表取一个新别名
- 可把经计算或总结的结果用另一个新名称来代替

```mysql
-- 这里是为列取别名(当然as关键词可以省略)
SELECT studentno AS 学号,studentname AS 姓名 FROM student;

-- 使用as也可以为表取别名
SELECT studentno AS 学号,studentname AS 姓名 FROM student AS s;

-- 使用as,为查询结果取一个新名字
-- CONCAT()函数拼接字符串
SELECT CONCAT('姓名:',studentname) AS 新姓名 FROM student;
```

> DISTINCT关键字的使用

作用 : 去掉SELECT查询返回的记录结果中重复的记录 ( 返回所有列的值都相同 ) , 只返回一条

```
-- # 查看哪些同学参加了考试(学号) 去除重复项
SELECT * FROM result; -- 查看考试成绩
SELECT studentno FROM result; -- 查看哪些同学参加了考试
SELECT DISTINCT studentno FROM result; -- 了解:DISTINCT 去除重复项 , (默认是ALL)
```

> 使用表达式的列

数据库中的表达式 : 一般由文本值 , 列值 , NULL , 函数和操作符等组成

应用场景 :

- SELECT语句返回结果列中使用

- SELECT语句中的ORDER BY , HAVING等子句中使用

- DML语句中的 where 条件语句中使用表达式

  ```mysql
  -- selcet查询中可以使用表达式
  SELECT @@auto_increment_increment; -- 查询自增步长
  SELECT VERSION(); -- 查询版本号
  SELECT 100*3-1 AS 计算结果; -- 表达式
  
  -- 学员考试成绩集体提分一分查看
  SELECT studentno,StudentResult+1 AS '提分后' FROM result;
  ```

- 避免SQL返回结果中包含 ' . ' , ' * ' 和括号等干扰开发语言程序.

### 3.2where条件语句

作用：用于检索数据表中 符合条件 的记录

搜索条件可由一个或多个逻辑表达式组成 , 结果一般为真或假.

> 逻辑操作符

| 操作符名称 | 语法               | 描述                             |
| ---------- | ------------------ | -------------------------------- |
| AND 或 &&  | a AND b 或 a && b  | 逻辑与，同时为真结果才为真       |
| OR 或 \|\| | a OR b 或 a \|\| b | 逻辑或，只要一个为真，则结果为真 |
| NOT 或 ！  | NOT a 或 ！a       | 逻辑非，若操作数为假，则结果为真 |

测试

```mysql
-- 满足条件的查询(where)
SELECT Studentno,StudentResult FROM result;

-- 查询考试成绩在95-100之间的
SELECT Studentno,StudentResult
FROM result
WHERE StudentResult>=95 AND StudentResult<=100;

-- AND也可以写成 &&
SELECT Studentno,StudentResult
FROM result
WHERE StudentResult>=95 && StudentResult<=100;

-- 模糊查询(对应的词:精确查询)
SELECT Studentno,StudentResult
FROM result
WHERE StudentResult BETWEEN 95 AND 100;

-- 除了1000号同学,要其他同学的成绩
SELECT studentno,studentresult
FROM result
WHERE studentno!=1000;

-- 使用NOT
SELECT studentno,studentresult
FROM result
WHERE NOT studentno=1000;
```

> 模糊查询 ：比较操作符

| 操作符名称  | 语法                   | 描述                                    |
| ----------- | ---------------------- | --------------------------------------- |
| IS NULL     | a is NULL              | 若操作符为NULL，则结果为真              |
| IS NOT NULL | a is NOT NULL          | 若操作符不为NULL，则结果为真            |
| BETWEEN     | a BETWEEN b AND c      | 若a范围在b和c之间，则结果为真           |
| LIKE        | a LIKE b               | SQL模式匹配，若a匹配b，则结果为真       |
| IN          | a IN (a1,a2,a3,......) | 若a是a1，a2，a3中的任意一个，则结果为真 |

注意：

- 数值数据类型的记录之间才能进行算术运算 ;
- 相同数据类型的数据之间才能进行比较 ;

测试：

```mysql
-- 模糊查询 between and \ like \ in \ null

-- =============================================
-- LIKE
-- =============================================
-- 查询姓刘的同学的学号及姓名
-- like结合使用的通配符 : % (代表0到任意个字符) _ (一个字符)
SELECT studentno,studentname FROM student
WHERE studentname LIKE '刘%';

-- 查询姓刘的同学,后面只有一个字的
SELECT studentno,studentname FROM student
WHERE studentname LIKE '刘_';

-- 查询姓刘的同学,后面只有两个字的
SELECT studentno,studentname FROM student
WHERE studentname LIKE '刘__';

-- 查询姓名中含有 嘉 字的
SELECT studentno,studentname FROM student
WHERE studentname LIKE '%嘉%';

-- 查询姓名中含有特殊字符的需要使用转义符号 '\'
-- 自定义转义符关键字: ESCAPE ':'

-- =============================================
-- IN
-- =============================================
-- 查询学号为1000,1001,1002的学生姓名
SELECT studentno,studentname FROM student
WHERE studentno IN (1000,1001,1002);

-- 查询地址在北京,南京,河南洛阳的学生
SELECT studentno,studentname,address FROM student
WHERE address IN ('北京','南京','河南洛阳');

-- =============================================
-- NULL 空
-- =============================================
-- 查询出生日期没有填写的同学
-- 不能直接写=NULL , 这是代表错误的 , 用 is null
SELECT studentname FROM student
WHERE BornDate IS NULL;

-- 查询出生日期填写的同学
SELECT studentname FROM student
WHERE BornDate IS NOT NULL;

-- 查询没有写家庭住址的同学(空字符串不等于null)
SELECT studentname FROM student
WHERE Address='' OR Address IS NULL;
```

### 3.3连接查询

> JOIN 对比

| 操作符名称 | 描述                                       |
| ---------- | ------------------------------------------ |
| INNER JOIN | 如果表中有至少一个匹配，则返回行           |
| LEFT JOIN  | 即使右表中没有匹配，也从左表中返回所有的行 |
| RIGHT JOIN | 即使左表中没有匹配，也从右表中返回所有的行 |

七种Join： 

![image-20230121160029760](/../mysql/image-20230121160029760.png)

测试

```mysql
/*
连接查询
   如需要多张数据表的数据进行查询,则可通过连接运算符实现多个查询
内连接 inner join
   查询两个表中的结果集中的交集
外连接 outer join
   左外连接 left join
       (以左表作为基准,右边表来一一匹配,匹配不上的,返回左表的记录,右表以NULL填充)
   右外连接 right join
       (以右表作为基准,左边表来一一匹配,匹配不上的,返回右表的记录,左表以NULL填充)
       
等值连接和非等值连接

自连接

思路：
(1):分析需求,确定查询的字段来自那些表。（连接查询）
(2):确定使用哪种连接查询?七种
*/

-- 查询参加了考试的同学信息(学号,学生姓名,科目编号,分数)
SELECT * FROM student;
SELECT * FROM result;

/*思路:
(1):分析需求,确定查询的列来源于两个类,student result,（连接查询）
(2):确定使用哪种连接查询?(内连接)
*/
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno

-- 右连接(也可实现)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
RIGHT JOIN result r
ON r.studentno = s.studentno

-- 等值连接
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s , result r
WHERE r.studentno = s.studentno

-- 左连接 (查询了所有同学,不考试的也会查出来)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
LEFT JOIN result r
ON r.studentno = s.studentno

-- 查一下缺考的同学(左连接应用场景)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
LEFT JOIN result r
ON r.studentno = s.studentno
WHERE StudentResult IS NULL

-- 思考题:查询参加了考试的同学信息(学号,学生姓名,科目名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON sub.subjectno = r.subjectno
```

> 自连接

```mysql
/*
自连接
   数据表与自身进行连接

需求:从一个包含栏目ID , 栏目名称和父栏目ID的表中
    查询父栏目名称和其他子栏目名称
*/

-- 创建一个表
CREATE TABLE `category` (
`categoryid` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主题id',
`pid` INT(10) NOT NULL COMMENT '父id',
`categoryName` VARCHAR(50) NOT NULL COMMENT '主题名字',
PRIMARY KEY (`categoryid`)
) ENGINE=INNODB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8

-- 插入数据
INSERT INTO `category` (`categoryid`, `pid`, `categoryName`)
VALUES('2','1','信息技术'),
('3','1','软件开发'),
('4','3','数据库'),
('5','1','美术设计'),
('6','3','web开发'),
('7','5','ps技术'),
('8','2','办公信息');

-- 编写SQL语句,将栏目的父子关系呈现出来 (父栏目名称,子栏目名称)
-- 核心思想:把一张表看成两张一模一样的表,然后将这两张表连接查询(自连接)
SELECT a.categoryName AS '父栏目',b.categoryName AS '子栏目'
FROM category AS a,category AS b
WHERE a.`categoryid`=b.`pid`

-- 思考题:查询参加了考试的同学信息(学号,学生姓名,科目名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON sub.subjectno = r.subjectno

-- 查询学员及所属的年级(学号,学生姓名,年级名)
SELECT studentno AS 学号,studentname AS 学生姓名,gradename AS 年级名称
FROM student s
INNER JOIN grade g
ON s.`GradeId` = g.`GradeID`

-- 查询科目及所属的年级(科目名称,年级名称)
SELECT subjectname AS 科目名称,gradename AS 年级名称
FROM SUBJECT sub
INNER JOIN grade g
ON sub.gradeid = g.gradeid

-- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
```

### 3.4排序和分页

测试

```mysql
/*============== 排序 ================
语法 : ORDER BY
   ORDER BY 语句用于根据指定的列对结果集进行排序。
   ORDER BY 语句默认按照ASC升序对记录进行排序。
   如果您希望按照降序对记录进行排序，可以使用 DESC 关键字。
   
*/

-- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩)
-- 按成绩降序排序
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
ORDER BY StudentResult DESC

/*============== 分页 ================
语法 : SELECT * FROM table LIMIT [offset,] rows | rows OFFSET offset
好处 : (用户体验,网络传输,查询压力)

推导:
   第一页 : limit 0,5
   第二页 : limit 5,5
   第三页 : limit 10,5
   ......
   第N页 : limit (pageNo-1)*pageSzie,pageSzie
   [pageNo:页码,pageSize:单页面显示条数]
   
*/

-- 每页显示5条数据
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
ORDER BY StudentResult DESC , studentno
LIMIT 0,5

-- 查询 JAVA第一学年 课程成绩前10名并且分数大于80的学生信息(学号,姓名,课程名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='JAVA第一学年'
ORDER BY StudentResult DESC
LIMIT 0,10`
```

### 3.5子查询

```mysql
/*============== 子查询 ================
什么是子查询?
   在查询语句中的WHERE条件子句中,又嵌套了另一个查询语句
   嵌套查询可由多个子查询组成,求解的方式是由里及外;
   子查询返回的结果一般都是集合,故而建议使用IN关键字;
*/

-- 查询 数据库结构-1 的所有考试结果(学号,科目编号,成绩),并且成绩降序排列
-- 方法一:使用连接查询
SELECT studentno,r.subjectno,StudentResult
FROM result r
INNER JOIN `subject` sub
ON r.`SubjectNo`=sub.`SubjectNo`
WHERE subjectname = '数据库结构-1'
ORDER BY studentresult DESC;

-- 方法二:使用子查询(执行顺序:由里及外)
SELECT studentno,subjectno,StudentResult
FROM result
WHERE subjectno=(
   SELECT subjectno FROM `subject`
   WHERE subjectname = '数据库结构-1'
)
ORDER BY studentresult DESC;


/*
练习题目:
   查 C语言-1 的前5名学生的成绩信息(学号,姓名,分数)
   使用子查询,查询郭靖同学所在的年级名称
*/
-- 查询课程为 高等数学-2 且分数不小于80分的学生的学号和姓名
-- 方法一:使用连接查询
SELECT s.studentno,studentname
FROM student s
INNER JOIN result r
ON s.`StudentNo` = r.`StudentNo`
INNER JOIN `subject` sub
ON sub.`SubjectNo` = r.`SubjectNo`
WHERE subjectname = '高等数学-2' AND StudentResult>=80

-- 方法二:使用连接查询+子查询
-- 分数不小于80分的学生的学号和姓名
SELECT r.studentno,studentname FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
WHERE StudentResult>=80

-- 在上面SQL基础上,添加需求:课程为 高等数学-2
SELECT r.studentno,studentname FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
WHERE StudentResult>=80 AND subjectno=(
   SELECT subjectno FROM `subject`
   WHERE subjectname = '高等数学-2'
)

-- 方法三:使用子查询
-- 分步写简单sql语句,然后将其嵌套起来
SELECT studentno,studentname FROM student WHERE studentno IN(
   SELECT studentno FROM result WHERE StudentResult>=80 AND subjectno=(
       SELECT subjectno FROM `subject` WHERE subjectname = '高等数学-2'
  )
)
-- 创建一个测试表
CREATE TABLE `test` (
`id` INT(4) NOT NULL AUTO_INCREMENT,
`coll` VARCHAR(20) NOT NULL,
PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

-- 插入几个测试数据
INSERT INTO test(coll) VALUES('row1'),('row2'),('row3');

-- 删除表数据(不带where条件的delete)
DELETE FROM test;
-- 结论:如不指定Where则删除该表的所有列数据,自增当前值依然从原来基础上进行,会记录日志.

-- 删除表数据(truncate)
TRUNCATE TABLE test;
-- 结论:truncate删除数据,自增当前值会恢复到初始值重新开始;不会记录日志.

-- 同样使用DELETE清空不同引擎的数据库表数据.重启数据库服务后
-- InnoDB : 自增列从初始值重新开始 (因为是存储在内存中,断电即失)
-- MyISAM : 自增列依然从上一个自增数据基础上开始 (存在文件中,不会丢失)
```

## 4.MySQL函数——避免使用

### 4.1常用函数

**数据函数**

```mysql
 SELECT ABS(-8);  /*绝对值*/
 SELECT CEILING(9.4); /*向上取整*/
 SELECT FLOOR(9.4);   /*向下取整*/
 SELECT RAND();  /*随机数,返回一个0-1之间的随机数*/
 SELECT SIGN(0); /*符号函数: 负数返回-1,正数返回1,0返回0*/
```

**字符串函数**

```mysql
 SELECT CHAR_LENGTH('狂神说坚持就能成功'); /*返回字符串包含的字符数*/
 SELECT CONCAT('我','爱','程序');  /*合并字符串,参数可以有多个*/
 SELECT INSERT('我爱编程helloworld',1,2,'超级热爱');  /*替换字符串,从某个位置开始替换某个长度*/
 SELECT LOWER('KuangShen'); /*小写*/
 SELECT UPPER('KuangShen'); /*大写*/
 SELECT LEFT('hello,world',5);   /*从左边截取*/
 SELECT RIGHT('hello,world',5);  /*从右边截取*/
 SELECT REPLACE('狂神说坚持就能成功','坚持','努力');  /*替换字符串*/
 SELECT SUBSTR('狂神说坚持就能成功',4,6); /*截取字符串,开始和长度*/
 SELECT REVERSE('狂神说坚持就能成功'); /*反转
 
 -- 查询姓周的同学,改成邹
 SELECT REPLACE(studentname,'周','邹') AS 新名字
 FROM student WHERE studentname LIKE '周%';
```

**日期和时间函数**

```mysql
 SELECT CURRENT_DATE();   /*获取当前日期*/
 SELECT CURDATE();   /*获取当前日期*/
 SELECT NOW();   /*获取当前日期和时间*/
 SELECT LOCALTIME();   /*获取当前日期和时间*/
 SELECT SYSDATE();   /*获取当前日期和时间*/
 
 -- 获取年月日,时分秒
 SELECT YEAR(NOW());
 SELECT MONTH(NOW());
 SELECT DAY(NOW());
 SELECT HOUR(NOW());
 SELECT MINUTE(NOW());
 SELECT SECOND(NOW());
```

**系统信息函数**

```mysql
 SELECT VERSION();  /*版本*/
 SELECT USER();     /*用户*/
```

### 4.2聚合函数

| 函数名称 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| COUNT()  | 返回满足Select条件的记录总和数，如 select count(*) 【不建议使用 *，效率低】 |
| SUM()    | 返回数字字段或表达式列作统计，返回一列的总和。               |
| AVG()    | 通常为数值字段或表达列作统计，返回一列的平均值               |
| MAX()    | 可以为数值字段，字符字段或表达式列作统计，返回最大的值。     |
| MIN()    | 可以为数值字段，字符字段或表达式列作统计，返回最小的值。     |

```mysql
 -- 聚合函数
 /*COUNT:非空的*/
 SELECT COUNT(studentname) FROM student;
 SELECT COUNT(*) FROM student;
 SELECT COUNT(1) FROM student;  /*推荐*/
 
 -- 从含义上讲，count(1) 与 count(*) 都表示对全部数据行的查询。
 -- count(字段) 会统计该字段在表中出现的次数，忽略字段为null 的情况。即不统计字段为null 的记录。
 -- count(*) 包括了所有的列，相当于行数，在统计结果的时候，包含字段为null 的记录；
 -- count(1) 用1代表代码行，在统计结果的时候，包含字段为null 的记录 。
 /*
很多人认为count(1)执行的效率会比count(*)高，原因是count(*)会存在全表扫描，而count(1)可以针对一个字段进行查询。其实不然，count(1)和count(*)都会对全表进行扫描，统计所有记录的条数，包括那些为null的记录，因此，它们的效率可以说是相差无几。而count(字段)则与前两者不同，它会统计该字段不为null的记录条数。
 
 下面它们之间的一些对比：
 
 1）在表没有主键时，count(1)比count(*)快
 2）有主键时，主键作为计算条件，count(主键)效率最高；
 3）若表格只有一个字段，则count(*)效率较高。
 */
 
 SELECT SUM(StudentResult) AS 总和 FROM result;
 SELECT AVG(StudentResult) AS 平均分 FROM result;
 SELECT MAX(StudentResult) AS 最高分 FROM result;
 SELECT MIN(StudentResult) AS 最低分 FROM result;
```

**题目：**

```mysql
 -- 查询不同课程的平均分,最高分,最低分
 -- 前提:根据不同的课程进行分组
 
 SELECT subjectname,AVG(studentresult) AS 平均分,MAX(StudentResult) AS 最高分,MIN(StudentResult) AS 最低分
 FROM result AS r
 INNER JOIN `subject` AS s
 ON r.subjectno = s.subjectno
 GROUP BY r.subjectno
 HAVING 平均分>80;
 
 /*
 where写在group by前面.
 要是放在分组后面的筛选
 要使用HAVING..
 因为having是从前面筛选的字段再筛选，而where是从数据表中的>字段直接进行的筛选的
 */
```

> MD5 加密

**一、MD5简介**

MD5即Message-Digest Algorithm 5（信息-摘要算法5），用于确保信息传输完整一致。是计算机广泛使用的杂凑算法之一（又译摘要算法、哈希算法），主流编程语言普遍已有MD5实现。将数据（如汉字）运算为另一固定长度值，是杂凑算法的基础原理，MD5的前身有MD2、MD3和MD4。

**二、实现数据加密**

新建一个表 testmd5

```mysql
 CREATE TABLE `testmd5` (
  `id` INT(4) NOT NULL,
  `name` VARCHAR(20) NOT NULL,
  `pwd` VARCHAR(50) NOT NULL,
  PRIMARY KEY (`id`)
 ) ENGINE=INNODB DEFAULT CHARSET=utf8
```

插入一些数据

```mysql
 INSERT INTO testmd5 VALUES(1,'kuangshen','123456'),(2,'qinjiang','456789')
```

如果我们要对pwd这一列数据进行加密，语法是：

```mysql
 update testmd5 set pwd = md5(pwd);
```

如果单独对某个用户(如kuangshen)的密码加密：

```mysql
 INSERT INTO testmd5 VALUES(3,'kuangshen2','123456')
 update testmd5 set pwd = md5(pwd) where name = 'kuangshen2';
```

插入新的数据自动加密

```mysql
 INSERT INTO testmd5 VALUES(4,'kuangshen3',md5('123456'));
```

查询登录用户信息（md5对比使用，查看用户输入加密后的密码进行比对）

```mysql
 SELECT * FROM testmd5 WHERE `name`='kuangshen' AND pwd=MD5('123456');
```

### 4.3小结

```mysql
 -- ================ 内置函数 ================
 -- 数值函数
 abs(x)            -- 绝对值 abs(-10.9) = 10
 format(x, d)    -- 格式化千分位数值 format(1234567.456, 2) = 1,234,567.46
 ceil(x)            -- 向上取整 ceil(10.1) = 11
 floor(x)        -- 向下取整 floor (10.1) = 10
 round(x)        -- 四舍五入去整
 mod(m, n)        -- m%n m mod n 求余 10%3=1
 pi()            -- 获得圆周率
 pow(m, n)        -- m^n
 sqrt(x)            -- 算术平方根
 rand()            -- 随机数
 truncate(x, d)    -- 截取d位小数
 
 -- 时间日期函数
 now(), current_timestamp();     -- 当前日期时间
 current_date();                    -- 当前日期
 current_time();                    -- 当前时间
 date('yyyy-mm-dd hh:ii:ss');    -- 获取日期部分
 time('yyyy-mm-dd hh:ii:ss');    -- 获取时间部分
 date_format('yyyy-mm-dd hh:ii:ss', '%d %y %a %d %m %b %j');    -- 格式化时间
 unix_timestamp();                -- 获得unix时间戳
 from_unixtime();                -- 从时间戳获得时间
 
 -- 字符串函数
 length(string)            -- string长度，字节
 char_length(string)        -- string的字符个数
 substring(str, position [,length])        -- 从str的position开始,取length个字符
 replace(str ,search_str ,replace_str)    -- 在str中用replace_str替换search_str
 instr(string ,substring)    -- 返回substring首次在string中出现的位置
 concat(string [,...])    -- 连接字串
 charset(str)            -- 返回字串字符集
 lcase(string)            -- 转换成小写
 left(string, length)    -- 从string2中的左边起取length个字符
 load_file(file_name)    -- 从文件读取内容
 locate(substring, string [,start_position])    -- 同instr,但可指定开始位置
 lpad(string, length, pad)    -- 重复用pad加在string开头,直到字串长度为length
 ltrim(string)            -- 去除前端空格
 repeat(string, count)    -- 重复count次
 rpad(string, length, pad)    --在str后用pad补充,直到长度为length
 rtrim(string)            -- 去除后端空格
 strcmp(string1 ,string2)    -- 逐字符比较两字串大小
 
 -- 聚合函数
 count()
 sum();
 max();
 min();
 avg();
 group_concat()
 
 -- 其他常用函数
 md5();
 default();
```

## 5.事务

> 什么是事务

- 事务就是将一组SQL语句放在同一批次内去执行
- 如果一个SQL语句出错,则该批次内的所有SQL都将被取消执行
- MySQL事务处理只支持InnoDB和BDB数据表类型

事务是关系型数据库的一种机制，目的最终为了保证数据的完整性!

当同时操作多个sq1语句或者是多张表时，那么需要保证数据的一致性(要么同时执行成功，要么同时执行失败！)

需要将业务功能代码(update/add/delete)，需要使用事务进行管理！

> 事务的ACID原则  百度 ACID

### 5.1事务的性质

**原子性(Atomic)**——要么都成功，要么都失败

- 整个事务中的所有操作，要么全部完成，要么全部不完成，不可能停滞在中间某个环节。事务在执行过程中发生错误，会被回滚（ROLLBACK）到事务开始前的状态，就像这个事务从来没有执行过一样。

**一致性(Consist)**——事务前后的数据完整性要保持一致

- 一个事务可以封装状态改变（除非它是一个只读的）。事务必须始终保持系统处于一致的状态，不管在任何给定的时间并发事务有多少。也就是说：如果事务是并发多个，系统也必须如同串行事务一样操作。其主要特征是保护性和不变性(Preserving an Invariant)，以转账案例为例，假设有五个账户，每个账户余额是100元，那么五个账户总额是500元，如果在这个5个账户之间同时发生多个转账，无论并发多少个，比如在A与B账户之间转账5元，在C与D账户之间转账10元，在B与E之间转账15元，五个账户总额也应该还是500元，这就是保护性和不变性。

**隔离性(Isolated)**——事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务, 不能被其他事务的操作数据所干扰，多个并发事务之间要相互隔离。

- 隔离状态执行事务，使它们好像是系统在给定时间内执行的唯一操作。如果有两个事务，运行在相同的时间内，执行相同的功能，事务的隔离性将确保每一事务在系统中认为只有该事务在使用系统。这种属性有时称为串行化，为了防止事务操作间的混淆，必须串行化或序列化请求，使得在同一时间仅有一个请求用于同一数据。

**持久性(Durable)**——事务一旦提交则不可逆，被持久化到数据库中

- 在事务完成以后，该事务对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。

#### 事务的隔离等级

##### **读未提交——read uncommitted**

安全性最差 (一个事务读取到另一个没有提交的事务，会造成脏读)

##### **读已提交——read committed**

相对读未提交安全一些，有效防止脏读，但是会出现不可重复读问题(一个事务当没有提交之前，-般通过update语句，造成前后内容不一致)

##### **可重复读——repeatable read**

能够有效防止脏读，不可重复读问题(mysql的默认隔离级别)

##### **序列化——serializale**

安全性最高,两个事务，当一个事务没有提交，这个事务不能操作其他事情

#### 隔离所导致的一些问题

##### **脏读**

指一个事务读取了另外-个事务未提交的数据。

##### 不可重复读

在一个事务内读取表中的某一行数据，多次读取结果不同。(这个不- 定是错误，只是某些场合不对)

##### 虚读(幻读)

是指在一个事务内读取到了别的事务插入的数据，导致前后读取不一致。

### 5.2基本语法与测试

#### 5.2.1基本语法

```mysql
-- 使用set语句来改变自动提交模式
SET autocommit = 0;   /*关闭*/
SET autocommit = 1;   /*开启*/

-- 注意:
--- 1.MySQL中默认是自动提交
--- 2.使用事务时应先关闭自动提交

-- 开始一个事务,标记事务的起始点
START TRANSACTION  

-- 提交一个事务给数据库
COMMIT

-- 将事务回滚,数据回到本次事务的初始状态
ROLLBACK

-- 还原MySQL数据库的自动提交
SET autocommit =1;

-- 保存点
SAVEPOINT 保存点名称 -- 设置一个事务保存点
ROLLBACK TO SAVEPOINT 保存点名称 -- 回滚到保存点
RELEASE SAVEPOINT 保存点名称 -- 删除保存点
```

#### 5.2.2测试

```mysql
/*
课堂测试题目

A在线买一款价格为500元商品,网上银行转账.
A的银行卡余额为2000,然后给商家B支付500.
商家B一开始的银行卡余额为10000

创建数据库shop和创建表account并插入2条数据
*/

CREATE DATABASE `shop`CHARACTER SET utf8 COLLATE utf8_general_ci;
USE `shop`;

CREATE TABLE `account` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`name` VARCHAR(32) NOT NULL,
`cash` DECIMAL(9,2) NOT NULL,
PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO account (`name`,`cash`)
VALUES('A',2000.00),('B',10000.00)

-- 转账实现
SET autocommit = 0; -- 关闭自动提交
START TRANSACTION;  -- 开始一个事务,标记事务的起始点
UPDATE account SET cash=cash-500 WHERE `name`='A';
UPDATE account SET cash=cash+500 WHERE `name`='B';
COMMIT; -- 提交事务
# rollback;
SET autocommit = 1; -- 恢复自动提交
```

## 6.索引

> MySQL官方对索引的定义为：索引是苍术MySQL高校获取数据的数据结构。
>
> 提取句子主干，就可以得到索引的本质：索引是数据结构

> 索引的作用

- 提高查询速度
- 确保数据的唯一性
- 可以加速表和表之间的连接 , 实现表与表之间的参照完整性
- 使用分组和排序子句进行数据检索时 , 可以显著减少分组和排序的时间
- 全文检索字段进行搜索优化.

### 6.1索引的分类

在一个表中，主键索引只能有一个，唯一索引可以有多个

- 主键索引 (Primary Key)

  唯一的表示，主键不可重复，只能有一个列作为主键

- 唯一索引 (Unique)

  避免重复的列出现，唯一索引可以重复，多个列都可以标识为唯一索引

- 常规索引 (Index)

  默认，index、key关键字来设置

- 全文索引 (FullText)

  在特定的数据库引擎下才有，MyISAM

  快速定位数据

### 6.2基础语法与测试

#### 6.2.1基础语法

```mysql
--索引的使用
--1、在创建表的时候给字段增加索引
-- 2、创建完毕后，增加索引

-- 显示所有的索引信息
SHOW INDEX FROM student

--增加一个全文索引 (索引名) 列名
ALTER TABLE school. student ADD FULLTEXT INDEX 'studentName' ('studentName' ) ;

--EXPLAIN 分析sq1执行的状况

EXPLAIN SELECT * FROM student; --非全文索引

EXPLAIN SELECT * FROM student WHERE MATCH(studentName) AGAINST('刘');
```

##### 主键索引

主键 : 某一个属性组能唯一标识的一条记录

特点 :

- 最常见的索引类型
- 确保数据记录的唯一性
- 确定特定数据记录在数据库中的位置

##### 唯一索引

作用 : 避免同一个表中某数据列中的值重复

与主键索引的区别

- 主键索引只能有一个
- 唯一索引可能有多个

```mysql
CREATE TABLE `Grade`(
  `GradeID` INT(11) AUTO_INCREMENT PRIMARYKEY,
  `GradeName` VARCHAR(32) NOT NULL UNIQUE
   -- 或 UNIQUE KEY `GradeID` (`GradeID`)
)
```

##### 常规索引

作用 : 快速定位特定数据

注意 :

- index 和 key 关键字都可以设置常规索引
- 应加在查询找条件的字段
- 不宜添加太多常规索引,影响数据的插入,删除和修改操作

```mysql
CREATE TABLE `result`(
   -- 省略一些代码
  INDEX/KEY `ind` (`studentNo`,`subjectNo`) -- 创建表时添加
)
-- 创建后添加
ALTER TABLE `result` ADD INDEX `ind`(`studentNo`,`subjectNo`);
```

##### 全文索引

百度搜索：全文索引

作用 : 快速定位特定数据

注意 :

- 只能用于MyISAM类型的数据表
- 只能用于CHAR , VARCHAR , TEXT数据列类型
- 适合大型数据集

```mysql
/*
#方法一：创建表时
  　　CREATE TABLE 表名 (
               字段名1 数据类型 [完整性约束条件…],
               字段名2 数据类型 [完整性约束条件…],
               [UNIQUE | FULLTEXT | SPATIAL ]   INDEX | KEY
               [索引名] (字段名[(长度)] [ASC |DESC])
               );

#方法二：CREATE在已存在的表上创建索引
       CREATE [UNIQUE | FULLTEXT | SPATIAL ] INDEX 索引名
                    ON 表名 (字段名[(长度)] [ASC |DESC]) ;

#方法三：ALTER TABLE在已存在的表上创建索引
       ALTER TABLE 表名 ADD [UNIQUE | FULLTEXT | SPATIAL ] INDEX
                            索引名 (字段名[(长度)] [ASC |DESC]) ;
                                            
#删除索引：DROP INDEX 索引名 ON 表名字;
#删除主键索引: ALTER TABLE 表名 DROP PRIMARY KEY;

#显示索引信息: SHOW INDEX FROM student;
*/

/*增加全文索引*/
ALTER TABLE `school`.`student` ADD FULLTEXT INDEX `studentname` (`StudentName`);

/*EXPLAIN : 分析SQL语句执行性能*/
EXPLAIN SELECT * FROM student WHERE studentno='1000';

/*使用全文索引*/
-- 全文搜索通过 MATCH() 函数完成。
-- 搜索字符串作为 against() 的参数被给定。搜索以忽略字母大小写的方式执行。对于表中的每个记录行，MATCH() 返回一个相关性值。即，在搜索字符串与记录行在 MATCH() 列表中指定的列的文本之间的相似性尺度。
EXPLAIN SELECT * FROM student WHERE MATCH(studentname) AGAINST('love');

/*
开始之前，先说一下全文索引的版本、存储引擎、数据类型的支持情况

MySQL 5.6 以前的版本，只有 MyISAM 存储引擎支持全文索引；
MySQL 5.6 及以后的版本，MyISAM 和 InnoDB 存储引擎均支持全文索引;
只有字段的数据类型为 char、varchar、text 及其系列才可以建全文索引。
测试或使用全文索引时，要先看一下自己的 MySQL 版本、存储引擎和数据类型是否支持全文索引。
*/
```

#### 6.2.2测试索引

**建表app_user：**

```mysql
CREATE TABLE `app_user` (
`id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
`name` varchar(50) DEFAULT '' COMMENT '用户昵称',
`email` varchar(50) NOT NULL COMMENT '用户邮箱',
`phone` varchar(20) DEFAULT '' COMMENT '手机号',
`gender` tinyint(4) unsigned DEFAULT '0' COMMENT '性别（0:男；1：女）',
`password` varchar(100) NOT NULL COMMENT '密码',
`age` tinyint(4) DEFAULT '0' COMMENT '年龄',
`create_time` datetime DEFAULT CURRENT_TIMESTAMP,
`update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='app用户表'
```

**批量插入数据：100w**

```mysql
DROP FUNCTION IF EXISTS mock_data;
DELIMITER $$--写函数之前必须要写，标志

CREATE FUNCTION mock_data()
RETURNS INT
BEGIN
	DECLARE num INT DEFAULT 1000000;
	DECLARE i INT DEFAULT 0;
	
	WHILE i < num DO
	  	INSERT INTO app_user(`name`, `email`, `phone`, `gender`, `password`, `age`)
	   	VALUES(CONCAT('用户', i), '24736743@qq.com', CONCAT('18', FLOOR(RAND()*(999999999-100000000)+100000000)),FLOOR(RAND()*2),UUID(), FLOOR(RAND()*100));
	  	SET i = i + 1;
	END WHILE;
	RETURN i;
END;
SELECT mock_data();
```

**索引效率测试**

无索引

```mysql
SELECT * FROM app_user WHERE name = '用户9999'; --0.993 sec
SELECT * FROM app_user WHERE name = '用户9999'; --1.098sec
SELECT * FROM app_user WHERE name = '用户9999'; --0.788sec

mysql> EXPLAIN SELECT * FROM app_user WHERE name = '用户9999'\G
*************************** 1. row ***************************
          id: 1
select_type: SIMPLE
       table: app_user
  partitions: NULL
        type: ALL
possible_keys: NULL
        key: NULL
    key_len: NULL
        ref: NULL
        rows: 992759
    filtered: 10.00
      Extra: Using where
1 row in set, 1 warning (0.00 sec)
```

创建索引

```mysql
CREATE INDEX idx_app_user_name ON app_user(name);
```

测试普通索引

```mysql
mysql> EXPLAIN SELECT * FROM app_user WHERE name = '用户9999'\G
*************************** 1. row ***************************
          id: 1
select_type: SIMPLE
       table: app_user
  partitions: NULL
        type: ref
possible_keys: idx_app_user_name
        key: idx_app_user_name
    key_len: 203
        ref: const
        rows: 1
    filtered: 100.00
      Extra: NULL
1 row in set, 1 warning (0.00 sec)

mysql> SELECT * FROM app_user WHERE name = '用户9999';
1 row in set (0.001 sec)

mysql> SELECT * FROM app_user WHERE name = '用户9999';
1 row in set (0.001 sec)

mysql> SELECT * FROM app_user WHERE name = '用户9999';
1 row in set (0.001 sec)
```

### 6.3索引准则

- 索引不是越多越好
- 不要对经常变动的数据加索引
- 小数据量的表建议不要加索引
- 索引一般应加在查找条件的字段

> 索引的数据结构

```mysql
-- 我们可以在创建上述索引的时候，为其指定索引类型，分两类
hash类型的索引：查询单条快，范围查询慢
btree类型的索引：b+树，层数越多，数据量指数级增长（我们就用它，因为innodb默认支持它）

-- 不同的存储引擎支持的索引类型也不一样
InnoDB 支持事务，支持行级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
MyISAM 不支持事务，支持表级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
Memory 不支持事务，支持表级别锁定，支持 B-tree、Hash 等索引，不支持 Full-text 索引；
NDB 支持事务，支持行级别锁定，支持 Hash 索引，不支持 B-tree、Full-text 等索引；
Archive 不支持事务，支持表级别锁定，不支持 B-tree、Hash、Full-text 等索引；
```

### 索引·阅读文献(重点)

`http://blog.codinglabs.org/articles/theory-of-mysql-index.html`

## 7.权限管理和备份

### 7.1用户管理

> 使用SQLyog 创建用户，并授予权限演示

![image-20230121165316366](/../mysql/image-20230121165316366.png)

> 基本命令

```mysql
/* 用户和权限管理 */ ------------------
用户信息表：mysql.user

-- 刷新权限
FLUSH PRIVILEGES

-- 增加用户 CREATE USER kuangshen IDENTIFIED BY '123456'
CREATE USER 用户名 IDENTIFIED BY [PASSWORD] 密码(字符串)
  - 必须拥有mysql数据库的全局CREATE USER权限，或拥有INSERT权限。
  - 只能创建用户，不能赋予权限。
  - 用户名，注意引号：如 'user_name'@'192.168.1.1'
  - 密码也需引号，纯数字密码也要加引号
  - 要在纯文本中指定密码，需忽略PASSWORD关键词。要把密码指定为由PASSWORD()函数返回的混编值，需包含关键字PASSWORD

-- 重命名用户 RENAME USER kuangshen TO kuangshen2
RENAME USER old_user TO new_user

-- 设置密码
SET PASSWORD = PASSWORD('密码')    -- 为当前用户设置密码
SET PASSWORD FOR 用户名 = PASSWORD('密码')    -- 为指定用户设置密码

-- 删除用户 DROP USER kuangshen2
DROP USER 用户名

-- 分配权限/添加用户
GRANT 权限列表 ON 表名 TO 用户名 [IDENTIFIED BY [PASSWORD] 'password']
  - all privileges 表示所有权限
  - *.* 表示所有库的所有表
  - 库名.表名 表示某库下面的某表

-- 查看权限   SHOW GRANTS FOR root@localhost;
SHOW GRANTS FOR 用户名
   -- 查看当前用户权限
  SHOW GRANTS; 或 SHOW GRANTS FOR CURRENT_USER; 或 SHOW GRANTS FOR CURRENT_USER();

-- 撤消权限
REVOKE 权限列表 ON 表名 FROM 用户名
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 用户名    -- 撤销所有权限
```

> 权限解释

```mysql
-- 权限列表
ALL [PRIVILEGES]    -- 设置除GRANT OPTION之外的所有简单权限
ALTER    -- 允许使用ALTER TABLE
ALTER ROUTINE    -- 更改或取消已存储的子程序
CREATE    -- 允许使用CREATE TABLE
CREATE ROUTINE    -- 创建已存储的子程序
CREATE TEMPORARY TABLES        -- 允许使用CREATE TEMPORARY TABLE
CREATE USER        -- 允许使用CREATE USER, DROP USER, RENAME USER和REVOKE ALL PRIVILEGES。
CREATE VIEW        -- 允许使用CREATE VIEW
DELETE    -- 允许使用DELETE
DROP    -- 允许使用DROP TABLE
EXECUTE        -- 允许用户运行已存储的子程序
FILE    -- 允许使用SELECT...INTO OUTFILE和LOAD DATA INFILE
INDEX     -- 允许使用CREATE INDEX和DROP INDEX
INSERT    -- 允许使用INSERT
LOCK TABLES        -- 允许对您拥有SELECT权限的表使用LOCK TABLES
PROCESS     -- 允许使用SHOW FULL PROCESSLIST
REFERENCES    -- 未被实施
RELOAD    -- 允许使用FLUSH
REPLICATION CLIENT    -- 允许用户询问从属服务器或主服务器的地址
REPLICATION SLAVE    -- 用于复制型从属服务器（从主服务器中读取二进制日志事件）
SELECT    -- 允许使用SELECT
SHOW DATABASES    -- 显示所有数据库
SHOW VIEW    -- 允许使用SHOW CREATE VIEW
SHUTDOWN    -- 允许使用mysqladmin shutdown
SUPER    -- 允许使用CHANGE MASTER, KILL, PURGE MASTER LOGS和SET GLOBAL语句，mysqladmin debug命令；允许您连接（一次），即使已达到max_connections。
UPDATE    -- 允许使用UPDATE
USAGE    -- “无权限”的同义词
GRANT OPTION    -- 允许授予权限

/* 表维护 */

-- 分析和存储表的关键字分布
ANALYZE [LOCAL | NO_WRITE_TO_BINLOG] TABLE 表名 ...
-- 检查一个或多个表是否有错误
CHECK TABLE tbl_name [, tbl_name] ... [option] ...
option = {QUICK | FAST | MEDIUM | EXTENDED | CHANGED}
-- 整理数据文件的碎片
OPTIMIZE [LOCAL | NO_WRITE_TO_BINLOG] TABLE tbl_name [, tbl_name] ...
```

### 7.2MySQL备份

数据库备份必要性

- 保证重要数据不丢失
- 数据转移

MySQL数据库备份方法

- 直接拷贝物理文件
- 使用命令手动导出，mysqldump备份工具（在想要导出的表或数据库中，右键，选择备份导出
- 数据库管理工具,如SQLyog
- 直接拷贝数据库文件和相关配置文件（data)

**mysqldump客户端**

作用 :

- 转储数据库
- 搜集数据库进行备份
- 将数据转移到另一个SQL服务器,不一定是MySQL服务器

![image-20230121165920940](/../mysql/image-20230121165920940.png)

```mysql
-- 导出
1. 导出一张表 -- mysqldump -uroot -p123456 school student >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表名 > 文件名(D:/a.sql)
2. 导出多张表 -- mysqldump -uroot -p123456 school student result >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表1 表2 表3 > 文件名(D:/a.sql)
3. 导出所有表 -- mysqldump -uroot -p123456 school >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 > 文件名(D:/a.sql)
4. 导出一个库 -- mysqldump -uroot -p123456 -B school >D:/a.sql
　　mysqldump -u用户名 -p密码 -B 库名 > 文件名(D:/a.sql)

可以-w携带备份条件

-- 导入
1. 在登录mysql的情况下：-- source D:/a.sql
　　source 备份文件
2. 在不登录的情况下
　　mysql -u用户名 -p密码 库名 < 备份文件
```

## 8.规范数据库设计

### 8.1为什么需要数据库设计

**当数据库比较复杂时我们需要设计数据库**

**糟糕的数据库设计 :** 

- 数据冗余,存储空间浪费
- 数据更新和插入的异常
- 程序性能差

**良好的数据库设计 :** 

- 节省数据的存储空间
- 能够保证数据的完整性
- 方便进行数据库应用系统的开发

 **软件项目开发周期中数据库设计 :**

- 需求分析阶段: 分析客户的业务和数据处理需求
- 概要设计阶段:设计数据库的E-R模型图 , 确认需求信息的正确和完整.

**设计数据库步骤**

- 收集信息，分析需求
- - 用户表(用户登录注销，用户的个人信息，写博客，创建分类)
  - 分类表（文章分类，谁创建的）
  - 文章表（文章的信息）
  - 友情链接表（友情链接的信息）
  - 自定义表（系统信息，某个关键的字，或者一些主字段）
  - 与该系统有关人员进行交流 , 座谈 , 充分了解用户需求 , 理解数据库需要完成的任务。
- 标识实体（把需求落地到每个字段）
- 标识实体之间的关系[Relationship]
- 在取得成绩的基础上，什么要继续什么创造新的台阶啊之类的标识数据库要管理的关键对象或实体,实体一般是名词
- 标识每个实体需要存储的详细信息[Attribute]

**常见的系统**

bbs（论坛）

crm（管理系统）

前段

### 8.2三大范式

**问题 : 为什么需要数据规范化?**

不合规范的表设计会导致的问题：

- 信息重复
- 更新异常
- 插入异常
- 无法正确表示信息
- 删除异常
- 丢失有效信息

> 三大范式（了解）

**第一范式 (1st NF)**

第一范式的目标是确保每列的原子性,如果每列都是不可再分的最小数据单元,则满足第一范式

**第二范式(2nd NF)**

第二范式（2NF）是在第一范式（1NF）的基础上建立起来的，即满足第二范式（2NF）必须先满足第一范式（ 1NF）。

第二范式要求每个表只描述一件事情

**第三范式(3rd NF)**

如果一个关系满足第二范式,并且除了主键以外的其他列都不传递依赖于主键列,则满足第三范式.

第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。

**规范化和性能的关系**（关联查询的表不能超过三张）

为满足某种商业目标 , 数据库性能比规范化数据库更重要

在数据规范化的同时 , 要综合考虑数据库的性能

通过在给定的表中添加额外的字段,以大量减少需要从中搜索信息所需的时间

通过在给定的表中插入计算列,以方便查询（故意增加一些计算列）

## 9.JDBC

### 9.1数据库驱动和JDBC

需要导入一个数据库驱动包 mysql-connector-java-8.0.***.jar

### 9.2第一个JDBC程序

#### 第一步：创建测试数据库

```mysql
CREATE DATABASE jdbcstudy CHARACTER SET utf8 COLLATE utf8_ general_ ci;

USE jdbcstudy;

CREATE TABLE users(
id INT PRIMARY KEY,
NAME VARCHAR(40)，
PASSWORD VARCHAR(40) ，
emai1 VARCHAR(60) ，
birthday DATE
);

INSERT INTO users(id, NAME , PASSWORD，emai1 ,bi rthday)
VALUES
(1, ' zhansan'，' 123456' , ' zs@sina.com'，'1980-12-04')，
(2,'lisi'，'123456', '1isi@sina.com', '1981-12-04'),
(3, 'wangwu' ,'123456','wangwu@sina. com',' 1979-12-04');|
```

#### 第二步：导入数据库驱动

#### 第三部：创建一个普通项目

1.加载驱动

2.链接数据库DriverManager

3.获得执行sql的对象Statement

4.获得返回的结果集

5.释放链接

```java
package com.kuang.lesson01;
//我的第一个JDBC程序
public class JdbcFirstDemo.{
    public static void main(String[] args) throws ClassNotFoundException {
        //1.加载驱动
        Class . forName( " com. mysq1. jdbc .Driver"); //固定写法,加载驱动
        
        //2.用户信息和url
        // useUnicode-true&characterEncoding=utf8&useSSL=true
        String url = "jdbc :mysq1://localhost: 3306/jdbcstudy？useUnicode=true&characterEncoding=utf8&useSSL=true";
        String username = "root" ;
        String password = "123456";
    
        //3.链接成功，数据库对象
        Connection connection = DriverManager.getConnection(url,username,password);
    
        //4.执行SQL的对象Statment 执行sql的对象
        Statement statement = connection.createStatement();
    
        //5.执行SQL的对象 去 执行SQL，可能存在结果，查看返回结果
        String sql = "SELECT * FROM users";
    
        ResultSet resultSet = sataement.executeQuery(sql);//返回的结果集，结果集中封装了我们全部的查询出来的结果
   
        while (resultSet.next()){
            System. out. print1n("1d=" + resultSet . getObject( columnLabel: "1d"));
            System. out. print1n("name=" + resultSet . getobject( columnLabel: "NAME"));
            System. out . print1n("pwd=" + resultSet . getObject( columnLabel: "PASSWORD" ));
            System. out. print1n("email=" + resultSet. getObject( columnLabel: "email"));
            System. . out. print1n("birth=" + resultSet . get0bject( columnLabel: "birthday")); 
            
        }
    
        //6.释放链接
        resultSet.close();
        statement.close();
        connection.close();
    }
}
```

> DriverManager

```java
//DriverManager.registerDriver(new.com.mysql.jdbc.Driver());
Class . forName( " com. mysq1. jdbc .Driver"); //固定写法，加载驱动
Connection connection = DriverManager.getConnection(url,username,password);

//connection 代表数据库
//数据库设置自动提交
//事务提交
//食物滚回
```

> URL

```java
String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=true";

//mysql -- 3306
//协议：//主机地址：端口号/数据库名？参数1&参数2&参数3

//oralce -- 1521
////jdbc:oracle:thin:@localhost:1521:sid
```

> Statement执行SQL的对象 prepareStatement执行的SQL的对象

```java
String sql = "SELECT * FROM users";//编写SQL


statement. executeQuery(); // 查询操作返回Resultset
statement. execute(); //执行任何SQL
statement. executeupdate(); // 更新、插入、删除。都是用这个，返回一个受影响的行劃
```

> ResultSet查询的结果集：封装了所有的查询结果

获得指定的数据类型

```java
resultset . getobject(); //在不知道列类型的情况下使用
//如果知道列的类型就使用指定的类型
resu1tSet. getstring();
resu1tset. getInt():
resultset. getFloat();
resu1tset. getDate();
resu1tset. getobject();
```

遍历，指针

```java
resultset.beforeFirst(): // 移动到最房面
resultSet.afterLast(); // 移动到最后两
resultSet.next(): //移动到下一“个数弟
resultSet. previousO); //移动到前一-行
resultset. absolute(row); //移动到指定行
```

> 释放资源必须完成

```java
resultSet.close();
statement.close();
connection.close();
```

### 9.3statement对象

jdbc中的statement对象用于向数据库发送SQL语句，想完成对数据库的增删改查，只需要通过这个对象向数据库发送增删改查语句即可。

Sataement对象的executeUpdate方式，用于向数据库发送增删改的sql语句，executeUpdate执行后，将会返回一个整数（即增删改语句导致了数据库几行数据发生了变化）。

Statement.executeQuery方法用于向数据库查询语句，executeQuery方法返回代表查询结果的ResultSet对象。

> CRUE操作-create

使用executeUpdate(String sql)方法完成数据添加操作，示例操作：

```java
Statement st = com.createStatement();
String sql = "insert into user(…) valuse(…)";
int num = st.executeUpdate(sql);
if(num>0){
    System.out.println("插入成功！");
}
```

> CRUE操作-delete

使用executeUpdate(String sql)方法完成数据删除操作，示例操作：

```java
Statement st = com.createStatement();
String sql = "delete from user where id=1";
int num = st.executeUpdate(sql);
if(num>0){
    System.out.println("删除成功！");
}
```

> CRUE操作-update

使用executeUpdate(String sql)方法完成数据修改操作，示例操作：

```java
Statement st = com.createStatement();
String sql = "update user set name='1' where name='2'"";
int num = st.executeUpdate(sql);
if(num>0){
    System.out.println("修改成功！");
}
```

> CRUE操作-read

使用executeQuery(String sql)方法完成数据查询操作，示例操作：

```java
Statement st = com.createStatement();
String sql = "select * from user where id=1";
ResultSet rs = st.executeQuery(sql);
if(rs.next()){
    //根据获取的数据类型，分别调用rs的相应方法映射到java对象中
    System.out.println("插入成功！");
    
}
```

> 代码实现

#### 9.3.1提取工具类

创建db.propertise(配置文件)

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=true
username=root
password=123456
```

创建jdbcUtils.java文件

```java
import java. io. IOException;
import java. io. InputStream;
import java.sq1. *;
import java. util. Properties;

public class Jdbcutils {

    private static string driver = nu11;
    private static string ur1 = nu11;
    private static string username = nu11;
    private static string password = nul1;

    static {
        try{
            Inputstream in = Jdbcutils. class. getClassLoader() . getResourceAsstream(" db. properties");
            Properties properties = new Properties();
            properties.load(in);
    
            driver = properties . getProperty("driver");
            ur1 = properties . getproperty("ur1");
            username = properties . getProperty("username");
            password = properties. getProperty("password");

            //1.驱动只用加载一次I
            Class . forName (driver);
        } catch (Exception e) {
            e. printstackTrace();                       
        }
    }
    
    //获取连接
    pub1ic static Connection getConnection() throws SQL Exception {
        return Drivermanager . getConnecti on(ur1，username, password);
    }
    
    //释放连接资源
    pub1ic static void release (Connection conn， Statement st， Resultset rs){
        if (rs!=nu11){
            try {
                rs.c1ose();
            } catch (SQLException e) {
                e. printStackTrace();
            }
        }
        if (st!=nu11){
            try {
                st.close();
            } catch (SQLException e) {
                e. printStackTrace O);
            }
        }
        if (conn!=nu11){
            try {
                conn. close();
            } catch (SQLException e) {
                e. printstackTrace();
            }
        }
    }
    
}
```

#### 9.3.2编写增删改方法(executeUpdate)

##### 增加

```java
import java. sq1. Connection;
import java. sq1. Resu1tSet;
import java. sq1. sQLException; 
import java. sq1. Statement;

public class TestInsert {
    public static void main(String[] args) {
        
        Connection conn = nu11;
        statement st = nu11;
        Resultset rs = nu11;
       
        try {
            conn = Jdbcutils. getConnection(); //获取 数据库连接
            st = conn. createstatement(); //获得SQL的执行对象
           
            String sq1 = "IWSERT INTO users(id,' NAME' ，PASSWORD^ ，email' ，birthday )" +
"VALUES(4, ' kuangshen' , '123456'，'24736743@qq. com' , ' 2020-01-01')";
            int i = st. executeupdate (sq1);
            if (i>0){
                System. out. print1n("插入成功! ");
            }
            
        } catch (SQLException e) {
            e.printstackTrace();
        } finally {
            IdbcUtils. release(conn,s,rs);
        }   
    }
}
```

##### 删

```java
import java. sq1. Connection;
import java. sq1. Resu1tSet;
import java. sq1. sQLException; 
import java. sq1. Statement;

public class TestInsert {
    public static void main(String[] args) {
        
        Connection conn = nu11;
        statement st = nu11;
        Resultset rs = nu11;
       
        try {
            conn = Jdbcutils. getConnection(); //获取 数据库连接
            st = conn. createstatement(); //获得SQL的执行对象
           
            String sq1 = "DELETE FROM users WHERE id = 4";
            int i = st. executeupdate (sq1);
            if (i>0){
                System. out. print1n("删除成功! ");
            }
            
        } catch (SQLException e) {
            e.printstackTrace();
        } finally {
            IdbcUtils. release(conn,st,rs);
        }   
    }
}
```

##### 改

```java
import java. sq1. Connection;
import java. sq1. Resu1tSet;
import java. sq1. sQLException; 
import java. sq1. Statement;

public class TestInsert {
    public static void main(String[] args) {
        
        Connection conn = nu11;
        statement st = nu11;
        Resultset rs = nu11;
       
        try {
            conn = Jdbcutils. getConnection(); //获取 数据库连接
            st = conn. createstatement(); //获得SQL的执行对象
           
            String sq1 = "UPDATE users SET NAME`='kuangshen',`email`='12345679@qq.com' WHERE id = 1";
            
            int i = st. executeupdate (sq1);
            if (i>0){
                System. out. print1n("修改成功! ");
            }
            
        } catch (SQLException e) {
            e.printstackTrace();
        } finally {
            IdbcUtils. release(conn,st,rs);
        }   
    }
}
```

#### 9.3.3编写查方法(executeQuery)

```java
public class TestSelect{
    public static void mian(String[] args){
        
        Connection conn = nu11;
        statement st = nu11;
        Resultset rs = nu11;
        
        try{
            conn = jdbcUtils.getConnection();
            
            st =  coon.createStaement();
            
            //SQL
            String sql = "SELECT * FEOM users where id = 1";
            
            st.executeQuery();//查询
            
            rs = st. executeupdate (sq1);//查询完毕会返回一个结果集
            
            if(rs.next()){
                System.out.println(rs.getString("NAME"));
            }
            
        }catch(SQLException e){
            e.printstackTrace();
        }finally{
            jdbcUtils.release(conn,st,rs);
        }        
    }
}
```

### 9.4SQL注入的问题

```java
import ...
    
public class SQL注入{
    public static void main(String[] args) {

     // login( "kuangshen", "123456");//正常的
        login( ''or '1=1", ''or'1=1"); //有问题的
    }

    //许录业务
    public static void 1ogin(String username, String password){
        Connection conn =nu1l;
        Statement st = null;
        ResultSet rs = nul1;
        try {
            conn=JdbcUtils . getConnection();
            st=conn. createStatement();
            
            // SELECT * FROM users WHERE ` Name` = ' kuangshen' AND `password' = '123456';
            // SELECT * FROM users. WHERE`Name` =' ' or '1=1' AND `password` = ' ' or '1=1';
            String sql ="select * from users where `NAME` =' " +username+"' AND `password` = '"+password+"';
                
                rs. = st.executeQuery(sq1); //查的完生会返回一个结果集
                while. (rs.next()){
                    System. out . print1n(rs . getString( columnLabel:"NAME"));
                    System. out . print1n(rs. getString( columnLabel: "password"));
                    System. out . println("=======================");
                }
                    
        } catch (SQLException e){
            e.printStackTrace0);  
        }finally{
            Jdbcutils . release(conn,st,rs);        
        }
    }
}
```

sql存在漏洞，会导致数据泄露

本质：SQL会被拼接 or 

#### 解决方法

SQL注入问题

```java
import ...
    
public class SQL注入{
    public static void main(String[] args) {

     // login( "kuangshen", "123456");//正常的
        login( ''or '1=1", ''or'1=1"); //有问题的
    }

    //许录业务
    public static void 1ogin(String username, String password){
        Connection conn =nu1l;
        PerpareStatement st = null;
        ResultSet rs = nul1;
        try {
            conn=JdbcUtils . getConnection();
            st=conn. createStatement();
            
            String sql ="select * from users where `NAME` = ? and `PASSWORD` = ? ";//Mybatis框架
            
            st = conn.perpareStatement(sql);
            st.setString(1,username);
            st.setString(2,password);
                
            
            rs = st.executeQuery(sq1); //查的完生会返回一个结果集
                while. (rs.next()){
                    System. out . print1n(rs . getString( columnLabel:"NAME"));
                    System. out . print1n(rs. getString( columnLabel: "password"));
                    System. out . println("=======================");
                }
                    
        } catch (SQLException e){
            e.printStackTrace0);  
        }finally{
            Jdbcutils . release(conn,st,rs);        
        }
    }
}
```

PrepareStatement放置SQL注入的本质，吧传递进来的参数当做字符。

假设你在转义字符，就直接忽略，’ ‘会直接转义

可以防止SQL注入，并且效率更高

### 9.5PrepareStatement对象

#### **增加**

```java
import java.sql . Connection;
import java. util.Date;
import java . sq1. PreparedStatement;
import java . sql . SQLException;

public class TestInsert {
    public static void main(String[] args) {
        Connection conn = nu11;
        PreparedStatement st = null;

        try {
            conn = JdbcUtils. getConnection();
            //区别
            //使用?占位符代替参数
            String sql = "insert into users(id," NAME , PASSWORD' , email' , birthday' ) values(?,?,?,?,?)"; 
                
                st = conn. prepareStatement(sq1); // 预编iISQL,先写sqL, 然后不执行
            //手动给参数赋值
            st. setInt(  1,  4); //id
            st. setString(  2,  "qinjiang");//name
            st. setString(  3, "1232112");//password
            st. setString(  4,  " 24734673@qq.com");//email
            //注意点:sql.Date 数据库java. sql . Date()
            //util.Date Java new Date(). getTime()获得时间裁
            st.setDate( parameterIndex: 5,new java. sql .Date (new Date().getTime()));//生日
            
            //执行
            int i = st. executeUpdate();
            if(i>0){
                System.out,println("插入成功！");
            }
        
        } catch (SQLException e) {
            e. printStackTrace();
        }finally{
            JdbcUtils.release(com,st,null);
        }
    }
}
```

#### **删除**

```java
import java.sql . Connection;
import java. util.Date;
import java . sq1. PreparedStatement;
import java . sql . SQLException;

public class TestInsert {
    public static void main(String[] args) {
        Connection conn = nu11;
        PreparedStatement st = null;

        try {
            conn = JdbcUtils. getConnection();
            //区别
            //使用?占位符代替参数
            String sql = "delete from users where id=?"; 
                
                st = conn. prepareStatement(sq1); // 预编iISQL,先写sqL, 然后不执行
            //手动给参数赋值
            st.setLnt(1,4)//id

            //执行
            int i = st. executeUpdate();
            if(i>0){
                System.out,println("删除成功！");
            }
        
        } catch (SQLException e) {
            e. printStackTrace();
        }finally{
            JdbcUtils.release(com,st,null);
        }
    }
}
```

#### 修改

```java
import java.sql . Connection;
import java. util.Date;
import java . sq1. PreparedStatement;
import java . sql . SQLException;

public class TestInsert {
    public static void main(String[] args) {
        Connection conn = nu11;
        PreparedStatement st = null;

        try {
            conn = JdbcUtils. getConnection();
            //区别
            //使用?占位符代替参数
            String sql = "update users set `Name` = ? where id = ? "; 
                
            //手动给参数赋值
            set.setString(1,"薛淼");//name
            set.setInt(2,1);//id

            //执行
            int i = st. executeUpdate();
            if(i>0){
                System.out,println("修改成功！");
            }
        
        } catch (SQLException e) {
            e. printStackTrace();
        }finally{
            JdbcUtils.release(com,st,null);
        }
    }
}
```

#### **查询**

```java
import java. sql.Connection;
import java . sq1. PreparedStatement;
import java. sq1. ResultSet;
import java. sq1. SQLException;

public class Testselect {
    pub1ic static void main(String[] args) {
       
        Connection conn = nu11;
        Preparedstatement st = nu11;
        Resultset rs = nu11;
        
        try {
            conn = JdbcUtils. getConnection();
            
            String sql = "select * from `users` where `id` = ?";
            
            st = conn.perpareStatement(sql);
            
            setInt(1,1);//传递参数
            
            rs = st.executeQuery();//执行
            
            if(rs.next()){
                System.out,println(rs.getString("NAME"));
            }finally{
                JdbcUtils.release(com,st,rs);
            }
            
        } catch (SQLException e) {
            e. printstackTrace();
        }
    }
}
```

## 10连接池

```
1.什么是连接池
 是一种容器(集合),存储数据库连接对象;防止用户操作数据库的时候频繁,创建连接对象,而且当数据库连接Connection使用完毕,就释放close()---->消耗内存空间!
 提供固定的可重用的连接对象的容器

 连接池的技术:
        c3p0
        dbcp
        druid(推荐)  德鲁伊   阿里的开源项目
        ...
   连接池本质就是需要实现sun公司提供的DataSource实现类(物理数据源代替 DriverManager)
   1)导包 druid-1.1.10.jar
                        druid-1.1.10.source.jar:原码包(下载看DruidDataSource原码)

          DruidDataSource 实现类


          需要数据库驱动包:连接数据库mysql-connector-java-8.0.23.jar


   2)准备好数据库连接池的配置文件
        配置文件以.properties :
        druid.properties
        配置文件名称可以任意命名,但是必须在src目录下


   3)读取配置文件
    如何创建DataSource接口对象?

          DruidDataSource-------> 此时会使用工厂模式

          DruidDataSourceFactory工厂类
            提供静态功能:创建数据源DruidDataSource
            public static DataSource createDataSource(Properties properties) throws Exception
```

## 11语法总结

### DDL语句

#### 数据库相关

**连接数据库**

```
mysql -h10.20.66.32 -uroot -p123456
```

**查看数据库**

```
show 库名
```

#### 表相关

**查看表**

```
show tables;
```

**查看表结构**

```
desc 表名
```

### DML语句

#### **建表**

```
语法 create table 表名称(
	字段名 字段类型 字段描述符，
	字段名 字段类型 字段描述符);
```

#### **删表**

```
drop table 表名;
```

#### **改表**

**添加字段**

```
alter table 表名 add(字段名 字段类型 字段描述);
```

**移除字段**

```
alter table 表名 drop column 字段名;
```

**变更字段**

```
alter table 表名 change 旧名 新名 字段描述;
```

#### 插入

**全字段插入**

```
insert into 表名 values(001,'zww'),(002,'rs');
```

**插入个别**

```
insert into winton values(001,'zww');
```

#### 删除

```
delete from 表名 where 改的地方;
```

#### 更新

```
update 表名 set 改的内容 where 在哪儿
```

# 12.数据库引擎

## MyIsm引擎

对数据库加锁（MyIsm只能支持到表锁），将表中的数据一个一个进行修改，当第99条改完才改第100条（对数据库的请求过多时，巨慢无比），且底层的数据结构为二叉树

## InnDB引擎

5.0之后，不仅支持表锁，还支持行锁，当第99条正在修改时，另一个线程会同时修改第100条数据（且在修改的同时会将此条数据锁住，而不影响其他数据，因此安全性也得到了保障），效率得到了极大的提升；且底层的数据结构为红黑树

# 13MySQL框架

## mysql的卸载

### win10

#### 步骤一：停止MySQL服务

在卸载之前，先停止MySQL8.0的服务。按键盘上的“Ctrl + Alt + Delete”组合键,打开“任务管理器”对话框,可以在“服务”列表找到“MySQL8.0”的服务，如果现在‘正在运行”状态，可以右键单击服务,选择“停止”选项停止MySQL8.0的服务，如图所示。

![image-20220125202340657](/../mysql/image-20230121173227728.png)

#### 步骤二：软件的卸载

卸载MySQL8.0的程序可以和其他桌面应用程序一样直接在‘控制面板”选择“卸载程序”，并在程序列表中找到MySQL8.0服务器程序，直接双击卸载即可，如图所示。这种方式删除，数据目录下的数据不会跟着删除。

![image-20230121173332149](/../mysql/image-20230121173332149.png)

#### 步骤三：删除本地数据数据

做好备份

![image-20230121173356933](/../mysql/image-20230121173356933.png)

#### 步骤四：注销注册表

如果上面的步骤做完了，还不能彻底卸载，就可以清理注册表

如何打开注册表编辑器：在系统的搜索框中输入`regedit`

```
HKEY_LOCAL_MACHINE \SYSTEM\ControlSet001 \Services\Eventlog\Application\MySQL服务目录删除

HKEY_LOCAL_MACHINE \SYSTEM\ControlSet001 \Services \MySQL服务目录删除

HKEY_LOCAL_MACHINE \SYSTEM\ControlSet002 \Services\Eventlog\Application\MySQL服务目录删除

HKEY_LOCAL_MACHINE \SYSTEM\ControlSet002\Services\MySQL服务目录删除

HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet \Services \Eventlog\Application\MySQL服务目录删除

HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet \Services \MySQL服务删除
```

> 注册表中的ControlSeto01、ControlSet002，不一定是001和002，可能是ControlSet005、006之类

#### 步骤五：删除环境变量path

![image-20230121173410880](/../mysql/image-20230121173410880.png)

### linux

#### 步骤一：关闭服务

```
systemctl stop mysqId.service
```

#### 步骤二：查看当前mysql安装状况

```
rpm -qa | grep -i mysql
或
yum list installed | grep mysql
```

#### 步骤三：卸载上述命令查询出的已安装程序

```
yum remove mysql-xxx mysql-xxx mysql-xxx mysqk-xxxx
```

务必卸载干净，反复执行rpm -qa | grep -i mysql 确认是否有删除残留

#### 步骤四：删除mysql相关文件

- 查找相关文件

```
find / -name mysql
```

- 删除上述命令查找出的相关文件

```
rm -rf xxx
```

#### 步骤五：删除my.cnf

```
rm -rf /etc/my.cnf
```

## sqlyog连接linux中的数据库服务器

### 步骤

将linux中mysql的端口，在防火墙中对外开放（或者直接关闭防火墙）

再次连接后，报错2058

![image-20230123131127318](/../mysql/image-20230123131127318.png)

mysql8底层密码的加密方式改变了

### 解决方法

```
ALTER USER 'root'@'%' IDENTIFIED WITH mysq1_native_password BY 'abc123';
```

然后在重新配置SQLyog的链接，则可连接成功

### mysql8.0的安全策略

在MySql8.0之前，MySQL使用的是validate_password插件检测、验证账号密码强度，保障账号的安全性。

安装/启用插件方式：运行时命令安装(推荐)

```
mysql> INSTALL PLUGIN validate_password SONAME 'validate_password.so';
Query 0K，0 rows affected, 1 warning (0.11 sec)
```

此方法也会注册到元数据，也就是mysql.plugin表中， 所以不用担心MySQL重启后插件会失效。

#### 1.validate_password说明

Mysql8.0引入了服务器主键（Components）这个特性，validate_password插件已用服务器主键重新实现。8.0.25版本中，默认自动安装validate_password主键

**未安装插件前，执行如下两个指令，执行效果：**

```
mysql> show variables like 'validate. password%';
Empty set (0.04 sec)

mysql> SELECT * FROM mysql . component ;
ERROR 1146 (42S02) : Table ' mysql.component' doesn't exist
```

**安装插件后，执行如下两个指令，执行结果：**

![image-20230123131217184](/../mysql/image-20230123131217184.png)

![image-20230123131429415](/../mysql/image-20230123131429415.png)

**关于validate. password组件对应的系统变量说明**

![image-20220125215411067](/../mysql/image-20230123131651001.png)

## 字符集的相关操作

### 修改5.7的字符集

#### 修改步骤

在MySQL 8.0版本之前，默认字符集为latinl，utf8字符集指向的是utf8mb3。网站开发人员在数据库设计的时候往往会将编码修改为utf8字符集。如果遗忘修改默认的编码，就会出现乱码的问题。从MySQL 8.0开始，数据库的默认编码将改为utf8mb4，从而避免上述乱码的问题。

##### 操作1：查看默认使用的字符集

```
show variables like ' character%';

#或者

show variables like'%char%;
```

**mysql8.0的执行结果**

![image-20230123132055306](/image-20230123132055306.png)

**mysql5.0的执行结果**

![image-20230123132708442](/image-20230123132708442.png)

由此可见，8.0是支持中文输入的，但是5.0需要修改字符集配置：

进入mysql安装路径：

```
vim /etc/my.cnf
```

在文件最后加上中文字符集配置

```
#添加如下 ：

[mysqld]
character_set_server=utf8
```

重启mysql服务

```
systemctl restart mysqld.service
```

再次查看字符集

![image-20230123132734211](/image-20230123132734211.png)

### 各级别的字符集

MySQL有4个级别的字符集和比较规则，分别是：

- 服务器级别
- 数据库级别
- 表级别
- 列级别

名词解释：

- `character_set_server`:服务器级别的字符集
- `character_set_database`: 当前数据库的字符集
- `character_set_client`: 服务器解码请求时使用的字符集
- `character_set_connection`:服务器处理请求时会把请求字符串从`character_set_client`转为`character_set_connection`
- `character_set_results`: 服务器向客户端返回数据时使用的字符集

### 字符集与比较规则

#### uft8与uft8mb4

- utf8mb3 :阉割过的utf8字符集，只使用1 ~ 3个字节表示字符。
- utf8mb4: 正宗的utf8字符集，使用1 ~4个字节表示字符。

#### 比较规则

eg. uft8_general_ci

| 后缀 | 英文释义           | 描述             |
| ---- | ------------------ | ---------------- |
| _ai  | accent insensitive | 不区分重音       |
| _as  | accent sensitive   | 区分重音         |
| _ci  | case insensitive   | 不区分大小写     |
| _cs  | case sensitive     | 区分大小写       |
| _bin | binary             | 以二进制方式比较 |

### 请求到响应过程中字符集的变化

![image-20230123132938852](/image-20230123132938852.png)

## SQL大小写规范

### 概述

在win中默认大小写不敏感，但在linux中大小写是有明确规范的

**MySQL在Linux下数据库名、表名、列名、别名大小写规则是这样的:**

1. 数据库名、表名、表的别名、变量名是严格区分大小写的；
2. 关键字、函数名称在SQL中不区分大小写；
3. 列名(或字段名)与列的别名(或字段别名)在所有的情况下均是忽略大小写的；

**MySQL在Windows的环境下全部不区分大小写**

### 大小写设置

当想设置为大小写不敏感时，要在my.cnf这个配置文件[mysqld]中加入lower_case_table_names=1，然后重启服务器。

- 但是要在重启数据库实例之前就需要将原来的数据库和表转换为小写，否则将找不到数据库名。
- 此参数适用于MySQL5.7。在MySQL8下禁止在重新启动MySQL服务时将lower_case_table_names设置成1，不同于初始化MySQL服务时设置的lower_case_table_names值。如果非要将MySQL8设置为大小写不敏感,具体步骤为：
  1. 停止MySQL服务
  2. 删除数据目录，即删除/var/lib/mysql目录
  3. 在MySQL配置文件(/etc/my.cnf)中添加lower_case_table_names=1
  4. 启动MySQL服务

建议：闲的没事不要改

### SQL编写建议

1. 关键字和函数名称全部大写;
2. 数据库名、表名、表别名、字段名、字段别名等全部小写;
3. SQL 语句必须以分号结尾。

## sql_mode的合理设置

sql_mode会影响MySQL支持的SQL语法以及它执行的数据验证检查。通过设置sql_mode, 可以完成不同严格程度的数据校验，有效地保障数据准确性。

MySQL服务器可以在不同的SQL模式下运行，并且可以针对不同的客户端以不同的方式应用这些模式，具体取决于sql_mode系统变量的值。

MySQL5.6和MySQL5.7默认的sql_mode模式参数是不一样的: 

- 5.6的mode默认值为空(即: NO_ ENGINE_ SUBSTITUTION)，其实表示的是一个空值， 相当于没有什么模式设置，可以理解为宽松模式。在这种设置下是可以允许一些非法操作的，比如允许一些非法数据的插入
- 5.7的mode是STRICT TRANS TABLES， 也就是严格模式。用于进行数据的严格校验，错误数据不能插入，报error (错误)，并且事务回滚。

### 宽松模式&严格模式

#### 宽松模式

如果设置的是宽松模式，那么我们在插入数据的时候，即便是给了一个错误的数据，也可能会被接受，并且不报错。

**应用场景：**

通过设置sql mode为宽松模式，来保证大多数sql符合标准的sql语法，这样应用在不同数据库之间进行迁移时，则不需要对业务sql进行较大的修改。

#### 严格模式

出现上面宽松模式的错误，应该报错才对,所以MySQL5.7版本就将sqL_mode默认值改为了严格模式。所以在生产等环境中，我们必须采用的是严格模式，进而开发、测试环境的数据库也必须要设置，这样在开发测试阶段就可以发现问题。并且我们即便是用的MySQL5.6,也应该自行将其改为严格模式。

**开发经验：** 

MySQL 等数据库总想把关于数据的所有操作都自己包揽下来,包括数据的校验,其实开发中，我们应该在自己开发的项目程序级别将这些校验给做了，虽然写项目的时候麻烦了一些步骤，但是这样做之后，我们在进行数据库迁移或者在项目的迁移时，就会方便很多。

### 模式查看与设置

#### 查看

```
select @@session.sql_mode

select @@global.sql_mode

#或者

show variables like 'sql_mode' ;
```

#### 临时设置

```
SET GLOBAL sql_ mode =‘modes. ..'; #全局
SET SESSION sq1_ mode = 'modes...'; #当前会话
```

#### 永久设置

在my.ini文件中，新增

```
[mysq1d]
sql_mode=ONLY_FULL_GROUP BY,STRICT_TRANS._TABLES, NO_ZERO_IN_DATE,NO.ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
```

然后重启mysql

## MySQL的数据目录

### mysql8的主要目录结构

#### 数据库文件的存放路径

MySQL数据库文件的存放路径: /var/lib/mysql/

MySQL服务器程序在启动时会到文件系统的某个目录下加载一些文件， 之后在运行过程中产生的数据也都会存储到这个目录下的某些文件中，这个目录就称为**数据目录**。

MySQL把数据都存到哪个路径下呢?其实**数据目录**对应着一个 系统变量`datadir`，我们在使用客户端与服务器建立连接之后查看这个系统变量的值就可以了:

![image-20230124192848760](/image-20230124192848760.png)

从结果中可以看出，在我们计算机上MySQL的数据目录就是`/var/lib/mysql/`

#### 相关命令目录

**相关命令某目录：/user/bin（mysqladmin、mysqlbinlog、mysqldump等命令）和/user/sbin。**

![image-20230124193002042](/image-20230124193002042.png)

安装目录下非常重要的bin目录，它里边存储了许多关于控制客户端程序和服务器程序的命令(许多可执行文件，比如mysql，mysqld， mysqld_ safe等)。而数据目录是用来存储MySQL在运行过程中产生的数据，注意区分开二者。

#### 配置文件目录

**配置文件目录: /usr/share/mysql-8.0 (命令及配置文件)，/etc/mysql (如my.cnf)**

### 数据库和文件系统的关系

像`InnoDB`、`MyISAM` 这样的存储引擎都是把表存储在磁盘上的，操作系统用来管理磁盘的结构被称为文件系统，所以用专业一点的话来表述就是：像`InnoDB`、`MyISAM` 这样的存储引擎都是把**表存储在文件系统上**的。当我们想读取数据的时候，这些存储引擎会从文件系统中把数据读出来返回给我们，当我们想写入数据的时候，这些存储引擎会把这些数据又写回文件系统。本章学习一下InnoDB和MyISAM这两个存储引擎的数据如何在文件系统中存储。

#### 查看默认数据库

```
mysql> SHOW DATABASES;
```

有四个数据库是MySQL自带的系统数据库

- mysql

  MySQL系统自带的核心数据库，它**存储了MySQL的用户账户和权限信息**，一些存储过程、事件的定义信息，一些运行过程中产生的日志信息，- -些帮助信息以及时区信息等。

- information_schema

  MySQL系统自带的数据库，这个数据库保存着MySQL服务器**维护的所有其他数据库的信息**，比如有**哪些表**、**哪些视图**、**哪些触发器**、**哪些列**、**哪些索引**。这些信息并不是真实的用户数据，而是一些描述性信息， 有时候也称之为**元数据**。在系统数据库information_ schema中提供了一些以innodb_sys开头的表，用于表示内部系统表。

- performance_schema

  MySQL系统自带的数据库，这个数据库里主要保存MySQL服务器运行过程中的一些状态信息，可以用来**监控MySQL服务的各类性能指标**。包括统计最近执行了哪些语句，在执行过程的每个阶段都花费了多长时间，内存的使用情况等信息。

- sys

  MySQL系统自带的数据库，**这个数据库主要是通过视图的形式**把information_schema 和performance_ schema 结合起来，帮助系统管理员和开发人员监控MySQL的技术性能。

#### 数据库在文件系统中的表示

> 储备知识: (索引章节会讲到)
>
> - InnoDB其实是使用页为基本单位来管理存储空间的，默认的页大小为16KB。
> - 对于InnoDB存储引擎来说，每个索引都对应着一 棵B+树，该B+树的每个节点都是一 个数据页，数据页之间不必要是物理连续的，因为数据页之间有双向链表来维护着这些页的顺序。
> - InnoDB的聚簇索引的叶子节点存储了完整的用户记录，也就是所谓的索引|即数据，数据即索引。

为了更好的管理这些页，InnoDB提出了一个表空间或者文件空间(英文名: table space 或者file space)的概念，这个表空间是一个抽象的概念，它可以对应文件系统上一个或多个真实文件(不同表空间对应的文件数量可能不同)。每一个表空间可以被划分为很多个页，我们的表数据就存放在某个表空间下的某些页里。这里表空间有几种不同的类型:

1. **系统表空间**

   - 默认情况下，InnDB会在数据目录下创建一个名为ibdatal、大小为12M的文件，这个文件就对应着系统表空间
   - 在5.5.6到5.6.6之间，表中的数据都被默认存储到这个系统表空间

2. **独立表空间**

   - 5.6.6以后，就会存在每个表自己的独立表空间中（xx.ibd文件中）

3. **系统表空间与独立表空间的设置**

   - ```
     [server]
     innodb_file_per_table=0 
     # 0:代表使用系统表空间，1: 代表使用独立表空间
     ```

     5.7以后存储在.ibd文件下，不再存储于ibtmpl目录下

     ![image-20230124195528674](/image-20230124195528674.png)

#### 表在文件系统中的表示

##### InnDB存储引擎模式

- **MySQL5.7**
  - ![image-20230124195545426](/image-20230124195545426.png)
  - **采用系统存储表结构**
- **MySQL8.0**
  - ![image-20230124195608637](/image-20230124195608637.png)
  - **采用独立存储表结构**

**问题：db.opt（字符集和比较规则）和 .frm（表结构文件,字段长度等）在系统表空间都不见了？**

> **原因：**
>
> **db.opt中的数据都被存放在了独立表空间中（自己的表空间里面了）**
>
> **.frm在8.0中被整合到了.ibd文件中**

**如何证明？**

> 这就需要解析ibd文件。Oracle 官方将frm文件的信息及更多信息移动到叫做序列化字典信息(Serialized Dictionary Information，SDI) ，SDI被写在ibd文件内部。
>
> MySQL 8.0属于Oradie旗下，同理。
>
> 1. 查看表结构
>
>    - 到存储.ibd文件的目录下，执行以下命令
>
>    - ```
>      ibd2sdi --dump-file=student.txt student.ibd
>      ```
>
>    - ![image-20230124200024783](/image-20230124200024783.png)

##### MyISAM存储引擎模式

**表结构**

在存储表结构方面，二者是一致的，也是在数据目录下对应的数据库子目录下创建一个专门用于描述表结构的文件：`表名frm`

**表中的数据和索引**

MyISAM中的索引全部都是二级索引，该存储引擎的数据和索引是分开的，因此目录下会有三个文件

```
test.frm  	存储表结构
test.MYD	存储数据
test.MYI	存储索引
```

**MySQL5.7**

![image-20230124200015378](/image-20230124200015378.png)

**MySQL8.0**

![image-20230124200046640](/image-20230124200046640.png)

#### 小结

举例：数据库a， 表b。

1. 如果表b采用InnoDB，data\a中会产生1个或者2个文件: .

   - b.frm :描述表结构文件,字段长度等

   - 如果采用**系统表空间**模式的，数据信息和索引信息都存储在**ibdatal**中

   - 如果采用**独立表空间**存储模式，data\a中还会产生 **b. ibd**文件(存储数据信息和索引信息)

     - 此外:

       ①MySQL5.7中会在data/a的目录下生成**db.opt**文件用于保存数据库的相关配置。比如：字符集、比较规则。而MySQL8.0不再提供db.opt文件。

       ②MySQL8.0中不再单独提供b.frm，而是合并在b.ibd文件中。

2. 如果表b采用MyISAM, data\a中会产生3个文件:

   - MySQL5.7中: b.frm: 描述表结构文件，字段长度等。

     MySQL8.0中b.xxx.sdi :描述表结构文件，字段长度等。

   - b.MYD (MYData): 数据信息文件，存储数据信息(如果采用独立表存储模式)

   - b.MYI (MYIndex): 存放索引信息文件

#### 视图在文件系统中的表示

我们知道MySQL中的视图其实是虚拟的表，也就是某个查询语句的一个别名而已，所以在存储视图的时候是不需要存储真实的数据的，只需要把它的结构存储起来就行了。和表一样，描述视图结构的文件也会被存储到所属数据库对应的子目录下边，只会存储一个视图名. frm的文件。emp_details_view.frm

#### 其他文件

除了我们上边说的这些用户自己存储的数据以外，数据目录下还包括为了更好运行程序的一些额外文件，主要包括这几种类型的文件：

- **服务器进程文件**
  - 我们知道每运行一个MySQL服务器程序, 都意味着启动一个进程。MySQL服务器会把自己的进程ID写入到一个文件中。
- **服务器日志文件**
  - 在服务器运行过程中，会产生各种各样的日志，比如常规的查询日志、错误日志、二进制日志、redo日志等。这些日志各有各的用途，后面讲解。
- **默认/自动生成的SSL和RSA证书和密钥文件**
  - 主要是为了客户端和服务器安全通信而创建的一些文件。

## 用户的权限管理

### 用户管理

启动MySQL服务后,可以通过mysq|命令来登录MySQL服务器，命令如下:

```
mysql-h hostname|hostIP -P port -u username -p DatabaseName -e"SQL语句"
```

下面详细绍命令中的参数:

- `-h`参数后面接主机名或者主机IP，hostname为主机， hostIP为主机IP。
- `-P`参数后面接MySQL服务的端口，通过该参数连接到指定的端口。MySQL服务的默认端口是3306,不使用该参数时自动连接到3306端口，port为连接的端口号。
- `-u`参数后面接用户名，username为用户名。
- `-p`参数会提示输入密码。
- `DatabaseName` 参数指明登录到哪一个数据库中。如果没有该参数，就会直接登录到MySQL数据库中，然后可以使用USE命令来选择数据库。
- `-e`参数后面可以直接加SQL语句。登录MySQL服务 器以后即可执行这个SQL语句，然后退出MySQL服务器。

举例:

```
mysql -uroot -p -h localhost -P 3306 mysql -e "select host, user from user”
```

#### 增加用户

在有host和user作为联合主键的user表中，创建新用户

既可以创建仅限本地登录的用户，也可以创建可以任意登录的用户

只要host和user中有一个不同就可以创建

![image-20230124200843443](/image-20230124200843443.png)

#### 修改用户

修改后一定要记得刷新

![image-20230124201309700](/image-20230124201309700.png)

#### 删除用户

##### 1.用DROP方式删除（推荐方式）

![image-20230124201345482](/image-20230124201345482.png)

```
DROP USER li4; # 默认删除host为%的用户

DROP USER ' kangshifu'@' localhost';
```

##### 2.用Delete方式删除(不推荐，会有残留)

![image-20230124201354241](/image-20230124201354241.png)

#### 设置当前用户密码

适用于root用户修改自己的密码，以及普通用户登录后修改自己的密码。

root用户拥有很高的权限，因此必须保证root用户的密码安全。root用户可以通过多种方式来修改密码，使用ALTER USER 修改用户密码是MySQL官方推荐的方式。此外，也可以通过SET语句修改密码。由于MySQL 8中已移除了PASSWORD()函数，因此不再使用UPDATE语句直接操作用户表修改密码。

##### 使用ALTER USER语句(推荐写法)

用户可以使用ALTER命令来修改自身密码，如下语句代表修改当前登录用户的密码。基本语法如下:

```
ALTER USER USER() IDENTIFIED BY 'new_password';
```

练习:下面使用ALTER命令来修改root用户的密码， 将密码改为“Hello_1234”。命令如下:

```
ALTER USER USER() IDENTIFIED BY 'Hello_1234';
```

##### 使用SET语句

使用root用户登录MySQL后，可以使用SET语句来修改密码，具体SQL语句如下:

```
SET PASSWORD= 'new_password' ;
```

该语句会自动将密码加密后再赋给当前用户。

#### 修改其他用户的密码

##### 使用ALTER USER语句(推荐写法)

基本语法如下:

```
ALTER USER user [IDENTIFIED BY ' 新密码'] 
[ , user [ IDENTIFIED BY '新密码'] ]
```

其中，user参数表示新用户的账户，由用户名和主机名构成；“IDENTIFIED BY"关键字用来设工。

练习：下面使用ALTER语句来修改zhangsan用户的密码， 将密码改为“HelloWorld_123”。

```
ALTER USER 'zhangsan'@'localhost' IDENTIFIED BY 'HelloWorld_123'
```

##### 使用SET语句

使用root用户登录到MySQL服务器后，可以使用SET语句来修改普通用户的密码。SET语句的代码如下:

```
SET PASSWORD FOR 'username'@'hostname'='new_password';
```

其中，username参数是普通用户的用户名；hostname参数是普通用户的主机名；new_password是新密码。

练习：下面使用SET语句来修改zhangsan用户的密码， 将密码改成“HelloWorld_123”。

```
SET PASSWORD for 'zhangsan'@'localhost'='HelloWorld_123';
```

### 权限管理

关于MySQL的权限简单的理解就是MySQL允许你做你权力以内的事情，可以越界。比如只允许你执行SELECT操作，那么你就不能执行UPDATE操作。允许你从某台机器上连接MySQL，那么你就不能从除那台机器以外的其他机器连接MySQL。

#### 权限列表

查看权限

```
show privileges;
```

| 权限分布 | 可能的设置的权限                                             |
| -------- | ------------------------------------------------------------ |
| 表权限   | ‘Select’,'Insert','Update','Delete','Create','Drop','Grant','Reference','Index','Alter' |
| 列权限   | 'Select','Insert','Update','Reference'                       |
| 过程权限 | 'Execute','Alter Routine','Grant'                            |

#### 授予权限的原则

权限控制主要是出于安全因素，因此需要遵循以下几个经验原则：

1. 只授予能`满足需要的最小权限`，防止用户干坏事。比如用户只是需要查询，那就只给select权限就可以了，不要给用户赋予update、insert或者delete权限。
2. 创建用户的时候`限制用户的登录主机`，一般是限制成指定IP或者内网IP段。
3. 为每个用户`设置清足密码复杂度的密码`。
4. `定期清理不需要的用户`，回收权限或者删除用户。

#### 授予权限

给用户授权的方式有2种，分别是通过把`角色赋予用户给用户授权`和`直接给用户授权`。用户是数据库的使用者，我们可以通过给用户授予访问数据库中资源的权限，来控制使用者对数据库的访问，消除安全隐患。

![image-20230124202537178](/image-20230124202537178.png)

![image-20230124202551187](/image-20230124202551187.png)

如果需要赋予包括GRANT的权限，添加参数**“WITH GRANT OPTION ”**这个选项即可，表示该用户可以将自己拥有的权限授权给别人。经常有人在创建操作用户的时候不指定WITH GRANT OPTION选项导致后来该用户不能使用GRANT命令创建用户或者给其它用户授权。

可以使用GRANT重复给用户添加权限，**权限叠加**，比如你先给用户添加一个SELECT权限， 然后又给用户添加 一个INSERT权限，那么该用户就同时拥有了SELECT和INSERT权限。

> 我们在开发应用的时候，经常会遇到一种需求，就是要根据用户的不同，对数据进行横向和纵向的分组。
>
> - 所谓横向的分组，就是指用户可以接触到的数据的范围，比如可以看到哪些表的数据；
> - 所谓纵向的分组，就是指用户对接触到的数据能访问到什么程度，比如能看、能改， 甚至是删除。

#### 查看权限

- 查看当前用户权限

```
SHOW GRANTS;
#或
SHOW GRANTS FOR CURRENT_USER;
#或
SHOW GRANTS FOR CURRENT_USER() ;
```

- 查看某用户的全局权限

```
SHOW GRANTS FOR 'user'@' 主机地址’;
```

#### 回收权限

收回权限就是取消已经赋予用户的某些权限。**收回用户不必要的权限可以在一定程度上保证系统的安全性。**MySQL中使用`REVOKE语句`取消用户的某些权限。使用REVOKE收回权限之后， 用户账户的记录将从db、host、tables_priv和columns_priv表中删除，但是用户账户记录仍然在user表中保存(删除user表中的账户记录使用DROP USER语句)。

**注意:在将用户账户从user表删除之前，应该收回相应用户的所有权限。**

- 收回权限命令

```
REVOKE 权限1,权限2.……权限n ON 数据库名称.表名称 FROM 用户名@用户地址:
```

- 注意
  - 需用户重新登录后才能生效

> 总结
>
> 有一些程序员喜欢使用Root超级用户来访问数据库，完全把权限控制放在应用层面实现。这样当然也是可以的。但建议大家，尽量使用数据库自己的角色和用户机制来控制访问权限，不要轻易用Root账号。因为Root账号密码放在代码里面不安全，一旦泄露，数据库就会完全失去保护。
>
> 而且，MySQL的权限控制功能十分分完善，应该尽量利用，可以提高效率，而且安全可靠

### 权限表

![image-20230124205304889](/image-20230124205304889.png)

MySQL服务器通过**权限表**来**控制用户对数据库的访问**，权限表存放在**mysql数据库**中。MySQL数据库系统会根据这些权限表的内容为每个用户赋予相应的权限。这些权限表中最重要的是**user表、db表**。 除此之外，还有`table_priv表`、`column_priv表`和`proc_priv表`等。**在MySQL启动时， 服务器将这些数据库表中权限信息的内容读入内存。**

#### user表

user表是MySQL中最重要的一个权限表，**记录用户账号和权限信息**，有49个字段。

#### db表

db表是MySQL数据中非常重要的权限表。db表中**存储了用户对某个数据库的操作权限**，决定用户能从那个主机存取哪个数据库

#### tables_priv表和columns_priv表

tables_priv表用来**对表设置操作权限**

columns_priv表用来**对表中的某一列设置权限**

#### procs_priv表

procs_priv表可以对**存储过程和存储函数设置操作权限**

### 访问控制

正常情况下，不希望每个用户都可以执行所有的数据库操作。当MySQL允许一个用户执行各种操作时， 它将首先核实该用户向MySQL服务器发送的连接请求，然后确认用户的操作请求是否被允许。这个过程称为MySQL中的访问控制过程。MySQL的访问控制分为两个阶段：连接核实阶段和请求核实阶段。

#### 连接核实阶段

当用户试图连接MySQL服务器时，服务器基于用户的身份以及用户是否能提供正确的密码验证身份来确定接受或者拒绝连接。即客户端用户会在连接请求中提供用户名主机地址、用户名密码，MySQL服务器接收到用户请求后，会**使用user表中的host、user和authentication_string(主机地址、用户名、密码)这3个字段匹配客户端提供信息**

服务器只有在user表记录的Host和User字段匹配客户端主机名和用户名，粗提供正确的密码时才接受连接。**如果连接核实没有通过，服务器就完全拒绝访问；否则，服务器接受连接，然后进入阶段2等待用户请求。**

#### 请求核实阶段

一旦建立了连接，服务器就进入了访问控制的阶段2，也就是请求核实阶段。对此连接上进来的每个请求，服务器检查该请求要执行什么操作、是否有足够的权限来执行它，这正是需要授权表中的权限列发挥作用的地方。这些权限可以来自user_db_table_priv和column_priv表。

确认权限时，MySQL首先检查**user表**，如果指定的权限没有在user表中被授予，那么MySQL就会继续**检查db表**，db表是下一安全层级，其中的权限限定于数据库层级，在该层级的SELECT权限允许用户查看指定数据库的所有表中的数据；如果在该层级没有找到限定的权限，则MySQL继续**检查tables_ priv表**以及**columns_priv表**， 如果所有权限表都检查完毕，但还是没有找到允许的权限操作,MySQL将**返回错误信息**，用户请求的操作不能执行，操作失败。请求核实的过程如图所示。

 ![image-20230124212300391](/image-20230124212300391.png)

> 提示：
>
> MySQL通过向下层级的顺序(从user表到columns_ priv表) 检查权限表，但并不是所有的权限都要执行该过程。例如，一个用户登录到MySQL服务器之后只执行对MySQL的管理操作，此时只涉及管理权限，因此MySQL只检查user表。另外，如果请求的权限操作不被允许，MySQL也不会继续检查下一层级的表。

### 角色管理

#### 角色的理解

角色是在MySQL 8.0中引入的新功能。在MySQL中，**角色是权限的集合**，可以为角色添加或移除权限。用户可以被赋予角色，同时也被授予角色包含的权限。对角色进行操作需要较高的权限。并且像用户账户-样，角色可以拥有授予和撤消的权限。

引入角色的目的是**方便管理拥有相同权限的用户**。**恰当的权限设定，可以确保数据的安全性，这是至关重要的。**

![image-20230124213239460](/image-20230124213239460.png)

#### 创建角色

在实际应用中，为了安全性，需要给用户授予权限。当用户数量较多时，为了避免单独给每一个用户授予多个权限，可以先将权限集合放入角色中，再賦予用户相应的角色。

创建角色使用**CREATE ROLE** 语句,语法如下：

```
CREATE ROLE 'role_name'[@'host_name'] [,'role_name'[@'host_name'l]...
```

角色名称的命名规则和用户名类似。如果**host_name省略，默认为%，role. name不可省略**，可为空。

实例

```
create ROLE manger；
```

#### 给角色赋予权限

创建角色之后，默认这个角色是没有任何权限的,我们需要给角色授权。给角色授权的语法结构是

```
GRANT privileges ON table_name TO 'role_ name'[@'host_name'];
```

上述语询中privileges代表权限的名称，多个权限以逗号隔开。

可以使用show语句查询权限名称

```
show PRIVILEGES\G
```

实例

```
GRANT SELECT,INSERT,DELETE ON demo.* TO 'manger'[@'host_name'];

GRANT SELECT,INSERT,DELETE ON demo.* TO 'manger';
```

#### 查看角色的权限

```
show GRANT FOR 'manger';
```

#### 回收角色的权限

```
REVOKE privileges ON tablename FROM 'rolename';
```

实例

```
REVOKE INSERT,UPDATE,DELETE ON school.* FROM 'school_ write' ;
```

#### 删除角色

```
DROP ROLE role[,role2]……;
```

实例

```
DROP ROLE 'school_read';
```

#### 给用户赋予角色

```
grant 'manger'@'%' TO 'wang5'@'%';
```

添加完成后使用SHOW语句查看是否添加成功，SQL语句如下。

```
SHOW GRANTS FOR 'wang5'@'localhost';
```

使用kangshifu用户登录，然后查询当前角色，如果角色未激活，结果将显示NONE。SQL语句如下。

```
SELECT CURRENT ROLE():
```

上面结果是NONE,说明用户未具备相应的角色。

或者你用赋予了角色的用户去登录、操作,你会发现，这个账号没有任何权限。这是因为，**MySQL 中创建了角色之后，默认都是没有被激活**，也就是不能用，必须要手动激活，激活以后用户才能拥有角色对应的权限。

#### 激活角色

**方式1:使用set default role命令激活角色**

举例:

```
SET DEFAULT ROLE ALL T0 'kangshifu' e' localhost' ;
```

举例:使用SET DEFAULT ROLE 为下面4个用户默认激活所有已拥有的角色如下:

```
SET DEFAULT ROLE ALL TO
	'dev1'@'localhost',
	'read_user1'@'localhost',
	'read_user2'@'localhost',
	'rw_user1'@'localhost';
```

> 注意：用户需要退出重新登录，才能看到赋予的角色

**方式2:将activate_ all_roles_on_login设置为ON**

```
SET GLOBAL activate_all_roles_on_login=ON;
```

这条SQL语句的意思是，对**所有角色永久激活**。运行这条语句之后,用户才真正拥有了赋予角色的所有权限。查看当前会话已激活的角色:

#### 撤销用户的角色

```
remove role from user;
```

#### 设置强制角色

强制角色是给每个创建账户的默认角色，不需要手动设置。强制角色无法被**REVOKE**或者**DROP**。

方式1:服务启动前设置

```
[mysq1d]
mandatory-roles='role1,role2@localhost,r3@%.atguigu.com'
```

方式2:运行时设置

```
SET PERSIST mandatory_roles = 'role1,role2@localhost,r3@%.example.com'; #系统重启后仍然有效
SET GLOBAL mandatory_roles = 'role1,role2@localhost,r3@%.example.com' ; #系统重启后 失效
```

#### 小结

| 语句                                                     | 作用                                     |
| -------------------------------------------------------- | ---------------------------------------- |
| `CREATE ROLE and DROP ROLE`                              | 创建和删除角色                           |
| `GRANT and REVOKE`                                       | 给角色或者账户分配权限                   |
| `SHOW GRANTS`                                            | 显示账户/角色所拥有的权限或者角色        |
| `SET DEFAULT ROLE`                                       | 设置账户默认使用什么角色                 |
| `SET ROLE`                                               | 改变当前会话的角色                       |
| `CURRENT_ROLE()`函数                                     | 显示当前会话的角色                       |
| `mandatory_roles`和`activate_all_roles_on_login`系统变量 | 允许定义角色登录时强制的活激活授权的角色 |

## 逻辑架构

### 逻辑架构剖析

首先MySQL是典型的C/S架构，即`Client/Server`架构，服务器端程序使用的`mysqld`。

不论客户端进程和服务器进程是采用哪种方式进行通信，最后实现的效果都是：**客户端进程向服务器进程发送一段文本(SQL语句)，服务器进程处理后再向客户端进程发送一段文本 (处理结果)。**

那服务器进程对客户端进程发送的请求做了什么处理，才能产生最后的处理结果呢？这里以查询请求为例展示:

![image-20230124214145050](/image-20230124214145050.png)

![image-20220126174355217](/image-20230124214233512.png)

#### Conectiors

Connectors，指的是不同语言中与SQL的交互。MySQL首先是一个网络程序，在TCP之上定义了自己的应用层协议。所以要使用MySQL,我们可以编写代码，跟MySQL Server`建立TCP连接`，之后按照其定义好的协议进行交互。或者比较方便的办法是调用SDK，比如Native CAPI、JDBC、 PHP等各语言MySQL Connector,或者通过JDDBC。但**通过SDK来访问MySQL，本质上还是在TCP连接上通过MySQL协议跟MySQL进行交互。**

**接下来的MySQL Server结构可以分为如下的三层：**

#### 连接层

系统(客户端)访问MySQL服务器前，做的第一件事就是建立TCP连接。

经过**三次握手**建立连接成功后，MySQL服务器对TCP传输过来的账号密码做身份认证、权限获取。

- **用户名或密码不对，会收到一个Access denied for user错误，客户端程序结束执行**
- **用户名密码认证通过，会从权限表查出账号拥有的权限与连接关联，之后的权限判断逻辑，都将依赖于此时读到的权限**

接着我们来思考一个问题

一个系统只会和MySQL服务器建立一个连接吗？只能有一个系统和MySQL服务器建立连接吗?

当然不是，多个系统都可以和MySQL服务器建立连接，每个系统建立的连接肯定不止一个。所以，为了解决TCP . 无限创建与TCP频繁创建销毁带来的资源耗尽、性能下降问题。MySQL服务器里有专门的**TCP连接池**限制连接数，采用**长连接模式**复用TCP连接，来解决上述问题。

![image-20230124215131257](/image-20230124215131257.png)

TCP连接收到请求后，必须要分配给一个线程专门与这个客户端的交互。所以还会有个线程池，去走后面的流程。每一个连接从线程池中获取线程，省去了创建和销毁线程的开销。

这些内容我们都归纳到**MySQL的连接管理**组件中。

所以**连接管理**的职责是负责**认证、管理连接、获取权限信息**。

#### 服务层

服务层架构主要完成大多数的核心服务功能，如SQL接口， 并完成**缓存的查询**，SQL的分析和优化及部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如过程、函数等。

在该层，服务器会**解析查询**并创建相应的内部**解析树**，并对其完成相应的**优化**：如确定查询表的顺序，是否利用索引等，最后生成相应的执行操作。

如果是SELECT语句，服务器还会**查询内部的缓存**。如果缓存空间足够大，这样在解决大量读操作的环境中能够很好的提升系统的性能。

- **SQL Interface：SQL接口**

  - 接收用户的SQL命令，并且返回用户需要查询的结果。比如SELECT...FROM就是调用SQL Interface
  - MySQL支持DML(数据操作语言)、DDL (数据定义语言)、存储过程、 视图、触发器、自定义函数等多种SQL语言接口

- **Parser：解析器**

  - 在解析器中对SQL语句进行语法分析、语义分析。将SQL语句分解成数据结构，并将这个结构传递到后续步骤，以后SQL语句的传递和处理就是基于这个结构的。如果在分解构成中遇到错误，那么就说明这个SQL语句是不合理的。
  - 在SQL命令传递到解析器的时候会被解析器验证和解析，并为其创建**语法树**，并根据数据字典丰富查询语法树，会**验证该客户端是否具有执行该查询的权限**。创建好语法树后，MySQL还会对SQL查询进行语法上的优化，进行查询重写。

- **Optimizer:查询优化器**

  - SQL语句在语法解析之后、查询之前会使用查询优化器确定SQL语句的执行路径，生成一个**执行计划**。

  - 这个执行计划表明应该**使用哪些索引**进行查询(全表检索还是使用索引检索)，表之间的连接顺序如何, 最后会按照执行计划中的步骤调用存储引擎提供的方法来真正的执行查询，并将查询结果返回给用户。

  - 它使用**“选取-投影-连接”**策略进行查询。例如:

    ```
    SELECT id, name FROM student WHERE gender ='女' ;
    ```

    这个SELECT查询先根据WHERE语句进行**选取**，而不是将表全部查询出来以后再进行gender过滤。

    这个SELECT查询先根据id和name进行属性**投影**，而不是将属性全部取出以后再进行过滤，将这两个查询条件**连接**起来生成最终查询结果。

- **Caches & Buffers: 查询缓存组件**

  - MySQL内部维持着一些Cache和Buffer, 比如Query Cache用来缓存一条SELECT语句的执行结果， 如果能够在其中找到对应的查询结果，那么就不必再进行查询解析、优化和执行的整个过程了，直接将结果反馈给客户端。
  - 这个缓存机制是由一-系列小缓存组成的。比如表缓存,记录缓存，key缓存,权限缓存等。
  - 这个查询缓存可以在**不同客户端之间共享**。
  - 从MySQL 5.7.20开始，不推荐使用查询缓存，**并在MySQL 8. 0中删除**。

#### 引擎层

和其它数据库相比，MySQL有点与众不同，它的架构可以在多种不同场景中应用并发挥良好作用，主要体现在存储引擎的架构上，**插件式的存储引擎架构**将查询处理和其它的系统任务以及数据的存储提取相分离。这种架构可以根据业务的需求和实际需要选择合适的存储引擎。同时开源的MySQL还允许**开发人员设置自己的存储引擎**。

这种高效的模块化架构为那些希望专门针对特定应用程序需求(例如数据仓库、事务处理或高可用性情况)的人提供了巨大的好处,同时享受使用一组独立于任何接口和服务的优势存储引擎。

插件式存储引擎层( Storage Engines)，**真正的负责了MySQL中数据的存储和提取，对物理服务器级别维护的底层数据执行操作**，服务器通过API与存储引擎进行通信。不同的存储引擎具有的功能不同，这样我们可以根据自己的实际需要进行选取。

MySQL8.0默认支持的引擎如下：

![image-20230124222021098](/image-20230124222021098.png)

#### 存储层

所有的数据，数据库、表的定义，表的每一行的内容，索引，都是存在**文件系统**上，以**文件**的方式存在的，并完成与存储引擎的交互。当然有些存储引擎比如InnoDB，也支持不使用文件系统直接管理裸设备，但现代文件系统的实现使得这样做没有必要了。在文件系统之下，可以使用本地磁盘，可以使用DAS、NAS、 SAN等各种存储系统。

查看存储的位置

![image-20230124222042727](/image-20230124222042727.png)

#### 小结

![image-20230124222159684](/image-20230124222159684.png)

简化为三层结构：

1. 连接层：客户端和服务器端建立连接，客户端发送SQL至服务器端；
2. SQL层(服务层)：对SQL语句进行查询处理；与数据库文件的存储方式无关；
3. 存储引擎层：与数据库文件打交道，负责数据的存储和读取。

### SQL执行流程

#### MYSQL中的SQL执行流程

![image-20230125123102155](/image-20230125123102155.png)

MySQL的查询流程：

1. **查询缓存**：Server如果在查村缓存中发现了这条SQL语句，就会直接将结果返回给客户端；如果没有就会进入到解析器阶段。需要说明的是，查询缓存往往效率不高，所以在MySQL8.0中就被舍弃了。

   - 查询缓存效率不高的原因：

     - 在MySQL中的查询缓存，**不是缓存查询计划，而是查询对应的结果。两个查询请求在任何字符上的不同(例如:空格、注释、大小写)，都会导致缓存不会命中。因此MySQL的查询缓存命中率不高。**
     - 同时，如果查询请求中包含某些系统函数、用户自定义变量和函数、一些系统表，如mysql、information_ schema、performance_ schema数据库中的表，那这个请求就不会被缓存。以某些系统函数举例，可能同样的函数的两次调用会产生不一样的结果，比如**函数NOW，每次调用都会产生最新的当前时间，如果在一个查询请求中调用了这个函数，那即使查询请求的文本信息都一样，那不同时间的两次查询也应该得到不同的结果，如果在第一次查询时就缓存了，那第二次查询的时候直接使用第一次查询的结果就是错误的**！
     - 此外，既然是缓存，那就有它缓存失效的时候。MySQL的缓存系统会监测涉及到的每张表，只要该表的结构或者数据被修改，如对该表使用了INSERT、UPDATE、 DELETE、 TRUNCATE TABLE、 ALTER TABLE、DROP TABLE或DROP DATABASE 语句，那使用该表的所有高速缓存查询都将变为无效并从高速缓存中删除！**对于更新压力大的数据库来说，查询缓存的命中率会非常低。**
     - 弊大于利，查询缓存的失效非常频繁。

   - MySQL5.7中可以通过以下语句对查询缓存进行配置

     - ```
       #query_cache_type有3个值 0代表关闭查询缓存0FF，1代表开启ON，2 (DEMAND)
       query_cache_type=2
       ```

     - 为2（按需查询）的情况下时，查询语句需要加上SQL_CACHE

     - ```
       select SQL_CACHE * from test where ID=5;
       ```

   - 监控查询缓存的命中率

     - ![image-20230125123310794](/image-20230125123310794.png)

2. 解析器：在解析器中对SQL语句进行语法分析、语义分析。

   - 如果没有命中查询缓存，就要开始真正执行语句了。首先，MySQL需要知道你要做什么，因此需要对SQL语句做解析。SQL语句的分析分为词法分析与语法分析。
   - 分析器先做“**词法分析**”。你输入的是由多个字符串和空格组成的一条SQL语句，MySQL 需要识别出里面的字符串分别是什么，代表什么。
   - MySQL从你输入的"select"这个关键字识别出来，这是一个查询语句。 它也要把字符串“T”识别成“表名T”，把字符串“ID”识别成“列ID”。
   - 接着，要做“**语法分析**”。根据词法分析的结果，语法分析器(比如: Bison) 会根据语法规则，判断你输入的这个SQL语句是否**满足MySQL语法**。
   - 如果你的语句不对，就会收到“You have an error in your SQL syntax' "的错误提醒。
   - 如果SQL语句正确，则会生成一个这样的语法树：
     - ![image-20230125123633021](/image-20230125123633021.png)
   - 下面是SQL词法分析的过程步骤：
     - ![image-20230125123904442](/image-20230125123904442.png)

3. 优化器：在优化器中会确定SQL语句的执行路径，比如是根据**全表检索**，还是根据**索引检索**等。

   - 经过了解析器，MySQL 就知道你要做什么了。在开始执行之前，还要先经过优化器的处理。**一条查询可以有很多种执行方式，最后都返回相同的结果。优化器的作用就是找到这其中最好的执行计划。**

   - 比如：优化器是在表里面有多个索引的时候，决定使用哪个索引；或者在一个语句有多表关联(join) 的时候，决定各个表的连接顺序，还有表达式简化、子查询转为连接、外连接转为内连接等。

   - ```
     select * from test1 join test2 using(ID) where test1.name='zhangwei' and test2.name='mysq1高级课程';
     ```

     可以先查test1.name，也可以先查test2.name，根据索引或是根据

   - 在查询优化器中，可以分为**逻辑查询**优化阶段和**物理查询**优化阶段。

     - **逻辑查询优化**就是通过改变SQL语句的内容来<font color='red'>**使得SQL查询更高效**</font>，同时为物理查询优化提供更多的候选执行计划。通常采用的方式是对SQL语句进行**等价变换**，对查询进行**重写**，而查询重写的数学基础就是关系代数。对条件表达式进行等价谓词重写、条件简化，对视图进行重写，对子查询进行优化，对连接语义进行了外连接消除、嵌套连接消除等。
     - **物理查询优化**是基于关系代数进行的查询重写，而关系代数的每一步都对应着物理计算，这些物理计算往往存在多种算法，因此需要计算各种物理路径的代价，从中<font color='red'>**选择代价最小的作为执行计划**</font>。在这个阶段里，对于单表和多表连接的操作，需要高效地使用索引，提升查询效率。

4. 执行器

   - 截止到现在，还没有真正读写真实的表，仅仅只是产出了一个执行计划。于是就进入了执行器阶段。

   - 在执行之前需要判断该用户是否**具备权限**。如果没有，就会返回权限错误。如果具备权限，就执行SQL查询并返回结果。在MySQL8.0以下的版本，如果设置了查询缓存，这时会将查询结果进行缓存。

     ```
     select * from test where id=1;
     ```

   - 如果有权限，就打开表继续执行。打开表的时候，执行器就会根据表的引擎定义，调用存储引擎**API**对表进行的读写。存储引擎**API**只是抽象接口，下面还有个**存储引擎层**，具体实现还是要看表选择的存储引擎。

   -  ![image-20230125124814503](/image-20230125124814503.png)![image-20230125124849973](/image-20230125124849973.png)

SQL语句在MySQL中的流程是：SQL语句→查询缓存→解析器→优化器→执行器

![image-20230125124949817](/image-20230125124949817.png)

#### MySQL8中SQL执行原理

前面的结构图很复杂，我们需要抓取最核心的部分: SQL的执行原理。不同的DBMS的SQL的执行原理是相通的，只是在不同的软件中，各有各的实现路径。

既然一条SQL语句会经历不同的模块，那我们就来看下，在不同的模块中，SQL执行所使用的资源(时间)是怎样的。如何在MySQL中对一条SQL语句的执行时间进行分析。

##### 确认Profiling是否开启

了解查询语句底层执行的过程: select eeprofiling; 或者show variables like %profiling%' 查看是否开启计划。开启它可以让MySQL收集在SQL执行时所使用的资源情况，命令如下：

```
mysql> select ceprofiling;

mysql> show variables like 'profiling';
```

profling=0代表关闭，我们需要把profling打开，即设置为1：

```
mysql> set profiling=1;
```

Profiling功能由MySQL会话变量：profiling控制。默认是OFF (关闭状态)。

##### 多次执行相同的SQL语句

然后我们执行一个SQL查询(你可以执行任何一个SQL查询) :

```
mysq1> select * from employees;
```

##### 查看profiles

show profiles和show profile语句可以展示当前会话(退出session后，profiling重置为0)中执行语句的资源使用情况。

查看当前会话所产生的所有profiles：

![image-20230125125753984](/image-20230125125753984.png)

##### 查看profile

mysql8中没有缓存查询。因此，两次查询的过程是一致的

![image-20230125125828727](/image-20230125125828727.png)

![image-20230125125846715](/image-20230125125846715.png)

#### MySQL5.7中SQL执行原理

##### 配置文件中开启查询缓存

查看缓存状态

![image-20230125125921539](/image-20230125125921539.png)

在/etc/my.cnf配置文件中新增一行：

```
query_cache_type=1
```

##### 重启mysql服务

```
systemctl restart mysqld
```

##### 开启查询执行计划

由于重启过服务，需要重新执行如下指令，开启profiling。

```
mysql> set profling=1;
```

##### 执行语句两次

```
mysql> select * from locations;

mysql> select * from locations;
```

##### 查看profiles

![image-20230125125938251](/image-20230125125938251.png)

##### 查看profile

mysql5.7中，通过配置可以开启查询缓存

因此第二次查询时，击中了查询缓存

![image-20230125130047831](/image-20230125130047831.png)

![image-20230125130056582](/image-20230125130056582.png)

但只要有一点点的不同，就不会击中

#### SQL语法顺序

**FROM、ON 、JOIN、WHERE、GROUP BY、AGG_FUNC(函数)、WITH、HAVING、SELECT、UNION、DISTINCT 、ORDER BY、LIMIT**

#### Oracle中的SQL执行流程

![image-20230125130257243](/image-20230125130257243.png)

### 数据库缓冲池

**InnoDB**存储引擎是以页为单位来管理存储空间的，我们进行的增删改查操作其实本质上都是在访问页面(包括读页面、写页面、创建新页面等操作) ，而磁盘I/O需要消耗的时间很多,而在内存中进行操作,效率则会高很多，为了能让数据表或者索引中的数据随时被我们所用，DBMS 会申请**占用内存来作为数据缓冲池**，在真正访问页面之前，需要把在磁盘上的页缓存到内存中的**Buffer Pool** 之后才可以访问。

这样做的好处是可以让磁盘活动最小化，从而**减少与磁盘直接进行I/O 的时间**。要知道，这种策略对提升SQL语句的查询性能来说至关重要。如果索引的数据在缓冲池里,那么访问的成本就会降低很多。

#### 缓冲池vs查询缓存

缓冲池和查询缓存是一个东西吗？不是！！！

##### 缓冲池

首先我们需要了解在InnoDB存储引擎中，缓冲池都包括了哪些。

在InnoDB存储引擎中有一部分数据会放到内存中，缓冲池则占了这部分内存的大部分，它用来存储各种数据的缓存，如下图所示: 

![image-20230125130737189](/image-20230125130737189.png)

**缓冲池的重要性**

在数据库中，各种信息都是以页的形式存放在表空间的，而所谓的表空间只不是InnDB对文件系统上的实际文件的抽象，数据依旧存储在磁盘上（磁盘的运行速度很慢，无法和高速的CPU配合），缓冲池就可以帮助我们消除磁盘与CPU之间的鸿沟。

当我们需要访问数据时，InnDB存储引擎会将需要访问的**完整的页的数据全部加载到内存中**。也就是说即使我们只需要访问一个页的一条记录，那也需要先把整个页的数据加载到内存中。将整个页加载到内存中后就可以进行读写访问了，在进行完读写访问之后并不着急把该页对应的内存空间释放掉，而是将其**缓存**起来，这样将来有请求再次访问该页面时，就可以**省去磁盘IO**的开销了。

**缓存原则**

“位置 * 频次"这个原则，可以帮我们对 I/O访问效率进行优化。

首先，位置决定效率，提供缓冲池就是为了在内存中可以直接访问数据。

其次，频次决定优先级顺序。因为缓冲池的大小是有限的，比如磁盘有200G，但是内存只有16G缓冲池大小只有1G，就无法将所有数据都加载到缓冲池里，这时就涉及到优先级顺序，会优先对使用频次高的热数据进行加载。

**缓冲池的预读特性**

了解了缓冲池的作用之后，我们还需要了解缓冲池的另外一个特性：预读

缓冲池的作用就是提升I/O效率,而我们进行读取数据的时候存在一个“ 局部性原理”，也就是说我们使用了一些数据，大概率还会使用它周围的一些数据，因此采用“预读”的机制提前加载，可以减少未来可能的磁盘I/O操作。

##### 查询缓存

那么什么是查询缓存呢?

查询缓存是提前把查询结果缓存起来，这样下次不需要执行就可以直接拿到结果。需要说明的是,在MySQL中的查询缓存，不是缓存查询计划，而是查询对应的结果。因为命中条件苛刻，而且只要数据表发生变化，查询缓存就会失效，因此命中率低。

缓冲池服务于数据库整体的I/O操作，它们的共同点都是通过缓存的机制来提升效率。

#### 缓冲池如何读取数据

缓冲池管理器会尽量将经常使用的数据保存起来，在数据库进行页面读操作的时候，首先会判断该页面是否在缓冲池中，如果存在就直接读取，如果不存在，就会通过内存或磁盘将页面存放到缓冲池中再进行读取。

缓存在数据库中的结构和作用如下图所示：

![image-20230125131107294](/image-20230125131107294.png)

**如果我们执行SQL语句的时候更新了缓存池中的数据，那么这些数据会马上同步到磁盘上吗?**

实际上，当我们对数据库中的记录进行修改的时候，首先会修改缓冲池中页里面的记录信息，然后数据库会**以一定的频率刷新到磁盘上**。注意并不是每次发生更新操作，都会立刻进行磁盘回写。缓冲池会采用一种叫做**checkpoint的机制**将数据回写到磁盘上，这样做的好处就是提升了数据库的整体性能。

比如，当**缓冲池不够用**时，需要释放掉一些不常用的页，此时就可以强行采用checkpoint的方式，将不常用的脏页回写到磁盘上，然后再从缓冲池中将这些页释放掉。这里脏页(dirty page)指的是缓冲池中被修改过的页，与磁盘上的数据页不一致。

#### buffer pool淘汰机制

整体采用的时LRU策略

![image-20230201170732260](/image-20230201170732260.png)

数据页第一次加载进来时，是放在冷数据的头部（防止立马把热数据区充满，以及在冷数据区被淘汰）

**冷数据区的缓存什么时候放入热数据区？**

Mysql规定了一个规则，在innodb_old_blocks_time参数中，默认值为1000ms。即当数据进入到冷数据区头部时，只要一秒内被使用到，就会被移动到热数据区头部

**为什么是一秒？**

因为通过预读机制和全表扫描加载进来的数据页通常是1秒内就加载了很多，然后对他们访问一下，这些都是1秒内完成，他们会存放在冷数据区域等待刷盘清空，基本上不太会有机会放入到热数据区域，除非在1秒后还有人访问，说明后续可能还会有人访问，才会放入热数据区域的头部。

#### 查看/设置缓冲池的大小

如果你使用的是MySQL MyISAM存储引擎，它只缓存索引，不缓存数据，对应的键缓存参数为 key_buffer_size，你可以用它进行查看。

如果你使用的是InnoDB存储引擎，可以通过查看innodb_buffer_pool_size变量来查看缓冲池的大小。命令如下:

```
show variables like 'innodb_buffer_pool_size';
```

![image-20230125131917598](/image-20230125131917598.png)

你能看到此时InnoDB的缓冲池大小只有134217728/1024/1024=128MB.我们可以修改缓冲池大小，比如改为256MB，方法如下：

```
set global innodb_buffer_pool_size = 268435456 ;
```

或者：

```
[server]
'innodb_buffer_pool_size'= 268435456 
```

![image-20230125132059350](/image-20230125132059350.png)

#### 多个Buffer Pool实例

Buffer Pool本质是InnoDB向操作系统申请的**一块连续的内存空间**，在多线程环境下，访问Buffer Pool中的数据都需要**加锁**处理。在Buffer Pool特别大而且多线程并发访问特别高的情况下，单一的Buffer Pool可能会影响请求的处理速度。所以在Buffer Pool特别大的时候，我们可以把它们**拆分成若干个小的Buffer Pool**，每个Buffer Pool都称为一个实例，它们都是独立的，独立的去申请内存空间，独立的管理各种链表。所以在多线程并发访问时并不会相互影响，从而提高并发处理能力。

我们可以在服务器启动的时候通过设置**innodb_buffer_pool_instances**的值来修改Buffer Pool实例的个数，比方说这样：

```
[server]
innodb_buffer_pool_inptances = 2
```

这样就表明我们要创建2个Buffer Pool 实例。

我们看下如何查看缓冲池的个数，使用命令：

```
show variables like 'innodb_buffer_pool_instances' :
```

![image-20230125132151374](/image-20230125132151374.png)

那每个Buffer Pool 实例实际占多少内存空间呢?其实使用这个公式算出来的：

```
innodb_buffer_pool_size/innodb_buffer_pool_instances
```

也就是总共的大小除以实例的个数，结果就是每个Buffer Pool 实例占用的大小。

不过也不是说Buffer Pool实例创建的越多越好，分别**管理各个Buffer Pool也是需要性能开销的**，InnoDB规定，当innodb_buffer_pool_size的值小于1G的时候设置多个实例是无效的，InnoDB会默认把innodb_buffer_pool_instances的值修改为1。**而我们鼓励在Buffer Pool大于或等于1G的时候设置多个Buffer Pool实例。**

#### 引申问题

Buffer Pool是MySQL内存结构中十分核心的一个组成，你可以先把它想象成一个黑盒子。

**黑盒下的更新数据流程**

当我们查询数据的时候，会先去Buffer Pool中查询。如果Buffer Pool中不存在，存储引擎会先将数据从磁盘加载到Buffer Pool中，然后将数据返回给客户端；同理，当我们更新某个数据的时候，如果这个数据不存在于Buffer Pool，同样会先数据加载进来，然后修改修改内存的数据。被修改过的数据会在之后统一刷入磁盘。

![image-20230125133605553](/image-20230125133605553.png)

这个过程看似没啥问题，实则是有问题的。假设我们修改Buffer Pool中的数据成功，但是还没来得及将数据刷入磁盘MySQL就挂了怎么办？按照上图的逻辑，此时更新之后的数据只存在于Buffer Pool中，如果此时MySQL宕机了，这部分数据将会永久地丢失；

缓冲池会以一定的频率将自己的信息刷入磁盘。因此，当数据更新时，并不会立即修改磁盘数据。就会导致脏数据。或是更新了一半，发生了宕机，只修改了一部分的数据。这时，就需要引入事务的概念。

再者，我更新到一半突然发生错误了，想要回滚到更新之前的版本，该怎么办？连数据持久化的保证、事务回滚都做不到还谈什么崩溃恢复？

解决方法：**Redo Log & Undo Log**

## 存储引擎

为了管理方便，人们把**连接管理**、**查询缓存**、 **语法解析**、**查询优化**这些并不涉及真实数据存储的功能划分为**MySQL server**的功能，把真实存取数据的功能划分为**存储引擎**的功能。所以在**MySQL server** 完成了查询优化后，只需按照生成的**执行计划**调用底层存储引擎提供的API，获取到数据后返回给客户端就好了。

MySQL中提到了存储引擎的概念。简而言之，**存储引擎就是指表的类型**。其实存储引擎以前叫做**表处理器**，后来改名为**存储引擎**，它的功能就是接收上层传下来的指令，然后对表中的数据进行提取或写入操作。 

例如：

InnDB中，表的结构是b.frm，b.ibd

MyISAM中，表的结构是b.frm，b.MYD，b.MYI

### 查看存储引擎

```
show engines;
```

![image-20230125135236201](/image-20230125135236201.png)

部分参数：

- Transactions 参数表示存储引擎是否支持事务: YES表示支持，N0表示不支持。
- XA参数表示存储引擎所支持的分布式是否符合XA规范: YES表示支持，NO表示不支持。代表着该存储引擎是否支持分布式事务。
- Savepoints 参数表示存储引擎是否支持事务处理的保存点: YES表示支持，N0表示不支持。也就是说，该存储引擎是否支持部分事务回滚。

### 设置系统默认的存储引擎

- 查看默认的存储弓|擎:

  - ```
    show variables like '%storage_engine%' ;
    #或
    SELECT @@default_storage_engine;
    ```

- 修改默认的存储引擎

  - ```
    SET DEFAULT_STORAGE_ENGINE=MyISAM;
    ```

  - 或者修改my.cnf

  - ```
    default-storage-engine-MyISAM
    
    #重启服务
    systemctl restart mysqld. service
    ```

  - 接着只要建表，都是设置的存储引擎

### 设置表的存储引擎

存储引擎是负责对表中的数据进行提取和写入工作的，我们可以为不同的表设置不同的存储引擎,也就是说不同的表可以有不同的物理存储结构，不同的提取和写入方式。

#### 创建表时指定存储引擎

我们之前创建表的语句都没有指定表的存储引擎,那就会使用默认的存储引擎InnoDB。如果我们想显式的指定一下表的存储引擎，那可以这么写:

```
CREATE TABLE 表名(
	建表语句;
)ENGINE = 存储引擎名称;
```

#### 修改表的存储引擎

如果表已经建好了，我们也可以使用下边这个语句来修改表的存储引擎：

```
ALTER TABLE 表名ENGINE =存储引擎名称:
```

### 引擎介绍

#### InnDB引擎：具备外键支持功能的事务存储引擎

**优点**

- MySQL从3.23.34a开始就包含InnoDB存储引擎。**大于等 于5.5之后，默认采用InnoDB引擎**。
- InnoDB是MySQL的**默认事务型引擎**，它被设计用来处理大量的短期(short-lived)事务。可以确保事务的完整提交(Commt)和回滚(Rollback)。
- 除了增加和查询外，还需要更新、删除操作，那么，应优先选择InnoDB存储引擎。
- **除非有非常特别的原因需要使用其他的存储引擎，否则应该优先考虑InnoDB引擎。**
- 数据文件结构: 
  - 表名.frm存储表结构(MySQL8.0时， 合并在表名.ibd中)
  - 表名.ibd存储数据和索引
- InnoDB是为**处理巨大数据最的最大性能设计**。
  - 在以前的版本中，字典数据以元数据文件、非事务表等来存储。现在这些元数据文件被删除了。比如: `.frm`， `.par`， `.trn`， `.isl`， `.db.opt`等都在MySQL8.0中不存在了。

**缺点**

- 对比MyISAM的存储引擎，**InnoDB写的处理效率差一些**，并且会占用更多的磁盘空间以保存数据和索引。
- MyISAM只缓存索引，不缓存真实数据；InnoDB不仅缓存索引还要缓存真实数据，**对内存要求较高**，而且内存大小对性能有决定性的影响。
- **不仅对内存要求较高，而且处理效率差**，是因为InnDB将数据和索引在一起存储

#### MyISAM引擎：主要的非事务处理存储引擎

- MylISAM提供了大量的特性，包括全文索引、压缩、空间函数(GIS)等，但MyISAM**不支持事务、行级锁、外键**，有一个毫无疑问的缺陷就是**崩溃后无法安全恢复**。
- **5.5之前默认的存储引擎**
- 优势是访问的**速度快**，对事务完整性没有要求或者以SELECT、 INSERT为主的应用
- 针对数据统计有额外的常数存储。故而count(*) 的查询效率很高
- 数据文件结构: (在《第02章_MySQL数据目录》章节已讲)
  - 表名.frm存储表结构
  - 表名.MYD存储数据(MYData)
  - 表名.MYI存储索引(MYIndex)
- 应用场景:只读应用或者以读为主的业务

#### Archive引擎：用于数据存档

- **archive**是**归档**的意思，仅仅支持**插入**和**查询**两种功能(行被插入后不能再修改)。
- 在MySQL5.5以后**支持索引**功能。
- 拥有很好的压缩机制，使用**zlib压缩库**，在记录请求的时候实时的进行压缩，经常被用来作为仓库使用。
- 创建ARCHIVE表时， 存储引擎会创建名称以表名开头的文件。数据文件的扩展名为**.ARZ**。
- 根据英文的测试结论来看，同样数据量下，**Archive表比MyISAM表要小大约75%， 比支持事务处理的InnoDB表小大约83%**。
- ARCHIVE存储引擎采用了**行级锁**。该ARCHIVE引擎支持**AUTO_ INCREMENT**列属性。AUTO_ INCREMENT列可以具有唯一索引或非唯一索引。尝试在任何其他列上创建索引会导致错误。
- Archive表**适合日志和数据采集(档案)**类应用；**适合存储大量的独立的作为历史记录的数据**。拥有**很高的插入速度,但是对查询的支持较差**。
- **下表展示了ARCHIVE存储引擎功能**

![image-20230125140132743](/image-20230125140132743.png)

#### Blackhole引擎：丢弃写操作，读操作会返回空内容

- Blackhole弓擎没有实现任何存储机制，它会**丢弃所有插入的数据**，不做任何保存。
- 但服务器会记录Blackhole表的日志，所以可以用于复制数据到备库，或者简单地记录到日志。但这种应用方式会碰到很多问题，因此并不推荐。

#### CSV引擎：存储数据时，以逗号分隔各个数据项

- CSV引擎可以将普通的CSV文件作为MySQL的表来处理，但不支持索引。
- CSV引擎可以作为一种数据交换的机制，非常有用。
- CSV存储的数据直接可以在操作系统里，用文本编辑器，或者excel读取。
- 对于数据的快速导入、导出是有明显优势的。

创建CSV表时，服务器会创建一个纯文本数据文件, 其名称以表名开头并带有**.CSV** 扩展名。当你将数据存储到表中时，存储弓|擎将其以逗号分隔值格式保存到数据文件中。

使用案例如下：

建表

![image-20230125140226089](/image-20230125140226089.png)

存储文件

![image-20230125140241764](/image-20230125140241764.png)

#### Memory引擎：置于内存的表（数据在内存上）

**概述：**

Memory采用的逻辑介质是**内存，响应速度很快**，但是当mysqld守护进程崩溃的时候**数据会丢失**。另外，**要求存储的数据是数据长度不变的格式**，比如，Blob和Text类型的数据不可用(长度不固定的)。

**主要特征：**

- Memory同时**支持哈希(HASH) 索引和B+树索引**。
  - 哈希索引相等的比较快，但是对于范围的比较慢很多。
  - **默认使用哈希(HASH) 索引**，其速度要比使用8型树(BTREE)索引快。
  - 如果希望使用B树索引，可以在创建索弓时选择使用。
- Memory表至少比MyISAM表要**快一个数量级**。
- MEMORY 表的**大小是受到限制的**。表的大小主要取决于两个参数，分别是max_rows和max_heap_table_size.其中，max_ rows可以在创建表时指定； max_ heap_ table_ size的大小默认为16MB，可以按需要进行扩大。
- 数据文件与索引文件分开存储。
  - 每个基于MEMORY存储引擎的表实际对应一个磁盘文件， 该文件的文件名与表名相同，类型为**frm类型**，该文件中只存储表的结构，而**其数据文件都是存储在内存中的**。
  - 这样有利于数据的快速处理，提供整个表的处理效率。
- 缺点：其数据易丢失，生命周期短。基于这个缺陷，选择MEMORY存储引擎时需要特别小心。

**使用Memory存储引擎的场景：**

1. **目标数据比较小**，而且非常**频繁的进行访问**，在内存中存放数据，如果太大的数据会造成**内存溢出**。可以通过参数**max_heap_table_size**控制Memory表的大小，限制Memory表的最大的大小。
2. 如果**数据是临时的**，而且**必须立即可用**得到，那么就可以放在内存中。
3. 存储在Memory表中的数据如果突然间**丢失的话也没有太大的关系**。

#### Federated引擎：访问远程表

- Federated引擎是访问其他MySQL服务器的一个代理，尽管该引擎看起来提供了一种很好的跨服务器的灵活性，但也经常带来问题，因此默认是禁用的。

#### Merge引擎：管理多个MyISAM表构成的表集合

#### NDB引擎：MySQL集群专用存储引擎

也叫做NDB Cluster存储引擎，主要用于MySQL Cluster 分布式集群环境，类似于Oracle的RAC集群。

### MyISAM和InnDB

**很多人对InnoDB和MyISAM的取舍存在疑问，到底选择哪个比较好呢？**

MySQL5.5之前的默认存储引擎是MyISAM，5.5之后改为 了InnoDB。

首先对于InnoDB存储引擎，提供了良好的事务管理、崩溃修复能力和并发控制。因为InnoDB存储引擎**支持事务**，所以对于要求事务完整性的场合需要选择InnoDB，比如数据操作除了插入和查询以外还包含有很多更新、删除操作，像财务系统等对数据准确性要求较高的系统。缺点是**其读写效率稍差，占用的数据空间相对比较大。**

其次对于MyISAM存储引擎，如果是**小型应用**，系统**以读操作和插入操作为主**，只有很少的更新、删除操作，并且事务的要求没有那么高，则可以选择这个存储引擎。MyISAM存储引擎的优势在于**占用空间小，处理速度快**；缺点是**不支持事务**的完整性和并发性。

这两种引擎各有特点，当然你也可以在MySQL中，针对不同的数据表，可以选择不同的存储引擎。

| 对比项         | MyISAM                                                 | InnDB                                                        |
| -------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| 外键           | 不支持                                                 | 支持                                                         |
| 事务           | 不支持                                                 | 支持                                                         |
| 行表锁         | 表锁，即使操作一条数据也会锁住整个表，不适合高并发操作 | 行锁，操作时只锁一行，不对其他行有影响，适合高并发的操作     |
| 缓存           | 只缓存索引，不缓存真实数据                             | 不仅缓存索引还要缓存真实数据，对内存要求较高，而且内存大小对性能有决定性的影响 |
| 自带系统表使用 | Y                                                      | N                                                            |
| 关注点         | 性能：节省资源、消耗少、简单业务                       | 事务：并发写、事务、更大资源                                 |
| 默认安装       | Y                                                      | Y                                                            |
| 默认使用       | N                                                      | Y                                                            |

### 阿里巴巴、淘宝用的哪个

![image-20230125141217254](/image-20230125141217254.png)

- **Percona**为MySQL数据库服务器进行了改进，在功能和性能上较MySQL有很显著的提升。
- 该版本提升了在高负载情况下的InnoDB的性能、为DBA提供一些非常有用的性能诊断工具；另外有更多的参数和命令来控制服务器行为。
- 该公司新建了一款存储引擎叫 **Xtradb**完全可以替代**Innodb**，并且在性能和并发上做得更好。
- 阿里巴巴大部分mysql数据库其实使用的percona的原型加以修改。

### 课外补充

#### InnDB表的优势

InnoDB存储引擎在实际应用中拥有诸多优势，比如操作便利、提高了数据库的性能、维护成本低等。如果由于硬件或软件的原因导致服务器崩溃,那么在重启服务器之后不需要进行额外的操作。InnoDB崩溃恢复功能自动将之前提交的内容定型,然后撤销没有提交的进程，重启之后继续从崩溃点开始执行。

InnoDB存储引擎在主内存中维护缓冲池，高频率使用的数据将在内存中直接被处理。这种缓存方式应用于多种信息，加速了处理进程。

在专用服务器上,物理内存中高达80%的部分被应用于缓冲池。如果需要将数据插入不同的表中，可以设置外键加强数据的完整性。更新或者删除数据，关联数据将会被自动更新或删除。如果试图将数据插入从表,但在主表中没有对应的数据，插入的数据将被自动移除。如果磁盘或内存中的数据出现崩溃，在使用脏数据之前，校验和机制会发出警告。当每个表的主键都设置合理时，与这些列有关的操作会被自动优化。插入、更新和删除操作通过做改变缓冲自动机制进行优化。InnoDB不仅支持当前读写，也会缓冲改变的数据到数据流磁盘。

InnoDB的性能优势不只存在于长时运行查询的大型表。在同一列多次被查询时，自适应哈希索引会提高查询的速度。使用InnoDB可以压缩表和相关的索引，可以在不影响性能和可用性的情况下创建或删除索引。对于大型文本和BLOB数据，使用动态行形式，这种存储布局更高效。通过查询INFORMATION_SCHEMA库中的表可以监控存储引擎的内部工作。在同一个语句中，InnoDB表可以与其他存储引擎表混用。即使有些操作系统限制文件大小为2GB，InnoDB仍然可以处理。当处理大数据量时，InnoDB兼顾CPU, 以达到最大性能。

#### InnDB和ACID模型

ACID模型是一系列数据库设计规则，这些规则着重强调可靠性，而可靠性对于商业数据和任务关键型应用非常重要。MySQL包含类似nnoDB存储引擎的组件，与ACID模型紧密相连，这样出现意外时，数据不会崩溃，结果不会失真，如果依赖ACID模型，可以不使用一致性检查和崩溃恢复机制。如果拥有额外的软件保护，极可靠的硬件或者应用可以容忍一小部分的数据丢失和不一致,可以将MySQL设置调整为只依赖部分ACID特性,以达到更高的性能。下面讲解InnoDB存储引擎与ACID模型相同作用的四个方面。

1. 原子性方面

   ACID的原子方面主要涉及InnoDB事务，与MySQL相关的特性主要包括：

   - 自动提交设置。
   - COMMIT语句。
   - ROLLBACK语句。
   - 操作INFORMATION_SCHEMA库中的表数据。

2. 一致性方面

   ACID模型的一致性主要涉及保护数据不崩溃的内部InnoDB处理过程，与MySQL相关的特性主要包括:

   - InnoDB双写缓存。
   - InnoDB崩溃恢复。

3. 隔离方面

   隔离是应用于事务的级别，与MySQL相关的特性主要包括:

   - 自动提交设置。
   - SET ISOLATION LEVEL语句。
   - InnoDB锁的低级别信息。

4. 耐久性方面

   ACID模型的耐久性主要涉及与硬件配置相互影响的MySQL软件特性。由于硬件复杂多样化，耐久性方面没有具体的规则可循。与MySQL相关的特性有：

   - InnoDB双写缓存，通过innodb_ doublewrite配置项配置。
   - 配置项innodb_flush_log_at_trx_commit。
   - 配置项sync_binlog。
   - 配置项innodb_file_ per_table。
   - 存储设备的写入缓存。
   - 存储设备的备用电池缓存。
   - 运行MySQL的操作系统。
   - 持续的电力供应。
   - 备份策略。
   - 对分布式或托管的应用，最主要的在于硬件设备的地点以及网络情况。

#### InnDB框架

1. 缓冲池
   - 缓冲池是主内存中的一部分空间，用来缓存已使用的表和索引数据。缓冲池使得经常被使用的数据能够直接在内存中获得，从而提高速度。
2. 更改缓存
   - 更改缓存是一个特殊的数据结构，当受影响的索引页不在缓存中时，更改缓存会缓存辅助索引页的更改。索引页被其他读取操作时会加载到缓存池，缓存的更改内容就会被合并。不同于集群索引，辅助索引并非独一无二的。当系统大部分闲置时，清除操作会定期运行，将更新的索引页刷入磁盘。更新缓存合并期间，可能会大大降低查询的性能。在内存中，更新缓存占用一部分InnoDB缓冲池。在磁盘中，更新缓存是系统表空间的一部分。更新缓存的数据类型由innodb_change_ buffering配置项管理。
3. 自适应哈希索引
   - 自适应哈希索引将负载和足够的内存结合起来，使得InnoDB像内存数据库一样运行， 不需要降低事务上的性能或可靠性。这个特性通过innodb_adaptive_hash_index选项配置,或者通过--skip innodb_adaptive_hash_ index命令行在服务启动时关闭。
4. 重做日志缓存
   - 重做日志缓存存放要放入重做日志的数据。重做日志缓存大小通过innodb_log_buffer_size配置项配置。重做日志缓存会定期地将日志文件刷入磁盘。大型的重做日志缓存使得大型事务能够正常运行而不需要写入磁盘。
5. 系统表空间
   - 系统表空间包括InnoDB数据字典、双写缓存、更新缓存和撤销日志，同时也包括表和索引数据。多表共享,系统表空间被视为共享表空间。
6. 双写缓存
   - 双写缓存位于系统表空间中，用于写入从缓存池刷新的数据页。只有在刷新并写入双写缓存后，InnoDB才会将数据页写入合适的位置。
7. 撤销日志
   - 撤销日志是一系列与事务相关的撤销记录的集合, 包含如何撤销事务最近的更改。如果其他事务要查询原始数据，可以从撤销日志记录中追溯未更改的数据。撤销日志存在于撤销日志片段中，这些片段包含于回滚片段中。
8. 每个表一个文件的表空间
   - 每个表一个文件的表空间是指每个单独的表空间创建在自身的数据文件中，而不是系统表空间中。这个功能通过innodb_ file_ per_table配置项开启。每个表空间由一个单独的.ibd数据文件代表,该文件默认被创建在数据库目录中。
9. 通用表空间
   - 使用CREATE TABLESPACE语法创建共享的InnoDB表空间。通用表空间可以创建在MySQL数据目录之外能够管理多个表并支持所有行格式的表。
10. 撤销表空间
    - 撤销表空间由一个或多个包含撤销日志的文件组成。撤销表空间的数量由innodb_undo_tablespaces配置项配置。
11. 临时表空间
    - 用户创建的临时表空间和基于磁盘的内部临时表都创建于临时表空间。innodb_temp_data_ file_ path配置项定义了相关的路径、名称、大小和属性。如果该值为空，默认会在innodb. *data* home_dir变量指定的目录下创建一个自 动扩展的数据文件。
12. 重做日志
    - 重做日志是基于磁盘的数据结构，在崩溃恢复期间使用，用来纠正数据。正常操作期间，重做日志会将请求数据进行编码，这些请求会改变InnoDB表数据。遇到意外崩溃后，未完成的更改会自动在初始化期间重新进行。

# 14 索引及调优

## 索引的数据结构

### 为什么使用索引

索引是存储引擎用于快速找到数据记录的一种数据结构，就好比一本教课书的目录部分， 通过目录中找到对应文章的页码，便可快速定位到需要的文章。MySQL中也是一样的道理， 进行数据查找时，首先查看查询条件是否命中某条索引，符合则**通过索引查找**相关数据，如果不符合则需要**全表扫描**，即需要一条一条地查找记录，直到找到与条件符合的记录。

![image-20230125143917859](/image-20230125143917859.png)

如上方左侧图所示，在没有索引的情况下，数据分布在硬盘的不同位置上，读取数据是，摆臂需要前后摆动来查找数据，这样的操作是十分费时的。

如果数据按顺序摆放，要查询第六个的数据，依旧要一层一层遍历（计算机中会有成千上万的数据），意味着需要很多次磁盘I/O才能找到。

CPU必须前序磁盘查找这条记录，找到之后加载到内存，在堆数据进行处理。这个过程最好时间的就是磁盘I/O（设计到磁盘旋转时间（转速越快），磁头的寻道时间（速度慢，费时间））。

![image-20230125144033622](/image-20230125144033622.png)

如上图，给数据使用**二叉搜索树**这样的数据结构进行存储（根据二叉搜索树的原理查找数据）。

建立索引的目的就是为了**减少磁盘I/O的次数**，加快查询速率。

### 索引及其优缺点

#### 索引概述

MySQL官方对索引的定义为：**索引(Index) 是（帮助MySQL高效获取数据的）数据结构。**

**索引的本质**：索引是数据结构。你可以简单理解为“排好序的快速查找数据结构”，满足特定查找算法。这些数据结构以某种方式指向数据，这样就可以在这些数据结构的基础上实现**高级查找算**法。

**索引是在存储引擎中实现的**，因此每种存储引擎的索引不一定完全相同，并且每种存储引擎不一定支持所有索引类型。同时，存储引擎可以定义每个表的**最大索引数**和**最大索引长度**。所有存储引擎支持每个表至少16个索引，总索引长度至少为256字节。有些存储引擎支持更多的索引数和更大的索引长度。

#### 优点

1. 类似大学图书馆建书目索引，提高数据检索的效率，降低**数据库的IO成本**，这也是创建索引最主要的原因。
2. 通过创建唯一索引，可以保证数据库表中每一行**数据的唯一性** 。
3. 在实现数据的参考完整性方面，可以**加速表和表之间的连接**。换句话说，对于有依赖关系的子表和父表联合查询时，可以提高查询速度。
4. 在使用分组和排序子句进行数据查询时，可以显著**减少查询中分组和排序的时间**，降低了CPU的消耗。

#### 缺点

增加索引也有许多不利的方面，主要表现在如下几个方面：

1. 创建索引和维护索引要**耗费时间**，并且随着数据量的增加，所耗费的时间也会增加。
2. 索引需要占**磁盘空间**，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，**存储在磁盘上**，如果有大量的索引，索引文件就可能比数据文件更快达到最大文件尺寸。
3. 虽然索引大大提高了查询速度，同时却会**降低更新表的速度**。当对表中的数据进行增加、删除和修改的时候，索引也要动态地维护，这样就降低了数据的维护速度。

因此，选择使用索引时，需要综合考虑索引的优点和缺点。

> 索引可以提高查询的速度，但是会影响插入记录的速度。这种情况下，最好的办法是先删除表中的索引，然后插入数据，插入完成后再创建索引。

### InnDB中索引的推演

#### 索引之前的查找

```
Select [列名列表] FROM 表名 WHERE 列名=XXX；
```

##### 在一个页中查找

假设目前表中的记录比较少，所有的记录都可以被存放到一个页中，在查找记录的时候可以根据搜索条件的不同分为两种情况:

- 以主键为搜索条件
  - 可以在页目录中使用**二分法**快速定位到对应的槽，然后再遍历该槽对应分组中的记录即可快速找到指定的记录。
- 以其他列作为搜索条件
  - 因为在数据页中并没有对非主键列建立所谓的页目录，所以我们无法通过二分法快速定位相应的槽。这种情况下只能从**最小记录**开始**依次遍历**单链表中的每条记录，然后对比每条记录是不是符合搜索条件。很显然，这种查找的效率是非常低的。

##### 在很多页查找

大部分情况下我们表中存放的记录都是非常多的，需要好多的数据页来存储这些记录。在很多页中查找记录的话可以分为两个步骤：

1. 定位到记录所在的页。
2. 从所在的页内中查找相应的记录。

在没有索引的情况下，不论是根据主键列或者其他列的值进行查找，由于我们并不能快速的定位到记录所在的页，所以只能从**第一个页**沿着**双向链表**一直往下找，在每一个页中根据我们上面的查找方式去查找指定的记录。因为要遍历所有的数据页，所以这种方式显然是**超级耗时**的。如果一个表有一亿条记录呢？此时**索引**应运而生。

#### 设计索引

```
mysql> CREATE TABLE index_demo(
	-> 	c1 INT,
	-> 	c2 INT,
	-> 	c3 CHAR(1)，
	-> 	PRIMARY KEY(c1)
	-> ) ROW_FORMAT = Compact;
```

这个新建的**index_ demo**表中有2个INT类型的列，1个CHAR(1)类型的列， 而且我们规定了c1列为主键，这个表使用**Compact**行格式来实际存储记录的。这里我们简化了index _demo表的行格式示意图：

![image-20230125153330350](/image-20230125153330350.png)

我们只在示意图里展示记录的这几个部分：

- **record_type**：记录头信息的一项属性，表示记录的类型，0表示普通记录、2表示最小记录、3表示最大记录、1暂时还没用过，下面讲。
- **next_record**：记录头信息的一项属性，表示下一条地址相对于本条记录的地址偏移量， 我们用箭头来表明下一条记录是谁。
- **各个列的值**：这里只记录在**index_demo**表中的三个列，分别是`c1`、`c2`和`c3` 。
- **其他信息**：除了上述3种信息以外的所有信息， 包括其他隐藏列的值以及记录的额外信息。

把一些记录放到页里的示意图就是：

![image-20230125153347138](/image-20230125153347138.png)

##### 一个简单的索引设计方案

我们在根据某个搜索条件查找一些记录时为什么要遍历所有的数据页呢?因为各个页中的记录并没有规律，我们并不知道我们的搜索条件匹配哪些页中的记录，所以不得不依次遍历所有的数据页。所以如果我们想快速的定位到需要查找的记录在哪些数据页中该咋办?我们可以为快速定位记录所在的数据页而建立一个目录，建这个目录必须完成下边这些事：

- **下一个数据页中用户记录的主键值必须大于上一个页中用户记录的主键值。**

  假设:每个数据页最多能存放3条记录(实际上一个数据页非常大，可以存放下好多记录)。有了这个假设之后我们向index_ demo 表插入3条记录：

  ```
  mysql> INSERT INTO index_demo VALUES(1,4,'u'),(3,9,'d'),(5, 3,'y');
  Query OK,3 rows affected (0.01 sec)
  Records: 3 Duplicates: 0 Warnings: 0
  ```

  那么这些记录已经按照主键值的大小串联成一个单向链表了，如图所示：

  ![image-20230125154243079](/image-20230125154243079.png)

  从图中可以看出来，index_ demo表中的3条记录都被插入到了编号为10的数据页中了。此时我们再来插入一条记录：

  ```
  mysql> INSERT INTO index_ demo VALUES(4， 4，'a');
  ```

  因为页10最多只能放3条记录，所以我们不得不再分配一个新页：

  ![image-20230125154507413](/image-20230125154507413.png)

  

  这个过程表明了在对页中的记录进行增删改操作的过程中，我们必须通过一些诸如记录移动的操作来始终保证这个状态一直成立：下一个数据页中用户记录的主键值必须大于上一个页中用户记录的主键值。这个过程我们称为页分裂。

- 给所有的页建立一个目录项

  由于数据页的**编号可能是不连续的**，所以在向index_demo表中插入许多条记录后，可能是这样的效果:

  ![image-20230125154544323](/image-20230125154544323.png)

  因为这些16KB的页在物理存储上是不连续的，所以如果想从这么多页中根据主键值快速定位某些记录所在的页，我们需要给它们做个目录，每个页对应一个目录项，每个目录项包括下边两个部分：

  - 页的用户记录中最小的主键值，我们用key来表示。
  - 页号，我们用page_no表示。

  所以我们为上边几个页做好的目录就像这样子：

  ![image-20230125154651591](/image-20230125154651591.png)     

  以**页28**为例，它对应**目录项2**，这个目录项中包含着该页的页号**28**以及该页中用户记录的最小主键值5。我们只需要把几个目录项在物理存储器上连续存储(比如:数组)， 就可以实现根据主键值快速查找某条记录的功能了。比如:查找主键值为**20**的记录，具体查找过程分两步:

  1. 先从目录项中根据**二分法**快速确定出主键值为**20**的记录在**目录项3**中(因为12 < 20 < 209) ，它对应的页是**页9**。
  2. 再根据前边说的在页中查找记录的方式去页9中定位具体的记录。

  至此，针对数据页做的简易目录就搞定了。这个目录有一个别名，称为**索引**。

##### InnDB中索引方案

- 迭代一次：目录项记录的页

  - 上边称为一个简易的索引方案，是因为我们为了在根据主键值进行查找时使用二分法快速定位具体的目录项而假设所有目录项都可以在物理存储器上连续存储，但是这样做有几个问题：

    - InnoDB是使用页来作为管理存储空间的基本单位，最多能保证16KB的连续存储空间，而随着表中记录数量的增多，需要非常大的连续的存储空间才能把所有的目录项都放下，这对记录数量非常多的表是不现实的。
    - 我们时常会对记录进行增删，假设我们把页28中的记录都删除了，那意味着目录顶2也就没有存在的必要了，这就需要把目录项2后的目录项都向前移动一 下，这样牵一发而动全身的操作效率很差。

  - 所以，我们需要一种可以灵活管理所有目录项的方式。我们发现目录项其实长得跟我们的用户记录差不多，只不过目录项中的两个列是主键和页号而已，为了和用户记录做一下区分，我们把这些用来表示目录项的记录称为目录项记录。那InnoDB怎么区分一条记录是普通的用户记录还是目录项记录呢?使用记录头信息里的record_ type属性，它的各个取值代表的意思如下:

    - 0：普通的用户记录
    - 1：目录项记录
    - 2：最小记录
    - 3：最大记录

  - 我们把前边使用到的目录项放到数据页中的样子就是这样:

    ![image-20230125155611507](/image-20230125155611507.png)

  - 不同点：

    - record不同
    - 目录项只记录主键值和页的编号，而普通用户可以存放很多信息

  - 相同点：

    - 两者用的是一样的数据页，都会为主键值生成Page Directory (页目录) ，从而在按照主键值进行查找时可以使用二分法来加快查询速度。

- 迭代两次：多个目录项记录的页

  - 虽然说**目录项记录**中只存储主键值和对应的页号，比用户记录需要的存储空间小多了，但是不论怎么说一个页只有16KB大小，能存放的目录项记录也是有限的，那如果表中的数据太多，以至于一个数据页不足以存放所有的 目录项记录，如何处理呢?

  - 这里我们假设一个存储目录项记录的页最多只能存放4条目录项记录，所以如果此时我们再向上图中插入一条主键值为320的用户记录的话，那就需要分配一个新的存储目录项记录的页：

    ![image-20230125160600816](/image-20230125160600816.png)

- 迭代三次，目录项记录页的记录页

  - 问题来了，在这个查询步骤的第1步中我们需要定位存储目录项记录的页，但是这些页是不连续的，如果我们表中的数据非常多则会产生很多存储目录项记录的页，那我们怎么根据主键值快速定位一个存储目录项记录的页呢?那

  - 就为这些存储目录项记录的页再生成一个更高级的目录，就像是一个多级目录一样，大目录里嵌套小目录，小目录里才是实际的数据，所以现在各个页的示意图就是这样子：

    ![image-20230125160657321](/image-20230125160657321.png)

  - 最终生成以下结构（B树）：

    ![image-20230125160721701](/image-20230125160721701.png)

- B+数

  - 不论是存放**用户记录**的数据页，还是存放**目录项记录**的数据页，我们都把它们存放到B+树这个数据结构中了，所以我们也称这些数据页为**节点**。从图中可以看出，我们的实际用户记录其实都存放在B+树的最底层的节点上，这些节点也被称为**叶子节点**，其余用来存放**目录项**的节点称为**非叶子节点**或者**内节点**，其中B+树最上边的那个节点也称为**根节点**。
  - 一个B+树的节点其实可以分成好多层，规定最下边的那层，也就是存放我们用户记录的那层为第0层，之后依次往上加。之前我们做了一个非常极端的假设：存放用户记录的页最多存放3条记录，存放目录项记录的页最多存放4条记录。其实真实环境中一个页存放的记录数量是非常大的，假设所有存放用户记录的叶子节点代表的数据页可以存放100条用尸记录，所有存放目录项记录的内节点代表的数据页可以存放1000条日录项记录，那么：
    - 如果B+树只有1层，也就是只有1个用于存放用户记录的节点，最多能存放**100**条记录。
    - 如果B+树有2层，最多能存放1000x100=100,000条记录。
    - 如果B+树有3层，最多能存放1000x1000x100=100,000,000条记录。
    - 如果B+树有4层，最多能存放1000x1000x1000x100=100,000,000,000条记录。相当多的记录! ! !
  - 你的表里能存放10000000000条记录吗?所以一般情况下，我们**用到的B+树都不会超过4层**，那我们通过主键值去查找某条记录最多只需要做4个页面内的查找(查找3个目录项页和一个用户记录页)，又因为在每个页面内有所谓的**Page Directory** (页目录) ，所以在页面内也可以通过**二分法**实现快速定位记录。
  - 树的层数越小，IO的次数越少

#### 常见索引的概念

索引按照物理实现方式，索引可以分为2种：聚簇(聚集)和非聚簇(非聚集)索引。我们也把非聚集索引称为二级索引或者辅助索引。

##### 聚簇索引

**聚簇索引**并不是一-种单独的索引类型，而是**一种数据存储方式**(所有的用户记录都存储在了叶子节点)，也就是所谓的**索引即数据，数据即索引**。

> 术语"聚簇"表示数据行和相邻的键值聚簇的存储在一起。

**特点：**

1. 使用记录主键值的大小进行记录和页的排序，这包括三个方面的含义:
   - **页内**的记录是按照主键的大小顺序排成一个**单向链表**。
   - 各个存放**用户记录的页**也是根据页中用户记录的主键大小顺序排成一个**双向链表**。
   - 存放**目录项记录的页**分为不同的层次，在同一层次中的页也是根据页中目录项记录的主键大小顺序排成一个**双向链表**。
2. B+树的叶子节点存储的是完整的用户记录。
   - 所谓的完整的用户记录，就是指这个记录存储了所有列的值（包括隐藏列）

我们把具有这两种特性的B+树称为**聚簇索引**，所有完整的用户记录都存放在这个**聚簇索引**的叶子节点处。这种聚簇索引并不需要我们在MySQL语句中显式的使用**INDEX**语句去创建，**InnoDB** 存储引擎会**自动**的为我们创建聚簇索引。

**优点**

- **数据访问更快**，因为聚簇索引将索引和数据保存在同-一个B+树中，因此从聚簇索引中获取数据比非聚簇索引更快
- 聚簇索引对于主键的**排序查找**和**范围查找**速度非常快
- 按照聚簇索引排列顺序,查询显示-定范围数据的时候，由于数据都是紧密相连，数据库不用从多个数据块中提取数据，所以**节省了大量的io操作**。

**缺点**

- **插入速度严重依赖于插入顺序**，按照主键的顺序插入是最快的方式，否则将会出现页分裂，严重影响性能。因此，对于InnoDB表, 我们一-般都会定义一个自增的ID列为主键
- **更新主键的代价很高**，因为将会导致被更新的行移动。因此，对于InnoDB表，我们一般定义**主键为不可更新**
- **二级索引访问需要两次索引查找**，第一次找到主键值，第二次根据主键值找到行数据

**限制**

- 对于MySQL数据库目前只有InnoDB数据引擎支持聚簇索引，而MyISAM并不支持聚簇索引。
- 由于数据物理存储排序方式只能有一种，所以每个MySQL的表只能有一个聚簇索引。一般情况下就是该表的主键。
- 如果没有定义主键，Innodb会选择**非空的唯一索引**代替。如果没有这样的索引，Innodb会隐式的定义一个主键来作为聚簇索引。
- 为了充分利用聚簇索引的聚簇的特性，所以innodb表的主键列尽量**选用有序的顺序id**，而不建议用无序的id，比如UUID、MD5、 HASH、 字符串列作为主键无法保证数据的顺序增长。

##### 二级索引（辅助索引、非聚簇索引）

上边介绍的**聚簇索引**只能在搜索条件是主键值时才能发挥作用，因为B+树中的数据都是按照主键进行排序的。那如果我们想以别的列作为搜索条件该怎么办呢？肯定不能是从头到尾沿着链表依次遍历记录一遍。

答案：我们可以**多建几棵B+树**，不同的B+树中的数据采用不同的排序规则。比方说我们用c2列的大小作为数据页、页中记录的排序规则，再建一棵B+树， 效果如下图所示：

![image-20230125161628613](/image-20230125161628613.png)

这个B+树与上边介绍的聚簇索引有几处不同：

- 使用记录c2列的大小进行记录和页的排序，这包括三个方面的含义：
  - 页内的记录是按照c2列的大小顺序排成一个**单向链表**。
  - 各个存放**用户记录的页**也是根据页中记录的c2列大小顺序排成一个**双向链表**。
  - 存放**目录项记录的页分**为不同的层次，在同一层次中的页也是根据页中目录项记录的c2列大小顺序排成一个**双向链表**。
- B+树的叶子节点存储的并不是完整的用户记录，而只是**c2列+主键**这两个列的值。
- 目录项记录中不再是**主键+页号**的搭配，而变成了**c2列+页号**的搭配。

**概念：回表**

我们根据这个以c2列大小排序的B+树只能确定我们要查找记录的主键值，所以如果我们想根据c2列的值查找到完整的用户记录的话，仍然需要到聚簇索引中再查一遍，这个过程称为回表。也就是根据c2列的值查询一条完整的用户记录需要使用到2棵B+树!

问题：为什么我们还需要一次回表操作呢？直接把完整的用户记录放到叶子节点不OK吗？

回答：

如果把完整的用户记录放到叶子节点是可以不用回表。但是太占地方了，相当于每建立一棵B+树都需要把所有的用户记录再都拷贝一遍，这就有点太浪费存储空间了。

因为这种按照非主键列建立的B+树需要一次回表操作才可以定位到完整的用户记录，所以这种B+树也被称为二级索引(英文名 secondary index)，或者辅助索引。由于我们使用的是c2列的大小作为B+树的排序规则，所以我们也称这个B+树是为c2列建立的索引。

非聚簇索弓|的存在不影响数据在聚簇索引中的组织，所以一张表可以有多个非聚簇索引。

![image-20230125162400936](/image-20230125162400936.png)

小结:聚簇索引与非聚簇索引的原理不同，在使用上也有一些区别：

1. 聚簇索引的**叶子节点**存储的就是我们的**数据记录**，非聚簇索引的叶子节点存储的是**数据位置**。非聚簇索引不会影响数据表的物理存储顺序。
2. 一个表**只能有一个聚簇索引**，因为只能有一种排序存储的方式，但可以有**多个非聚簇索引**，也就是多个索引目录提供数据检索。
3. 使用聚簇索引的时候，数据的查询效率高，但如果对数据进行插入，删除，更新等操作，效率会比非聚簇索引低。

##### 联合索引

我们也可以同时以多个列的大小作为排序规则，也就是同时为多个列建立索引，比方说我们想让B+树按照c2和c3列的大小进行排序，这个包含两层含义：

- 先把各个记录和页按照c2列进行排序。
- **在记录的c2列相同的情况下，采用c3列进行排序**

为c2和c3列建立的索引的示意图如下：

![image-20230125163158185](/image-20230125163158185.png)

如图所示，我们需要注意以下几点：

- 每条**目录项记录**都由c2、c3、页号这三个部分组成，各条记录先按照c2列的值进行排序，如果记录的c2列相同，则按照c3列的值进行排序。
- B+树**叶子节点**处的用户记录由c2、 c3和主键c1列组成。

注意一点，以c2和c3列的大小为排序规则建立的B+树称为联合索引，本质上也是一个二级索引。它的意思与分别为c2和c3列分别建立索引的表述是不同的，不同点如下：

- 建立联合索引只会建立如上图一样的1棵B+树。
- 为c2和c3列分别建立索引会分别以c2和c3列的大小为排序规则建立2棵B+树。

#### InnDB的B+树索引的注意事项

##### 根页面位置万年不变

我们前边介绍B+树索引的时候，为了大家理解上的方便，先把存储用户记录的叶子节点都画出来，然后接着画存储目录项记录的内节点，实际上B+树的形成过程是这样的:

- 每当为某个表创建一个B+树索引 (聚簇索引不是人为创建的，默认就有)的时候，都会为这个索引创建一个**根节点**页面。最开始表中没有数据的时候，每个B+树索引对应的**根节点**中既没有用户记录，也没有目录项记录。
- 随后向表中插入用户记录时，先把用户记录存储到这个**根节点**中。
- 当根节点中的**可用空间用完时**继续插入记录，此时会将根节点中的所有记录复制到一个新分配的页，比如页a中，然后对这个新页进行页分裂的操作，得到另一个新页，比如页b。这时新插入的记录根据键值(也就是聚簇索引中的主键值，二级索引中对应的索引列的值)的大小就会被分配到**页a**或者页b中,而根节点便升级为存储目录项记录的页。

这个过程特别注意的是：一个B+树索引的根节点自诞生之日起，便不会再移动。这样只要我们对某个表建立一个索引，那么它的根节点的页号便会被记录到某个地方，然后凡是`InnoDB`存储引擎需要用到这个索引的时候，都会从那个固定的地方取出根节点的页号，从而来访问这个索引。

##### 内节点中目录项纪录的唯一性

我们知道B+树索引的内节点中目录项记录的内容是索引列+页号的搭配，但是这个搭配对于二级索引来说有点儿不严谨。还拿index_demo表为例，假设这个表中的数据是这样的：

| c1   | c2   | c3   |
| ---- | ---- | ---- |
| 1    | 1    | 'u'  |
| 3    | 1    | 'd'  |
| 5    | 1    | 'y'  |
| 7    | 1    | 'a'  |

如果二级索引中目录项记录的内容只是`索引列+页号`的搭配的话，那么为c2列建立索引后的B+树应该长这样：

![image-20230125174019502](/image-20230125174019502.png)

此时，如果要添加一行`9、1、'c'`记录时，由于c2都是1，直接懵了，加不进去了！

为了让新插入记录能找到自己在那个页里，我们需要**保证在B+树的同一层内节点的目录项记录除页号这个字段以外是唯一的**。所以对于二级索引的内节点的目录项记录的内容实际上是由三个部分构成的：

- 索引列的值
- 主键值
- 页号

也就是我们把主键值也添加到二级索引内节点中的目录项记录了，这样就能保证B+树每一-层节点中各条目录项记录除页号这个字段外是唯一的，所以我们为c2列建立二级索引后的示意图实际上应该是这样子的：

![image-20230125174538465](/image-20230125174538465.png)

这时，再插入`9、1、'c'`时，9和7比较后，加到页5。

##### 一个页面最少存储两条记录

一个B+树只需要很少的层级就可以轻松存储数亿条记录，查询速度相当不错！这是因为B+树本质上就是一个大的多层级目录，每经过一个目录时都会过滤掉许多无效的子目录，直到最后访问到存储真实数据的目录。那如果一个大的目录中只存放一个子目录是个啥效果呢？那就是目录层级非常非常非常多，而且最后的那个存放真实数据的目录中只能存放一条记录。费了半天劲只能存放一条真实的用户记录? **所以InnoDB的一个数据页至少可以存放两条记录。**

### MyISAM中索引的方案

B树索引适用存储引擎如表所示：

| 索引/存储引擎 | MyIASM | InnDB | Memory |
| ------------- | ------ | ----- | ------ |
| B-Tree索引    | 支持   | 支持  | 支持   |

即使多个存储引擎支持同一种类型的索引，但是他们的实现原理也是不同的。Innodb和MyISAM默认的索引是Btree索引；而Memory默认的索引是Hash索引。

MyISAM引擎使用`B+Tree`作为索引结构，叶子节点的data域存放的是数据记录的地址。

#### MyISAM中索引的原理

下图是MyISAM索引的原理图。

我们知道InnoDB中索引即数据，也就是聚簇索引的那棵B+树的叶子节点中已经把所有完整的用户记录都包含了；而MyISAM的索引方案虽然也使用树形结构，但是却**将索引和数据分开存储**：

- 将表中的记录**按照记录的插入顺序**单独存储在一个文件中，称之为数据文件。这个文件并不划分为若干个数据页，有多少记录就往这个文件中塞多少记录就成了。由于在插入数据的时候并**没有刻意按照主键大小排序**，所以我们并不能在这些数据上使用二分法进行查找。

- 使用MyISAM存储引擎的表会把索引信息另外存储到一个称为**索引文件**的另一个文件中。MyISAM 会单独为表的主键创建一个索引，只不过在索弓的叶子节点中存储的不是完整的用户记录，而是**主键值+数据记录地址**的组合。

  ![image-20230125174600414](/image-20230125174600414.png)

这里设表一共有三列, 假设我们以Col1为主键，上图是一 个MyISAM表的主索引(Primary key)示意。可以看出MyISAM的索引文件仅仅保存数据记录的地址。在MyISAM中， 主键索引和二级索引 (Secondary key)在结构上没有任何区别，只是主键索引要求key是唯一的， 而二级索引的key可以重复。如果我们在Col2上建立一个二级索引，则此索引的结构如下图所示：

![image-20230125174930257](/image-20230125174930257.png)

同样也是一棵B+Tree，data域保存数据记录的地址。因此，MyISAM中索引检索的算法为：首先按照B+Tree搜索算法搜索索引，如果指定的Key存在，则取出其data域的值， 然后以data域的值为地址，读取相应数据记录。

#### MyISAM和InnoDB的对比

**MyISAM的索引方式都是“非聚簇’'的，与InnoDB包含 1个聚簇索引是不同的。小结两种引擎中索引的区别：**

①在InnoDB存储引擎中，我们只需要根据主键值对**聚簇索引**进行一次查找就能找到对应的记录；而在MyISAM中却需要进行一次回表操作，意味着MyISAM中建立的索引相当于全部都是**二级索引**。

②InnoDB的数据文件本身就是索引文件；而MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址。

③InnoDB的非聚簇索引data域存储相应记录主键的值；而MyISAM索引记录的是地址。换句话说，InnoDB的所有非聚簇索引都引用主键作为data域。

④**MyISAM的回表操作是十分快速的**，因为是拿着**地址偏移量**直接到文件中取数据的，反观InnoDB是通过获取主键之后再去聚簇索引里找记录，虽然说也不慢，但还是比不上直接用地址去访问。

⑤InnoDB要求表**必须有主键**(MyISAM可以没有)。如果没有显式指定,则MySQL系统会自动选择一个可以非空且唯一标识数据记录的列作为主键。如果不存在这种列，则MySQL自动为InnoDB表生成一个隐含字段作为主键，这个字段长度为6个字节，类型为长整型。

> 小结:
>
> 了解不同存储引擎的索引实现方式对于正确使用和优化索引都非常有帮助。比如：
>
> 举例1：知道了InnoDB的索引实现后，就很容易明白**为什么不建议使用过长的字段**作为主键，因为所有二级索引都引用主键索引，过长的主键索引会令二级索引变得过大。
>
> 举例2：用非单调的字段作为主键在InnoDB中不是个好主意，因为InnoDB数据文件本身是一棵B+Tree，非单调的主键会造成在插入新记录时，数据文件为了维持B+Tree的特性而频繁的分裂调整，十分低效，而使用**自增字段作为主键则是一个很好的选择。**

![image-20230125175614692](/image-20230125175614692.png)

### 索引的代价

索引是个好东西，可不能乱建，它在空间和时间上都会有消耗：

- 空间上的代价
  - 每建立一个索引都要为它建立一棵B+树, 每一棵B+树的每一个节点都是一个数据页，一个页默认会占用16KB的存储空间，一棵很大的B+树由许多数据页组成，那就是很大的一片存储空间。
- 时间上的代价
  - 每次对表中的数据进行增、刪、改操作时，都需要去修改各个B+树索引。而且我们讲过，B+树每层节点都是按照索引列的值从小到大的顺序排序而组成了双向链表。不论是叶子节点中的记录，还是内节点中的记录(也就是不论是用户记录还是目录项记录)都是按照索引列的值从小到大的顺序而形成了一个单向链表。而增、删、改操作可能会对节点和记录的排序造成破坏，所以存储引擎需要额外的时间进行一些记录移位，页面分裂、页面回收等操作来维护好节点和记录的排序。如果我们建了许多索引，每个索引对应的B+树都要进行相关的维护操作，会给性能拖后腿。

> 一个表上索引建的越多，就会占用越多的存储空间，在增删改记录的时候性能就越差。为了能建立又好又少的索引，我们得学学这些索引在哪些条件下起作用的。

### MySQL数据结构选择的合理性

从MySQL的角度讲，不得不考虑一个现实问题就是磁盘IO。 如果我们能让索引的数据结构尽量减少硬盘的I/O操作，所消耗的时间也就越小。可以说，磁盘的I/O 操作次数对索引的使用效率至关重要。

查找都是索引操作，一般来说索引非常大，尤其是关系型数据库，当数据量比较大的时候，索引的大小有可能几个G甚至更多，为了减少索引在内存的占用，**数据库索引是存储在外部磁盘上的**。当我们利用索引查询的时候，不可能把整个索引全部加载到内存，只能**逐一加载**，那么MySQL衡量查询效率的标准就是磁盘IO次数。

#### 全表遍历

不多逼逼

#### Hash结构

Hash本身是一个函数,又被称为散列函数,它可以帮助我们大幅提升检索数据的效率。

Hash算法是通过某种确定性的算法(比如MD5、SHA1、 SHA2、 SHA3) 将输入转变为输出。**相同的输 入永远可以得到相同的输出**，假设输入内容有微小偏差，在输出中通常会有不同的结果。

举例：如果你想要验证两个文件是否相同，那么你不需要把两份文件直接拿来比对，只需要让对方把Hash函数计算得到的结果告诉你即可，然后在本地同样对文件进行Hash函数的运算，最后通过比较这两个Hash函数的结果是否相同，就可以知道这两个文件是否相同。

**加速查找速度的数据结构，常见的有两类：**

(1)树，例如平衡二叉搜索树，查询/插入修改/删除的平均时间复杂度都是O(log2N) ;

(2)哈希，例如HashMap，查询/插入修改/删除的平均时间复杂度都是O(1) ;

![image-20230125182351546](/image-20230125182351546.png)

采用Hash进行检索效率非常高，基本上一次检索就可以找到数据，而B+树需要自顶向下依次查找，多次访问节点才能找到数据，中间需要多次I/0操作，**从效率来说 Hash比B+树更快。**

在哈希的方式下，一个元素k处于h(k)中，即利用哈希函数h，根据关键字k计算出槽的位置。函数h将关键字域映射到哈希表T[0...m-1]的槽位上。

![image-20230125183234937](/image-20230125183234937.png)

上图中哈希函数h有可能将两个不同的关键字映射到相同的位置，这叫做碰撞，在数据库中一般采用链接法来解决。在链接法中，将散列到同一槽位的元素放在一个链表中， 如下图所示：

![image-20230125183307634](/image-20230125183307634.png)

**Hash结构效率高，那为什么索引结构要设计成树型呢?**

原因1：Hash 索引仅能满足 (=)  (<>) 和 IN 查询。如果进行范围查询，哈希型的索引，时间复杂度会退化为O(n);而树型的“有序”特性,依然能够保持O(log2N) 的高效率。

原因2：Hash 索引还有一个缺陷，数据的存储是没有顺序的，在ORDER BY的情况下，使用Hash索引还需要对数据重新排序。

原因3：对于联合索引的情况，Hash 值是将联合索引键合并后一起来计算的，无法对单独的一个键或者几个索引键进行查询。

原因4：对于等值查询来说，通常Hash索引的效率更高，不过也存在一种情况， 就是**索引列的重复值如果很多，效率就会降低**。这是因为遇到Hash冲突时，需要遍历桶中的行指针来进行比较，找到查询的关键字，非常耗时。所以，Hash索引通常不会用到重复值多的列上，比如列为性别、年龄的情况等。

**Hash索引适用存储引擎如表所示：**

| 索引/存储引擎 | MyISAM | InnoDB | Memory |
| ------------- | ------ | ------ | ------ |
| HASH索引      | 不支持 | 不支持 | 支持   |

**Hash索引的适用性：**

Hash索引存在着很多限制，相比之下在数据库中B+树索引的使用面会更广，不过也有一些场景采用Hash索引效率更高，比如在键值型(Key-Value) 数据库中，**Redis存储的核心就是Hash表。**

MySQL中的Memory存储引擎支持Hash存储，如果我们需要用到查询的临时表时，就可以选择Memory存储引擎，把某个字段设置为Hash索引，比如字符串类型的字段，进行Hash计算之后长度可以缩短到几个字节。当字段的重复度低，而且经常需要进行等值查询的时候，采用Hash索引是个不错的选择。

另外，InnoDB 本身不支持Hash索引，但是提供**自适应Hash索引**(Adaptive Hash Index)。什么情况下才会使用自适应Hash索引呢？**如果某个数据经常被访问，当满足一定条件的时候，就会将这个数据页的地址存放到Hash表中**。这样下次查询的时候，就可以直接找到这个页面的所在位置。这样让B+树也具备了Hash索引的优点。

![image-20230125184719049](/image-20230125184719049.png)

采用自适应Hash索引目的是方便根据SQL的查询条件加速定位到叶子节点，特别是当B+树比较深的时候，通过自适应Hash索引可以明显提高数据的检索效率。

我们可以通过innodb_ adaptive_ hash_ index 变量来查看是否开启了自适应Hash，比如：

```
mysql> show variables like'%adaptive_hash_index';
```

#### 二叉搜索树

见 算法与数据结构

#### AVL树

见 算法与数据结构 

#### B树

见 算法与数据结构 

#### B+树

见 算法与数据结构 

**B+树和B树的差异在于以下几点：**

1. B+树有k个孩子的节点就有k个关键字。也就是孩子数量=关键字数，而B树中，孩子数量=关键字数+1。
2. B+树非叶子节点的关键字也会同时存在在子节点中，并且是在子节点中所有关键字的最大(或最小)。
3. B+树的非叶子节点仅用于索引，不保存数据记录，跟记录有关的信息都放在叶子节点中。而B树中，非叶子节点既保存索引，也保存数据记录。
4. B+树所有关键字都在叶子节点出现，叶子节点构成一个有序链表，而且叶子节点本身按照关键字的大小从小到大顺序链接。

B树

![image-20230125190532373](/image-20230125190532373.png)

B+树

![image-20220130164503945](/image-20230125190600617.png)

B+树和B树的查询过程差不多，但是B+树和B树有个根本的差异在于，**B+树的中间节点并不直接存储数据**。这样的好处都有什么呢?

首先，**B+树查询效率更稳定**。因为B+树每次只有访问到叶子节点才能找到对应的数据，而在B树中，非叶子节点也会存储数据，这样就会造成查询效率不稳定的情况，有时候访问到了非叶子节点就可以找到关键字，而有时需要访问到叶子节点才能找到关键字。

其次，**B+树的查询效率更高**。这是因为通常B+树比B树**更矮胖**(阶数更大， 深度更低)，查询所需要的磁盘I/O也会更少。同样的磁盘页大小，B+ 树可以存储更多的节点关键字。

不仅是对单个关键字的查询上，**在查询范围上，B+ 树的效率也比B树高**。这是因为所有关键字都出现在B+树的叶子节点中，叶子节点之间会有指针，数据又是递增的，这使得我们范围查找可以通过指针连接查找。而在B树中则需要通过中序遍历才能完成查询范围的查找，效率要低很多。

#### 关于B树和B+树的面试题

**思考题：为了减少IO，索引树会一次性加载吗?**

> 1、数据库索引是存储在磁盘上的，如果数据量很大，必然导致索引的大小也会很大，超过几个G。
>
> 2、当我们利用索引查询时候，是不可能将全部几个G的索弓都加载进内存的，我们能做的只能是：逐一加载每一个磁盘页。因为磁盘页对应着系引树的节点。

**思考题：B+树的存储能力如何？为何说一般查找行记录，最多只需1 ~3次磁盘lO**

> InnoDB存储引擎中页的大小为16KB，一般表的主键类型为INT (占用4个字节)或BIGINT (占用8个字节)，指针类型也一般为4或8个字节，也就是说一个页(B+Tree 中的一个节点)中大概存储16KB/(8B+8B)=1K个键值(因为是估值，为方便计算，这里的K取值为10 ^3。也就是说一个深度为3的B+Tree索引可以维护10 ^3 * 10^3 * 10^3= 10亿条记录。(这里假定一个数据页也存储10^3条行记录数据了)
>
> 实际情况中每个节点可能不能填充满，因此在数据库中，B+Tree 的高度一般都在2~4层。MySQL 的InnoDB存储引擎在设计时是将根节点常驻内存的，也就是说查找某一键值的行记录时最多只需要1~3次磁盘I/O操作。

**思考题：为什么说B+树比B树更适合实际应用中操作系统的文件索引和数据库索引?**

> 1、B+树的磁盘读写代价更低
>
> B+树的内部结点并没有指向关键字具体信息的指针。因此其内部结点相对B树更小。如果把所有同一内部结点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多。一次性读入内存中的需要查找的关键字也就越多。相对来说IO读写次数也就降低了。
>
> 2、B+树的查询效率更加稳定
>
> 由于非终结点并不是最终指向文件内容的结点，而只是叶子结点中关键字的索引。所以任何关键字的查找必须走一条从根结点到叶子结点的路。所有关键字查询的路径长度相同，导致每一个数据的查询效率相当。

**思考题：Hash索引与B+树索引的区别**

> 我们之前讲到过B+树索弓|的结构，Hash 索弓|结构和B+树的不同，因此在索引|使用上也会有差别。
>
> 1、Hash 索引**不能进行范围查询**，而B+树可以。这是因为Hash索引指向的数据是无序的，而B+树的叶子节点是个有序的链表。
>
> 2、Hash 索引**不支持联合索引的最左侧原则**(即联合索引的部分索引无法使用)，而B+树可以。对于联合索引来说，Hash 索引在计算Hash值的时候是将索引键合并后再一起计算Hash值，所以不会针对每个索引单独计算Hash值。因此如果用到联合索引的一个或者几个索引时，联合索引无法被利用。
>
> 3、Hash 索引**不支持ORDER BY排序**，因为Hash索引指向的数据是无序的，因此无法起到排序优化的作用，而B+树索引数据是有序的，可以起到对该字段ORDER BY排序优化的作用。同理，我们也无法用Hash索引进行模糊查询，而B+树使用LIKE进行模糊查询的时候，LIKE 后面后模糊查询(比如%结尾)的话就可以起到优化作用
>
> 4、InnoDB不支持哈希索引

**思考题: Hash 索引与B+树索引是在建索引的时候手动指定的吗?**

> 如果使用的是MySQL的话，我们需要了解MySQL的存储引擎都支持哪些索引结构。
>
> 一些不支持Hash索引
>
> 需要注意的是，InnoDB提供自适应Hash（不需要手动指定）；Memory/Heap和NDB是支持的

#### R树

R-Tree在MySQL很少使用，仅支持geometry数据类型，支持该类型的存储引擎只有myisam、bdb、 innodb、ndb、archive几种。 举个R树在现实领域中能够解决的

例子：查找20英里以内所有的餐厅。

如果没有R树你会怎么解决? 一般情况下我们会把餐厅的坐标(x,y)分为两个字段存放在数据库中，一个字段记录经度，另一个字段记录纬度。这样的话我们就需要遍历所有的餐厅获取其位置信息，然后计算是否满足要求。如果一个地区有100家餐厅的话，我们就要进行100次位置计算操作了，如果应用到谷歌、百度地图这种超大数据库中，这种方法便必定不可行了。

R树就很好的**解决了这种高维空间搜索问题**。它把B树的思想很好的扩展到了多维空间，采用了B树分割空间的思想，并在添加、删除操作时采用合并、分解结点的方法，保证树的平衡性。因此，R树就是一棵用来存储高维数据的平衡树。相对于B-Tree，R-Tree的优势在于范围查找。

| 索引/存储引擎 | MyISAM | InnoDB | Memory |
| ------------- | ------ | ------ | ------ |
| R-Tree索引    | 支持   | 支持   | 不支持 |

#### 小结

使用索引可以帮助我们从海量的数据中快速定位想要查找的数据，不过索引也存在一些不足， 比如占用存储空间、降低数据库写操作的性能等，如果有多个索引还会增加索引选择的时间。当我们使用索引时，需要平衡索引的利(提升查询效率)和弊(维护索引所需的代价)。

在实际工作中，我们还需要基于需求和数据本身的分布情况来确定是否使用索引，尽管**索引不是万能的**，但**数据量大的时候不使用索引是不可想象的**，毕竟索引的本质，是帮助我们提升数据检索的效率。

## InnDB数据存储结构

### 数据库的存储结构：页

索引结构给我们提供了高效的索引方式，不过索引信息以及数据记录都是保存在文件上的，确切说是存储在页结构中。另一方面，索引是在存储引擎中实现的，MySQL服务器 上的**存储引擎**负责对表中数据的读取和写入工作。不同存储引擎中**存放的格式**一般是不同的，甚至有的存储引擎比如Memory都不用磁盘来存储数据。

由于**InnoDB**是MySQL的**默认存储引擎**，所以本章剖析InnoDB存储引擎的数据存储结构。

#### 磁盘与内存交互的基本单位：页

InnoDB将数据划分为若干个页，InnoDB中页的大小默认为16KB。

以页作为磁盘和内存之间交互的**基本单位**，也就是一次最少从磁盘中读取16KB的内容到内存中，一次最少把内存中的16KB内容刷新到磁盘中。也就是说，**在数据库中，不论读一行，还是读多行，都是将这些行所在的页进行加载。也就是说，数据库管理存储空间的基本单位是页(Page) ，数据库I/O操作的最小单位是页**。一个页中可以存储多个行记录。

> 记录是按照行来存储的，但是数据库的读取并不以行为单位，否则一次读取(也就是一次I/O操作)只能处理一行数据，效率会非常低。

![image-20230125193855609](/image-20230125193855609.png)

#### 页结构概述

页a、页b、页c ..页n，这些页可以**不在物理结构上相连**，只要**通过双向链表相关联**即可。每个数据页中的记录会按照主键值从小到大的顺序组成一个**单向链表**，每个数据页都会为存储在它里边的记录生成一个页目录，在通过主键查找某条记录的时候可以在页目录中**使用二分法**快速定位到对应的槽，然后再遍历该槽对应分组中的记录即可快速找到指定的记录。

#### 页的大小

不同的数据库管理系统(简称DBMS )的页大小不同。比如在MySQL的InnoDB存储引擎中,默认页的大小是16KB，我们可以通过下面的命令来进行查看：

```
mysql> show variables like '%innodb_page_size%';
```

![image-20230125194233951](/image-20230125194233951.png)

SQL Server中页的大小为8KB，而在Oracle中我们用术语“块”(Block) 来代表页”，Oralce支持的块大小为2KB， 4KB，8KB，16KB，32KB和64KB。

#### 页的上层结构

另外在数据库中，还存在着区(Extent) 、段(Segment) 和表空间(Tablespace) 的概念。行、页、区、段、表空间的关系如下图所示：

![image-20230125194247138](/image-20230125194247138.png)

**区(Extent)**

 是比页大一级的存储结构，在InnoDB存储引擎中，一个区会分配**64个连续的页**。因为InnoDB中的页大小默认是16KB，所以**一个区的大小是**64*16KB= **1MB**。

**段(Segment)**

由一个或多个区组成，区在文件系统是一个连续分配的空间 (在InnoDB中是连续的64个页) ，不过在段中不要求区与区之间是相邻的。**段是数据库中的分配单位，不同类型的数据库对象以不同的段形式存在**。当我们创建数据表、索引的时候，就会相应创建对应的段，比如创建一张表时会创建一个表段，创建一个索引时会创建一个索引段。

**表空间(Tablespace)** 

是一个逻辑容器，表空间存储的对象是段，在一个表空间中可以有一个或多个段，但是一个段只能属于一个表空间。数据库由一个或多个表空间组成，表空间从管理上可以划分为**系统表空间、用户表空间、撤销表空间、临时表空间**等。

### 页的内部结构

页如果按类型划分的话，常见的有数据页(保存B+ 树节点)、系统页、 Undo 页和事务数据页等。数据页是我们最常使用的页。

数据页的16KB大小的存储空间被划分为七个部分，分别是文件头(File Header)、页头(Page Header)、最大最小记录(Infimum+supremum) 、用户记录(User Records)、空闲空间(Free Space)、页目录(Page Directory)和文件尾(File Tailer)。

页结构的示意图如下所示：

![image-20230125194700988](/image-20230125194700988.png)

这7个部分作用分别如下，我们简单梳理如下表所示：

![image-20230125194713486](/image-20230125194713486.png)

我们可以把这7个结构分为三部分。

![image-20230125194745611](/image-20230125194745611.png)

#### 一：File Header和File Trailer

##### File Header(文件头)

- 作用
  - 描述各种页的通用信息。（比如页的编号、其上一页、下一页是谁等）
- 大小
  - 共38的字节
- 构成
  - ![image-20230125194845789](/image-20230125194845789.png)
- FIL_PAGE_OFFSET（4字节）
  - 每一个页都有一个单独的**页号**，就跟你的身份证号码一样，InnoDB通过页号可以唯一定位一个页。
- FIL_PAGE_TYPE(2字节)
  - 这个表示当前页的类型
  - ![image-20230125194924821](/image-20230125194924821.png)
- FIL_PAGE_PREV（4字节）和FIL_PAGE_NEXT（4字节）
  - InnoDB都是以页为单位存放数据的，如果数据分散到多个不连续的页中存储的话需要把这些页关联起来，FIL_PAGE_PREV和FIL_PAGE_NEXT就分别代表本页的上一个和下一个页的页号。这样通过建立一个双向链表把许许多多的页就都串联起来了，保证这些页之间**不需要是物理上的连续，而是逻辑上的连续**。
  - ![image-20230125195110537](/image-20230125195110537.png)
  - ![image-20230125195123361](/image-20230125195123361.png)
- FIL_PAGE_SPACE_OR_CHKSUM（4字节）
  - 代表当前页面的校验和(checksum)。
  - **什么是校验和?**
    - 就是对于一个很长的字节串来说，我们会通过某种算法来计算一个比较短的值来代表这个很长的字节串，这个比较短的值就称为校验和。（hsah)
    - 在比较两个很长的字节串之前，先比较这两个长字节串的校验和，如果校验和都不一样，则两个长字节串肯定是不同的，所以省去了直接比较两个比较长的字节串的时间损耗。
  - 文件头部和文件尾部都有属性: FIL_ PAGE_ SPACE_ OR_ CHKSUM
  - **作用：**
    - InnoDB存储引擎以页为单位把数据加载到内存中处理，如果该页中的数据在内存中被修改了，那么**在修改后的某个时间需要把数据同步到磁盘中**。但是在同步了一半的时候断电了，造成了该页传输的不完整。
    - 为了检测一个页是否完整(也就是在同步的时候有没有发生只同步一半的尴尬情况)，这时可以通过文件尾的校验和(checksum值)与文件头的校验和做比对，如果两个值不相等则证明页的传输有问题，需要重新进行传输，否则认为页的传输已经完成。
  - **具体的：**
    - 每当一个页面在内存中修改了，在同步之前就要把它的校验和算出来，因为File Header在页面的前边，所以校验和会被首先同步到磁盘，当完全写完时，校验和也会被写到页的尾部，如果完全同步成功，则页的首部和尾部的校验和应该是一致的。如果写了一半儿断电了，那么在File Header中的校验和就代表着已经修改过的页，而在File Trailer中的校验和代表着原先的页，二者不同则意味着同步中间出了错。这里，校验方式就是采用Hash算法进行校验。
- FIL_PAGE_LSN（8字节）
  - 页面被最后修改时对应的日志序列位置(英文名是: Log Sequence Num!ber)

##### File Trailer(文件尾)

- 前4个字节代表页的校验和：
  - 这个部分是和File Header中的校验和相对应的。
- 后4个字节代表页面被最后修改时对应的日志序列位置(LSN) ：
  - 这个部分也是为了校验页的完整性的，如果首部和尾部的LSN值校验不成功的话，就说明同步过程出现了问题。

#### 二：User Records、最大最小记录、Free Space

第二个部分是记录部分，页的主要作用是存储记录，所以“最大和最小记录”和“用户记录”部分占了页结构的主要空间。

![image-20230125195445511](/image-20230125195445511.png)

##### Free Space(空闲空间)

我们自己存储的记录会按照指定的**行格式**存储到**User Records**部分。但是在一开始生成页的时候，其实并没有User Records这个部分，**每当我们插入一条记录，都会从Free Space部分，也就是尚未使用的存储空间中申请一个记录大小的空间划分到User Records部分**，当Free Space部分的空间全部被User Records部分替代掉之后，也就意味着这个页使用完了，如果还有新的记录插入的话，就需要去申请新的页了。

![image-20230125200721848](/image-20230125200721848.png)

##### User Records(用户记录)

User Records中的这些记录按照**指定的行格式**一条一条摆在User Records部分，相互之间形成**单链表**。

**用户记录里的一条条数据如何记录？**

这里需要讲讲记录行格式的**记录头信息**（下文中：COMPACT行格式，记录头信息 heap_no）。

##### 最大最小记录

**记录可以比较大小吗？**

是的，记录可以比大小，对于一条完整的记录来说，比较记录的大小就是**比较主键**的大小。比方说我们插入的4行记录的主键值分别是：1、2、3、4，这也就意味着这4条记录是从小到大依次递增。

InnoDB规定的最小记录与最大记录这两条记录的构造十分简单，都是由5字节大小的记录头信息和8字节大小的一个固定的部分组成的，如图所示：

![image-20230125201018491](/image-20230125201018491.png)

这两条记录**不是我们自己定义的记录**，所以它们并不存放在页的User Records部分，他们被单独放在一个称为Infimum + Supremum的部分，如图所示：

![image-20230125201100580](/image-20230125201100580.png)

#### 三：Page Directory、Page Header

##### Page Directory(页目录)

为什么需要页目录？

在页中，记录是以**单向链表**的形式进行存储的。单向链表的特点就是插入、删除非常方便，但是**检索效率不高**，最差的情况下需要遍历链表上的所有节点才能完成检索。因此在页结构中专门设计了页目录这个模块，**专门给记录做一个目录**，通过**二分查找法**的方式进行检索，提升效率。

需求：根据主键值查找页中的某条记录，如何实现快速查找呢？

```
SELECT * FROM page_demo WHERE c1 = 3;
```

**方式1：顺序查找**

从Infimum记录（最小记录）开始，沿着链表一直往后找，总有一天会找到（或者找不到），在找的时候还能投机取巧，因为链表中各个记录的值是按照从小到大顺序排列的，所以当链表的某个节点代表的记录的主键值大于你想要查找的主键值时，你就可以停止查找了，因为该节点后边的节点的主键值依次递增。

如果一个页中存储了非常多的记录，这么查找性能很差。

**方式2：使用页目录，二分法查找**

1. 将所有的记录分成几个组，这些记录包括最小记录和最大记录，但不包括标记为“已删除”的记录。
2. 第 1 组，也就是最小记录所在的分组只有 1 个记录；
   - 最后一组，就是最大记录所在的分组，会有 1-8 条记录；
   - 其余的组记录数量在 4-8 条之间。
   - 这样做的好处是，除了第 1 组（最小记录所在组）以外，其余组的记录数会**尽量平分**。
3. 在每个组中最后一条记录的头信息中会存储该组一共有多少条记录，作为 n_owned 字段(记录头信息中)。
4. **页目录用来存储每组最后一条记录的地址偏移量，这些地址偏移量会按照先后顺序存储起来**，每组的地址偏移量也被称之为槽（slot），每个槽相当于指针指向了不同组的最后一个记录。

**举例1：**

![image-20230125201446138](/image-20230125201446138.png)

**举例2：**

现在的page_demo表中正常的记录共有6条，InnoDB会把它们分成两组，第一组中只有一个最小记录，第二组中是剩余的5条记录。如下图：

![image-20230125202719428](/image-20230125202719428.png)

从这个图中我们需要注意这么几点：

- 现在页目录部分中有两个槽，也就意味着我们的记录被分成了两个组，槽1中的值是112，代表最大记录的地址偏移量（就是从页面的0字节开始数，数112个字节）；槽0中的值是99，代表最小记录的地址偏移量。
- 注意最小和最大记录的头信息中的n_owned属性
- 最小记录的n_owned值为1，这就代表着以最小记录结尾的这个分组中只有1条记录，也就是最小记录本身。
- 最大记录的n_owned值为5，这就代表着以最大记录结尾的这个分组中只有5条记录，包括最大记录本身还有我们自己插入的4条记录。

用箭头指向的方式替代数字，这样更易于我们理解，修改后如下：

![image-20230125202922754](/image-20230125202922754.png)

再换个角度看一下：（单纯从逻辑上看一下这些记录和页目录的关系）

![image-20230125202933611](/image-20230125202933611.png)

**页目录分组的个数如何确定？**

为什么最小记录的n_owned值为1，而最大记录的n_owned值为5呢？

**InnoDB规定**：对于最小记录所在的分组只能有1条记录，最大记录所在的分组拥有的记录条数只能在1~8条之间，剩下的分组中记录的条数范围只能在是 4~8 条之间。

###### 分组是按照下边的步骤进行的：

- 初始情况下一个数据页里只有最小记录和最大记录两条记录，它们分属于两个分组。
- 之后每插入一条记录，都会从页目录中找到主键值比本记录的主键值大并且差值最小的槽，然后把该槽对应的记录的n_owned值加1，表示本组内又添加了一条记录，直到该组中的记录数等于8个。
- 在一个组中的记录数等于8个后再插入一条记录时，会将组中的记录拆分成两个组，一个组中4条记录，另一个5条记录。这个过程会在页目录中新增一个槽来记录这个新增分组中最大的那条记录的偏移量。

**页目录结构下如何快速查找记录？**

现在向page_demo表中添加更多的数据。如下：

```
INSERT INTO page_demo 
VALUES
(5, 500, 'zhou'), 
(6, 600, 'chen'), 
(7, 700, 'deng'), 
(8, 800, 'yang'), 
(9, 900, 'wang'), 
(10, 1000, 'zhao'), 
(11, 1100, 'qian'), 
(12, 1200, 'feng'), 
(13, 1300, 'tang'), 
(14, 1400, 'ding'), 
(15, 1500, 'jing'), 
(16, 1600, 'quan');
```

添加了12条记录，现在页里一共有18条记录了（包括最小和最大记录），这些记录被分成了5个组，如图所示：

![image-20230125203554052](/image-20230125203554052.png)

这里只保留了16条记录的记录头信息中的n_owned和next_record属性，省略了各个记录之间的箭头。

现在看怎么从这个页目录中查找记录。因为各个槽代表的记录的主键值都是从小到大排序的，所以我们可以使用二分法来进行快速查找。5个槽的编号分别是：0、1、2、3、4，所以初始情况下最低的槽就是low=0，最高的槽就是high=4。比方说我们想找主键值为6的记录，过程是这样的：

1. 计算中间槽的位置：(0+4)/2=2，所以查看槽2对应记录的主键值为8，又因为8 > 6，所以设置high=2，low保持不变。
2. 重新计算中间槽的位置：(0+2)/2=1，所以查看槽1对应的主键值为4，又因为4 
3. 因为high - low的值为1，所以确定主键值为6的记录在槽2对应的组中。此刻我们需要找到槽2中主键值最小的那条记录，然后沿着单向链表遍历槽2中的记录。

但是我们前边又说过，每个槽对应的记录都是该组中主键值最大的记录，这里槽2对应的记录是主键值为8的记录，怎么定位一个组中最小的记录呢？别忘了各个槽都是挨着的，我们可以很轻易的拿到槽1对应的记录（主键值为4），该条记录的下一条记录就是槽2中主键值最小的记录，该记录的主键值为5。所以我们可以从这条主键值为5的记录出发，遍历槽2中的各条记录，直到找到主键值为6的那条记录即可。

由于一个组中包含的记录条数只能是1~8条，所以遍历一个组中的记录的代价是很小的。

**小结：**

在一个数据页中查找指定主键值的记录的过程分为两步：

1. 通过二分法确定该记录所在的槽，并找到该槽所在分组中主键值最小的那条记录。
2. 通过记录的next_record属性遍历该槽所在的组中的各个记录。

##### Page Header(页面头部)

为了能得到一个数据页中存储的记录的状态信息，比如本页中已经存储了多少条记录，第一条记录的地址是什么，页目录中存储了多少个槽等等，特意在页中定义了一个叫Page Header的部分，这个部分占用固定的56个字节，专门存储各种状态信息。

![image-20230125204144296](/image-20230125204144296.png)

- PAGE_DIRECTION
  - 假如新插入的一条记录的主键值比上一条记录的主键值大，我们说这条记录的插入方向是右边，反之则是左边。用来表示最后一条记录插入方向的状态就是PAGE_DIRECTION。
- PAGE_N_DIRECTION
  - 假设连续几次插入新记录的方向都是一致的，InnoDB会把沿着同一个方向插入记录的条数记下来，这个条数就用PAGE_N_DIRECTION这个状态表示。当然，如果最后一条记录的插入方向改变了的话，这个状态的值会被清零重新统计。

#### 从数据页的角度看B+树如何查询

一棵B+树按照节点类型可以分成两部分：

1. 叶子节点，B+ 树最底层的节点，节点的高度为0，存储行记录。
2. 非叶子节点，节点的高度大于o,存储索引键和页面指针，并不存储行记录本身。

![image-20230125204221594](/image-20230125204221594.png)

当我们从页结构来理解B+树的结构的时候，可以帮我们理解一些通过索引进行检索的原理：

1. <font color='red'>**B+树如何进行记录索引的？**</font>
   - 如果通过B+树的索引查询行记录，首先是从B+树的根开始，逐层检索，直到找到叶子节点，也就是找到对应的数据页为止，将数据页加载到内存中，对页目录中的槽(slot) 采用**二分查找**的方式先找到一个粗略的记录分组，然后再在分组中通过**链表遍历**的方式查找记录。
2. <font color='red'>**普通索引和唯一索引在查询效率上有什么不同？**</font>
   - 我们创建索引的时候可以是普通索引，也可以是唯一索引， 那么这两个索引在查询效率上有什么不同呢?
   - 唯一索引就是在普通索引上增加了约束性，也就是关键字唯一，找到了关键字就停止检索。而普通索引，可能会存在用户记录中的关键字相同的情况，根据页结构的原理，当我们读取一条记录的时候，不是单独将这条记录从磁盘中读出去，而是将这个记录所在的页加载到内存中进行读取。InnoDB存储引擎的页大小为16KB，在一个页中可能存储着上千个记录，因此在普通索引的字段上进行查找也就是在内存中多几次“判断下一条记录“的操作，对于CPU来说，这些操作所消耗的时间是可以忽略不计的。所以对一个索引字段进行检索，采用普通索引还是唯一索引在检索效率上基本上没有差别。

### InnoDB行格式

我们平时的数据以行为单位来向表中插入数据，这些记录在磁盘上的存放方式也被称为`行格式`或者`记录格式`。InnoDB存储引擎设计了4种不同类型的`行格式`，分别是`Compact`、`Redundant`、`Dynamic`和`Compressed`行格式。

查看MySQL8和5的默认行格式：

```
mysql> SELECT @@innodb_default_row_format;
+-------------------------------------+
| @@innodb_default_row_format         |
+-------------------------------------+
| dynamic                             |
+-------------------------------------+
1 row in set (0.00 sec)
```

也可以使用如下语法查看具体表使用的行格式：

```
SHOW TABLE STATUS like '表名'\G
```

#### 指定行格式的语法

在创建或修改表的语句中指定行格式：

CREATE TABLE 表名 (列的信息) ROW_FORMAT=行格式名称

ALTER TABLE 表名 ROW_FORMAT=行格式名称

举例：

```
mysql> CREATE TABLE record_test_table (
  ->   col1 VARCHAR(8),
  ->   col2 VARCHAR(8) NOT NULL,
  ->   col3 CHAR(8),
  ->   col4 VARCHAR(8)
  -> ) CHARSET=ascii ROW_FORMAT=COMPACT;
Query OK, 0 rows affected (0.03 sec)
```

向表中插入两条记录：

```
INSERT INTO record_test_table(col1, col2, col3, col4) 
VALUES
('zhangsan', 'lisi', 'wangwu', 'laoliu'), 
('laoqi', 'laoba', NULL, NULL);
```

#### COMPACT行格式

在MySQL 5.1版本中，默认设置为Compact行格式。一条完整的记录其实可以被分为记录的额外信息和记录的真实数据两大部分。

![image-20230125204740723](/image-20230125204740723.png)

##### 变长字段长度列表

MySQL支持一些变长的数据类型，比如VARCHAR(M)、VARBINARY(M)、TEXT类型，BLOB类型，这些数据类型修饰列称为**变长字段**，变长字段中存储多少字节的数据不是固定的，所以我们在存储真实数据的时候需要顺便把这些数据占用的字节数也存起来。**在Compact行格式中，把所有变长字段的真实数据占用的字节长度都存放在记录的开头部位，从而形成一个变长字段长度列表**。

注意：这里面存储的变长长度和字段**顺序是反过来的**。比如两个varchar字段在表结构的顺序是a(10)，b(15)。那么在变长字段长度列表中存储的长度顺序就是15，10，是反过来的。

以record_test_table表中的第一条记录举例：因为record_test_table表的col1、col2、col4列都是VARCHAR(8)类型的，所以这三个列的值的长度都需要保存在记录开头处，注意record_test_table表中的各个列都使用的是ascii字符集（每个字符只需要1个字节来进行编码）。

![image-20230125204900124](/image-20230125204900124.png)

又因为这些长度值需要按照列的逆序存放，所以最后变长字段长度列表的字节串用十六进制表示的效果就是（各个字节之间实际上没有空格，用空格隔开只是方便理解）：

06 04 08 

把这个字节串组成的变长字段长度列表填入上边的示意图中的效果就是：

![image-20230125204914598](/image-20230125204914598.png)

##### NULL值列表

Compact行格式会把可以为NULL的列统一管理起来，存在一个标记为NULL值列表中。如果表中没有允许存储 NULL 的列，则 NULL值列表也不存在了。

**为什么定义NULL值列表？**

之所以要存储NULL是因为数据都是需要对齐的，如果**没有标注出来NULL值的位置**，就有可能在查询数据的时候**出现混乱**。如果**使用一个特定的符号**放到相应的数据位表示空置的话，虽然能达到效果，但是这样很**浪费空间**，所以直接就在行数据得头部开辟出一块空间专门用来记录该行数据哪些是非空数据，哪些是空数据，格式如下：

1. 二进制位的值为1时，代表该列的值为NULL。
2. 二进制位的值为0时，代表该列的值不为NULL。

**例如**：字段 a、b、c，其中a是主键，在某一行中存储的数依次是 a=1、b=null、c=2。那么Compact行格式中的NULL值列表中存储：01。第一个0表示c不为null，第二个1表示b是null。这里之所以没有a是因为数据库会自动跳过主键，因为主键肯定是非NULL且唯一的，在NULL值列表的数据中就会自动跳过主键。

record_test_table的两条记录的NULL值列表就如下：

第一条记录：

![image-20230125210441607](/image-20230125210441607.png)

第二条记录：

![image-20230125210453899](/image-20230125210453899.png)

##### 记录头信息

```
mysql> CREATE TABLE page_ demo(
-> 	c1 INT,
->	c2 INT,
->	c3 VARCHAR( 10000),
->	PRIMARY KEY (C1)
-> ) CHARSET=ascii ROW_ FORMAT=Compact;
Query OK, 0 rows affected (0.03 sec)
```

这个表中记录的行格式示意图：

![image-20230125210512435](/image-20230125210512435.png)

这些记录头信息中各个属性如下：

![image-20230125210529117](/image-20230125210529117.png)

简化后的行格式示意图：

![image-20230125210537931](/image-20230125210537931.png)

插入数据：

```
INSERT INTO page_demo 
VALUES
(1, 100, 'song'), 
(2, 200, 'tong'), 
(3, 300, 'zhan'), 
(4, 400, 'lisi');
```

图示如下：

![image-20230125210751674](/image-20230125210751674.png)

- **delete_mask**

  - 这个属性标记着当前记录是否被删除，占用1个二进制位。
    - 值为0：代表记录并没有被删除
    - 值为1：代表记录被删除掉了
  - **被删除的记录为什么还在页中存储呢？**
  - 你以为它删除了，可它还在真实的磁盘上。这些被删除的记录之所以不立即从磁盘上移除，是因为移除它们之后其他的记录在磁盘上需要**重新排列，导致性能消耗**。所以只是打一个删除标记而已，所有被删除掉的记录都会组成一个所谓的**垃圾链表**，在这个链表中的记录占用的空间称之为**可重用空间**，之后如果有新记录插入到表中的话，可能把这些被删除的记录占用的存储空间覆盖掉。

- **min_rec_mask**

  - B+树的每层非叶子节点中的最小记录都会添加该标记，min_rec_mask值为1。
  - 我们自己插入的四条记录的min_rec_mask值都是0，意味着它们都不是B+树的非叶子节点中的最小记录。

- **record_type**

  - 这个属性表示当前记录的类型，一共有4种类型的记录：

    0：表示普通记录

    1：表示B+树非叶节点记录

    2：表示最小记录

    3：表示最大记录

  - 从图中我们也可以看出来，我们自己插入的记录就是普通记录，它们的record_type值都是0，而最小记录和最大记录的record_type值分别为2和3。至于record_type为1的情况，我们在索引的数据结构章节讲过。

- **heap_no**

  - 这个属性表示当前记录在本页中的位置。
  - 从图中可以看出来，我们插入的4条记录在本页中的位置分别是：2、3、4、5。
  - **怎么不见heap_no值为0和1的记录呢？**
    - **MySQL会自动给每个页里加了两个记录**，由于这两个记录并不是我们自己插入的，所以有时候也称为**伪记录**或者**虚拟记录**。这两个伪记录一个代表**最小记录**，一个代表**最大记录**。最小记录和最大记录的heap_no值分别是0和1，也就是说它们的位置最靠前。（上文中：最小记录，最大记录）

- **n_owned**

  - 页目录中每个组中最后一条记录的头信息中会存储该组一共有多少条记录，作为 n_owned 字段。
  - 详情见page directory。

- **next_record**

  - 记录头信息里该属性非常重要，它表示从当前记录的真实数据到下一条记录的真实数据的**地址偏移量**。

  - 比如：第一条记录的next_record值为32，意味着从第一条记录的真实数据的地址处向后找**32个字节**便是下一条记录的真实数据。

  - **注意，下一条记录指得并不是按照我们插入顺序的下一条记录，而是按照主键值由小到大的顺序的下一条记录**。而且规定Infimum记录（也就是最小记录）的下一条记录就是本页中主键值最小的用户记录，而本页中主键值最大的用户记录的下一条记录就是 Supremum记录（也就是最大记录）。下图用箭头代替偏移量表示next_record。

    ![image-20230125211045085](/image-20230125211045085.png)

  - 演示：删除操作

    - 删除操作：

    - 从表中删除掉一条记录，这个链表也是会跟着变化：

      ```
      mysql> DELETE FROM page_demo WHERE c1 = 2;
      Query OK, 1 row affected (0.02 sec)
      ```

      - 删掉第2条记录后的示意图就是：

      ![image-20230125211118993](/image-20230125211118993.png)

      从图中可以看出来，删除第2条记录前后主要发生了这些变化：

      - 第2条记录并没有从存储空间中移除，而是把该条记录的delete_mask值设置为1。
      - 第2条记录的next_record值变为了0，意味着该记录没有下一条记录了。
      - 第1条记录的next_record指向了第3条记录。
      - 最大记录的n_owned值从 5 变成了 4 。

      **所以，不论我们怎么对页中的记录做增删改操作，InnoDB始终会维护一条记录的单链表，链表中的各个节点是按照主键值由小到大的顺序连接起来的。**

  - 演示：添加操作

    - 添加操作：

    - 主键值为2的记录被我们删掉了，但是存储空间却没有回收，如果我们再次把这条记录插入到表中，会发生什么事呢？

      ```
      mysql> INSERT INTO page_demo VALUES(2, 200, 'tong');
      Query OK, 1 row affected (0.00 sec)
      ```

      - 我们看一下记录的存储情况：

      ![image-20230125211137887](/image-20230125211137887.png)

      - 直接复用了原来被删除记录的存储空间。

    - 说明：

    - **当数据页中存在多条被删除掉的记录时，这些记录的next_record属性将会把这些被删除掉的记录组成一个垃圾链表，以备之后重用这部分存储空间。**

##### 记录的真实数据

记录的真实数据除了我们自己定义的列的数据以外，还会**有三个隐藏列**：

![image-20230125211222810](/image-20230125211222810.png)

实际上这几个列的真正名称其实是：DB_ROW_ID、DB_TRX_ID、DB_ROLL_PTR。

- 一个表没有手动定义主键，则会选取一个Unique键作为主键，如果连Unique键都没有定义的话，则会为表默认添加一个名为row_id的隐藏列作为主键。所以row_id是在没有自定义主键以及Unique键的情况下才会存在的。
- 事务ID和回滚指针在后面的《第14章_MySQL事务日志》章节中讲解。

举例：分析Compact行记录的内部结构：

```
CREATE TABLE mytest(
col1 VARCHAR(10),
col2 VARCHAR(10),
col3 CHAR(10),
col4 VARCHAR(10)
)ENGINE=INNODB CHARSET=LATIN1 ROW_FORMAT=COMPACT;

INSERT INTO mytest
VALUES('a','bb','bb','ccc');

INSERT INTO mytest
VALUES('d','ee','ee','fff');

INSERT INTO mytest
VALUES('d',NULL,NULL,'fff');
```

在Windows操作系统下，可以选择通过程序UltraEdit打开表空间文件mytest.ibd这个二进制文件。内容如下：

![image-20230125211305388](/image-20230125211305388.png)

该行记录从0000c070开始，若整理一下，相信大家会有更好的理解：

```
03 02 01                     	/*变长字段长度列表，逆序*/
00                              /*NULL标志位，第一行没有NULL值*/
00 00 10 00 2c            		/*Record Header，固定5字节长度*/
00 00 00 2b 68 00       		/*RowID InnoDB自动创建，6字节*/
00 00 00 00 06 05       		/*TransactionID*/
80 00 00 00 32 01 10   			/*Roll Pointer*/
61                            	/*列1数据'a'*/
62 62                          	/*列2数据'bb'*/
62 62 20 20 20 20 20 20 20 20	/*列3数据'bb'*/
63 63 63                     	/*列4数据'ccc'*/
```

注意1：InnoDB每行有隐藏列TransactionID和Roll Pointer。:

注意2：固定长度CHAR字段在未能完全占用其长度空间时，会用0x20来进行填充。

接着再来分析下Record Header的最后两个字节，这两个字节代表next_recorder，0x2c代表下一个记录的偏移量，即当前记录的位置加上偏移量0x2c就是下条记录的起始位置。

第二行将不做整理，除了RowID不同外，它和第一行大同小异，现在来分析有NULL值的第三行：

```
03 01                          	/*变长字段长度列表，逆序*/
06                         		/*NULL标志位，第三行有NULL值 0110 => 6*/
00 00 20 ff 98                  /*Record Header*/
00 00 00 2b 68 02          	 	/*RowID*/
00 00 00 00 06 07           	/*TransactionID*/
80 00 00 00 32 01 10       		/*Roll Pointer*/
64                              /*列1数据'd'*/
66 66 66                        /*列4数据'fff'*/
```

第三行有NULL值，因此NULL标志位不再是00而是06，转换成二进制为00000110，为1的值代表第2列和第3列的数据为NULL。在其后存储列数据的部分，用户会发现没有存储NULL列，而只存储了第1列和第4列非NULL的值。

因此这个例子很好地说明了：不管是CHAR类型还是VARCHAR类型，在compact格式下NULL值都不占用任何存储空间。

#### Dynamic 和 Compresssed 行格式

##### 行溢出

**InnoDB存储引擎可以将一条记录中的某些数据存储在真正的数据页面之外。**

很多DBA喜欢MySQL数据库提供的VARCHAR(M)类型，认为可以存放65535字节。这是真的吗？如果我们使用 ascii字符集的话，一个字符就代表一个字节，我们看看VARCHAR(65535)是否可用。

```
CREATE TABLE varchar_size_demo(
 c VARCHAR(65535)
 ) CHARSET=ascii ROW_FORMAT=Compact;
```

结果如下：

```
ERROR 1118 (42000): Row size too large. The maximum row size for the used table type, not counting BLOBs, is 65535. This includes storage overhead, check the manual. You have to change some columns to TEXT or BLOBs
```

报错信息表达的意思是：MySQL对一条记录占用的最大存储空间是有限制的，除BLOB或者TEXT类型的列之外， 其他所有的列（不包括隐藏列和记录头信息）占用的字节长度加起来不能超过65535个字节。

这个65535个字节除了列本身的数据之外，还包括一些其他的数据，以Compact行格式为例，比如说我们为了存储一个VARCHAR(M)类型的列，除了真实数据占有空间以外，还需要记录的额外信息。

如果该VARCHAR类型的列没有NOT NULL属性，那最多只能存储65532个字节的数据，因为**变长字段的长度占用 2个字节**，**NULL值标识需要占用1个字节**。

```
CREATE TABLE varchar_size_demo(
  c VARCHAR(65532)
) CHARSET=ascii ROW_FORMAT=Compact;
```

\#如果有not null属性，那么就不需要NULL值标识，也就可以多存储一个字节，即65533个字节

```
CREATE TABLE varchar_size_demo( 
 c VARCHAR(65533) not null
) CHARSET=ascii ROW_FORMAT=Compact; 
```

通过上面的案例，我们可以知道一个页的大小一般是16KB，也就是16384字节，而一个VARCHAR(M)类型的列就最多可以存储65533个字节，这样就可能出现一个页存放不了一条记录，这种现象称为**行溢出**。 

在Compact和Reduntant行格式中，对于占用存储空间非常大的列，在记录的真实数据处只会存储该列的一部分数据，把剩余的数据分散存储在几个其他的页中进行**分页存储**，然后记录的真实数据处用20个字节存储指向这些页的地址（当然这20个字节中还包括这些分散在其他页面中的数据的占用的字节数），从而可以找到剩余数据所在的页。

这称为页的扩展，举例如下：

![image-20230125212017052](/image-20230125212017052.png)

##### Dynamic 和 Compresssed 行格式

在MySQL 8.0中，默认行格式就是Dynamic，Dynamic、Compressed行格式和Compact行格式挺像，只不过在处理行溢出数据时有分歧：

- Compressed和Dynamic两种记录格式对于存放在BLOB中的数据采用了完全的行溢出的方式。如图，在数据页中只存放20个字节的指针（溢出页的地址），实际的数据都**存放在Off Page（溢出页）中**。
- Compact和Redundant两种格式会在记录的真实数据处存储一部分数据（存放768个前缀字节）。

Compressed行记录格式的另一个功能就是，存储在其中的行数据会以zlib的算法进行压缩，因此对于BLOB、TEXT、VARCHAR这类大长度类型的数据能够进行非常有效的存储。

![image-20230125212120952](/image-20230125212120952.png)

#### Redundant行结构

Redundant是MySQL 5.0版本之前InnoDB的行记录存储方式，MySQL 5.0支持Redundant是为了兼容之前版本的页格式。

现在我们把表record_test_table的行格式修改为Redundant：

```
ALTER TABLE record_test_table ROW_FORMAT=Redundant;

Query OK, 0 rows affected (0.05 sec)

Records: 0 Duplicates: 0 Warnings: 0
```

![image-20230125212223082](/image-20230125212223082.png)

从上图可以看到，不同于Compact行记录格式，Redundant行格式的首部是一个字段长度偏移列表，同样是按照列的顺序**逆序放置**的。

下边我们从各个方面看一下Redundant行格式有什么不同的地方。

##### 字段长度便宜列表

注意Compact行格式的开头是变长字段长度列表，而Redundant行格式的开头是字段长度偏移列表，与变长字段长度列表有两处不同：

- 少了“变长”两个字：Redundant行格式会把该条记录中所有列（包括隐藏列）的长度信息都按照逆序存储到字段长度偏移列表。
- 多了“偏移”两个字：这意味着计算列值长度的方式不像Compact行格式那么直观，它是采用两个相邻数值的差值来计算各个列值的长度。

举例：比如第一条记录的字段长度偏移列表就是：

```
2B 25 1F 1B 13 0C 06
```

因为它是逆序排放的，所以按照列的顺序排列就是：

```
06 0C 13 17 1A 24 25
```

按照两个相邻数值的差值来计算各个列值的长度的意思就是：

第一列(row_id)的长度就是 0x06个字节，也就是6个字节。

第二列(transaction_id)的长度就是 (0x0C - 0x06)个字节，也就是6个字节。

第三列(roll_pointer)的长度就是 (0x13 - 0x0C)个字节，也就是7个字节。

第四列(col1)的长度就是 (0x1B - 0x13)个字节，也就是8个字节。

第五列(col2)的长度就是 (0x1F - 0x1B)个字节，也就是4个字节。

第六列(col3)的长度就是 (0x25 - 0x1F)个字节，也就是6个字节。

第七列(col4)的长度就是 (0x2B - 0x25)个字节，也就是6个字节。

##### 记录头信息

不同于Compact行格式，Redundant行格式中的记录头信息固定占用**6个字节**（48位），每位的含义见下表。

![image-20230125212246893](/image-20230125212246893.png)

- 与Compact行格式的记录头信息对比来看，有两处不同：
  - Redundant行格式多了n_field和1byte_offs_flag这两个属性。
  - Redundant行格式没有record_type这个属性。

### 区、段、碎片区

![image-20230125215041164](/image-20230125215041164.png)

#### 为什么要有区？

**B+**树的每一层中的页都会形成一个双向链表， 如果是以**页为单位**来分配存储空间的话，双向链表相邻的两个页之间的**物理位置可能离得非常远**。我们介绍B+树索引的适用场景的时候特别提到范围查询只需要定位到最左边的记录和最右边的记录，然后沿着双向链表一直扫描就可以了 ，而如果链表中相邻的两个页物理位置离得非常远，就是所谓的**随机I/O**。再一次强调，磁盘的速度和内存的速度差了好几个数量级，**随机I/O是非常慢的**，所以我们应该尽量让链表中相邻的页的物理位置也相邻，这样进行范围查询的时候才可以使用所谓的**顺序I/O**。

引入**区**的概念，一个区就是在物理位置上连续的**64个页**。因为InnoDB中的页大小默认是16KB，所以一个区的大小是64*16KB=**1MB**。在表中**数据量大**的时候，为某个索引分配空间的时候就不再按照页为单位分配了，而是按照**区为单位分配**，甚至在表中的数据特别多的时候，可以一次性分配多个连续的区。虽然可能造成**一点点空间的浪费**(数据不足以填充满整个区) ，但是从性能角度看，**可以消除很多的随机I/O**，功大于过！

#### 为什么要有段？

对于范围查询，其实是对B+树叶子节点中的记录进行顺序扫描，而如果不区分叶子节点和非叶子节点，统统把节点代表的页面放到申请到的区中的话，进行范围扫描的效果就大打折扣了。所以InnoDB对B+树的**叶子节点**和**非叶子节**点进行了区别对待，也就是说叶子节点有自己独有的区，非叶子节点也有自己独有的区。存放叶子节点的区的集合就算是一个**段(segment)**，存放非叶子节点的区的集合也算是一个段。 也就是说一个索引会生成2个段，一个**叶子节点段**，一个**非叶子节点段**。

除了索引的叶子节点段和非叶子节点段之外，InnoDB中还有为存储一些特殊的数据而定义的段， 比如回滚段。所以，常见的段有**数据段、索引段、 回滚段**。数据段即为B+树的叶子节点，索引段即为B+树的非叶子节点。

在InnoDB存储引擎中，对段的管理都是由引擎自身所完成，DBA不能也没有必要对其进行控制。这从一定程度上简化了DBA对于段的管理。

段其实**不对应表空间中某一个连续的物理区域**， 而是一个**逻辑上的概念**，由若千个零散的页面以及一些完整的区组成。

#### 为什么要有碎片区？

默认情况下，一个使用InnoDB存储引擎的表只有一个聚簇索引， 一个索引会生成2个段,而段是以区为单位申请存储空间的，一个区默认占用1M (64 * 16Kb = 1024Kb)存储空间，所以**默认情况下一个只存了几条记录的小表也需要2M的存储空间么？**以后每次添加一个索引都要多申请2M的存储空间么？这对于存储记录比较少的表简直是天大的浪费。这个问题的症结在于到现在为止我们介绍的区都是非常纯粹的，也就是一个区被整个分配给某一个段，或者说区中的所有页面都是为了存储同一个段的数据而存在的，即使段的数据填不满区中所有的页面，那余下的页面也不能挪作他用。

为了考虑以完整的区为单位分配给某个段对于**数据量较小**的表太浪费存储空间的这种情况，InnoDB提出了一个**碎片(fragment) 区**的概念。在一个碎片区中， 并不是所有的页都是为了存储同一个段的数据而存在的，而是碎片区中的页可以用于不同的目的，比如有些页用于段A，有些页用于段B，有些页甚至哪个段都不属于。**碎片区直属于表空间，并不属于任何一个段。**

所以此后为某个段分配存储空间的策略是这样的：

- 在刚开始向表中插入数据的时候，段是从某个碎片区以单个页面为单位来分配存储空间的。
- 当某个段已经占用了**32个碎片区**页面之后，就会申请以完整的区为单位来分配存储空间。

所以现在段不能仅定义为是某些区的集合，更精确的应该是**某些零散的页面**以及**一些完整的区**的集合。

#### 区的分类

区大体上可以分为4种类型：

- **空闲的区(FREE) ：**现在还没有用到这个区中的任何页面。
- **有剩余空间的碎片区(FREE_FRAG) ：**表示碎片区中还有可用的页面。
- **没有剩余空间的碎片区(FULL_FRAG) ：**表示碎片区中的所有页面都被使用，没有空闲页面。
- **附属于某个段的区(FSEG) ：**每一个索引都可以分为叶子节点段和非叶子节点段。

处于**FREE、FREE_ FRAG 以及FULL_ FRAG**这三种状态的区都是独立的，直属于表空间。而处于FSEG状态的区是 附属于某个段的。

> 如果把表空间比作是一个集团军，段就相当于师，区就相当于团。一般的团都是隶属于某个师的，就像是处于**FSEG**的区全都隶属于某个段，而处于**FREE、FREE FRAG以及FULL. FRAG**这三种状态的区却直接隶属于表空间，就像独立团直接听命于军部-样。

### 表空间

表空间可以看做是InnoDB存储引擎逻辑结构的最高层，所有的数据都存放在表空间中。

表空间是一个**逻辑容器**，表空间存储的对象是段，在一个表空间中可以有一个或多个段，但是一个段只能属于一个表空间。表空间数据库由一个或多个表空间组成，表空间从管理上可以划分为**系统表空间**(System tablespace)、**独立表空间**(ile-per-table tablespace)、**撤销表空间**(Undo Tablespace)和**临时表空间**(Temporary Tablespace)等。

#### 独立表空间

独立表空间，即每张表有一个独立的表空间，也就是数据和索引信息都会保存在自己的表空间中。独立的表空间(即:单表)可以在不同的数据库之间进行**迁移**。

空间可以回收(DROP TABLE操作可自动回收表空间;其他情况，表空间不能自己回收)。如果对于统计分析或是日志表，删除大量数据后可以通过：`alter table TableName engine=innodb` ；回收不用的空间。对于使用独立表空间的表，不管怎么删除，表空间的碎片不会太严重的影响性能，而且还有机会处理。

**独立表空间结构**

独立表空间由段、区、页组成。前面已经讲解过了。

**真实表空间对应的文件大小**

我们到数据目录里看，会发现一个新建的表对应的**. ibd**文件**只占用了96K，才6个页面大小(MySQL5.7中)** ，这是因为一开始表空间占用的空间很小，因为表里边都没有数据。不过别忘了这些.ibd文件是**自扩展的**，随着表中数据的增多，表空间对应的文件也逐渐增大。

**8.0中. ibd文件占用了112K，7个页面大小，为什么？**

- 表名.frm存储表结构(MySQL8.0时， 合并在表名.ibd中)
- 表名.ibd存储数据和索引

**查看InnoDB的表空间类型**

```
mysql> show variables like 'innodb_file_per_table';
```

![image-20230125215741153](/image-20230125215741153.png)

你能看到innodb_file_per_table=ON, 这就意味着每张表都会单独保存为一个.ibd文件。

#### 系统表空间

系统表空间的结构和独立表空间基本类似，只不过由于整个MySQL进程只有一个系统表空间，在系统表空间中会额外记录一些有关整个系统信息的页面，这部分是独立表空间中没有的。

**InnoDB数据字典**

每当我们向一个表中插入一条记录的时候，MySQL校验过程 如下：

先要校验一下插入语句对应的表存不存在，插入的列和表中的列是否符合，如果语法没有问题的话，还需要知道该表的聚簇索引和所有二级索引对应的根页面是哪个表空间的哪个页面，然后把记录插入对应索引的B+树中。所以说，MySQL除了保存着我们插人的用户数据之外，还需要保存许多额外的信息，比方说：

> 某个表属于哪个表空间，表里边有多少列
>
> 表对应的每一个列的类型是什么
>
> 该表有多少索引，每个索引对应哪几个字段，该索引对应的根页面在哪个表空间的哪个页面
>
> 该表有哪些外键，外键对应哪个表的哪些列
>
> 某个表空间对应文件系统上文件路径是什么
>
> . . . . . .

上述这些数据并不是我们使用**INSERT**语句插入的用户数据，实际上是为了更好的管理我们这些用户数据而不得已引入的一些额外数据，这些数据也称为**元数据**。InnoDB存储引擎特意定义了一些列的**内部系统表**(internalsystem table)来记录这些这些元数据：

![image-20230125215758848](/image-20230125215758848.png)

这些系统表也被称为**数据字典**，它们都是以B+树的形式保存在系统表空间的某些页面中，其中**SYS_TABLES、SYS_COLUMNS、SYS_ INDEXES、 SYS_ FIELDS** 这四个表尤其重要，称之为基**本系统表**(basic system tables) 

**注意：**

用户是**不能直接访问**InnoDB的这些内部系统表，除非你直接去解析系统表空间对应文件系统上的文件。不过考虑到查看这些表的内容可能有助于大家分析问题，所以在系统数据库**information_ schema**中提供了一些以innodb_sys开头的表：

```
mysql> USE information_schema;
Database changed

mysql> SHOW TABLES LIKE 'innodb)sys%';
```

在**information_ schema** 数据库中的这些以**INNODB_SYS**开头的表并不是真正的内部系统表(内部系统表就是我们上边以**SYS**开头的那些表)，而是在存储引擎启动时读取这些以SYS开头的系统表，然后填充到这些以INNODB_SYS 开头的表中。以**INNODB_SYS**开头的表和以**SYS**开头的表中的字段并不完全一样，但供参考已经足矣。

### 数据页加载的三种方式

InnoDB从磁盘中读取数据的最小单位是数据页。而你想得到的id = xxx的数据，就是这个数据页众多行中的一-行。

对于MySQL存放的数据，逻辑概念上我们称之为表,在磁盘等物理层面而言是按数据页形式进行存放的，当其加载到MySQL中我们称之为缓存页。

如果缓冲池中没有该页数据，那么缓冲池有以下三种读取数据的方式，每种方式的读取效率都是不同的：

1. 内存读取

   - 如果该数据存在于内存中，基本上执行时间在1ms左右，效率还是很高的。

     ![image-20230125220021032](/image-20230125220021032.png)

2. 随机读取

   - 如果数据没有在内存中，就需要在磁盘上对该页进行查找，整体时间预估在10ms 左右，这10ms中有6ms是磁盘的实际繁忙时间(包括了**寻道和半圈旋转时间**)，有3ms是对可能发生的排队时间的估计值，另外还有1ms的传输时间，将页从磁盘服务器缓冲区传输到数据库缓冲区中。这10ms看起来很快，但实际上对于数据库来说消耗的时间已经非常长了，因为这还只是一个页的读取时间。

     ![image-20230125220030836](/image-20230125220030836.png)

3. 顺序读取

   - 顺序读取其实是一种批量读取的方式，因为我们请求的**数据在磁盘上往往都是相邻存储的**，顺序读取可以帮我们批量读取页面，这样的话，一次性加载到缓冲池中就不需要再对其他页面单独进行磁盘I/O操作了。如果一个磁盘的吞吐量是40MB/S，那么对于一个16KB大小的页来说，一次可以顺序读取2560 (40MB/16KB) 个页，相当于一个页的读取时间为0.4ms。采用批量读取的方式，即使是从磁盘上进行读取，效率也比从内存中只单独读取一个页的效率要高。

## 索引的创建和设计原则

### 索引的创建与设计使用

#### 索引的分类

MySQL的索引包括普通索引、唯一性索引、全文索引、单列索引、多列索引和空间索引等。

- 从**功能逻辑**上说，索引主要有4种，分别是普通索引、唯一索引、主键索引、全文索引。
- 按照**物理实现方式** ，索引可以分为 2 种：聚簇索引和非聚簇索引。
- 按照**作用字段个数**进行划分，分成单列索引和联合索引。

1. **普通索引**
   - 在创建普通索引时，不附加任何限制条件，只是用于提高查询效率。这类索引可以创建在任何数据类型中，其值是否唯一和非空，要由字段本身的完整性约束条件决定。建立索引以后，可以通过索引进行查询。
   - 例如，在表student的字段name上建立一个普通索引，查询记录时就可以根据该索引进行查询。
2. **唯一性索引**
   - 使用**UNIQUE**参数可以设置索引为唯一性索引，在创建唯一性索引时，限制该索引的值必须是唯一的， 但允许有空值。在一张数据表里可以有多个唯一索引。
   - 例如，在表student的字段email中创建唯一性索引，那么字段email的值就必须是唯一的。通过唯一性索引，可以更快速地确定某条记录。
3. **主键索引**
   - 主键索引就是一种**特殊的唯性索引** ，在唯一索引的基础上增加了不为空的约束，也就是NOT NULL+UNIQUE，一张表里**最多只有一个**主键索引。
   - **Why？**这是由主键索引的物理实现方式决定的，因为数据存储在文件中只能按照一种顺序进行存储。
4. **单列索引**
   - 在表中的单个字段上创建索引。单列索引只根据该字段进行索引。单列索引可以是普通索引，也可以是唯一性索引，还可以是全文索引。只要保证该索引只对应一个字段即可。一个表可以有多个单列索引。
5. **多列(组合、联合)索引**
   - 多列索引是在表的**多个字段组合**，上创建一个索引。 该索引指向创建时对应的多个字段，可以通过这几个字段进行查询，但是只有查询条件中使用了这些字段中的第一个字段时才会被使用。例如，在表中的字段id. name和gender上建立一个多列索引**idx_ id_name_ gender** ,只有在查询条件中使用了字段id时该索引才会被使用。使用组合索引时遵循**最左前缀集合**。
6. **全文索引**
   - 全文索引(也称全文检索)是目前搜索引擎使用的一种关键技术。它能够利用【**分词技术**】等多种算法智能分析出文本文字中关键词的频率和重要性，然后按照一定的算法规则智能地筛选出我们想要的搜索结果。 全文索引非常适合大型数据集，对于小的数据集，它的用处比较小。
   - 使用参数**FULLTEXT**可以设置索引为全文索引。在定义索引的列上支持值的全文查找，允许在这些索引例中插入重复值和空值。全文索引只能创建在**CHAR、VARCHAR 或TEXT**类型及其系列类型的字段上，**查询数据量较大的字符串类型的字段时，使用全文索引可以提高查询速度**。
   - 例如，表student的字段information是TEXT类型，该字段包含了很多文字信息。在字段information. 上建立全文索引后，可以提高查询字段information的速度。
   - 全文索引典型的有两种类型：自然语言的全文索引 和 布尔全文索引。
     - 自然语言搜索引擎将计算每一个文档对象和查询的相关度。这里， 相关度是基于匹配的关键词的个数，以及关键词在文档中出现的次数。**在整个索引中出现次数越少的词语，匹配时的相关度就越高。**相反，非常常见的单词将不会被搜索，如果一个词语的在超过50%的记录中都出现了，那么自然语言的搜索将不会搜索这类词语。
   - MySQL数据库从3.23.23版开始支持全文索引，但MySQL5.6.4以前只有Myisam支持，5.6.4版本以后 innodb才支持，但是官方版本不支持中文分词，需要第3三E方分词插件。在5.7.6版本，MySQL内置了 ngram全文解析器，用来支持亚洲语种的分词。测试或使用全文索引时，要先看一下自己的MySQL版本、存储弓|擎和数据类型是否支持全文索引。
   - 随着大数据时代的到来，关系型数据库应对全文索引的需求已力不从心，逐渐被**solr 、ElasticSearch**等专门的搜索引擎所替代。
7. **补充：空间索引**
   - 使用参数**SPATIAL**可以设置索引为**空间索引**。空间索引只能建立在空间数据类型上,这样可以提高系统获取空间数据的效率。MySQL中的空间数据类型包括**GEOMETRY、POINT 、LINESTRING 和POLYGON**等。目前只有MyISAM存储引擎支持空间检索，而且索引的字段不能为空值。对于初学者来说，这类索引很少会用到。

**小结：**

**InnoDB：支持B-tree、Full-text 等索引，不支持Hash索引；**

**MyISAM：支持B-tree、Full-text 等索引，不支持Hash索引；**

Memory：支持B-tree、Hash 等索引，不支持Full-text 索引；

NDB：支持Hash索引，不支持B-tree、Full-text 等索引；

Archive：不支持B-tree、Hash、 Fll-text 等索引。

#### 创建索引

MySQL支持多种方法在单个或多个列上创建索引：在创建表的定义语句**CREATE TABLE** 中指定索引列，使用`ALTER TABLE`语句在存在的表上创建索引，或者使用`CREATE INDEX`语句在已存在的表上添加索引。

##### 创建表的时候创建索引

**隐式的方式创建索引**，在声明有主键约束、唯一性约束、外键约束的字段上，会自动添加相关的索引。

```
CREATE TABLE dept( 
    dept_id INT PRIMARY KEY AUTO_INCREMENT, 
    dept_name VARCHAR(20) 
);

CREATE TABLE emp( 
    emp_id INT PRIMARY KEY AUTO_INCREMENT, 
    emp_name VARCHAR(20) UNIQUE, 
    dept_id INT, 
    CONSTRAINT emp_dept_id_fk FOREIGN KEY(dept_id) REFERENCES dept(dept_id) 
);
```

但是，如果显式创建表时创建索引的话，基本语法格式如下：

```
CREATE TABLE table_name [col_name data_type] 
[UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY] [index_name] (col_name [length]) [ASC | DESC]
```

- **UNIQUE** 、 **FULLTEXT** 和 **SPATIAL** 为可选参数，分别表示唯一索引、全文索引和空间索引；
- **INDEX** 与 **KEY** 为同义词，两者的作用相同，用来指定创建索引；
- **index_name** 指定索引的名称，为可选参数，如果不指定，那么MySQL默认col_name为索引名；
- **col_name** 为需要创建索引的字段列，该列必须从数据表中定义的多个列中选择；
- **length** 为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度；
- **ASC** 或 **DESC** 指定升序或者降序的索引值存储。

**显式的方式创建索引**

1. **创建普通索引**

   - ```
     CREATE TABLE book( 
         book_id INT , 
         book_name VARCHAR(100), 
         authors VARCHAR(100), 
         info VARCHAR(100) , 
         comment VARCHAR(100), 
         year_publication YEAR, 
         INDEX idx_name(book_name)
     );
     ```

   - 查看索引的方式

     - 方式一：

     - ```
       SHOW CREATE TABLE book/G;
       ```

     - 方式二：

     - ```
       SHOW INDEX FEOM book;
       ```

     - 性能分析工具：Explain

     - ```
       Explain select * from book where book_name="mysql"
       ```

2. **创建唯一索引**

   - ```
     CREATE TABLE book( 
         book_id INT , 
         book_name VARCHAR(100), 
         authors VARCHAR(100), 
         info VARCHAR(100) , 
         comment VARCHAR(100), 
         year_publication YEAR, 
         UNIQUE INDEX uk_idx_cmt)(comment)
     );
     ```

   - 声明唯一索引的字段，再添加数据时，要保证唯一性，但是可以添加null值。

3. **主键索引**

   - 通过定义主键约束的方式定义主键索引

   - ```
     CREATE TABLE book( 
         book_id INT PRIMARY KEY, 
         book_name VARCHAR(100), 
         authors VARCHAR(100), 
         info VARCHAR(100), 
         comment VARCHAR(100), 
         year_publication YEAR, 
     );
     ```

   - 通过删除主键约束的方式，删除主键索引

   - ```
     ALTER  TABLE book
     DROP primary KEY;
     ```

4. **创建单列索引**

   - ```
     CREATE TABLE book( 
         book_id INT , 
         book_name VARCHAR(100), 
         authors VARCHAR(100), 
         info VARCHAR(100) , 
         comment VARCHAR(100), 
         year_publication YEAR, 
         #声明索引
         UNIQUE INDEX idx_name(book_name)
     );
     ```

5. **创建组合索引**

   - ```
     CREATE TABLE book( 
         book_id INT , 
         book_name VARCHAR(100), 
         authors VARCHAR(100), 
         info VARCHAR(100) , 
         comment VARCHAR(100), 
         year_publication YEAR, 
         #声明索引
         INDEX idx_id_name_info(book_id,book_name,info)
     );
     ```

6. **创建全文索引（了解）**

   - ```
     CREATE TABLE test4( 
         id INT NOT NULL, 
         name CHAR(30) NOT NULL, 
         age INT NOT NULL, 
         info VARCHAR(255), 
         FULLTEXT INDEX futxt_idx_info(info) 
         #FULLTEXT INDEX futxt_idx_info(info(50))  前50个字符
     ) ENGINE=MyISAM;
     ```

   - ```
     CREATE TABLE articles ( 
         id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY, 
         title VARCHAR (200), 
         body TEXT, FULLTEXT index (title, body) 
     ) ENGINE = INNODB ;
     ```

   - ```
     CREATE TABLE `papers` ( 
         `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
         `title` varchar(200) DEFAULT NULL, 
         `content` text, 
         PRIMARY KEY (`id`), 
         FULLTEXT KEY `title` (`title`,`content`) 
     ) ENGINE=MyISAM DEFAULT CHARSET=utf8;
     ```

   - 不同于like方式的的查询：

     - ```
       SELECT * FROM papers WHERE content LIKE ‘%查询字符串%’;
       ```

   - 全文索引用match+against方式查询：

     - ```
       SELECT * FROM papers WHERE MATCH(title,content) AGAINST (‘查询字符串’);
       ```

   - > **注意点**
     >
     > 1. 使用全文索引前，搞清楚版本支持情况；(5.5myisam支持，innodb不支持)
     > 2. 全文索引比 like + % 快 N 倍，但是可能存在精度问题；
     > 3. 如果需要全文索引的是大量数据，建议先添加数据，再创建索引。

7. **创建空间索引**

   - 空间索引创建中，要求空间类型的字段必须为非空 。

   - 举例：创建表test5，在空间类型为GEOMETRY的字段上创建空间索引，SQL语句如下：

   - ```
     CREATE TABLE test5( 
         geo GEOMETRY NOT NULL, 
         SPATIAL INDEX spa_idx_geo(geo) 
     ) ENGINE=MyISAM;
     ```

##### 在已经存在的表上创建索引

在已经存在的表中创建索引可以使用ALTER TABLE语句或者CREATE INDEX语句。

1. 使用**ALTER TABLE**语句创建索引**ALTER TABLE**语句创建索引的基本语法如下：

   - ```
     ALTER TABLE table_name ADD [UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY] 
     [index_name] (col_name[length],...) [ASC | DESC] 
     ```

   - ```
     ALTER TABLE books ADD INDEX idx_cmt(COMMENT);
     ```

   - ```
     ALTER TABLE books ADD INDEX idx_id_name_info(book_id,book_name,info);
     ```

2. 使用**CREATE INDEX**创建索引**CREATE INDEX**语句可以在已经存在的表上添加索引，在MySQL中，CREATE INDEX被映射到一个ALTER TABLE语句上，基本语法结构为：

   - ```
     CREATE [UNIQUE | FULLTEXT | SPATIAL] INDEX index_name ON table_name (col_name[length],...) [ASC | DESC]
     ```

   - ```
     CREATE INDEX idx_cmt ON book(COMMENT);
     ```

   - ```
     CREATE INDEX idx_id_name_info ON book(book_id,book_name,info);
     ```

#### 删除索引

1. 使用**ALTER TABLE**删除索引**ALTER TABLE**删除索引的基本语法格式如下：

   - ```
     ALTER TABLE table_name DROP INDEX index_name;
     ```

   - > **提示**
     >
     > 添加AUTO_INCREMENT约束字段的唯一索引不能被删除。

2. 使用**DROP INDEX**语句删除索引**DROP INDEX**删除索引的基本语法格式如下：

   - ```
     DROP INDEX index_name ON table_name;
     ```

> **提示**
>
> 删除表中的列时，如果要删除的列为索引的组成部分，则该列也会从索引中删除。如果组成索引的所有列都被删除，则整个索引将被删除。

### MySQL8.0索引新特性

#### 支持降序索引

降序索引虽然在MySQL4中就已经支持降序索引的语法了，但实际上直到MySQL8.0才真正支持降序索引（InnoDB存储引擎）。

**MySQL在8.0之前创建的仍然是升序索引，使用时如果进行反向扫描，这大大降低了数据库的效率。**

举例：分别在MySQL 5.7版本和MySQL 8.0版本中创建数据表ts1，结果如下：

```
CREATE TABLE ts1(a int,b int,index idx_a_b(a,b desc));
```

mysql5.7

![image-20230126144019857](/image-20230126144019857.png)

mysql8.0

![image-20230126144034567](/image-20230126144034567.png)

从结果可以看出，索引已经是降序了。

使用存储过程，在表中插入800条数据用于检索

```
DELIMITER //
CREATE PROCEDURE ts_insert() 
BEGIN
	DECLARE i INT DEFAULT 1; 
	WHILE i < 800 
	DO 
		insert into ts1 select rand()*80000,rand()*80000;
        SET i = i + 1; 
    END WHILE; 
    commit; 
END // 
DELIMITER ; 

#调用
CALL ts_insert();
```

在mysql5.7和mysql8.0中，都使用以下优化测试：

```
EXPLAIN SELECT * FROM ts1 ORDER BY a,b DESC LIMIT 5;
```

mysql5.7：

从结果可以看出，执行计划中扫描数为799，而且使用了Using filesort。

> 提示 Using filesort是MySQL中一种速度比较慢的外部排序，能避免是最好的。多数情况下，管理员可以通过优化索引来尽量避免出现Using filesort，从而提高数据库执行速度。

mysql8.0

在MySQL 8.0版本中查看数据表ts1的执行计划。从结果可以看出，执行计划中扫描数为5，而且没有使用Using filesort。

> 注意 降序索引只对查询中特定的排序顺序有效，如果使用不当，反而查询效率更低。例如，上述查询排序条件改为order by a desc, b desc，MySQL 5.7的执行计划要明显好于MySQL 8.0。

**将排序条件修改为order by a desc, b desc后**，下面来对比不同版本中执行计划的效果。 

```
EXPLAIN SELECT * FROM ts1 ORDER BY a DESC,b DESC LIMIT 5;
```

从结果可以看出，修改后MySQL 5.7的执行计划要明显好于MySQL 8.0。 可以再创建一个a DESC,b DESC的索引，效率就会提升。

#### 隐藏索引

直接删除索引，可能会导致一些连锁效应，导致牵一发而动全身。因此可以先将需要删除的索引隐藏起来，如果隐藏起来后对整个数据库没有影响，那么再彻底删除。 

**这种通过先将索引设置为隐藏索引，再删除索引的方式就是软删除。**

> **注意**
>
> 主键不能被隐藏，当表中没有显式主键时，表中第一个唯一非空索引会成为隐式主键，也不能设置为隐藏索引。

1. **创建表时直接创建** 在MySQL中创建隐藏索引通过SQL语句INVISIBLE来实现，其语法形式如下：

```
CREATE TABLE tablename( 
    propname1 type1[CONSTRAINT1], 
    propname2 type2[CONSTRAINT2], 
    ……
    propnamen typen, 
    INDEX [indexname](propname1 [(length)]) INVISIBLE 
);
CREATE TABLE book( 
    book_id INT , 
    book_name VARCHAR(100), 
    authors VARCHAR(100), 
    info VARCHAR(100) , 
    comment VARCHAR(100), 
    year_publication YEAR, 
    INDEX idx_name(book_name) invisible
);
```

上述语句比普通索引多了一个关键字INVISIBLE，用来标记索引为不可见索引。

1. **在已经存在的表上创建**

语法形式如下：

```
CREATE INDEX indexname 
ON tablename(propname[(length)]) INVISIBLE; 
CREATE INDEX book
ON index_name (book_name) INVISIBLE; 
```

1. **通过**ALTER TABLE**语句创建**

语法形式如下：

```
ALTER TABLE tablename 
ADD INDEX indexname (propname [(length)]) INVISIBLE;
ALTER TABLE book
ADD INDEX index_name (book_name) INVISIBLE; 
```

1. **切换索引可见状态** 已存在的索引可通过如下语句切换可见状态：

```
ALTER TABLE tablename ALTER INDEX index_name INVISIBLE; #切换成隐藏索引 
ALTER TABLE tablename ALTER INDEX index_name VISIBLE; #切换成非隐藏索引
```

如果将index_cname索引切换成可见状态，通过explain查看执行计划，发现优化器选择了index_cname索引。

> 注意 
>
> 当索引被隐藏时，它的内容仍然是和正常索引一样实时更新的。如果一个索引需要长期被隐藏，那么可以将其删除，因为**索引的存在会影响插入、更新和删除的性能。**

通过设置隐藏索引的可见性可以查看索引对调优的帮助。

1. **使隐藏索引对查询优化器可见**

在MySQL 8.x版本中，为索引提供了一种新的测试方式，可以通过查询优化器的一个开关（use_invisible_indexes）来打开某个设置，使隐藏索引对查询优化器可见。如果 use_invisible_indexes设置为off(默认)，优化器会忽略隐藏索引。如果设置为on，即使隐藏索引不可见，优化器在生成执行计划时仍会考虑使用隐藏索引。

（1）在MySQL命令行执行如下命令查看查询优化器的开关设置。

```
mysql> select @@optimizer_switch \G
```

在输出的结果信息中找到如下属性配置。

```
use_invisible_indexes=off
```

此属性配置值为off，说明隐藏索引默认对查询优化器不可见。

（2）使隐藏索引对查询优化器可见，需要在MySQL命令行执行如下命令：

```
mysql> set session optimizer_switch="use_invisible_indexes=on"; 
Query OK, 0 rows affected (0.00 sec)
```

SQL语句执行成功，再次查看查询优化器的开关设置。

```
mysql> select @@optimizer_switch \G 
*************************** 1. row *************************** 
@@optimizer_switch: 
index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=on,mrr_cost_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on,use_invisible_indexes=on,skip_scan=on,hash_join=on 
1 row in set (0.00 sec)
```

此时，在输出结果中可以看到如下属性配置。

```
use_invisible_indexes=on
```

use_invisible_indexes属性的值为on，说明此时隐藏索引对查询优化器可见。

（3）使用EXPLAIN查看以字段invisible_column作为查询条件时的索引使用情况。

```
explain select * from classes where cname = '高一2班';
```

查询优化器会使用隐藏索引来查询数据。

（4）如果需要使隐藏索引对查询优化器不可见，则只需要执行如下命令即可。

```
mysql> set session optimizer_switch="use_invisible_indexes=off"; 
Query OK, 0 rows affected (0.00 sec)
```

再次查看查询优化器的开关设置。

```
mysql> select @@optimizer_switch \G 
```

此时，use_invisible_indexes属性的值已经被设置为“off”。

### 索引的设计原则

为了使索弓|的使用效率更高，在创建索引时，必须考虑在哪些字段上创建索引和创建什么类型的索引。**索引设计不合理或者缺少索引都会对数据库和应用程序的性能造成障碍**。高效的索弓|对于获得良好的性能非常重要。设计索引时，应该考虑相应准则。

#### 数据准备

**第1步：创建数据库、创建表**

```
#1.创建学生表和课程表
CREATE TABLE `student_info` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`student_id` INT NOT NULL ,
`name` VARCHAR(20) DEFAULT NULL,
`course_id` INT NOT NULL ,
`class_id` INT(11) DEFAULT NULL,
`create_time` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`) 
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

CREATE TABLE `course` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`course_id` INT NOT NULL ,
`course_name` VARCHAR(40) DEFAULT NULL,
PRIMARY KEY (`id`) 
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

**第2步：创建模拟数据必需的存储函数**

```
#函数1：创建随机产生字符串函数
DELIMITER // 
CREATE FUNCTION rand_string(n INT) 
	RETURNS VARCHAR(255) #该函数会返回一个字符串 
BEGIN
	DECLARE chars_str VARCHAR(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ'; 
	DECLARE return_str VARCHAR(255) DEFAULT '';
	DECLARE i INT DEFAULT 0; 
	WHILE i < n DO 
		SET return_str =CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1)); 
		SET i = i + 1; 
	END WHILE; 
	RETURN return_str; 
END // 
DELIMITER ;
#函数2：创建随机数函数 
DELIMITER // 
CREATE FUNCTION rand_num (from_num INT ,to_num INT) RETURNS INT(11) 
BEGIN 
DECLARE i INT DEFAULT 0; 
SET i = FLOOR(from_num +RAND()*(to_num - from_num+1)) ; 
RETURN i; 
END // 
DELIMITER ;
```

创建函数，假如报错：

```
This function has none of DETERMINISTIC......
```

由于开启过慢查询日志bin-log, 我们就必须为我们的function指定一个参数。

主从复制，主机会将写操作记录在bin-log日志中。从机读取bin-log日志，执行语句来同步数据。如果使用函数来操作数据，会导致从机和主键操作时间不一致。所以，默认情况下，mysql不开启创建函数设置。

- 查看mysql是否允许创建函数：

```
show variables like 'log_bin_trust_function_creators';
```

- 命令开启：允许创建函数设置：

```
set global log_bin_trust_function_creators=1; # 不加global只是当前窗口有效。
```

- mysqld重启，上述参数又会消失。永久方法：

  - windows下：my.ini[mysqld]加上：

    ```
    log_bin_trust_function_creators=1
    ```

  - linux下：/etc/my.cnf下my.cnf[mysqld]加上：

    ```
    log_bin_trust_function_creators=1
    ```

第3步：创建插入模拟数据的存储过程

```
#存储过程1：创建插入课程表存储过程
DELIMITER//
CREATE PROCEDURE insert_course(max_num INT)
BEGIN
DECLARE iINT DEFAULT 0;
SET autocommit=0;  #设置手动提交事务
REPEAT #循环
SET i=i+1; #赋值
INSERT INTO course(course_id,course_name)VALUES (rand_num(10000,10100),rand_string(6));
UNTIL i=max_num
END REPEAT;
COMMIT;#提交事务
END//
DELIMITER;


#存储过程2：创建插入学生信息表存储过程
DELIMITER//
CREATE PROCEDURE insert_stu(max_numINT)
BEGIN
DECLARE i INT DEFAULT 0;
SET autocommit=0; #设置手动提交事务
REPEAT #循环
SET i=i+1; #赋值
INSERT INTO student_info ( course_id,class_id,student_id,NAME) VALUES (rand_num(10000,10100),rand_num(10000,10200),rand_num(1,200000),rand_string(6));
UNTIL i=max_num
END REPEAT; 
COMMIT; #提交事务
END//
DELIMITER;
```

**第4步：调用存储过程**

```
CALL insert_course(100);#100门课
CALL insert_stu(1000000);#1000000个学生
```

#### 哪些情况适合创建索引（11种）

##### 1.字段的数值有唯一性的约束

索引本身可以起到约束的作用，比如唯-索引.主键索引都是可以起到唯一性约束的，因此在我们的数据表中，如果**某个字段是唯一性的**，就可以直接**创建唯一性索引**，或者**主键索引**。这样可以更快速地通过该索引来确定某条记录。

例如，学生表中**学号**是具有唯一性的字段， 为该字段建立唯一性索引可以很快确定某个学生的信息，如果使用**姓名**的话，可能存在同名现象，从而降低查询速度。

> **业务上具有唯一特性的字段，即使是组合字段，也必须建成唯一索引。**(来源: Alibaba)
>
> 说明：不要以为唯一索引影响了insert 速度，这个速度损耗可以忽略，但提高查找速度是明显的。

##### 2.频繁作为WHERE查询条件的字段

某个字段在SELECT语句的WHERE条件中经常被使用到，那么就需要给这个字段创建索引了。尤其是在数据量大的情况下，创建普通索引就可以大幅提升数据查询的效率。

比如student_ info数据表(含100万条数据)，假设我们想要查询student_id=123110 的用户信息。

![image-20230126144131017](/image-20230126144131017.png)

##### 3.经常GROUP BY和ORDER BY的列

索引就是让数据按照某种顺序进行存储或检索，因此当我们使用GROUP BY对数据进行分组查询，或者使用ORDER BY对数据进行排序的时候，就需要**对分组或者排序的字段进行索引**。如果待排序的列有多个，那么可以在这些列上建立**组合索引**。

ORDER BY：索引已经帮忙排好序了，可以节省排序时间，提高效率；

GROUP BY：索引建立时已经排好序了，相同的数据离的很近，可以节省查找的时间，提高了效率。

![image-20230126144144118](/image-20230126144144118.png)

gruop by+order by：

没有索引：

![image-20230126144206996](/image-20230126144206996.png)

创建联合索引：

![image-20230126144218132](/image-20230126144218132.png)

##### 4.UPDATE、DELETE的WHERE条件列

对数据按照某个条件进行查询后再进行UPDATE或DELETE的操作，如果对WHERE字段创建了索引，就能大幅提升效率。原理是因为我们需要先根据WHERE条件列检索出来这条记录，然后再对它进行更新或删除。**如果进行更新的时候，更新的字段是非索引字段，提升的效率会更明显，这是因为非索引字段更新不需要对索引进行维护。**

![image-20230126144339029](/image-20230126144339029.png)

##### 5.DISTINCT字段需要创建索引

有时候我们需要对某个字段进行去重，使用DISTINCT，那么对这个字段创建索引，也会提升查询效率。

##### 6.多表JOIN连接操作时，创建索引注意事项

首先，**连接表的数量尽量不要超过3张**，因为每增加一-张表就相当于增加了-次嵌套的循环，数量级增长会非常快，严重影响查询的效率。

其次，**对WHERE条件创建索引**，因为WHERE才是对数据条件的过滤。如果在数据量非常大的情况下，没有WHERE条件过滤是非常可怕的。

最后，**对用于连接的字段创建索引**，并且该字段在多张表中的**类型必须一致**。比如course_id 在student_info 表和course表中都为int(11)类型，而不能一个为 int另一个为varchar类型。

举个例子，如果我们只对student. id创建索引，执行SQL语句：

![image-20230126151746842](/image-20230126151746842.png)

##### 7.使用列的类型小的创建索引

我们这里所说的**类型大小**指的就是该类型表示的数据范围的大小。

我们在定义表结构的时候要显式的指定列的类型，以整数类型为例，有**INYINT、MEDIUMINT、 INT、BIGINT**等，它们占用的存储空间依次递增，能表示的整数范围当然也是依次递增。如果我们想要对某个整数列建立索引的话，在表示的整数范围允许的情况下，尽量让索引列使用较小的类型，比如我们能使用INT就不要使用**BIGINT**，能使用**MEDIUMINT**就不要使用**INT**。这是因为：

●数据类型越小，在查询时进行的比较操作越快

●数据类型越小，索引占用的存储空间就越少，在一个数据页内就可以**放下更多的记录**，从而减少磁盘I/O带来的性能损耗，也就意味着可以把更多的数据页缓存在内存中，从而加快读写效率。

这个建议对于表的**主键来说更加适用**，因为不仅是聚簇索引中会存储主键值，其他所有的二级索引的节点处都会存储一份记录的主键值，如果主键使用更小的数据类型，也就意味着节省更多的存储空间和更高效的I/O。

##### 8.使用字符串前缀创建索引

假设我们的字符串很长，那存储一个字符串就需要 占用很大的存储空间。在我们需要为这个字符串列建立索引时，那就意味着在对应的B+树中有这么两个问题：

- B+树索引中的记录需要把该列的完整字符串存储起来，更费时。而且**字符串越长，在索引中占用的存储空间越大**。
- 如果B+树索引中索引列存储的字符串很长，那在做字符串**比较时会占用更多的时间。**

我们可以通过截取字段的前面一部分内容建立索引，这个就叫**前缀索引**。这样在查找记录时虽然不能精确的定位到记录的位置，但是能定位到相应前缀所在的位置，然后根据前缀相同的记录的主键值回表查询完整的字符串值。既**节约空间**，又**减少了字符串的比较时间**，还大体能解决排序的问题。

**那么，问题来了，截取多少呢？**

```
select count( distinct address) /count (*) from shop;

count(distinct left(列名，索引长度))/count(*)#越接近1越好
```

**拓展：Alibaba《java开发手册》**

【强制】在varchar字段上建立索引时，必须指定索引长度，没必要对全字段建立索引，根据实际文本区分度决定索引长度。

说明：索引的长度与区分度是一对矛盾体，一般对字符串类型数据，长度为20的索引，区分度会高达90%以上，可以使用count(distinct left(列名，索引长度) / count(*) 的区分度来确定。

##### 9.区分度高(散列性高)的列适合作为索引

列的基数指的是某一列中不重复数据的个数, 比方说某个列包含值2，5，8， 2，5，8，2，5，8，虽然有9条记录，但该列的基数却是3。也就是说，**在记录行数一定的情况下，列的基数越大,该列中的值越分散：列的基数越小，该列中的值越集中。** 这个列的基数指标非常重要，直接影响我们是否能有效的利用索引。最好为列的基数大的列建立索引，为基数太小列的建立索引效果可能不好。

可以使用公式`select count (distinct a)/count(*) from t1`计算区分度，越接近1越好，一般超过 33%就算是比较高效的索引了。

拓展：联合索引把区分度高(散列性高)的列放在前面。

##### 10.使用最频繁的列放到联合索引的左侧

这样也可以较少的建立一些索引。同时，由于"**最左前缀原则**"，可以增加联合索引的使用率。

##### 11.在多个字段都要创建索引的情况下，联合索引优于单值索引

#### 限制索引的数目

在实际工作中，我们也需要注意平衡，索引的数目不是越多越好。我们需要限制每张表上的索引数量，建议单张表索引数量**不超过6个**。原因：

①每个索引都需要占用**磁盘空间**，索引越多，需要的磁盘空间就越大。

②**索引会影响INSERT、DELETE、 UPDATE等语句的性能**，因为表中的数据更改的同时，索引也会进行调整和更新，会造成负担。

③优化器在选择如何优化查询时，会根据统一信息，对每一个可以用到的**索引来进行评估**，以生成出一个最好的执行计划，如果同时有很多个索引都可以用于查询，会增加MySQL优化器生成执行计划时间，降低查询性能。

#### 哪些情况下不适合索引（7种）

##### 1.在where中使用不到的字段，不要设置索引

闲的没事干，加上也不用

##### 2.数据量小的表最好不要使用索引

杀鸡用牛刀，没必要，真没必要

##### 3.有大量重复数据的列上不要建立索引

和上面第九点呼应

在条件表达式中经常用到的不同值较多的列上建立索引，但字段中如果有大量重复数据，也不用创建索引。比如在学生表的“性别”字段上只有“男”与“女”两个不同值，因此无须建立索引。如果建立索引，不但不会提高查询效率，反而会**严重降低数据更新速度。**

##### 4.避免对经常更新的表创建过多的索引

**索引会影响INSERT、DELETE、 UPDATE等语句的性能**

第一层含义：频繁更新的字段不一定要创建索引。因为更新数据的时候，也需要更新索引，如果索引太多，在更新索引的时候也会造成负担，从而影响效率。

第二层含义：避免对经常更新的表创建过多的索引，并且索引中的列尽可能少。此时，虽然提高了查询速度，同时却会降低更新表的速度。

##### 5.不建议用无序的值作为索引

例如身份证、UUID(在索引比较时需要转为ASCII，并且插入时可能造成页分裂)、MD5、HASH、无序长字符串等。

##### 6.删除不再使用或者很少使用的索引

表中的数据被大量更新，或者数据的使用方式被改变后，原有的一些索引可能不再需要。数据库管理员应当定期找出这些索引，将它们删除，从而减少索引对更新操作的影响。

##### 7.不要定义冗余或重复的索引

1. 冗余索引

   有时候有意或者无意的就对同一个列创建了多个索引，比如：index(a,b,c)相当于index(ai、index(a,b)、index(a,b,c)。

   举例:建表语句如下

   ```
   CREATE TABLE person_info( 
       id INT UNSIGNED NOT NULL AUTO_INCREMENT, 
       name VARCHAR(100) NOT NULL, 
       birthday DATE NOT NULL, 
       phone_number CHAR(11) NOT NULL, 
       country varchar(100) NOT NULL, 
       PRIMARY KEY (id), 
       KEY idx_name_birthday_phone_number (name(10), birthday, phone_number), 
       KEY idx_name (name(10)) 
   );
   ```

   我们知道，通过 `idx_name_birthday_phone_number` 索引就可以对 `name` 列进行快速搜索，再创建一个专门针对 `name` 列的索引就算是一个**冗余索引** ，维护这个索引只会增加维护的成本，并不会对搜索有什么好处。

2. 重复索引

   另一种情况，我们可能会对某个列 重复建立索引 ，比方说这样：

   ```
   CREATE TABLE repeat_index_demo ( 
       col1 INT PRIMARY KEY,
       col2 INT, 
       UNIQUE uk_idx_c1 (col1), 
       INDEX idx_c1 (col1) 
   );
   ```

   我们看到，col1 既是主键、又给它定义为一个唯一索引，还给它定义了一个普通索引，可是主键本身就会生成聚簇索引，所以定义的唯一索引和普通索引是重复的，这种情况要避免。

### 小结

索引是一把**双刃剑**，可提高查询效率,但也会降低插入和更新的速度并占用磁盘空间。

选择索引的最终目的是为了使查询的速度变快，上面给出的原则是最基本的准则，但不能拘泥于上面的准则，大家要在以后的学习和工作中进行不断的实践，根据应用的实际情况进行分析和判断，选择最合适的索引方式。

## 性能分析工具的使用

在数据库调优中，我们的目标就是响应时间更快，吞吐量更大。利用宏观的监控具和微观的日志分析可以帮我们快速找到调优的思路和方式。

### 数据库服务器的优化步骤

整个流程划分成了**观察(Show status )**和**行动(Action)** 两个部分。字母S的部分代表观察(会使用相应的分析工具)，字母A代表的部分是行动(对应分析可以采取的行动)

![image-20230126152226925](/image-20230126152226925.png)

**小结：**

![image-20230126152241331](/image-20230126152241331.png)

### 查看系统性能参数

在MySQL中，可以使用 SHOW STATUS 语句查询一些MySQL数据库服务器的 性能参数 、 执行频率 。 SHOW STATUS语句语法如下：

```
SHOW [GLOBAL|SESSION] STATUS LIKE '参数';
```

**一些常用的性能参数如下：**

- Connections：连接MySQL服务器的次数。
- Uptime：MySQL服务器的上线时间。
- Slow_queries：慢查询的次数。
- Innodb_row_read：Select查询返回的行数
- Innodb_ rows_inserted：执行INSERT操作插入的行数
- Innodb_rows_updated：执行UPDATE操作更新的行数
- Innodb_rows_deleted：执行DELETE操作删除的行数
- Com_ select：查询操作的次数。
- Com_insert：插入操作的次数。对于批量插入的INSERT操作，只累加一-次。
- Com_update：更新操作的次数。
- Com_delete：删除操作的次数。

```
SHOW STATUS LIKE '参数';
```

### 统计SQL的查询成本：last_query_cost

一条SQL查询语句在执行前需要确定查询执行计划，如果存在多种执行计划的话，MySQL 会计算每个执行计划所需要的成本，从中选择**成本最小**的一个作为最终执行的执行计划。

如果我们想要查看某条SQL语句的查询成本，可以在执行完这条SQL语句之后，通过查看当前会话中的`last_query_cost`变量值来得到当前查询的成本。它通常也是我们**评价一个查询的执行效率**的一个常用指标。这个查询成本对应的是**SQL语句所需要读取的页的数量**。

```
CREATE TABLE `student_info` ( 
    `id` INT(11) NOT NULL AUTO_INCREMENT, 
    `student_id` INT NOT NULL ,
    `name` VARCHAR(20) DEFAULT NULL, 
    `course_id` INT NOT NULL , 
    `class_id` INT(11) DEFAULT NULL, 
    `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, 
    PRIMARY KEY (`id`) 
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

如果我们想要查询 id=900001 的记录，然后看下查询成本，我们可以直接在聚簇索引上进行查找：

```
SELECT student_id, class_id, NAME, create_time FROM student_info WHERE id = 900001;
```

运行结果（1 条记录，运行时间为 0.042s ）

然后再看下查询优化器的成本，实际上我们只需要检索一个页即可：

```
mysql> SHOW STATUS LIKE 'last_query_cost';
+-----------------+----------+
| Variable_name   | Value    | 
+-----------------+----------+
| Last_query_cost | 1.000000 | 
+-----------------+----------+
```

如果我们想要查询 id 在 900001 到 9000100 之间的学生记录

```
SELECT student_id, class_id, NAME, create_time FROM student_info WHERE id BETWEEN 900001 AND 900100;
```

运行结果（100 条记录，运行时间为 0.046s ）：

然后再看下查询优化器的成本，这时我们大概需要进行 20 个页的查询

```
mysql> SHOW STATUS LIKE 'last_query_cost'; 
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| Last_query_cost | 21.134453 | 
+-----------------+-----------+
```

你能看到页的数量是刚才的 20 倍，但是查询的效率并没有明显的变化，实际上这两个SQL查询的时间基本上一样，就是因为采用了**顺序读取**的方式将页面一次性加载到缓冲池中，然后再进行查找。虽然页数量（last_query_cost）增加了不少 ，但是通过缓冲池的机制，并没有增加多少查询时间 。

**使用场景：**它对于比较开销是非常有用的，特别是我们有好几种查询方式可选的时候。

> SQL查询是一个动态的过程，从页加载的角度来看，我们可以得到以下两点结论:
>
> 1. **位置决定效率。** 如果页就在数据库**缓冲池**中，那么效率是最高的，否则还需要从内存或者磁盘中进行读取，当然针对单个页的读取来说，如果页存在于内存中，会比在磁盘中读取效率高很多。
> 2. **批量决定效率。**如果我们从磁盘中对单一页进行随机读，那么效率是很低的(差不多10ms) ，而采用顺序读取的方式，批量对页进行读取，平均一页的读取效率就会提升很多，甚至要快于单个页面在内存中的随机读取。
>
> 所以说，遇到I/O并不用担心，方法找对了，效率还是很高的。我们首先要考虑数据存放的位置，如果是经常使用的数据就要尽量放到**缓冲池**中，其次我们可以充分利用磁盘的吞吐能力，一次性批量读取数据，这样单个页的读取效率也就得到了提升。

### **定位执行慢的** SQL：慢查询日志

MySQL的慢查询日志，用来记录在MySQL中**响应时间超过阀值的语句**，具体指运行时间超过**long_query_time** 值的SQL，则会被记录到慢查询日志中。long_query_time的默认值为**10**，意思是运行10秒以上(不含10秒)的语句，认为是超出了我们的最大忍耐时间值。

它的主要作用是，帮助我们发现那些执行时间特别长的SQL查询，并且有针对性地进行优化，从而提高系统的整体效率。当我们的数据库服务器发生阻塞、运行变慢的时候， 检查一下慢查询日志， 找到那些慢查询，对解决问题很有帮助。比如一条sql执行超过5秒钟， 我们就算慢SQL, 希望能收集超过5秒的sql,结合explain进行全面分析。

默认情况下，MySQL数据库**没有开启慢查询日志**，需要我们手动来设置这个参数。**如果不是调优需要的话，一般不建议启动该参数**，因为开启慢查询日志会或多或少带来一定的性能影响。

慢查询日志支持将日志记录写入文件。

#### 开启慢查询日志参数

1. **开启slow_query_log**

   在使用之前，我们需要先看一下慢查询是否已经开启，使用以下命令

```
mysql > show variables like '%slow_query_time';
```

![image-20230126154705153](/image-20230126154705153.png)

开启：

```
mysql > set global slow_query_log='ON'
```

1. **修改long_query_time阈值**

来查看慢查询日志文件位置：

```
mysql > show variables like '%slow_query_time%';
```

![image-20230126154715683](/image-20230126154715683.png)

接下来我们来看下慢查询的时间阈值设置，使用如下命令：

```
mysql > show variables like '%long_query_time%';
```

![image-20230126154815012](/image-20230126154815012.png)

这里如果我们想把时间缩短，比如设置为 1 秒，可以这样设置：

```
#测试发现：设置global的方式对当前session的long_query_time失效。对新连接的客户端有效。所以可以一并 执行下述语句 
mysql > set global long_query_time = 1; 
mysql> show global variables like '%long_query_time%'; 

mysql> set long_query_time=1; 
mysql> show variables like '%long_query_time%';
```

**补充：配置文件中一并设置参数**

如下的方式相较于前面的命令行方式，可以看作是永久设置的方式。

修改`my.cnf`文件，[mysqld]下增加或修改参数`long_query_time、slow_query_log`和 `slow_query_log_file`后，然后重启MySQL服务器。

```
[mysqld]
slow_query_log=ON #开启慢查询日志的开关
slow_query_log_file=/var/lib/mysq1/atguigu-slow.log #慢查询日志的目录和文件名信息
long_query_time=3 #设置慢查询的阈值为3秒，超出此设定值的SQL即被记录到慢查询日志
log_output=FILE
```

如果不指定存储路径，慢查询日志将默认存储到MySQL数据库的数据文件夹下。如果不指定文件名，默认文件名为hostname-slow.log。

#### 查看慢查询数目

查询当前系统中有多少条慢查询记录

```
SHOW GLOBAL STATUS LIKE '%Slow_queries%';
```

#### 案例演示

**步骤1. 建表**

```
CREATE TABLE `student` ( 
    `id` INT(11) NOT NULL AUTO_INCREMENT, 
    `stuno` INT NOT NULL , 
    `name` VARCHAR(20) DEFAULT NULL, 
    `age` INT(3) DEFAULT NULL, 
    `classId` INT(11) DEFAULT NULL, 
    PRIMARY KEY (`id`) 
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

**步骤2：设置参数 log_bin_trust_function_creators**

创建函数，假如报错：

```
This function has none of DETERMINISTIC......
```

命令开启：允许创建函数设置：

```
set global log_bin_trust_function_creators=1; #不加global只是当前窗口有效。
```

**步骤3：创建函数**

随机生成字符串（同上章）

生成随机数（同上章）

**步骤**4**：创建存储过程**

```
DELIMITER // 
CREATE PROCEDURE insert_stu1( START INT , max_num INT ) 
BEGIN 
DECLARE i INT DEFAULT 0;
	SET autocommit = 0; #设置手动提交事务 
	REPEAT #循环 
	SET i = i + 1; #赋值 
	INSERT INTO student (stuno, NAME ,age ,classId ) VALUES ((START+i),rand_string(6),rand_num(10,100),rand_num(10,1000));
	UNTIL i = max_num 
	END REPEAT; 
	COMMIT; #提交事务 
END // DELIMITER ;
```

**步骤5：调用存储过程**

```
#调用刚刚写好的函数, 4000000条记录,从100001号开始 
CALL insert_stu1(100001,4000000);
```

#### 测试及分析

1. **测试**

   ![image-20230126155651787](/image-20230126155651787.png)

   ![image-20230126155718091](/image-20230126155718091.png)

   从上面的结果可以看出来，查询的信息都已经达到了秒的数量级，说明目前查询效率是比较低的，下面 的小节我们分析一下原因。

2. **分析**

   ```
   show status like 'slow_queries';
   ```

   ![image-20230126155909124](/image-20230126155909124.png)

#### 慢查询日志分析工具：mysqldumpslow

在生产环境中，如果要手工分析日志，查找、分析SQL，显然是个体力活，MySQL提供了日志分析工具

`mysqldumpslow` 。

查看mysqldumpslow的帮助信息

```
mysqldumpslow --help
```

![image-20230126160057280](/image-20230126160057280.png)

**mysqldumpslow 命令的具体参数如下：**

- **-a: 不将数字抽象成N，字符串抽象成S** 
- **-s: 是表示按照何种方式排序：**
  - c: 访问次数
  - l: 锁定时间
  - r: 返回记录
  - t: 查询时间
  - al:平均锁定时间
  - ar:平均返回记录数
  - at:平均查询时间 （默认方式）
  - ac:平均查询次数
- **-t: 即为返回前面多少条的数据；**
- **-g: 后边搭配一个正则匹配模式，大小写不敏感的；**

举例：我们想要按照查询时间排序，查看前五条 SQL 语句，这样写即可：

```
mysqldumpslow -s t -t 5 /var/lib/mysql/atguigu01-slow.log
[root@bogon ~]# mysqldumpslow -s t -t 5 /var/lib/mysql/atguigu01-slow.log 

Reading mysql slow query log from /var/lib/mysql/atguigu01-slow.log 
Count: 1 Time=2.39s (2s) Lock=0.00s (0s) Rows=13.0 (13), root[root]@localhost 
	SELECT * FROM student WHERE name = 'S' 

Count: 1 Time=2.09s (2s) Lock=0.00s (0s) Rows=2.0 (2), root[root]@localhost 
	SELECT * FROM student WHERE stuno = N 

Died at /usr/bin/mysqldumpslow line 162, <> chunk 2.
```

**工作常用参考：**

```
#得到返回记录集最多的10个SQL
mysqldumpslow -s r -t 10 /var/lib/mysql/atguigu-slow.log

#得到访问次数最多的10个SQL 
mysqldumpslow -s c -t 10 /var/lib/mysql/atguigu-slow.log 

#得到按照时间排序的前10条里面含有左连接的查询语句 
mysqldumpslow -s t -t 10 -g "left join" /var/lib/mysql/atguigu-slow.log 

#另外建议在使用这些命令时结合 | 和more 使用 ，否则有可能出现爆屏情况 
mysqldumpslow -s r -t 10 /var/lib/mysql/atguigu-slow.log | more
```

#### 关闭慢查询日志

MySQL服务器停止慢查询日志功能有两种方法：

**方式1：永久性方式**

```
[mysqld] 
slow_query_log=OFF 
```

或者，把slow_query_log一项注释掉 或 删除

```
[mysqld] 
#slow_query_log =OFF 
```

重启MySQL服务，执行如下语句查询慢日志功能。

```
SHOW VARIABLES LIKE '%slow%'; #查询慢查询日志所在目录 
SHOW VARIABLES LIKE '%long_query_time%'; #查询超时时长
```

**方式2：临时性方式**

使用SET语句来设置。 

（1）停止MySQL慢查询日志功能，具体SQL语句如下。

```
SET GLOBAL slow_query_log=off;
```

（2）重启MySQL服务，使用SHOW语句查询慢查询日志功能信息，具体SQL语句如下

```
SHOW VARIABLES LIKE '%slow%'; 
#以及
SHOW VARIABLES LIKE '%long_query_time%';
```

#### 删除慢查询日志

使用SHOW语句显示慢查询日志信息，具体SQL语句如下。

```
SHOW VARIABLES LIKE ' slow_query_log%' ;
```

从执行结果可以看出,慢查询日志的目录默认为MySQL的数据目录,在该目录下**手动删除慢查询日志文件即可**。

使用命令`mysqladmin flush-logs` 来重新生成查询日志文件，具体命令如下，执行完毕会在数据目录下重新生成慢查询日志文件。

重新生成前，记得开启慢查询日志。

```
mysqladmin -uroot -p flush-logs slow
```

**只会出来新的文件，没有之前的数据，如果要之前的查询数据，要记得先备份。**

### 查看 SQL **执行成本：**SHOW PROFILE

Show Profile是MySQL提供的可以用来分析当前会话中SQL都做了什么、执行的资源消耗情况的工具，可用于sql调优的测量。**默认情况下处于关闭状态**，并保存最近15次的运行结果。

```
mysql > show variables like 'profiling';
```

![image-20230126160540158](/image-20230126160540158.png)

通过设置 profiling='ON’ 来开启 show profile：

```
mysql > set profiling = 'ON';
```

![image-20230126160551380](/image-20230126160551380.png)

然后执行相关的查询语句。接着看下当前会话都有哪些 profiles，使用下面这条命令：

```
mysql > show profiles;
```

![image-20230126160740676](/image-20230126160740676.png)

你能看到当前会话一共有 2 个查询。如果我们想要查看最近一次查询的开销，可以使用：

```
mysql > show profile;
```

![image-20230126161151179](/image-20230126161151179.png)

```
mysql> show profile cpu,block io for query 3;
```

![image-20230126161206055](/image-20230126161206055.png)

![image-20230126161219456](/image-20230126161219456.png)

**show profile的常用查询参数：**

① ALL：显示所有的开销信息。

② BLOCK IO：显示块IO开销。 

③ CONTEXT SWITCHES：上下文切换开销。

④ CPU：显示CPU开销信息。 

⑤ IPC：显示发送和接收开销信息。 

⑥ MEMORY：显示内存开销信息。

⑦ PAGE FAULTS：显示页面错误开销信息。 

⑧ SOURCE：显示和Source_function，Source_file， Source_line相关的开销信息。 

⑨ SWAPS：显示交换次数开销信息。

**日常开发需注意的结论：**

①`converting HEAP to MyISAM:` 查询结果太大，内存不够，数据往磁盘上搬了。

②`Creating tmp table` :创建临时表。先拷贝数据到临时表,用完后再删除临时表。

③`Copying to tmp table on disk` :把内存中临时表复制到磁盘上，警惕!

④`locked`。

如果在show profile诊断结果中出现了以上4条结果中的任何一条,则sql语句需要优化。

**注意:**

不过SHOW PROFILE命令将被弃用，我们可以从information_ schema 中的profiling数据表进行查看。

### **分析查询语句：**EXPLAIN(主要使用的工具)

#### 概述

**定位了查询慢的SQL之后，我们就可以使用EXPLAIN或DESCRIBE工具做针对性的分析查询语句。**DESCRIBE语句的使用方法与EXPLAIN语句是一样的， 并且分析结果也是一样的。

MySQL中有专门负责优化SELECT语句的优化器模块，主要功能：通过计算分析系统中收集到的统计信息，为客户端请求的Query提供它认为最优的**执行计划**(他认为最优的数据检索方式， 但不见得是DBA认为是最优的，这部分最耗费时间)。

这个执行计划展示了接下来具体执行查询的方式，比如多表连接的顺序是什么，对于每个表采用什么访问方法来具体执行查询等等。MySQL为我们提供了**EXPLAIN**语句来帮助我们查看某个查询语句的具体执行计划，大家看懂**EXPLAIN**语句的各个输出项，可以有针对性的提升我们查询语句的性能。

1. **能做什么？**
   - 表的读取顺序
   - 数据读取操作的操作类型
   - 哪些索引可以使用
   - **哪些索引被实际使用**
   - 表之间的引用
   - **每张表有多少行被优化器查询**
2. **版本情况**

MySQL 5.6.3以前只能 `EXPLAIN SELECT` ；MYSQL 5.6.3以后就可以 `EXPLAIN SELECT，UPDATE， DELETE` 

在5.7以前的版本中，想要显示`partitions` 需要使用 `explain partitions`命令；想要显示`filtered`需要使用 `explain extended` 命令。在5.7版本后，默认explain直接显示partitions和 filtered中的信息。

#### 基本语法

EXPLAIN 或 DESCRIBE语句的语法形式如下：

```
EXPLAIN SELECT select_options 
或者
DESCRIBE SELECT select_options
```

如果我们想看看某个查询的执行计划的话，可以在具体的查询语句前边加一个 EXPLAIN ，就像这样：

```
mysql> EXPLAIN SELECT 1;
```

![image-20230126161259747](/image-20230126161259747.png)

**注意：explain并没有真正的执行该语句，因此可以安全的查看执行计划。**

EXPLAIN 语句输出的各个列的作用如下：

| 列名               | 描述                                                     |
| ------------------ | -------------------------------------------------------- |
| id                 | 在一个打的查询语句中，每个SELECT关键字都对应一个唯一的ID |
| select_type        | SELECT关键字对应的那个查询的类型                         |
| table              | 表名                                                     |
| partitions（可略） | 匹配的分区信息                                           |
| type（*）          | 针对单表的访问方式                                       |
| possible_keys      | 可能用到的索引                                           |
| key                | 实际上使用的索引                                         |
| key_len（*）       | 实际使用到的索引长度                                     |
| ref                | 当时用索引列等值查询时，与索引列进行等值匹配的对象信息   |
| rows（*）          | 预估需要读取的记录条数                                   |
| filtered           | 某个表经过搜索条件过滤后剩余记录条数的百分比             |
| Extra（*）         | 一些额外的信息                                           |

#### 数据准备

1. **建表**

```
CREATE TABLE s1 (
    id INT AUTO_INCREMENT,
    key1 VARCHAR(100), 
    key2 INT, 
    key3 VARCHAR(100), 
    key_part1 VARCHAR(100),
    key_part2 VARCHAR(100), 
    key_part3 VARCHAR(100), 
    common_field VARCHAR(100), 
    PRIMARY KEY (id), 
    INDEX idx_key1 (key1), 
    UNIQUE INDEX idx_key2 (key2), 
    INDEX idx_key3 (key3), 
    INDEX idx_key_part(key_part1, key_part2, key_part3) 
) ENGINE=INNODB CHARSET=utf8;
CREATE TABLE s2 ( 
    id INT AUTO_INCREMENT,
    key1 VARCHAR(100), 
    key2 INT, 
    key3 VARCHAR(100),
    key_part1 VARCHAR(100),
    key_part2 VARCHAR(100),
    key_part3 VARCHAR(100), 
    common_field VARCHAR(100), 
    PRIMARY KEY (id), 
    INDEX idx_key1 (key1),
    UNIQUE INDEX idx_key2 (key2),
    INDEX idx_key3 (key3), 
    INDEX idx_key_part(key_part1, key_part2, key_part3) 
) ENGINE=INNODB CHARSET=utf8;
```

1. **设置参数 log_bin_trust_function_creators**

创建函数，假如报错，需开启如下命令：允许创建函数设置：

```
set global log_bin_trust_function_creators=1; # 不加global只是当前窗口有效。
```

1. **创建函数**

```
DELIMITER // 
CREATE FUNCTION rand_string1(n INT)
	RETURNS VARCHAR(255) #该函数会返回一个字符串 
BEGIN
	DECLARE chars_str VARCHAR(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ'; 
	DECLARE return_str VARCHAR(255) DEFAULT ''; 
	DECLARE i INT DEFAULT 0;
    WHILE i < n DO 
    	SET return_str =CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1)); 
    	SET i = i + 1; 
    END WHILE; 
    RETURN return_str; 
END // 
DELIMITER ;
```

1. **创建存储过程**

```
创建往s1表中插入数据的存储过程：
DELIMITER // 
CREATE PROCEDURE insert_s1 (IN min_num INT (10),IN max_num INT (10)) 
BEGIN
	DECLARE i INT DEFAULT 0; 
	SET autocommit = 0; 
	REPEAT SET i = i + 1; 
	INSERT INTO s1 VALUES( 
        (min_num + i),
        rand_string1(6),
        (min_num + 30 * i + 5),
        rand_string1(6), 
        rand_string1(10), 
        rand_string1(5),
        rand_string1(10),
        rand_string1(10));
    UNTIL i = max_num 
    END REPEAT;
    COMMIT;
END // 
DELIMITER ;
```

1. **调用存储过程**

s1表数据的添加：加入1万条记录：

```
CALL insert_s1(10001,10000);
```

s2表数据的添加：加入1万条记录：

```
CALL insert_s2(10001,10000);
```

#### 各列的作用

为了让大家有比较好的体验，我们调整了下 `EXPLAIN` 输出列的顺序。

##### table

不论我们的查询语句有多复杂，里边儿`包含了多少个表`，到最后也是需要对每个表进行`单表访问`的，所以MySQL规定EXPLAIN**语句输出的每条记录都对应着某个单表的访问方法**，该条记录的table列代表着该表的表名（有时不是真实的表名字，可能是简称）。

```
#1. table：表名
 #查询的每一行记录都对应着一个单表
 EXPLAIN SELECT * FROM s1;

 #s1:驱动表 s2:被驱动表
 EXPLAIN SELECT * FROM s1 INNER JOIN s2;
```

![image-20230126163202447](/image-20230126163202447.png)

第一行的是驱动表，第二行的是被驱动表。

##### id

我们写的查询语句一般都以 SELECT 关键字开头，比较简单的查询语句里只有一个 SELECT 关键字，比如下边这个查询语句：

```
#2. id：在一个大的查询语句中每个SELECT关键字都对应一个唯一的id
 SELECT * FROM s1 WHERE key1 = 'a';

 SELECT * FROM s1 INNER JOIN s2
 ON s1.key1 = s2.key1
 WHERE s1.common_field = 'a'; #1，1

 SELECT * FROM s1 
 WHERE key1 IN (SELECT key3 FROM s2);# 会出现1，2

 SELECT * FROM s1 UNION SELECT * FROM s2;
 
 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';
 
 EXPLAIN SELECT * FROM s1 INNER JOIN s2; 
 EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2) OR key3 = 'a';
```

![image-20230126163443822](/image-20230126163443822.png)

```
 # 查询优化器可能对涉及子查询的查询语句进行重写,转变为多表查询的操作
 EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key2 FROM s2 WHERE common_field = 'a');
```

![image-20230126170515575](/image-20230126170515575.png)

```
 # Union去重
 EXPLAIN SELECT * FROM s1 UNION SELECT * FROM s2;
```

![image-20230126170529248](/image-20230126170529248.png)

因为要去重，因此会建立一个临时表用于去重

```
  EXPLAIN SELECT * FROM s1  UNION ALL SELECT * FROM s2;
```

![image-20230126170538373](/image-20230126170538373.png)

**小结:**

- **id如果相同，可以认为是一组，从上往下顺序执行**
- **在所有组中，id值越大，优先级越高，越先执行**
- **关注点：id号每个号码，表示一趟独立的查询, 一个sql的查询趟数越少越好**

##### select_type

一条大的查询语句里边可以包含若干个SELECT关键字，**每个SELECT关键字代表着一个小的查询语句**， 而每个SELECT关键字的FROM子句中都可以包含若干张表(这些表用来做连接查询)，**每一 张表都对应着执行计划输出中的一条记录**，对于在同一个SELECT关键字中的表来说，它们的id值是相同的。

MySQL为每一个SELECT关键字代表的小查询都定义了一个称之为**select_type** 的属性，意思是我们只要知道了某个小查询的**select_type**属性，就知道了这个**小查询在整个大查询中扮演了一个什么角色**，我们看一下select_type都能取哪些值，请看官方文档：

![image-20230126171027899](/image-20230126171027899.png)

```
 #3. select_type：SELECT关键字对应的那个查询的类型,确定小查询在整个大查询中扮演了一个什么角色
 
 #`SIMPLE`
 #查询语句中不包含`UNION`或者子查询的查询都算作是`SIMPLE`类型
 EXPLAIN SELECT * FROM s1;
 
 #连接查询也算是`SIMPLE`类型
 EXPLAIN SELECT * FROM s1 INNER JOIN s2;
 
 #`PRIMARY`、`UNION`、`UNION RESULT`
 #对于包含`UNION`或者`UNION ALL`或者子查询的大查询来说，它是由几个小查询组成的，其中最左边的那个查询的`select_type`值就是`PRIMARY`
 #对于包含`UNION`或者`UNION ALL`的大查询来说，它是由几个小查询组成的，其中除了最左边的那个小查询以外，其余的小查询的`select_type`值就是`UNION`
 #`MySQL`选择使用临时表来完成`UNION`查询的去重工作，针对该临时表的查询的`select_type`就是`UNION RESULT`
 EXPLAIN SELECT * FROM s1 UNION SELECT * FROM s2;
 
 EXPLAIN SELECT * FROM s1 UNION ALL SELECT * FROM s2;
 
 #`SUBQUERY`
 #子查询：
 #如果包含子查询的查询语句不能够转为对应的`semi-join`的形式，并且该子查询是不相关子查询。
 #该子查询的第一个`SELECT`关键字代表的那个查询的`select_type`就是`SUBQUERY`
 EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2) OR key3 = 'a';
 
 #`DEPENDENT SUBQUERY`
 #如果包含子查询的查询语句不能够转为对应的`semi-join`的形式，并且该子查询是相关子查询，则该子查询的第一个`SELECT`关键字代表的那个查询的`select_type`就是`DEPENDENT SUBQUERY`
 EXPLAIN SELECT * FROM s1 
 WHERE key1 IN (SELECT key1 FROM s2 WHERE s1.key2 = s2.key2) OR key3 = 'a';
 #注意的是，select_type为`DEPENDENT SUBQUERY`的查询可能会被执行多次。
 
 #`DEPENDENT UNION`
 #在包含`UNION`或者`UNION ALL`的大查询中，如果各个小查询都依赖于外层查询的话，那除了最左边的那个小查询之外，其余的小查询的`select_type`的值就是`DEPENDENT UNION`。
 EXPLAIN SELECT * FROM s1 
 WHERE key1 IN (SELECT key1 FROM s2 WHERE key1 = 'a' UNION SELECT key1 FROM s1 WHERE key1 = 'b');
 
 #`DERIVED`
 #对于包含`派生表`的查询，该派生表对应的子查询的`select_type`就是`DERIVED`
 EXPLAIN SELECT * 
 FROM (SELECT key1, COUNT(*) AS c FROM s1 GROUP BY key1) AS derived_s1 WHERE c > 1;
 
 #`MATERIALIZED`
 #当查询优化器在执行包含子查询的语句时，选择将子查询物化之后与外层查询进行连接查询时，该子查询对应的`select_type`属性就是`MATERIALIZED`
 EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2); #子查询被转为了物化表
```

##### partitions(可略)

代表分区表中的命中情况，非分区表,该项为NULL。一般情况下我们的查询语句的执行计划的partitions 列的值都是NULL。

##### type*

执行计划的一条记录就代表着MySQL对某个表的**执行查询时的访问方法**，又称“访问类型”，其中的**type**列就表明了这个访问方法是啥，是较为重要的一个指标。比如，看到**type**列的值是**ref**，表明**MySQL**即将使用**ref**访问方法来执行对**s1**表的查询。

完整的访问方法如下： **system ， const ， eq_ref ， ref ， fulltext ， ref_or_null ， index_merge ， unique_subquery ， index_subquery ， range ， index ， ALL** 。

```
 # 5. type：针对单表的访问方法
 
 #`system`
 #当表中`只有一条记录`并且该表使用的存储引擎的统计数据是精确的，比如MyISAM、Memory，那么对该表的访问方法就是`system`。
 CREATE TABLE t(i INT) ENGINE=MYISAM;
 INSERT INTO t VALUES(1);
 EXPLAIN SELECT * FROM t;
 #换成InnoDB
 CREATE TABLE tt(i INT) ENGINE=INNODB;
 INSERT INTO tt VALUES(1);
 EXPLAIN SELECT * FROM tt;
 
 #`const`
 #当我们根据主键或者唯一二级索引列与常数进行等值匹配时，对单表的访问方法就是`const`
 EXPLAIN SELECT * FROM s1 WHERE id = 10005;
 
 EXPLAIN SELECT * FROM s1 WHERE key2 = 10066;
 
 #`eq_ref`
 #在连接查询时，如果被驱动表是通过主键或者唯一二级索引列等值匹配的方式进行访问的（如果该主键或者唯一二级索引是联合索引的话，所有的索引列都必须进行等值比较），则对该被驱动表的访问方法就是`eq_ref`
 EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.id = s2.id;
  
 #`ref`
 #当通过普通的二级索引列与常量进行等值匹配时来查询某个表，那么对该表的访问方法就可能是`ref`
 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';
 
 #`ref_or_null`
 #当对普通二级索引进行等值匹配查询，该索引列的值也可以是`NULL`值时，那么对该表的访问方法就可能是`ref_or_null`
 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key1 IS NULL;
 
 #'index_merge'
 #单表访问方法时在某些场景下可以使用`Intersection`、`Union`、`Sort-Union`这三种索引合并的方式来执行查询
 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key3 = 'a';
 
 #`unique_subquery`
 #`unique_subquery`是针对在一些包含`IN`子查询的查询语句中，如果查询优化器决定将`IN`子查询转换为`EXISTS`子查询，而且子查询可以使用到主键进行等值匹配的话，那么该子查询执行计划的`type`列的值就是`unique_subquery`
 EXPLAIN SELECT * FROM s1 
 WHERE key2 IN (SELECT id FROM s2 WHERE s1.key1 = s2.key1) OR key3 = 'a';
 
 #`range`
 #如果使用索引获取某些`范围区间`的记录，那么就可能使用到`range`访问方法
 EXPLAIN SELECT * FROM s1 WHERE key1 IN ('a', 'b', 'c');
 #同上
 EXPLAIN SELECT * FROM s1 WHERE key1 > 'a' AND key1 < 'b';
 
 #`index`
 #当我们可以使用索引覆盖，但需要扫描全部的索引记录时，该表的访问方法就是`index`
 EXPLAIN SELECT key_part2 FROM s1 WHERE key_part3 = 'a';
 
 #'ALL'
 #最熟悉的全表扫描
 EXPLAIN SELECT * FROM s1;
```

一般来说，这些访问方法中除了All这个访问方法外，其余的访问方法都能用到索引，除了index_merge访问方法外，其余的访问方法都最多只能用到一个索引。

**小结：**

**结果值从最好到最坏依次是：** 

**system > const > eq_ref > ref> fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index> ALL** 

**其中比较重要的几个提取出来。SQL 性能优化的目标：至少要达到 range 级别，要求是 ref 级别，最好是 consts级别。（阿里巴巴开发手册要求）**

##### possible_keys和key

在EXPLAIN语句输出的执行计划中，possible_keys列表示在某个查询语句中，对某个表执行单表查询时可能用到的索引有哪些。一般查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询使用。key列表示实际用到的索引有哪些，如果为NULL，则没有使用索引。比方说下边这个查询：

```
 #6. possible_keys和key：可能用到的索引 和  实际上使用的索引
 EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' AND key3 = 'a';
```

![image-20230126181824749](/image-20230126181824749.png)

##### key_len*

```
#7.key_len：实际使用到的索引长度(即：字节数)
#帮你检查`是否充分的利用上了索引`，`值越大越好`,主要针对于联合索引，有一定的参考意义。
 EXPLAIN SELECT * FROM s1 WHERE id = 10005; #4

 EXPLAIN SELECT * FROM s1 WHERE key2 = 10126; #5


 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a'; #303=3*100(utf-8三个字节表示一个字符，一个字节（创建时候定的）100，)+1（可能为空）+2（边长的字符，需要记录一下边长的长度）

 EXPLAIN SELECT * FROM s1 WHERE key_part1 = 'a'; #303

 EXPLAIN SELECT * FROM s1 WHERE key_part1 = 'a' AND key_part2 = 'b'; #606=3030*2

 EXPLAIN SELECT * FROM s1 WHERE key_part1 = 'a' AND key_part2 = 'b' AND key_part3 = 'c'; #909=303*3
 
 EXPLAIN SELECT * FROM s1 WHERE key_part3 = 'a'; #NULL没有用到索引
 
#练习：
#varchar(10)变长字段且允许NULL 	= 10 * ( character set：utf8=3,gbk=2,latin1=1)+1(NULL)+2(变长字段)

#varchar(10)变长字段且不允许NULL 	= 10 * ( character set：utf8=3,gbk=2,latin1=1)+2(变长字段)

#char(10)固定字段且允许NULL    	= 10 * ( character set：utf8=3,gbk=2,latin1=1)+1(NULL)

#char(10)固定字段且不允许NULL  		= 10 * ( character set：utf8=3,gbk=2,latin1=1)
```

##### ref

```
 # 8. ref：当使用索引列等值查询时，与索引列进行等值匹配的对象信息。
 #比如只是一个常数或者是某个列。
EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';
```

![image-20230128122400284](/image-20230128122400284.png)

```
 EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.id = s2.id;
```

![image-20230128122415487](/image-20230128122415487.png)

```
 EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s2.key1 = UPPER(s1.key1);
```

![image-20230128122428880](/image-20230128122428880.png)

##### rows*

```
 # 9. rows：预估的需要读取的记录条数
 # `值越小越好`
 # 越小，页越少，IO次数就越少
 EXPLAIN SELECT * FROM s1 WHERE key1 > 'z';
```

##### filtered

```
 # 10. filtered: 某个表经过搜索条件过滤后剩余记录条数的百分比
 
 #如果使用的是索引执行的单表扫描，那么计算时需要估计出满足除使用到对应索引的搜索条件外的其他搜索条件的记录有多少条。
 EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' AND common_field = 'a';
```

![image-20230128122714076](/image-20230128122714076.png)

```
 #对于单表查询来说，这个filtered列的值没什么意义，我们`更关注在连接查询中驱动表对应的执行计划记录的filtered值`，它决定了被驱动表要执行的次数(即：rows * filtered)
 EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key1 WHERE s1.common_field = 'a';
```

![image-20230128122723105](/image-20230128122723105.png)

##### Extra*

顾名思义，**Extra**列是用来说明一些额外信息的，包含不适合在其他列中显示但十分重要的额外信息。我们可以通过这些额外信息来**更准确的理解MySQL到底将如何执行给定的查询语句**。MySQL提供的额外信息有好几十个，我们就不一个一个介绍了，所以我们只挑比较重要的额外信息介绍给大家。

```
 #11. Extra:一些额外的信息
 #更准确的理解MySQL到底将如何执行给定的查询语句
 
 #'No tables used'
 #当查询语句的没有`FROM`子句时将会提示该额外信息
 EXPLAIN SELECT 1;
 
 #'Impossible WHERE'
 #查询语句的`WHERE`子句永远为`FALSE`时将会提示该额外信息
 EXPLAIN SELECT * FROM s1 WHERE 1 != 1;
 
 #'Using WHERE'
 #当我们使用全表扫描来执行对某个表的查询，并且该语句的`WHERE`子句中有针对该表的搜索条件时，在`Extra`列中会提示上述额外信息。
 EXPLAIN SELECT * FROM s1 WHERE common_field = 'a';
 
 #'Using WHERE'
 #当使用索引访问来执行对某个表的查询，并且该语句的`WHERE`子句中有除了该索引包含的列之外的其他搜索条件时，在`Extra`列中也会提示上述额外信息。
 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' #'(NULL)'
 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' AND common_field = 'a';
 
 #'No matching min/max row'
 #'Select tavles optimized away'
 #当查询列表处有`MIN`或者`MAX`聚合函数，但是并没有符合`WHERE`子句中的搜索条件的记录时，将会提示该额外信息
 EXPLAIN SELECT MIN(key1) FROM s1 WHERE key1 = 'abcdefg';#表中华不存在的字段  'No matching min/max row'
 
 EXPLAIN SELECT MIN(key1) FROM s1 WHERE key1 = 'NlPros'; #NlPros 是 s1表中key1字段真实存在的数据 'Select tavles optimized away'
 
 #select * from s1 limit 10;
 
 #'Using index' very good!!!
 #当我们的查询列表以及搜索条件中只包含属于某个索引的列，也就是在可以使用覆盖索引的情况下，在`Extra`列将会提示该额外信息。比方说下边这个查询中只需要用到`idx_key1`而不需要回表操作：
 EXPLAIN SELECT key1,id FROM s1 WHERE key1 = 'a';
 
 #'Using index condition'
 #有些搜索条件中虽然出现了索引列，但却不能使用到索引
 #‘索引条件下推’
 #我们说回表操作其实是一个随机I0，比较耗时，所以上述修改虽然只改进了一点点，但是可以省去好多回表操作的成本。MySQL把他们的这个改进称之为‘索引条件下推’(英文名: Index Condition Pushdown)。
 EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' AND key1 LIKE '%a';
 
 #'USING join buffer(hash join)'
 #在连接查询执行过程中，当被驱动表不能有效的利用索引加快访问速度，MySQL一般会为其分配一块名叫`join buffer`的内存块来加快查询速度，也就是我们所讲的`基于块的嵌套循环算法`
 #见课件说明
 EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.common_field = s2.common_field;
 
 #`Not exists`
 #当我们使用左（外）连接时，如果`WHERE`子句中包含要求被驱动表的某个列等于`NULL`值的搜索条件，而且那个列又是不允许存储`NULL`值的，那么在该表的执行计划的Extra列就会提示`Not exists`额外信息
 EXPLAIN SELECT * FROM s1 LEFT JOIN s2 ON s1.key1 = s2.key1 WHERE s2.id IS NULL;
 
 #'Using intersect(...)/Using union(...)/Using sort_union(...)'
 #如果执行计划的`Extra`列出现了`Using intersect(...)`提示，说明准备使用`Intersect`索引合并的方式执行查询，括号中的`...`表示需要进行索引合并的索引名称；
 #如果出现了`Using union(...)`提示，说明准备使用`Union`索引合并的方式执行查询；
 #出现了`Using sort_union(...)`提示，说明准备使用`Sort-Union`索引合并的方式执行查询。
 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key3 = 'a';
 
 #'Zero limit'
 #当我们的`LIMIT`子句的参数为`0`时，表示压根儿不打算从表中读出任何记录，将会提示该额外信息
 EXPLAIN SELECT * FROM s1 LIMIT 0;
 
 #'(NULL)'
 #有一些情况下对结果集中的记录进行排序是可以使用到索引的。
 #比如：
 EXPLAIN SELECT * FROM s1 ORDER BY key1 LIMIT 10;
 
 #'Using filesort' 无法使用到索引
 #很多情况下排序操作无法使用到索引，只能在内存中（记录较少的时候）或者磁盘中（记录较多的时候）进行排序，MySQL把这种在内存中或者磁盘上进行排序的方式统称为文件排序（英文名：`filesort`）。
 
 #如果某个查询需要使用文件排序的方式执行查询，就会在执行计划的`Extra`列中显示`Using filesort`提示
 EXPLAIN SELECT * FROM s1 ORDER BY common_field LIMIT 10;
 
 #'Using temporary' 不能有效利用索引来完成查询
 #在许多查询的执行过程中，MySQL可能会借助临时表来完成一些功能，比如去重、排序之类的，比如我们在执行许多包含`DISTINCT`、`GROUP BY`、`UNION`等子句的查询过程中，如果不能有效利用索引来完成查询，MySQL很有可能寻求通过建立内部的临时表来执行查询。如果查询中使用到了内部的临时表，在执行计划的`Extra`列将会显示`Using temporary`提示
 EXPLAIN SELECT DISTINCT common_field FROM s1;
 
 #EXPLAIN SELECT DISTINCT key1 FROM s1;
 
 #同上。
 EXPLAIN SELECT common_field, COUNT(*) AS amount FROM s1 GROUP BY common_field;
 
 #执行计划中出现`Using temporary`并不是一个好的征兆，因为建立与维护临时表要付出很大成本的，所以我们`最好能使用索引来替代掉使用临时表`。比如：扫描指定的索引idx_key1即可
 EXPLAIN SELECT key1, COUNT(*) AS amount FROM s1 GROUP BY key1;
 
#json格式的explain
EXPLAIN FORMAT=JSON SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key2 
WHERE s1.common_field = 'a';
```

##### 小结

- **EXPLAIN不考虑各种Cache** 
- **EXPLAIN不能显示MySQL在执行查询时所作的优化工作**
- **EXPLAIN不会告诉你关于触发器、存储过程的信息或用户自定义函数对查询的影响情况**
- **部分统计信息是估算的，并非精确值**

### EXPLAIN的进一步使用

#### EXPLAIN四种输出格式

这里谈谈EXPLAIN的输出格式。EXPLAIN可以输出四种格式： `传统格式` ，`JSON格式` ， `TREE格式` 以及`可视化输出` 。用户可以根据需要选择适用于自己的格式。

##### 传统模式

传统格式简单明了，输出是一个表格形式，概要说明查询计划。

##### JSON格式

第1种格式中介绍的EXPLAIN语句输出中缺少了一个衡量执行计划好坏的重要属性——**成本**。而JSON格式是四种格式里面输出信息最详尽的格式，里面包含了执行的成本信息。

- JSON格式：在EXPLAIN单词和真正的查询语句中间加上 FORMAT=JSON 。

```
EXPLAIN FORMAT=JSON SELECT ....
EXPLAIN FORMATE=JSON SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key2
```

我们使用 **#** 后边跟随注释的形式为大家解释了 **EXPLAIN FORMAT=JSON** 语句的输出内容，但是大家可能有疑问 **"cost_info"** 里边的成本看着怪怪的，它们是怎么计算出来的？

先看 **s1** 表的 **"cost_info"** 部分：

```
"cost_info": {
	"read_cost": "1840.84",
    "eval_cost": "193.76", 
    "prefix_cost": "2034.60", 
    "data_read_per_join": "1M" 
}
```

- **read_cost** 是由下边这两部分组成的：

  - **IO** 成本
  - 检测 **rows × (1 - filter)** 条记录的 **CPU** 成本

  > 小贴士： rows和filter都是我们前边介绍执行计划的输出列，在JSON格式的执行计划中，rows相当于rows_examined_per_scan，filtered名称不变。

- **eval_cost** 是这样计算的：

  - 检测 **rows × filter** 条记录的成本。

- **prefix_cost** 就是单独查询 s1 表的成本，也就是：

  - **read_cost + eval_cost**

- **data_read_per_join**表示在此次查询中需要读取的数据量。

对于 **s2** 表的 **"cost_info“** 部分是这样的：

```
"cost_info": {
    "read_cost": "968.80",
    "eval_cost": "193.76",
    "prefix_cost": "3197.16", 
    "data_read_per_join": "1M" 
}
```

由于 `s2` 表是被驱动表，所以可能被读取多次，这里的 `read_cost` 和 `eval_cost` 是访问多次 `s2` 表后累加起来的值，大家主要关注里边儿的`prefix_cost` 的值代表的是整个连接查询预计的成本，也就是单次查询 `s1` 表和多次查询 `s2` 表后的成本的和，也就是：

```
968.80 + 193.76 + 2034.60 = 3197.16
```

##### TREE格式

TREE格式是8.0.16版本之后引入的新格式，主要根据查询的 **各个部分之间的关系** 和 **各部分的执行顺序** 来描述如何查询。

```
EXPLAIN FORMAT=tree SELECT ....
```

##### 可视化输出

可视化输出，可以通过MySQL Workbench可视化查看MySQL的执行计划。通过点击Workbench的放大镜图标，即可生成可视化的查询计划。

![image-20220201215745651](/image-20230128123736689.png)

上图按从左到右的连接顺序显示表。红色框表示`全表扫描` ，而绿色框表示使用`索引查找`。对于每个表，显示使用的索引。还要注意的是，每个表格的框上方是每个表访问所发现的行数的估计值以及访问该表的成本。

#### SHOW WARNINGS的使用

在我们使用EXPLAIN语句查看了某个查询的执行计划后，紧接着还可以使用SHOW WARNINGS 语句查看与这个查询的执行计划有关的一-些扩展信息，比如这样：

```
mysql> EXPLAIN SELECT s1.key1, s2.key1 FROM s1 LEFT JOIN s2 ON s1.key1 = s2.key1 WHERE s2.common_field IS NOT NULL;
```

![image-20230128123924191](/image-20230128123924191.png)

```
mysql> SHOW WARNINGS\G
*************************** 1. row *************************** 
	Level: Note 
	Code: 1003 
	Message: /* select#1 */ select `atguigu`.`s1`.`key1` AS `key1`,`atguigu`.`s2`.`key1` AS `key1` from `atguigu`.`s1` join `atguigu`.`s2` where ((`atguigu`.`s1`.`key1` = `atguigu`.`s2`.`key1`) and (`atguigu`.`s2`.`common_field` is not null)) 1 row in set (0.00 sec)
```

大家可以看到`SHOW WARNINGS` 展示出来的信息有三个字段，分别是`Level、Code、 MesSaye`。 我们最常见的就是Code为1003的信息，当Code值为1003时， `Message` 字段展示的信息类似于查询优化器将我们的查询语句重写后的语句。比如我们上边的查询本来是一个左(外) 连接查询，但是有一个s2.common_ field IS NOT NULL的条件，这就会导致查询优化器把左(外)连接查询优化为内连接查询，从`SHOW WARNINGS` 的`Message`字段也可以看出来，原本的LEFT JOIN已经变成了JOIN。

### **分析优化器执行计划：**trace

`OPTIMIZER_ TRACE`是MySQL 5.6引入的一项跟踪功能，它可以跟踪优化器做出的各种决策(比如访问表的方法、各种开销计算、各种转换等)，并将跟踪结果记录到 `INFORMATION_ SCHEMA .  OPTIMIZER_TRACE`表中。

此功能默认关闭。开启trace，并设置格式为JSON，同时设置trace最大能够使用的内存大小，避免解析过程中因为默认内存过小而不能够完整展示。

```
SET optimizer_trace="enabled=on",end_markers_in_json=on; 

set optimizer_trace_max_mem_size=1000000;#内存
```

开启后，可分析如下语句：

- SELECT 
- INSERT 
- REPLACE
- UPDATE 
- DELETE 
- EXPLAIN 
- SET 
- DECLARE 
- CASE 
- IF
- RETURN 
- CALL

测试：执行如下SQL语句

```
select * from student where id < 10;
```

最后， 查询 information_schema.optimizer_trace 就可以知道MySQL是如何执行SQL的 ： 

```
select * from information_schema.optimizer_trace\G
 //第1部分：查询语句 
 QUERY:select*fromstudentwhereid<10 
 
//第2部分：QUERY字段对应语句的跟踪信息 
 TRACE:
{...}
 
 //第3部分：跟踪信息过长时，被截断的跟踪信息的字节数。
 MISSING_BYTES_BEYOND_MAX_MEM_SIZE:0 //丢失的超出最大容量的字节
 
 //第4部分：执行跟踪语句的用户是否有查看对象的权限。
 //当不具有权限时，该列信息为1且TRACE字段为空，一般在调用带有SQLSECURITYDEFINER的视图或者是存储过程的情况下，会出现此问题。
 INSUFFICIENT_PRIVILEGES:0 //缺失权限
 1 row in set(0.00sec)
```

### MySQL监控分析视图-sys schema

关于MySQL的性能监控和问题诊断，我们一般都从performance_ schema中去获取想要的数据，在MySQL5.7.7版本中新增sys schema，它将performance_ schema和information_schema中的数据以更容易理解的方式总结归纳为”视图”，其目的就是为了`降低查询performance_schema的复杂度`，让DBA能够快速的定位问题。下面看看这些库中都有哪些监控表和视图，掌握了这些，在我们开发和运维的过程中就起到了事半功倍的效果。

#### Sys schema视图摘要

1. **主机相关：**以host_summary开头，主要汇总了IO延迟的信息。
2. **Innodb相关**：以innodb开头，汇总了innodb buffer信息和事务等待innodb锁的信息。
3. **I/O相关**：以io开头，汇总了等待I/O、I/O使用量情况。
4. **内存使用情况：**以memory开头，从主机、线程、事件等角度展示内存的使用情况
5. **连接与会话信息：**processlist和session相关视图，总结了会话相关信息。
6. **表相关：**以schema_table开头的视图，展示了表的统计信息。
7. **索引信息：**统计了索引的使用情况，包含冗余索引和未使用的索引情况。
8. **语句相关：**以statement开头，包含执行全表扫描、使用临时表、排序等的语句信息。
9. **用户相关：**以user开头的视图，统计了用户使用的文件I/O、执行语句统计信息。
10. **等待事件相关信息：**以wait开头，展示等待事件的延迟情况。

#### Sys schema视图使用场景

**索引情况**

```
#1. 查询冗余索引 
select * from sys.schema_redundant_indexes;

#2. 查询未使用过的索引 
select * from sys.schema_unused_indexes; 

#3. 查询索引的使用情况 
select index_name,rows_selected,rows_inserted,rows_updated,rows_deleted
from sys.schema_index_statistics where table_schema='dbname' ;
```

**表相关**

```
# 1. 查询表的访问量 
select table_schema,table_name,sum(io_read_requests+io_write_requests) as io from sys.schema_table_statistics group by table_schema,table_name order by io desc; 

# 2. 查询占用bufferpool较多的表 
select object_schema,object_name,allocated,data
from sys.innodb_buffer_stats_by_table order by allocated limit 10; 

# 3. 查看表的全表扫描情况 
select * from sys.statements_with_full_table_scans where db='dbname';
```

**语句相关**

```
#1. 监控SQL执行的频率 
select db,exec_count,query from sys.statement_analysis order by exec_count desc; 

#2. 监控使用了排序的SQL
select db,exec_count,first_seen,last_seen,query
from sys.statements_with_sorting limit 1; 

#3. 监控使用了临时表或者磁盘临时表的SQL 
select db,exec_count,tmp_tables,tmp_disk_tables,query
from sys.statement_analysis where tmp_tables>0 or tmp_disk_tables >0 order by (tmp_tables+tmp_disk_tables) desc;
```

**IO相关**

```
#1. 查看消耗磁盘IO的文件 
select file,avg_read,avg_write,avg_read+avg_write as avg_io
from sys.io_global_by_file_by_bytes order by avg_read limit 10;
```

**Innodb 相关**

```
#1. 行锁阻塞情况 
select * from sys.innodb_lock_waits;
```

> 风险提示：
>
> 通过sys库去查询时，MySQL会`消耗大量资源`去收集相关信息，严重的可能会导致业务请求被阻塞，从而引起故障。建议生产上`不要频繁`的去查询sys或者performance_ schema、 information_ schema来完成监控、巡检等工作。

## 索引优化与查询优化

都有哪些维度可以进行数据库调优？简言之：

- 索引失效、没有充分利用到索引——索引建立
- 关联查询太多JOIN (设计缺陷或不得已的需求) ——SQL优化
- 服务器调优及各个参数设置(缓冲、 线程数等)——调整my.cnf
- 数据过多——分库分表

关于数据库调优的知识点非常分散。不同的DBMS,不同的公司，不同的职位，不同的项目遇到的问题都不尽相同。这里我们分为三个章节进行细致讲解。

虽然SQL查询优化的技术有很多，但是大方向上完全可以分成**物理查询优化**和**逻辑查询优化**两大块。

- 物理查询优化是通过**索引**和**表连接方式**等技术来进行优化，这里重点需要掌握索引的使用。
- 逻辑查询优化就是通过SQL**等价变换**提升查询效率，直白一点就是说，换一种查询写法执行效率可能更高。

### 数据准备

学员表插50万条， 班级表插1万条

**步骤1：建表**

```
CREATE TABLE `class` (
    `id` INT(11) NOT NULL AUTO_INCREMENT, 
    `className` VARCHAR(30) DEFAULT NULL, 
    `address` VARCHAR(40) DEFAULT NULL, 
    `monitor` INT NULL , 
    PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8; 

CREATE TABLE `student` ( 
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `stuno` INT NOT NULL , 
    `name` VARCHAR(20) DEFAULT NULL, 
    `age` INT(3) DEFAULT NULL, 
    `classId` INT(11) DEFAULT NULL,
    PRIMARY KEY (`id`) 
    #CONSTRAINT `fk_class_id` FOREIGN KEY (`classId`) REFERENCES `t_class` (`id`) 
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

**步骤2：设置参数**

命令开启：允许创建函数设置：

```
set global log_bin_trust_function_creators=1; # 不加global只是当前窗口有效。
```

**步骤3：创建函数**

保证每条数据都不同。

```
#随机产生字符串 
DELIMITER // 
CREATE FUNCTION rand_string(n INT) RETURNS VARCHAR(255)
BEGIN 
DECLARE chars_str VARCHAR(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ'; 
DECLARE return_str VARCHAR(255) DEFAULT ''; 
DECLARE i INT DEFAULT 0;
WHILE i < n DO 
SET return_str =CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1)); 
SET i = i + 1;
END 
WHILE;
RETURN return_str; 
END // 
DELIMITER ; 

#假如要删除 
#drop function rand_string;
```

随机产生班级编号

```
#用于随机产生多少到多少的编号 
DELIMITER // 
CREATE FUNCTION rand_num (from_num INT ,to_num INT) RETURNS INT(11) 
BEGIN 
DECLARE i INT DEFAULT 0; 
SET i = FLOOR(from_num +RAND()*(to_num - from_num+1)) ; 
RETURN i; 
END // 
DELIMITER ;

#假如要删除
#drop function rand_num;
```

**步骤4：创建存储过程**

创建往stu表中插入数据的存储过程 

```
#创建往stu表中插入数据的存储过程 
DELIMITER //
CREATE PROCEDURE insert_stu( START INT , max_num INT )
BEGIN 
DECLARE i INT DEFAULT 0; 
	SET autocommit = 0; #设置手动提交事务 
	REPEAT #循环 
	SET i = i + 1; #赋值 
	INSERT INTO student (stuno, name ,age ,classId ) VALUES 
	((START+i),rand_string(6),rand_num(1,50),rand_num(1,1000)); 
	UNTIL i = max_num 
END 
REPEAT; 
COMMIT; 
#提交事务 
END // 
DELIMITER ; 

#假如要删除 
#drop PROCEDURE insert_stu;
```

创建往class表中插入数据的存储过程

```
#执行存储过程，往class表添加随机数据 
DELIMITER // 
CREATE PROCEDURE `insert_class`( max_num INT ) 
BEGIN 
DECLARE i INT DEFAULT 0;
SET autocommit = 0; 
REPEAT
	SET i = i + 1; 
	INSERT INTO class ( classname,address,monitor ) VALUES 
	(rand_string(8),rand_string(10),rand_num(1,100000)); 
	UNTIL i = max_num 
	END REPEAT;
	COMMIT;
END // 
DELIMITER ; 
#假如要删除
#drop PROCEDURE insert_class;
```

**步骤5：调用存储过程**

class

```
#执行存储过程，往class表添加1万条数据 
CALL insert_class(10000);
```

stu

```
#执行存储过程，往stu表添加50万条数据 
CALL insert_stu(100000,500000);
```

**步骤6：删除某表上的索引**

创建存储过程

```
DELIMITER //
CREATE PROCEDURE `proc_drop_index`(dbname VARCHAR(200),tablename VARCHAR(200)) 
BEGIN 
	DECLARE done INT DEFAULT 0; 
	DECLARE ct INT DEFAULT 0; 
	DECLARE _index VARCHAR(200) DEFAULT ''; 
	DECLARE _cur CURSOR FOR SELECT index_name FROM information_schema.STATISTICS WHERE table_schema=dbname AND table_name=tablename AND seq_in_index=1 AND index_name <>'PRIMARY' ; 
	#每个游标必须使用不同的declare continue handler for not found set done=1来控制游标的结束 
	DECLARE CONTINUE HANDLER FOR NOT FOUND set done=2 ; 
	#若没有数据返回,程序继续,并将变量done设为2 
	OPEN _cur;
    FETCH _cur INTO _index; 
    WHILE _index<>'' DO 
    	SET @str = CONCAT("drop index " , _index , " on " , tablename ); 
    	PREPARE sql_str FROM @str ; 
    	EXECUTE sql_str; 
    	DEALLOCATE PREPARE sql_str; 
    	SET _index='';
        FETCH _cur INTO _index; 
    END WHILE;
    CLOSE _cur; 
END //
DELIMITER ;
```

执行存储过程

```
CALL proc_drop_index("dbname","tablename");
```

### 索引失效案例

MySQL中提高性能最有效的方式就是对数据表设计合理的索引。索引提供了高校访问数据的方法，并且加快了查询的速度，因此索引对查询的速度有着至关重要的影响。

- 使用索引可以**快速地定位**表中的某条记录，从而提高数据库查询的速度，提高数据库的性能。
- 如果查询时没有使用索引，查询语句就会**扫描表中的所有记录**。在数据量打的情况下，这样查询的速度会很慢。

大多数情况下都会默认采用B+树。

其实，用不用索引，最终都是优化器说了算。优化器是基于什么的优化器？

基于cost**开销**(CostBase0ptimizer)，它不是基于**规则**(Rule-BasedOptimizer)，也不是基于**语义**。怎么样开销小就怎么来。另外，**SQL语句是否使用索引，跟数据库版本、数据量、数据选择度都有关系**。

#### 全值匹配我最爱（所有的索引字段都用上）

SQL语句：

```
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30;
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4;
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4 AND NAME = 'abcd';
```

建立索引：

```
CREATE INDEX idx_age ON student(age);#在id上加入索引
CREATE INDEX idx_age_classid ON student(age,classId);#在id、classid上加入索引
CREATE INDEX idx_age_classid_name ON student(age,classId,NAME);#在id、classid、name上加入索引
```

建立索引后执行：

```
SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4 AND NAME = 'abcd';
```

创建索引前后的查询时间，快了百倍的数量级，且用到的字符越多，查询速度越快。

#### 最佳左前缀法则（最左边的索引必须匹配到）

在MySQL建立联合索引时会遵守最佳左前缀匹配原则，即最左优先，在检索数据时**从联合索引的最左边开始匹配**。

```
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.name = 'abcd' ;#用到了age索引

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.classid=1 AND student.name = 'abcd';#没有age，所以用不上联合索引
 
EXPLAIN SELECT SQL_NO_CACHE * FROM student 
WHERE classid=4 AND student.age=30 AND student.name = 'abcd'; #可以用上联合索引

DROP INDEX idx_age ON student;
DROP INDEX idx_age_classid ON student;

EXPLAIN SELECT SQL_NO_CACHE * FROM student 
WHERE student.age=30 AND student.name = 'abcd'; 
#用到了联合索引，但是只对age进行了索引查询
```

结论：MySQL可以为多个字段创建索引, 一个索引可以包括16个字段。对于多列索引，**过滤条件要使用索引必须按照索引建立时的顺序，依次满足一旦跳过某个字段，索引后面的字段都无法被使用**。如果查询条件中没有使用这些字段中第1个字段时，多列(或联合)索引不会被使用。

#### 主键按顺序插入

对于一个使用InnoDB存储引擎的表来说，在我们没有显式的创建索引时，表中的数据实际上都是存储在聚簇索引的叶子节点的。而记录又是存储在数据页中的，数据页和记录又是按照记录**主键值从小到大**的顺序进行排序，所以如果我们插入的记录的**主键值是依次增大**的话，那我们每插满一个数据页就换到下一 个数据页继续插，而如果我们插入的主键值忽大忽小的话，就比较麻烦了。

假设某个数据页存储的记录已经满了，它存储的主键值在1~100之间，如果要插入一条主键为9的记录时，就会导致**页面分裂**，就意味着**性能损耗**！为了避免，最好让插入的记录**主键依次递增**。

建议：让主键具有`AUTO_INCREMENT`，让存储引擎自己为表生成主键，而不是我们手动插入。

```
CREATE TABLE XXX(
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    ...  
);
```

#### 计算、函数、类型转换（手动或自动）导致索引失效

```
#4)计算、函数、类型转换(自动或手动)导致索引失效

#此语句比下一条要好！（能够使用上索引）
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name LIKE 'abc%';

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE LEFT(student.name,3) = 'abc'; #用到了函数，索引失效

#给name创建索引
CREATE INDEX idx_name ON student(NAME);
#给stuno创建索引
CREATE INDEX idx_sno ON student(stuno);

EXPLAIN SELECT SQL_NO_CACHE id, stuno, NAME FROM student WHERE stuno+1 = 900001;#用到了计算，索引失效

EXPLAIN SELECT SQL_NO_CACHE id, stuno, NAME FROM student WHERE stuno = 900000;

EXPLAIN SELECT id, stuno, NAME FROM student WHERE SUBSTRING(NAME, 1,3)='abc';#用了函数
```

#### 类型转换导致索引失效

```
#5)类型转换导致索引失效
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE NAME = 123; #失效
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE NAME = '123'; 
```

#### 范围条件右边的列索引失效

```
#6)范围条件右边的列索引失效
#右边指的是创建索引时的左右顺序
SHOW INDEX FROM student;

CALL proc_drop_index('atguigudb2','student');

#创建一个联合索引
CREATE INDEX idx_age_classId_name ON student(age,classId,NAME);

EXPLAIN SELECT SQL_NO_CACHE * FROM student 
WHERE student.age=30 AND student.classId>20 AND student.name = 'abc' ; #会用到，但因为classID是一个范围查询，因此索引没有作用到name

EXPLAIN SELECT SQL_NO_CACHE * FROM student 
WHERE student.age=30 AND student.name = 'abc' AND student.classId>20; #依旧没有作用到name，但是下面的索引可以用到

CREATE INDEX idx_age_name_cid ON student(age,NAME,classId);
```

#### 不等于（!=或者<>）索引失效

```
#7)不等于(!= 或者<>)索引失效
CREATE INDEX idx_name ON student(NAME);

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name <> 'abc' ;

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name != 'abc' ;
```

#### is null可以使用索引，is not null无法使用索引

```
#8）is null可以使用索引，is not null无法使用索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age IS NULL; 

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age IS NOT NULL; 
```

> 结论：最好在设计数据表的时候就将`字段设置为NOT NULL约束`，比如你可以将INT类型的字段，默认值设置为0。将字符类型的默认值设置为空字符串(")。
>
> 拓展：同理，在查询中使用`not like`也无法使用索引，导致全表扫描。

#### like以通配符%开头索引失效

```
#9)like以通配符%开头索引失效

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE NAME LIKE 'ab%'; #不会失效

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE NAME LIKE '%ab%';#会失效
```

#### OR前后存在非索引的列，索引失效

```
#10)OR 前后存在非索引的列，索引失效
SHOW INDEX FROM student;

CALL proc_drop_index('atguigudb2','student');

CREATE INDEX idx_age ON student(age);

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 10 OR classid = 100;

CREATE INDEX idx_cid ON student(classid);
```

#### 数据库和表的字符集统一使用utf8mb4

统一使用utf8mb4( 5.5.3版本以上支持)兼容性更好，统一字符集可以避免由于**字符集**转换生的乱码。不同的字符集进行比较前需要进行**转换**会造成索引失效。

**建议：**

- **对于单列索引，尽量选择针对当前query过滤性更好的索引**
- **在选择组合索引的时候，当前query中过滤性最好的字段在索引字段顺序中，位置越靠前越好。**
- **在选择组合索引的时候，尽量选择能够包含当前query中的where子句中更多字段的索引。**
- **在选择组合索引的时候，如果某个字段可能出现范围查询时，尽量把这个字段放在索引次序的最后面。**

**总之，书写SQL语句时，尽量避免造成索引失效的情况。**

### 关联查询优化

#### 数据准备

```
#分类
CREATE TABLE IF NOT EXISTS `type` (
`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
`card` INT(10) UNSIGNED NOT NULL,
PRIMARY KEY (`id`)
);

#图书
CREATE TABLE IF NOT EXISTS `book` (
`bookid` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
`card` INT(10) UNSIGNED NOT NULL,
PRIMARY KEY (`bookid`)
);

#向分类表中添加20条记录
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO TYPE(card) VALUES(FLOOR(1 + (RAND() * 20)));

#向图书表中添加20条记录
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
```

#### 采用左外链接

```
EXPLAIN SELECT SQL_NO_CACHE * FROM `type` LEFT JOIN book ON type.card = book.card;
```

![image-20230128124828312](/image-20230128124828312.png)

```
#给book添加索引
CREATE INDEX Y ON book(card);

EXPLAIN SELECT SQL_NO_CACHE * FROM `type` LEFT JOIN book ON type.card = book.card;
```

![image-20230128124842829](/image-20230128124842829.png)

```
#给card添加索引
CREATE INDEX X ON `type`(card);

EXPLAIN SELECT SQL_NO_CACHE * FROM `type` LEFT JOIN book ON type.card = book.card;
```

![image-20230128124852474](/image-20230128124852474.png)

```
#删除book的索引
DROP INDEX Y ON book;

EXPLAIN SELECT SQL_NO_CACHE * FROM `type` LEFT JOIN book ON type.card = book.card;
```

#### 采用内连接

```
#删除现有的索引
DROP INDEX X ON `type`;

EXPLAIN SELECT SQL_NO_CACHE * FROM `type` INNER JOIN book ON type.card = book.card;#都是ALL

#添加book中card索引
CREATE INDEX Y ON book(card);

EXPLAIN SELECT SQL_NO_CACHE * FROM `type` INNER JOIN book ON type.card = book.card;#book用到索引了，type没有

#添加type中card索引
CREATE INDEX X ON `type`(card);

#结论：对于内连接来说，查询优化器可以决定谁作为驱动表，谁作为被驱动表出现的
EXPLAIN SELECT SQL_NO_CACHE * FROM `type` INNER JOIN book ON type.card = book.card;
#都用索引了，且book为驱动表，type为被驱动表

#删除索引
DROP INDEX Y ON book;
#结论：对于内连接来讲，如果表的连接条件中只能有一个字段有索引，则有索引的字段所在的表会被作为被驱动表出现。
#被驱动表有索引的情况整体成本较低
EXPLAIN SELECT SQL_NO_CACHE * FROM `type` INNER JOIN book ON type.card = book.card;

#再加上索引
CREATE INDEX Y ON book(card);
EXPLAIN SELECT SQL_NO_CACHE * FROM `type` INNER JOIN book ON type.card = book.card;

#向type（驱动表）表中添加数据（20条数据）
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));

#结论：对于内连接来说，在两个表的连接条件都存在索引的情况下，会选择小表作为驱动表。“小表驱动大表”
EXPLAIN SELECT SQL_NO_CACHE * FROM `type` INNER JOIN book ON type.card = book.card;
```

**结论**

- 查询优化器可以决定谁作为驱动表，谁作为被驱动表出现的
- 如果表的连接条件中只能有一个字段有索引，则有索引的字段所在的表会被作为被驱动表出现。（被驱动表有索引的情况整体成本较低）
- 在两个表的连接条件都存在索引的情况下，会选择小表作为驱动表。“小表驱动大表”

#### join语句原理

join方式连接多介表，本质就是各个表之间数据的循环匹配。MySQL5.5 版本之前，MySQL只支持一种表间关联方式，就是**嵌套循环(Nested Loop Join)**。如果关联表的数据量很大，则join关联的执行时间会非常长。在MySQL5.5以后的版本中，MySQL通过引入BNLJ算法来优化嵌套执行。

##### 驱动表和被驱动表

驱动表就是主表，被驱动表就是从表、非驱动表

- 对于内连接

  - ```
    Select * from A join B on ...
    ```

    表A不一定是驱动表。驱动表的选择是依据优化器根据你的查询语句做优化，先查询哪张表哪张表就是驱动表，可以通过explain查看。

- **对于外连接**

  - 表数据

    - ```
      CREATE TABLE a(f1 INT, f2 INT, INDEX(f1))ENGINE=INNODB;
      
      CREATE TABLE b(f1 INT, f2 INT)ENGINE=INNODB;
      
      
      INSERT INTO a VALUES(1,1),(2,2),(3,3),(4,4),(5,5),(6,6);
      
      INSERT INTO b VALUES(3,3),(4,4),(5,5),(6,6),(7,7),(8,8);
      ```

  - b为驱动表，a为被驱动表：

    ```
    #测试1
    EXPLAIN SELECT * FROM a LEFT JOIN b ON(a.f1=b.f1) WHERE (a.f2=b.f2);
    ```

    ![image-20230128125116110](/image-20230128125116110.png)

    a为驱动表，b为被驱动表：

    ```
    #测试2
    EXPLAIN SELECT * FROM a LEFT JOIN b ON(a.f1=b.f1) AND (a.f2=b.f2);
    ```

    ![image-20230128125127475](/image-20230128125127475.png)

    b为驱动表，a为被驱动表：

    ```
    #测试3
    EXPLAIN SELECT * FROM a JOIN b ON(a.f1=b.f1) WHERE (a.f2=b.f2);
    ```

    ![image-20230128125140495](/image-20230128125140495.png)

##### 简单嵌套循环连接（SNLJ）

算法相当简单，从表A中取出一条数据1，遍历表B，将匹配到的数据放到resul..以此类推，驱动表A中的每一条记录与被驱动表B的记录进行判断：

![image-20230128125231471](/image-20230128125231471.png)

可以看到这种方式效率是非常低的，以上述表A数据100条，表B数据1000条计算，则A*B= 10万次。

开销消耗如下：

| 开销统计          | SNLJ  |
| ----------------- | ----- |
| 外表（A）扫描次数 | 1     |
| 内表（B）扫描次数 | A     |
| 读取次数          | A+A*B |
| JOIN比较次数      | A*B   |
| 回表读取记录次数  | 0     |

##### 索引嵌套循环连接（INLJ）

Index Nested-Loop Join其优化的思路主要是为了**减少内层表数据的匹配次数**，所以要求被驱动表上必须**有索引**才行。通过外层表匹配条件直接与内层表索引进行匹配，避免和内层表的每条记录去进行比较，这样极大的减少了对内层表的匹配次数。

![image-20230128125445686](/image-20230128125445686.png)

驱动表中的每条记录都通过被驱动表的索引进行访问，因为索引查询的成本是比较固定的，故mysql优化器都倾向于使用记录数少的表作为驱动表（外表）。

| 开销统计          | SNLJ  | INLJ                  |
| ----------------- | ----- | --------------------- |
| 外表（A）扫描次数 | 1     | 1                     |
| 内表（B）扫描次数 | A     | 0                     |
| 读取次数          | A+A*B | A+B（match)           |
| JOIN比较次数      | A*B   | A*Index(Height)       |
| 回表读取记录次数  | 0     | B(match)(if possible) |

如果被驱动表加索引，效率是非常高的，但如果索引不是主键索引，所以还得进行一次回表查询。 相比，被驱动表的索弓是主键索引，效率会更高。

##### 块嵌套循环链接（BNLJ）

如果存在索引，那么会使用index的方式进行join，如果join的列没有索引，被驱动表要扫描的次数太多了。每次访问被驱动表，其表中的记录都会被加载到内存中，然后再从驱动表中取一条与其匹配，匹配结束后清除内存，然后再从驱动表中加载一条记录，然后把被驱动表的记录在加载到内存匹配，这样周而复始，大大增加了IO的次数。为了减少被驱动表的IO次数，就出现了Block Nested-Loop Join的方式。

不再是逐条获取驱动表的数据，而是一块一块的获取，引入了**join buffer**缓冲区 ，将驱动表join相关的部分数据列(大小受join buffer的限制)缓存到join buffer中，然后全表扫描被驱动表，被驱动表的每一条记录一次性和join buffer中的所有驱动表记录进行匹配(内存中操作)，将简单嵌套循环中的多次比较合并成一次， 降低了被驱动表的访问频率。

> 注意：
>
> 这里缓存的不只是关联表的列，select 后面的列也会缓存起来。
>
> 在一个有N个join关联的sql中会分配N-1个join buffer。 所以查询的时候尽量减少不必要的字段，可以让join buffer中可以存放更多的列。

![image-20230128125456577](/image-20230128125456577.png)

| 开销统计          | SNLJ  | INLJ                  | BNLJ                                        |
| ----------------- | ----- | --------------------- | ------------------------------------------- |
| 外表（A）扫描次数 | 1     | 1                     | 1                                           |
| 内表（B）扫描次数 | A     | 0                     | A*used_column_size/join_buffer_size+1       |
| 读取次数          | A+A*B | A+B（match)           | A+B*(A *userd_column_size/join_buffer_size) |
| JOIN比较次数      | A*B   | A*Index(Height)       | A*B                                         |
| 回表读取记录次数  | 0     | B(match)(if possible) | 0                                           |

used_column_size：要查询的列所占的空间大小

join_buffer_size：每次join_buffer开辟的缓冲空间的大小

**参数设置：**

- block_nested_loop
  - 通过`show variables like '%optimizer_ switch%'`、 查看 `block_nested_loop` 状态。默认是开启的。
- join_buffer_size
  - 驱动表能不能一次加载完，要看join buffer能不能存储所有的数据，默认情况下`join_buffer_size`=256k。
  - join_ buffer _size的最大值在32位系统可以申请4G，而在64位操做系统下可以申请大于4G的Join Buffr空间(64 位Windows除外，其大值会被截断为4GB并发出警告)。

##### join小结

1. **整体效率比较：INLJ> BNLJ> SNLJ**

2. 永远用**小结果集驱动大结果集**(其本质就是减少外层循环的数据数量) (小的度量单位指的是表行数*每行大小) 

   ```
   select t1.b,t2.* from t1 straight_join t2 on (t1.b=t2.b) where t2.id<=100; #推荐，驱动表中只用查询t1中结果为b的数据
   
   select t1.b,t2.* from t2 straight_join t1 on (t1.b=t2.b) where t2.id<=100; #不推荐，驱动表中只用查询t2中的所有数据
   ```

3. 为被驱动表匹配的条件增加索引(减少内层表的循环匹配次数)

4. 增大join buffer size的大小(一次缓存的数据越多，那么内层包的表次数就越少)

5. 减少驱动表不必要的字段查询(字段越少，join buffer所缓存的数据就越多)

##### Hash join

**从MySQL的8.0.20版本开始将废弃BNLJ，因为从MySQL8.0.18版本开始就加入了hash join默认都会使用hash join**

| 类别         | Nested Loop                                                  | Hash Join                                                    |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **使用条件** | 任何条件                                                     | 等值连接（=）                                                |
| **相关资源** | CPU、磁盘IO                                                  | 内存、临时空间                                               |
| **特点**     | 当有高选择性索引或进行限制性搜索时效率比较高，能够快速返回第一次的搜索结果 | 当缺乏索引或者索引条件模糊时，Hash Join比Nested Loop有效。在数据仓库环境下，如果表的记录数多，效率高 |
| **缺点**     | 当索引丢失或者查询条件不够时，效率很低；当标的记录数多时，效率低 | 为建立哈希表，需要大量内存。第一次的结果返回较慢             |

#### 小结

- 保证被驱动表的JOIN字段已经创建了索引
- 需要JOIN的字段，数据类型保持绝对一致。
- LEFT JOIN时，**选择小表作为驱动表，大表作为被驱动表**。减少外层循环的次数。
- INNER JOIN时，**MySQL会自动将小结果集的表选为驱动表**。选择相信MySQL优化策略。
- 能够直接多表关联的尽量直接关联，不用子查询。(减少查询的趟数)
- 不建议使用子查询，建议将子查询SQL拆开结合程序多次查询，或使用JOIN来代替子查询。
- 衍生表建不了索引

### 子查询优化

MySQL从4.1版本开始支持子查询，使用子查询可以进行SELECT语 句的嵌套查询，即一个SELECT查询的结果作为另一个SELECT语句的条件。子查询可以一次性完成很多逻辑上需要多个步骤才能完成的SQL操作。

**子查询是MySQL的一项重要的功能，可以帮助我们通过一个SQL语句实现比较复杂的查询。但是，子查询的执行效率不高。**原因：

①执行子查询时，MySQL需要为内层查询语句的查询结果**建立一个临时表**，然后外层查询语句从临时表中查询记录。查询完毕后，再**撤销这些临时表**。这样会消耗过多的CPU和IO资源，产生大量的慢查询。

②子查询的结果集存储的临时表，不论是内存临时表还是磁盘临时表都**不会存在索引**，所以查询性能会受到一定的影响。

③对于返回结果集比较大的子查询，其对查询性能的影响也就越大。

**在MySQL中，可以使用连接(JOIN) 查询来替代子查询。**连接查询**不需要建立临时表**，其**速度比子查询要快**，如果查询中使用索引的话，性能就会更好。

> 结论：尽量不要使用NOT IN或者NOT EXISTS,用LEFT JOIN XXx ON xx WHERE xx IS NULL替代

```
#创建班级表中班长的索引
CREATE INDEX idx_monitor ON class(monitor);

#查询班长的信息
EXPLAIN SELECT * FROM student stu1
WHERE stu1.`stuno` IN (
SELECT monitor
FROM class c
WHERE monitor IS NOT NULL
);

EXPLAIN SELECT stu1.* FROM student stu1 JOIN class c 
ON stu1.`stuno` = c.`monitor`
WHERE c.`monitor` IS NOT NULL;
#查询不为班长的学生信息
EXPLAIN SELECT SQL_NO_CACHE a.* 
FROM student a 
WHERE  a.stuno  NOT  IN (
			SELECT monitor FROM class b 
			WHERE monitor IS NOT NULL) 

EXPLAIN SELECT SQL_NO_CACHE a.*
FROM  student a LEFT OUTER JOIN class b 
ON a.stuno =b.monitor
WHERE b.monitor IS NULL;
```

**结论：尽量不要使用NOT IN或者NOT EXISTS，用LEFT JOIN xxx ON xx WHERE xx IS NULL代替**

### 排序优化

#### 排序优化

**问题：**在 WHERE 条件字段上加索引，但是为什么在 ORDER BY 字段上还要加索引呢？

**回答：**

在MySQL中，支持两种排序方式，分别是`FileSort` 和`Index`排序。

- Index排序中，索引可以保证数据的有序性，不需要再进行排序，`效率更高`。
- FileSort排序则一般在`内存中`进行排序，占用`CPU较多`。如果待排结果较大，会产生临时文件I/O到磁盘进行排序的情况，效率较低。

**优化建议：**

1. SQL 中，可以在 WHERE 子句和 ORDER BY 子句中使用索引，目的是在 WHERE 子句中`避免全表扫描`，在 ORDER BY 子句`避免使用 FileSort 排序` 。当然，某些情况下全表扫描，或者 FileSort 排序不一定比索引慢。但总的来说，我们还是要避免，以提高查询效率。
2. 尽量使用 Index 完成 ORDER BY 排序。如果 WHERE 和 ORDER BY 后面是相同的列就使用单索引列；如果不同就使用联合索引。
3. 无法使用 Index 时，需要对 FileSort 方式进行调优。

#### 测试

```
#5. 排序优化
#删除student和class表中的非主键索引
CALL proc_drop_index('atguigudb2','student');
CALL proc_drop_index('atguigudb2','class');

#查看表中的索引，已经全部被删除了
SHOW INDEX FROM student;
SHOW INDEX FROM class;


#过程一：
EXPLAIN SELECT SQL_NO_CACHE * FROM student ORDER BY age,classid; #无索引

EXPLAIN SELECT SQL_NO_CACHE * FROM student ORDER BY age,classid LIMIT 10; #无索引


#过程二：order by时不limit，索引失效
#创建索引  
CREATE  INDEX idx_age_classid_name ON student (age,classid,NAME);

#不限制,索引失效
EXPLAIN  SELECT SQL_NO_CACHE * FROM student ORDER BY age,classid; #没有用索引，原因：因为要查询所有的数据，要不停地要回表，数据量太大了，查询的效率降低了

#EXPLAIN  SELECT SQL_NO_CACHE age,classid,name,id FROM student ORDER BY age,classid; #用上索引了，原因：没有回表——覆盖索引

#增加limit过滤条件，使用上索引了。
EXPLAIN  SELECT SQL_NO_CACHE * FROM student ORDER BY age,classid LIMIT 10;  #原因：数据量小了，所以可以用索引了


#过程三：order by时顺序错误，索引失效

#创建索引age,classid,stuno
CREATE  INDEX idx_age_classid_stuno ON student (age,classid,stuno); 

#以下哪些索引失效?
EXPLAIN  SELECT * FROM student ORDER BY classid LIMIT 10;#失效

EXPLAIN  SELECT * FROM student ORDER BY classid,NAME LIMIT 10;  #失效

EXPLAIN  SELECT * FROM student ORDER BY age,classid,stuno LIMIT 10; #没失效

EXPLAIN  SELECT * FROM student ORDER BY age,classid LIMIT 10;#没失效

EXPLAIN  SELECT * FROM student ORDER BY age LIMIT 10;#没失效


#过程四：order by时规则不一致, 索引失效 （顺序错，不索引；方向反，不索引）
索引什么都不写默认是ASC
EXPLAIN  SELECT * FROM student ORDER BY age DESC, classid ASC LIMIT 10; #有降序，失效

EXPLAIN  SELECT * FROM student ORDER BY classid DESC, NAME DESC LIMIT 10; #都升序，但没索引，失效

EXPLAIN  SELECT * FROM student ORDER BY age ASC,classid DESC LIMIT 10; #有降序，失效

EXPLAIN  SELECT * FROM student ORDER BY age DESC, classid DESC LIMIT 10;#可以

#过程五：无过滤，不索引

EXPLAIN  SELECT * FROM student WHERE age=45 ORDER BY classid;#没失效，索引只作用到了age

EXPLAIN  SELECT * FROM student WHERE  age=45 ORDER BY classid,NAME;#没失效，索引只作用到了age

EXPLAIN  SELECT * FROM student WHERE  classid=45 ORDER BY age;#失效，数据量太大了

EXPLAIN  SELECT * FROM student WHERE  classid=45 ORDER BY age LIMIT 10;#不失效，数据量小了

CREATE INDEX idx_cid ON student(classid);
EXPLAIN  SELECT * FROM student WHERE  classid=45 ORDER BY age;#用到了刚刚创建的索引
```

**小结：**

```
INDEX a_b_c(a,b,c)

order by 
#能使用索引最左前缀
- ORDER BY a
- ORDER BY a,b
- ORDER BY a,b,c
- ORDER BY a DESC,b DESC,c DESC 

#如果WHERE使用索引的最左前缀定义为常量，则order by 能使用索引
- WHERE a = const ORDER BY b,c
- WHERE a = const AND b = const ORDER BY c
- WHERE a = const ORDER BY b,c
- WHERE a = const AND b > const ORDER BY b,c 

#不能使用索引进行排序
- ORDER BY a ASC,b DESC,c DESC 	/*排序不一致 */
- WHERE g = const ORDER BY b,c 	/*丢失a索引*/
- WHERE a = const ORDER BY c 	/*丢失b索引*/
- WHERE a = const ORDER BY a,d 	/*d不是索引的一部分*/
- WHERE a in (...) ORDER BY b,c /*对于排序来说，多个相等条件也是范围查询*/
```

#### 案例实战

```
#实战：测试filesort和index排序
CALL proc_drop_index('atguigudb2','student');

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 30 AND stuno <101000 ORDER BY NAME ;#0.114sec

#方案一: 为了去掉filesort我们可以把索引建成

CREATE INDEX idx_age_name ON student(age,NAME);

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 30 AND stuno <101000 ORDER BY NAME ;#0.039sec

#方案二：不去掉

CREATE INDEX idx_age_stuno_name ON student(age,stuno,NAME);

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 30 AND stuno <101000 ORDER BY NAME ;#索引作用范围：age、stuno 0.002sec 
#虽然用了filesort,但是stuno也用到了索引，所以更快乐

DROP INDEX idx_age_stuno_name ON student;

CREATE INDEX idx_age_stuno ON student(age,stuno);
```

> 结论:
>
> 1.两个索引同时存在，mysql自动选择最优的方案。(对于这个例子，mysql选择idx_age_stuno_name) 。 但是，**随着数据量的变化，选择的索引也会随之变化的。**
>
> **2.当【范围条件】【group by或者order by】的字段出现二选一时，优先观察条件字段的过滤数量，如果过滤的数据足够多，而需要排序的数据并不多时，优先把索引放在范围字段上。反之，亦然。**

#### filesort算法：双路排序和单路排序

排序的字段若不在索引列上，则filesort会有两种算法：**双路排序**和**单路排序**

**双路排序 （慢）**

- **MySQL 4.1之前是使用双路排序** ，字面意思就是两次扫描磁盘，最终得到数据， 读取行指针和order by列 ，对他们进行排序，然后扫描已经排序好的列表，按照列表中的值重新从列表中读取对应的数据输出。
- 从磁盘取排序字段，在buffer进行排序，再从磁盘取其他字段 。

取一批数据，要对磁盘进行两次扫描，众所周知，IO是很耗时的，所以在mysql4.1之后，出现了第二种改进的算法，就是单路排序。

**单路排序 （快）**

从磁盘读取查询需要的**所有列** ，按照order by列在buffer对它们进行排序，然后扫描排序后的列表进行输出， 它的效率更快一些，避免了第二次读取数据。并且把随机IO变成了顺序IO，但是它会使用更多的空间， 因为它把每一行都保存在内存中了。**对内存的要求更高些。**

**结论及引申出的问题**

- 由于单路是后出的，总体而言好过双路
- 但是用单路有问题
  - 在sort_buffer中， **单路比多路要多占用很多空间**，因为单路是把所有字段都取出，所以有可能取出的数据的总大小超出了sort_buffer 的容量，导致每次只能取sort_buffer容量大小的数据，进行排序(创建tmp文件，多路合并)，排完再取sort_buffer容量大小，再排....从而多次I/O。
  - 单路本来想省一次I/O操作， **反而导致了大量的I/O操作**，反而得不偿失。

**优化策略**

1. **尝试提高** sort_buffer_size 
2. **尝试提高** max_length_for_sort_data 
   - 但是如果设的太高，数据总容量超出sort_buffer_size的概率就增大，明显症状是高的磁盘I/O活动和低的处理器使用率。如果需要返回的列的总长度大于max_ length_for_sort_data，使用双路算法，否则使用单路算法。1024-8192字节之间调整
3. **Order by 时`select *` 是一个大忌。最好只Query需要的字段。**
   - 当Query的字段大小总和小于max_length_for_sort_data，而且排序字段不是TEXT|BLOB类型时，会用改进后的算法——单路排序，否则用老算法——多路排序。
   - 两种算法的数据都有可能超出sort_buffer_size的容量，超出之后，会创建tmp文件进行合并排序，导致多次I/O，但是用单路排序算法的风险会更大一些，所以要提高sort_buffer_size。

### GROUP BY优化

- group by使用索引的原则几乎跟order by一致，group by**即使没有过滤条件用到索引，也可以直接使用索引**。
- group by先排序再分组，遵照索引建的最佳左前缀法则
- 当无法使用索引列，增大max_length_for_sort_data和sort_buffer_size参数的设置
- **where效率高于having**，能写在where限定的条件就不要写在having中了
- **减少使用order by**，和业务沟通能不排序就不排序，或将排序放到程序端去做。Order by、group by、distinct这些语句较为**耗费CPU**，数据库的CPU资源是极其宝贵的。
- 包含了order by、 group by、 distinct这些查询的语句，where条件过滤出来的结果集请**保持在1000行以内**，否则SQL会很慢。

### 优化分页查询

一般分页查询时， 通过创建覆盖索引能够比较好地提高性能。一个常见又非常头疼的问题就是limit 200000,10，此时需要MySQL排序前2000010记录，仅仅返向回20000000 - 2000010的记录，其他记录丢弃，查询排序的代价非常大。

```
EXPLAIN SELECT * FROM student LIMIT 2000000,10
```

用了很大的力气，做了最简单的事。不值。

**优化思路一：**

在索引上完成排序分页操作，最后根据主键关联回原表查询所需要的其他列内容。

```
EXPLAIN SELECT * FROM student t,(SELECT id FROM student ORDER BY id LIMIT 2000000,10) a WHERE t.id = a.id;
```

**优化思路二：**

该方案适用于主键自增的表，可以把Limit查询转换成某个位置的查询。

```
EXPLAIN SELECT * FROM student WHERE id > 2000000 LIMIT 10;
```

### 优先考虑覆盖索引

**理解方式一**：索引是高效找到行的一个方法，但是一般数据库也能使用索引找到一个列的数据，因此它不必读取整个行。毕竟索引叶子节点存储了它们索引的数据；当能通过读取索引就可以得到想要的数据，那就不需要读取行了。**一个索引包含了满足查询结果的数据就叫做覆盖索引。**

**理解方式二**：非聚簇复合索引一种形式，它包括在查询里的SELECT、JOIN和WHERE子句用到的所有列（即建索引的字段正好是覆盖查询条件中所涉及的字段）。

**简单说就是， 索引列+主键 包含 SELECT 到 FROM之间查询的列 。** 

#### 为什么要覆盖索引

```
#删除之前的索引
#举例1：
DROP INDEX idx_age_stuno ON student;

CREATE INDEX idx_age_name ON student (age,NAME);

EXPLAIN SELECT * FROM student WHERE age <> 20;#未使用到索引

EXPLAIN SELECT age,NAME FROM student WHERE age <> 20;#用到了索引


#举例2：
EXPLAIN SELECT * FROM student WHERE NAME LIKE '%abc';#未使用到索引

EXPLAIN SELECT id,age FROM student WHERE NAME LIKE '%abc';#用到了索引

###
SELECT CRC32('hello') FROM DUAL;
```

#### 覆盖索引的利弊

**好处：**

1. **避免Innodb表进行索引的二次查询（回表）**
   - 在覆盖索引中，二级索引的键值中可以获取所要的数据，避免了 对主键的二次查询，减少了IO操作，提升了查询效率。
2. **可以把随机IO变成顺序IO加快查询效率**
   - 回表的时候，数据可能在不同的页里，因此是随机IO的；而在索引中，读取到的数据都是挨着的，因此就是顺序索引。

**由于覆盖索引可以减少树的搜索次数，显著提升查询性能，所以使用覆盖索引是一个常用的性能优化手段。**

**弊端：**

**索引字段的维护**总是有代价的。因此，在建立冗余索引来支持覆盖索引时就需要权衡考虑了。这是业务DBA，或者称为业务数据架构师的工作。

### 如何给字符串添加索引

有一张教师表，表定义如下：

```
create table teacher( 
    ID bigint unsigned primary key, 
    email varchar(64),
    ... 
)engine=innodb;
```

讲师要使用邮箱登录，所以业务代码中一定会出现类似于这样的语句：

```
mysql> select col1, col2 from teacher where email='xxx';
```

如果email这个字段上没有索引，那么这个语句就只能做`全表扫描` 。 

#### 前缀索引

MySQL是支持前缀索引的。默认地，如果你创建索引的语句不指定前缀长度，那么索引就会包含整个字符串。

```
mysql> alter table teacher add index index1(email); 
#或
mysql> alter table teacher add index index2(email(6));
```

这两种不同的定义在数据结构和存储上有什么区别呢？下图就是这两个索引的示意图。

![image-20230128131848429](/image-20230128131848429.png)

![image-20230128131857226](/image-20230128131857226.png)

**如果使用的是index1**（即email整个字符串的索引结构），执行顺序是这样的：

1. 从index1索引树找到满足索引值是 [zhangssxyz@xxx.com](mailto:zhangssxyz@xxx.com) 的这条记录，取得ID2的值；
2. 到主键上查到主键值是ID2的行，判断email的值是正确的，将这行记录加入结果集；
3. 取index1索引树上刚刚查到的位置的下一条记录，发现已经不满足email=' [zhangssxyz@xxx.com](mailto:zhangssxyz@xxx.com) ’的条件了，循环结束。

这个过程中，只需要回主键索引取一次数据，所以系统认为只扫描了一行。

**如果使用的是index2**（即email(6)索引结构），执行顺序是这样的：

1. 从index2索引树找到满足索引值是’zhangs’的记录，找到的第一个是ID1； 
2. 到主键上查到主键值是ID1的行，判断出email的值不是’ [zhangssxyz@xxx.com](mailto:zhangssxyz@xxx.com) ’，这行记录丢弃；
3. 取index2上刚刚查到的位置的下一条记录，发现仍然是’zhangs’，取出ID2，再到ID索引上取整行然后判断，这次值对了，将这行记录加入结果集；
4. 重复上一步，直到在idxe2上取到的值不是’zhangs’时，循环结束。

也就是说**使用前缀索引，定义好长度，就可以做到既节省空间，又不用额外增加太多的查询成本**。前面已经讲过区分度，区分度越高越好。因为区分度越高，意味着重复的键值越少。

#### 前缀索引对覆盖索引的影响

**使用前缀索引就用不上覆盖索引对查询性能的优化了，这也是你在选择是否使用前缀索引时需要考虑的一个因素。**

### 索引下推

#### 概述

Index Condition Pushdown(ICP)是MySQL 5.6中新特性，是一种在存储引擎层使用索引过滤数据的优化方式。

- 如果没有ICP，存储引擎会遍历索引以定位基表中的行，并将它们返回给MySQL服务器， 由MySQL服务器评估WHERE后面的条件是否保留行。
- 启用ICP后，如果部分WHERE条件可以仅使用索引中的列进行筛选，则MySQL服务器会把这部分WHERE条件放到存储引擎筛选。然后，存储引擎通过使用索引条目来筛选数据，并且只有在满足这一条件时才从表中读取行。
  - 好处：ICP可以减少存储引擎必须访问基表的次数和MySQL服务器必须访问存储引擎的次数。
  - 但是，ICP的加速效果取决于在存储引擎内通过ICP筛选掉的数据的比例。

#### ICP的开启/关闭

- 默认情况下启用索引条件下推。可以通过设置系统变量`optimizer switch` 控制： index_condition_pushdown

```
#关闭索引下推
SET optimizer_switch ='index_ condition_pushdown=off' ;

#打开索引下推
SET optimizer_switch ='index_ condition_pushdown=on' ;
```

- 当使用索引条件下推时， **EXPLAIN**语句输出结果中Extra列内容显示为**Using index condition**。

#### ICP使用案例

```
#举例1：
USE atguigudb1;

EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' AND key1 LIKE '%a';
#并没有直接去回表，而是先判断完and前面的索引，再判断and后面的索引，再将都满足的索引回表。减少了整体的回表次数。

#举例2：
CREATE TABLE `people` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `zipcode` VARCHAR(20) COLLATE utf8_bin DEFAULT NULL,
  `firstname` VARCHAR(20) COLLATE utf8_bin DEFAULT NULL,
  `lastname` VARCHAR(20) COLLATE utf8_bin DEFAULT NULL,
  `address` VARCHAR(50) COLLATE utf8_bin DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `zip_last_first` (`zipcode`,`lastname`,`firstname`)
) ENGINE=INNODB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb3 COLLATE=utf8_bin;

#添加数据
INSERT INTO `people` VALUES 
('1', '000001', '三', '张', '北京市'), 
('2', '000002', '四', '李', '南京市'), 
('3', '000003', '五', '王', '上海市'), 
('4', '000001', '六', '赵', '天津市');


EXPLAIN SELECT * FROM people
WHERE zipcode='000001'
AND lastname LIKE '%张%'
AND address LIKE '%北京市%';
#虽然索引只作用到了zipcode，`lastname`,`firstname`的部分都失效了
#但是因为ICP的原因，虽然没有作用到lastname，但是也会回表前对数据进行一次过滤。

EXPLAIN SELECT * FROM people
WHERE zipcode='000001'
AND lastname LIKE '张%'
AND firstname LIKE '三%';

#打开索引下推
SET optimizer_switch = 'index_condition_pushdown=on';
```

#### 开启和关闭ICP的性能对比

```
#创建存储过程，向people表中添加1000000条数据，测试ICP开启和关闭状态下的性能
DELIMITER //
CREATE PROCEDURE  insert_people( max_num INT )
BEGIN  
DECLARE i INT DEFAULT 0;   
 SET autocommit = 0;    
 REPEAT  
 SET i = i + 1;  
 INSERT INTO people ( zipcode,firstname,lastname,address ) VALUES ('000001', '六', '赵', '天津市');  
 UNTIL i = max_num  
 END REPEAT;  
 COMMIT; 
END //
DELIMITER ;

#调用存储过程
CALL insert_people(1000000);

#打开profiling
set profiling=1;

#执行SQL语句，此时默认打开索引下推
SELECT * FROM people WHERE zipcode= '000001' AND lastname LIKE '%张%';

#再次执行SQL语句，不使用索引下推
SELECT /** no_ icp (people) */ * FROM people WHERE zipcode= '000001' AND lastname LIKE '%张%' ;

#查看当前会话所产生的所有Profiles
show Profiles\G;
```

![image-20230128132027525](/image-20230128132027525.png)

多次测试效率对比来看，使用ICP优化的查询效率会好一些，这里建议多存储一些数据效果更明显。

#### ICP的使用条件

1. 如果表访问的类型为 range 、 ref 、 eq_ref 或者 ref_or_null 可以使用ICP。
2. ICP可以用于`MyISAM`和`InnnoDB`表，包括分区表`MyISAM`和`InnnoDB`表。
3. 对于**InnoDB表**，ICP仅用于**二级索引**。ICP的目标是减少全行读取次数，从而减少I/O操作。
4. 当SQL使用覆盖索引时（没有回表操作），不支持ICP。因为这种情况下使用ICP不会减少I/O。
5. 相关子查询的条件不能使用ICP
6. **主要还是在回表前，多做一次过滤**

### 其他查询优化（面试题）

#### EXISTS 和 IN的区别

**问题：**

不太理解哪种情况下应该使用EXISTS，哪种情况应该用IN。选择的标准是看能否使用表的索引吗?

**回答：**

索引是个前提，其实选择与否还是要看表的大小。你可以将选择的标准理解为小表驱动大表。在这种方式下效率是最高的。

比如下面这样：

```
SELECT * FROM A WHERE cc IN (SELECT cc FROM B)#全查出来给外面用

SELECT * FROM A WHERE EXISTS (SELECT cc FROM B WHERE B.cc=A.cc)#查到一条判断一条
```

当A小于B时，用EXISTS。因为EXISTS的实现，相当于外表循环，实现的逻辑类似于:

```
for i in A
	for j in B
		if j.cc == i.cc then...
```

当B小于A时用IN,因为实现的逻辑类似于:

```
for i in B
	for j in A
		if j.cc == i.cc then...
```

**哪个表小就用哪个表来驱动，A表小就用EXISTS，B表小就用IN。**

#### COUNT(*)与COUNT(具体字段)效率

**问：**

在MySQL中统计数据表的行数，可以使用三种方式：`SELECT COUNT(*)、 SELECT COUNT(1) 和SELECT COUNT(具体字段)`，使用这三者之间的查询效率是怎样的?

**答：**

前提：如果你要统计的是某个字段的非空数据行数，则另当别论，毕竟比较执行效率的前提是结果一样才可以。

**环节1：**

`COUNT(*)`和`COUNT(1)`都是对所有结果进行`COUNT，COUNT(*)`和`COUNT(1)`本质上并没有区别(二者执行时间可能略有差别，不过你还是可以把它俩的执行效率看成是相等的)。

- 如果有WHERE子句，则是对所有符合筛选条件的数据行进行统计；
- 如果没有WHERE子句，则是对数据表的数据行数进行统计。

**环节2：**

- 如果是**MyISAM存储引擎**，统计数据表的行数只需要`O(1)`的复杂度，这是因为每张MyISAM的数据表都有一个meta信息存储了row_count值，而一致性则由**表级锁**来保证。
- 如果是**InnoDB存储引擎**，因为InnoDB支持事务，采用**行级锁和MVCC机制**，所以无法像MyISAM一样，维护一个row_count变量，因此需要采用**扫描全表**，是`O(n)`的复杂度，进行循环+计数的方式来完成统计。

**环节3：**

在InnoDB引擎中，如果采用`COUNT(具体字段)`来统计数据行数，要尽量采用二级索引。因为主键采用的索引是聚簇索引，聚簇索引包含的信息多，明显会大于二级索引(非聚簇索引)。

对于`COUNT(*)和COUNT(1)`来说，它们不需要查找具体的行，只是统计行数，系统会自动采用占用空间更小的二级索引来进行统计。

如果有多个二级索引，会使用key_len小的二级索引进行扫描。当没有二级索引的时候，才会采用主键索引来进行统计。

#### 关于SELEXT(*)

在表查询中，建议明确字段,不要使用 * 作为查询的字段列表，推荐使用SELECT <字段列表>查询。原因：

①MySQL在解析的过程中，会通过查询数据字典将"*"按序转换成所有列名，这会大大的耗费资源和时间。

②**无法使用覆盖索引**。

#### LIMIT 1 对优化的影响

针对的是会扫描全表的SQL语句，如果你可以确定结果集只有一条， 那么加上LIMIT 1的时候，当找到一条结果的时候就不会继续扫描了，这样会加快查询速度。

如果数据表已经对字段建立了唯一索引，那么可以通过索引进行查询，不会全表扫描的话，就不需要加上LIMIT 1了。

#### 多实用COMMIT

只要有可能，在程序中尽量多使用COMMIT，这样程序的性能得到提高，需求也会因为COMMIT所释放的资源而减少。

COMMIT所释放的资源：

- 回滚段上用于恢复数据的信息
- 被程序语句获得的锁
- redo / undo log buffer中的空间
- 管理上述3种资源中的内部花费

### 淘宝数据库，主键如何设计？

聊一个实际问题：淘宝的数据库，主键是如何设计的？

某些错的离谱的答案还在网上年复一年的流传着 ，甚至还成为了所谓的MySQL军规。其中，一个最明显的错误就是关于MySQL的主键设计。

大部分人的回答如此自信：用8字节的BIGINT做主键，而不要用INT。**错!**

这样的回答，只站在了数据库这一层，而没有**从业务的角度**思考主键。主键就是一个自增ID吗？ 站在2022年的新年档口，用自增做主键，架构设计上可能**连及格都拿不到**。

#### 自增ID的问题

自增ID做主键，简单易懂，几乎所有数据库都支持自增类型，只是实现上各自有所不同而已。自增ID除了简单，其他都是缺点，总体来看存在以下几方面的问题：

1. **可靠性不高**

  存在自增ID回溯的问题，这个问题直到最新版本的MySQL 8.0才修复。

1. **安全性不高**

  对外暴露的接口可以非常容易猜测对应的信息。比如：/User/1/这样的接口，可以非常容易猜测用户ID的值为多少，总用户数量有多少，也可以非常容易地通过接口进行数据的爬取。

1. **性能差**

  自增ID的性能较差，需要在数据库服务器端生成。

1. **交互多**

  业务还需要额外执行一次类似 last_insert_id() 的函数才能知道刚才插入的自增值，这需要多一次的网络交互。在海量并发的系统中，多1条SQL，就多一次性能上的开销。

1. **局部唯一性**

  最重要的一点，自增ID是局部唯一，只在当前数据库实例中唯一，而不是全局唯一，在任意服务器间都是唯一的。对于目前分布式系统来说，这简直就是噩梦。

#### 业务字段做主键

为了能够唯一地标识一个会员的信息，需要为 **会员信息表** 设置一个主键。那么，怎么为这个表设置主键，才能达到我们理想的目标呢？ 这里我们考虑业务字段做主键。

表数据如下：

![image-20230128133231993](/image-20230128133231993.png)

在这个表里，哪个字段比较合适呢？

- **选择卡号（cardno）**

会员卡号（cardno）看起来比较合适，因为会员卡号不能为空，而且有唯一性，可以用来 标识一条会员记录。

```
mysql> CREATE TABLE demo.membermaster
-> (
-> cardno CHAR(8) PRIMARY KEY, -- 会员卡号为主键
-> membername TEXT,
-> memberphone TEXT,
-> memberpid TEXT,
-> memberaddress TEXT,
-> sex TEXT,
-> birthday DATETIME
-> ); Query OK, 0 rows affected (0.06 sec)
```

不同的会员卡号对应不同的会员，字段“cardno”唯一地标识某一个会员。如果都是这样，会员卡号与会员一一对应，系统是可以正常运行的。

但实际情况是， **会员卡号可能存在重复使用** 的情况。比如，张三因为工作变动搬离了原来的地址，不再到商家的门店消费了 （退还了会员卡），于是张三就不再是这个商家门店的会员了。但是，商家不想让这个会 员卡空着，就把卡号是“10000001”的会员卡发给了王五。

从系统设计的角度看，这个变化只是修改了会员信息表中的卡号是“10000001”这个会员 信息，并不会影响到数据一致性。也就是说，修改会员卡号是“10000001”的会员信息， 系统的各个模块，都会获取到修改后的会员信息，不会出现“有的模块获取到修改之前的会员信息，有的模块获取到修改后的会员信息，而导致系统内部数据不一致”的情况。因此，从 **信息系统层面** 上看是没问题的。

但是从使用 **系统的业务层面** 来看，就有很大的问题 了，会对商家造成影响。

比如，我们有一个销售流水表（trans），记录了所有的销售流水明细。2020 年 12 月 01 日，张三在门店购买了一本书，消费了 89 元。那么，系统中就有了张三买书的流水记录，如下所示：

![image-20230128133242464](/image-20230128133242464.png)

接着，我们查询一下 2020 年 12 月 01 日的会员销售记录：

```
mysql> SELECT b.membername,c.goodsname,a.quantity,a.salesvalue,a.transdate
-> FROM demo.trans AS a
-> JOIN demo.membermaster AS b
-> JOIN demo.goodsmaster AS c
-> ON (a.cardno = b.cardno AND a.itemnumber=c.itemnumber);
+------------+-----------+----------+------------+---------------------+
| membername | goodsname | quantity | salesvalue | transdate 		   | 
+------------+-----------+----------+------------+---------------------+
| 张三 		| 书 	   | 1.000    | 89.00      | 2020-12-01 00:00:00 | 
+------------+-----------+----------+------------+---------------------+
1 row in set (0.00 sec)
```

如果会员卡“10000001”又发给了王五，我们会更改会员信息表。导致查询时

```
mysql> SELECT b.membername,c.goodsname,a.quantity,a.salesvalue,a.transdate
-> FROM demo.trans AS a
-> JOIN demo.membermaster AS b
-> JOIN demo.goodsmaster AS c
-> ON (a.cardno = b.cardno AND a.itemnumber=c.itemnumber); 
+------------+-----------+----------+------------+---------------------+
| membername | goodsname | quantity | salesvalue | transdate           | 
+------------+-----------+----------+------------+---------------------+
| 王五 		| 书 	   | 1.000    | 89.00	   | 2020-12-01 00:00:00 | 
+------------+-----------+----------+------------+---------------------+
1 row in set (0.01 sec)
```

这次得到的结果是：王五在 2020 年 12 月 01 日，买了一本书，消费 89 元。显然是错误的！结论：千万不能把会员卡号当做主键。

- 选择会员电话 或 身份证号

会员电话可以做主键吗？不行的。在实际操作中，手机号也存在 **被运营商收回** ，重新发给别人用的情况。

那身份证号行不行呢？好像可以。因为身份证决不会重复，身份证号与一个人存在一一对应的关系。可问题是，身份证号属于 **个人隐私** ，顾客不一定愿意给你。要是强制要求会员必须登记身份证号，会把很多客人赶跑的。其实，客户电话也有这个问题，这也是我们在设计会员信息表的时候，允许身份证号和电话都为空的原因。

**所以，建议尽量不要用跟业务有关的字段做主键。毕竟，作为项目设计的技术人员，我们谁也无法预测** **在项目的整个生命周期中，哪个业务字段会因为项目的业务需求而有重复，或者重用之类的情况出现。**

> 经验：
>
> 刚开始使用 MySQL 时，很多人都很容易犯的错误是喜欢用业务字段做主键，想当然地认为了解业 务需求，但实际情况往往出乎意料，而更改主键设置的成本非常高。

#### 淘宝的主键设计

在淘宝的电商业务中，订单服务是一个核心业务。请问， **订单表的主键** 淘宝是如何设计的呢？是自增ID吗？

打开淘宝，看一下订单信息：

![image-20230128133256904](/image-20230128133256904.png)

从上图可以发现，订单号不是自增ID！我们详细看下上述4个订单号：

```
1550672064762308113 
1481195847180308113 
1431156171142308113
1431146631521308113
```

订单号是19位的长度，且订单的最后5位都是一样的，都是08113。且订单号的前面14位部分是单调递增的。

大胆猜测，淘宝的订单ID设计应该是：

```
订单ID = 时间 + 去重字段 + 用户ID后6位尾号
```

这样的设计能做到全局唯一，且对分布式系统查询及其友好。

#### 推荐的主键设计

非核心业务 ：对应表的主键自增ID，如告警、日志、监控等信息。

核心业务 ：**主键设计至少应该是全局唯一且是单调递增**。全局唯一保证在各系统之间都是唯一的，单调递增是希望插入时不影响数据库性能。

这里推荐最简单的一种主键设计：UUID。

##### UUID的特点：

全局唯一，占用36字节，数据无序，插入性能差

##### 认识UUID

- 为什么UUID是全局唯一的？
- 为什么UUID占用36个字节？
- 为什么UUID是无序的？

MySQL数据库的UUID组成如下所示：

```
UUID = 时间+UUID版本（16字节）- 时钟序列（4字节） - MAC地址（12字节）
```

我们以UUID值e0ea12d4-6473-11eb-943c-00155dbaa39d举例：

![image-20230128133401660](/image-20230128133401660.png)

**为什么UUID是全局唯一的？** 

在UUID中时间部分占用60位，存储的类似TIMESTAMP的时间戳，但表示的是从1582-10-15 00：00：00.00到现在的100ns的计数。可以看到UUID存储的时间精度比TIMESTAMPE更高，时间维度发生重复的概率降低到1/100ns。

时钟序列是为了避免时钟被回拨导致产生时间重复的可能性。MAC地址用于全局唯一。

**为什么UUID占用36个字节？**

UUID根据字符串进行存储，设计时还带有无用"-"字符串，因此总共需要36个字节。

**为什么UUID是随机无序的呢？**

因为UUID的设计中，将时间低位放在最前面，而这部分的数据是一直在变化的，并且是无序。

##### 改造UUID

若将时间高低位互换，则时间就是单调递增的了，也就变得单调递增了。MySQL 8.0可以更换时间低位和时间高位的存储方式，这样UUID就是有序的UUID了。

MySQL 8.0还解决了UUID存在的空间占用的问题，除去了UUID字符串中无意义的"-"字符串，并且将字符串用二进制类型保存，这样存储空间降低为了16字节。

可以通过MySQL8.0提供的uuid_to_bin函数实现上述功能，同样的，MySQL也提供了bin_to_uuid函数进行转化：

```
SET @uuid = UUID();

SELECT @uuid,uuid_to_bin(@uuid),uuid_to_bin(@uuid,TRUE);
```

![image-20230128133420099](/image-20230128133420099.png)

**通过函数uuid_to_bin(@uuid,true)将UUID转化为有序UUID了**。全局唯一 + 单调递增，这不就是我们想要的主键！

##### 有序UUID性能测试

16字节的有序UUID，相比之前8字节的自增ID，性能和存储空间对比究竟如何呢？

我们来做一个测试，插入1亿条数据，每条数据占用500字节，含有3个二级索引，最终的结果如下所示：

![image-20230128133432470](/image-20230128133432470.png)

从上图可以看到插入1亿条数据有序UUID是最快的，而且在实际业务使用中有序UUID在 **业务端就可以生成** 。还可以进一步减少SQL的交互次数。

另外，虽然有序UUID相比自增ID多了8个字节，但实际只增大了3G的存储空间，还可以接受。

> 在当今的互联网环境中，非常不推荐自增ID作为主键的数据库设计。更推荐类似有序UUID的全局唯一的实现。
>
> 另外在真实的业务系统中，主键还可以加入业务和系统属性，如用户的尾号，机房的信息等。这样的主键设计就更为考验架构师的水平了。

##### 如果不是MySQL8.0 肿么办？

手动赋值字段做主键！

比如，设计各个分店的会员表的主键，因为如果每台机器各自产生的数据需要合并，就可能会出现主键重复的问题。

可以在总部 MySQL 数据库中，有一个管理信息表，在这个表中添加一个字段，专门用来记录当前会员编号的最大值。

门店在添加会员的时候，先到总部 MySQL 数据库中获取这个最大值，在这个基础上加 1，然后用这个值作为新会员的“id”，同时，更新总部 MySQL 数据库管理信息表中的当 前会员编号的最大值。

这样一来，各个门店添加会员的时候，都对同一个总部 MySQL 数据库中的数据表字段进 行操作，就解决了各门店添加会员时会员编号冲突的问题。

## 数据库的设计规范

### 为什么需要数据库设计

**我们在设计数据表的时候，要考虑很多问题**。比如：

- 用户都需要什么数据？需要在数据表中保存哪些数据？
- 如何保证数据表中数据的正确性，当插入、删除、更新的时候该进行怎样的**约束检查**?
- 如何降低数据表的**数据冗余度**，保证数据表不会因为用户量的增长而迅速扩张?
- 如何让负责数据库维护的人员**更方便**地使用数据库?
- 使用数据库的应用场景也各不相同，可以说针对不同的情况，设计出来的数据表可能**千差万别**。

**现实情况中，面临的场景**：

当数据库运行了一段时间之后，我们才发现数据表设计的有问题。重新调整数据表的结构，就需要做数据迁移，还有可能影响程序的业务逻辑，以及网站正常的访问。

**如果是糟糕的数据库设计可能会造成以下问题：**

- 数据冗余、信息重复，存储空间浪费
- 数据更新、插入、删除的异常
- 无法正确表示信息
- 丢失有效信息
- 程序性能差

**良好的数据库设计则有以下优点：**

- 节省数据的存储空间
- 能够保证数据的完整性
- 方便进行数据库应用系统的开发

总之，开始设置数据库的时候，我们就需要重视数据表的设计。为了建立**冗余较小、结构合理**的数据库，设计数据库时必须遵循一定的规则。

### 范式

#### 范式简介

**在关系型数据库中，关于数据表设计的基本原则、规则就称为范式**。可以理解为，一张数据表的设计结构需要满足的某种设计标准的`级别`。要想设计一个结构合理的关系型数据库，必须满足一定的范式。

范式的英文名称是Normal Form，简称NF。它是英国人E.F.Codd在上个世纪70年代提出关系数据库模型后总结出来的。范式是关系数据库理论的基础，也是我们在设计数据库结构过程中所要遵循的**规则**和**指导方法**。

#### 范式都包括哪些

目前关系型数据库有六种常见范式，按照范式级别，从低到高分别是:**第一范式(1NF) 、第二范式(2NF) 、第三范式(3NF) 、巴斯-科德范式(BCNF) 、第四范式(4NF) 和第五范式(5NF, 又称完美范式)**。

**数据库的范式设计越高阶，冗余度就越低**，同时高阶的范式-定符合低阶范式的要求， 满足最低要求的范式是第一范式(1NF) 。在第一范式的基础上进一步满足更多规范要求的称为第二范式(2NF) ，其余范式以次类推。

一般来说，在关系型数据库设计中，最高也就遵循到`BCNF`，普遍还是`3NF`。但也不绝对，有时候为了提高某些查询性能，我们还需要破坏范式规则，也就是`反规范化`。

![image-20230128133517384](/image-20230128133517384.png)

#### 键和相关属性的概念

范式的定义会使用到主键和候选键，数据库中的键(Key) 由一-个或者多个属性组成。数据表中常用的几种键和属性的定义：

- 超键：能唯一标识元组的**属性集**叫做超键。
- 候选键：如果超键不包括多余的属性，那么这个超键就是候选键。
- 主键：用户可以从候选键中选择一 个作为主键。
- 外键：如果数据表R1中的某属性集不是R1的主键，而是另一个数据表R2的主键，那么这个属性集就是数据表R1的外键。
- 主属性：包含在任一候选键中的属性称为主属性。
- 非主属性：与主属性相对，指的是不包含在任何一个候选键中的属性。

通常，我们也将候选键称之为“**码**”，把主键也称为“**主码**”。因为键可能是由多个属性组成的，针对单个属性，我们还可以用主属性和非主属性来进行区分。

**举例：**

这里有两个表:

```
球员表(player) :球员编号|姓名|身份证号|年龄|球队编号
球队表(team) :球队编号|主教练|球队所在地
```

- **超键**：对于球员表来说，超键就是包括球员编号或者身份证号的任意组合，比如(球员编号) (球员编号,姓名) (身份证号, 年龄)等。
- **候选键**：就是最小的超键，对于球员表来说，候选键就是(球员编号)或者(身份证号)。
- **主键**：我们自己选定，也就是从候选键中选择一个，比如(球员编号)。
- **外键：**球员表中的球队编号。
- **主属性、非主属性**：在球员表中，主属性是(球员编号) (身份证号)，其他的属性(姓名) (年龄) (球队编号)都是非主属性。

#### 第一范式

第一范式主要是确保数据表中每个字段的值必须具有**原子性**，也就是说数据表中每个字段的值为**不可再次拆分**的最小数据单元。

我们在设计某个字段的时候，对于字段X来说，不能把字段X拆分成字段X-1和字段X-2。事实上，任何的DBMS都会满足第一范式的要求，不会将字段进行拆分。

**举例**

user表的设计不符合第一范式

![image-20230128133538097](/image-20230128133538097.png)

其中user_info字段为用户信息，可以进一步拆分成更小粒度的字段，不符合数据库射击队第一范式的要求，将user_info拆分后如下：

![image-20230128133551485](/image-20230128133551485.png)

#### 第二范式

第二范式要求,在满足第一范式的基础上，还要**满足数据表里的每一条数据记录，都是可唯一标识的。 而且所有非主键字段，都必须完全依赖主键，不能只依赖主键的一部分。**如果知道主键的所有属性的值，就可以检索到任何元组(行)的任何属性的任何值。(要求中的主键，其实可以拓展替换为候选键)。

**举例**

成绩表(学号，课程号，成绩)关系中，(学号，课程号)可以决定成绩，但是学号不能决定成绩，课程号也不能决定成绩，所以“**(学号，课程号)→成绩**”就是完全依赖关系。

**举例**

比赛表player_game，里面包含球员编号、姓名、年龄、比赛编号、比赛时间和比赛场地等属性，这里候选键和主键都为(球员编号，比赛编号)，我们可以通过候选键(或主键) 来决定如下的关系：

```
(球员编号，比赛编号)→(姓名，年龄，比赛时间，化赛场地，得分)
```

但是这个数据表不满足第二范式，因为数据表中的字段之间还存在着如下的对应关系：

```
(球员编号)→(姓名,年龄)
(比赛编号)→(比赛时间,比赛场地)
```

对于非主属性来说，并非完全依赖候选键。这样会产生怎样的问题呢？

1. **数据冗余**：如果一个球员可以参加m场比费，那么球员的姓名和年龄就重复m-1次。一个比赛也可能会有n个球员参加，比赛的时间和地点就重复了n-1次。
2. **插入异常**：如果我们想要添加一场新的比赛，但是这时还没有确定参加的球员都有谁，那么就没法插入。
3. **删除异常**：如果我要删除某个球员编号，如果没有单独保存比赛表的话，就会同时把比赛信息删除掉。
4. **更新异常**：如果我们调整了某个比赛的时间，那么数据表中所有这个比赛的时间都需要进行调整， 否则就会出现一场比赛时间不同的情况。

为了避免上述情况，我们可以将表设计为下面三张表。

```
(球员编号)→(姓名,年龄)
(比赛编号)→(比赛时间,比赛场地)
(球员编号，比赛编号)→(得分)
```

> **1NF告诉我们字段属性需要是原子性的，而2NF告诉我们一张表就是一个独立的对象，一张表只表达一个意思。**
>
> **小结：**
>
> **第二范式(2NF) 要求实体的属性完全依赖主关键字。如果存在不完全依赖，那么这个属性和主关键字的这一部分应该分离出来形成一个新的实体，新实体与元实体之间是一对多的关系。**

#### 第三范式

第三范式是在第二范式的基础上，确保数据表中的每一个非主键字段都和主键字段直接相关， 也就是说，**要求数据表中的所有非主键字段不能依赖于其他非主键字段**。(即，不能存在非主属性A依赖于非主属性B，非主属性B依赖于主键C的情况，即存在“A→B→C“的决定关系)通俗地讲，该规则的意思是所有非主键属性之间不能有依赖关系，必须相互独立。

这里的主键可以拓展为候选键。

**举例**

**部门信息表**：每个部门有部门编号(dept_id) 、部门名称、部门简介等信息。

**员工信息表**：每个员工有员工编号、姓名、部门编号。列出部门编号后就不能再将部门名称、部门简介等与部门有关的信息再加入员工信息表中。

如果不存在部门信息表，则根据第三范式(3NF) 也应该构建它，否则就会有大量的数据冗余。

**举例**

![image-20230128133741635](/image-20230128133741635.png)

以上表不符合第三范式，但是符合第二范式。要想复合第三范式，应作出以下调整：

![image-20230128133813177](/image-20230128133813177.png)

![image-20230128133823076](/image-20230128133823076.png)

> 符合3NF后的数据模型通俗地讲，2NF和3NF通常以这句话概括：“每个非键属性依赖于键，依赖于整个键，并且除了键别无他物”。

#### 小结

关于数据表的设计，有三个范式要遵循。

1. 第一范式(1NF) ，**确保每列保持原子性**

   数据库的每一-列都是不可分割的原子数据项，不可再分的最小数据单元，而不能是集合、数组、记录等非原子数据项。

2. 第二范式(2NF) ，**确保每列都和主键完全依赖**

   尤其在复合主键的情况下，非主键部分不应该依赖于部分主键。

3. 第三范式(3NF) ，**确保每列都和主键列直接相关，而不是间接相关**

**范式的优点**：数据的标准化有助于消除数据库中的**数据冗余**，第三范式(3NF) 通常被认为在性能、扩展性和数据完整性方面达到了最好的平衡。

**范式的缺点**：范式的使用，可能**降低查询的效率**。因为范式等级越高，设计出来的数据表就越多、越精细，数据的冗余度就越低，进行数据查询的时候就可能需要关联多张表，这不但代价昂贵，也可能使一些索引策略无效。

范式只是提出了设计的标准，实际上设计数据表时，未必一定要符合这些标准。开发中，我们会出现为了性能和读取效率违反范式化的原则，通过增加少量的冗余或重复的数据来提高数据库的读性能，减少关联查询，join 表的次数，实现空间换取时间的目的。因此在实际的设计过程中要理论结合实际，灵活运用。

> **范式本身没有优劣之分，只有适用场景不同。没有完美的设计，只有合适的设计，我们在数据表的设计中，还需要根据需求将范式和反范式混合使用。**

### 反范式化

#### 概述

有的时候不能简单按照规范要求设计数据表，因为有的数据看似冗余，其实对业务来说十分重要。这个时候，我们就要遵循**业务优先**的原则，首先满足业务需求，再尽量减少冗余。

如果数据库中的数据量比较大，系统的UV和PV访问频次比较高，则完全按照MySQL的三大范式设计数据表，读数据时会产生大量的关联查询，在一定程度上会影响数据库的读性能。如果我们想对查询效率进行优化，反范式优化也是一种优化思路。此时，可以通过在数据表中增加冗余字段来提高数据库的读性能。

规范化vs性能

> 1. 为满足某种商业目标，数据库性能比规范化数据库更重要
> 2. 在数据规范化的同时，要综合考虑数据库的性能
> 3. 通过在给定的表中添加额外的字段，以大量减少需要从中搜索信息所需的时间
> 4. 通过在给定的表中插入计算列，以方便查询！

#### 应用举例

**举例**

员工的信息存储在 **employees 表** 中，部门信息存储在 **departments 表** 中。通过 employees 表中的 department_id 字段与 departments 表建立关联关系。如果要查询一个员工所在部门的名称：

```
select employee_id,department_name
from employees e join departments d 
on e.department_id = d.department_id;
```

如果经常需要进行这个操作，连接查询就会浪费很多时间。可以在 employees 表中增加一个冗余字段department_name，这样就不用每次都进行连接操作了。

**举例**

反范式化的 goods商品信息表 设计如下：

![image-20230128133834296](/image-20230128133834296.png)

**举例**

我们有 2 个表，分别是 商品流水表（atguigu.trans ）和 商品信息表 （atguigu.goodsinfo） 。商品流水表里有 400 万条流水记录，商品信息表里有 2000 条商品记录。

商品流水表：

![image-20230128133843038](/image-20230128133843038.png)

商品信息表：

![image-20230128133911394](/image-20230128133911394.png)

当我们老是要查商品名称，就要不停的将两个表连接起来，这样就降低了查询的效率。因此可以直接将商品名称加到商品流水表中。

新的商品流水表如下所示：

![image-20230128133919107](/image-20230128133919107.png)

**举例**

```
#学生表
CREATE TABLE student(
stu_id INT PRIMARY KEY AUTO_INCREMENT,
stu_name VARCHAR(25),
create_time DATETIME
);

#课程评论表
CREATE TABLE class_comment(
comment_id INT PRIMARY KEY AUTO_INCREMENT,
class_id INT,
comment_text VARCHAR(35),
comment_time DATETIME,
stu_id INT
);

###创建向学生表中添加数据的存储过程
DELIMITER //

CREATE PROCEDURE batch_insert_student(IN START INT(10), IN max_num INT(10))
BEGIN
DECLARE i INT DEFAULT 0;
DECLARE date_start DATETIME DEFAULT ('2017-01-01 00:00:00');
DECLARE date_temp DATETIME;
SET date_temp = date_start;
SET autocommit=0;
REPEAT
SET i=i+1;
SET date_temp = DATE_ADD(date_temp, INTERVAL RAND()*60 SECOND);
INSERT INTO student(stu_id, stu_name, create_time)
VALUES((START+i), CONCAT('stu_',i), date_temp);
UNTIL i = max_num
END REPEAT;
COMMIT;
END //

DELIMITER ;

#调用存储过程，学生id从10001开始，添加1000000数据
CALL batch_insert_student(10000,1000000);

####创建向课程评论表中添加数据的存储过程
DELIMITER //

CREATE PROCEDURE batch_insert_class_comments(IN START INT(10), IN max_num INT(10))
BEGIN
DECLARE i INT DEFAULT 0;
DECLARE date_start DATETIME DEFAULT ('2018-01-01 00:00:00');
DECLARE date_temp DATETIME;
DECLARE comment_text VARCHAR(25);
DECLARE stu_id INT;
SET date_temp = date_start;
SET autocommit=0;
REPEAT
SET i=i+1;
SET date_temp = DATE_ADD(date_temp, INTERVAL RAND()*60 SECOND);
SET comment_text = SUBSTR(MD5(RAND()),1, 20);
SET stu_id = FLOOR(RAND()*1000000);
INSERT INTO class_comment(comment_id, class_id, comment_text, comment_time, stu_id)
VALUES((START+i), 10001, comment_text, date_temp, stu_id);
UNTIL i = max_num
END REPEAT;
COMMIT;
END //

DELIMITER ;

#添加数据的存储过程的调用，一共1000000条记录
CALL batch_insert_class_comments(10000,1000000);

#########
SELECT COUNT(*) FROM student;

SELECT COUNT(*) FROM class_comment;

###需求######
SELECT p.comment_text, p.comment_time, stu.stu_name 
FROM class_comment AS p LEFT JOIN student AS stu 
ON p.stu_id = stu.stu_id 
WHERE p.class_id = 10001 
ORDER BY p.comment_id DESC 
LIMIT 10000;


#####进行反范式化的设计######
#表的复制
CREATE TABLE class_comment1
AS
SELECT * FROM class_comment;

#添加主键，保证class_comment1 与class_comment的结构相同
ALTER TABLE class_comment1
ADD PRIMARY KEY (comment_id);

SHOW INDEX FROM class_comment1;

#向课程评论表中增加stu_name字段
ALTER TABLE class_comment1
ADD stu_name VARCHAR(25);

#给新添加的字段赋值
UPDATE class_comment1 c
SET stu_name = (
SELECT stu_name
FROM student s
WHERE c.stu_id = s.stu_id
);

#查询同样的需求
SELECT comment_text, comment_time, stu_name 
FROM class_comment1 
WHERE class_id = 10001 
ORDER BY comment_id DESC 
LIMIT 10000;
```

两个查询语句对比：

```
#范式化
SELECT p.comment_text, p.comment_time, stu.stu_name 
FROM class_comment AS p LEFT JOIN student AS stu 
ON p.stu_id = stu.stu_id 
WHERE p.class_id = 10001 
ORDER BY p.comment_id DESC 
LIMIT 10000;
#0.100sec

#反范式化
SELECT comment_text, comment_time, stu_name 
FROM class_comment1 
WHERE class_id = 10001 
ORDER BY comment_id DESC 
LIMIT 10000;
#0.036sec
```

反范式化所花费的时间小于范式化所花费的时间

#### 反范式的新问题

反范式可以**通过空间换时间**，提升查询的效率，但是反范式也会带来一些新问题：

- 存储**空间变大**了
- 一个表中字段做了修改，另一个表中冗余的字段也需要做同步修改，否则**数据不一致**
- 若采用存储过程来支持数据的更新、删除等额外操作，如果更新频繁，会非常**消耗系统资源**
- 在**数据量小**的情况下，反范式不能体现性能的优势，可能还会让数据库的设计更加复杂

#### 反范式的使用场景

当冗余信息有价值或者能**大幅度提高查询效率**的时候，我们才会采取反范式的优化。

##### 增加冗余字段的建议

增加冗余字段-定要符合如下两个条件。只有满足这两个条件，才可以考虑增加冗余字段。

1. 这个冗余字段**不需要经常进行修改**；
2. 这个冗余字段**查询的时候不可或缺**。

##### 历史快照、历史数据的需要

在现实生活中，我们经常需要一些冗余信息，比如订单中的收货人信息，包括姓名、电话和地址等。每次发生的 **订单收货信息** 都属于 **历史快照** ，需要进行保存，但用户可以随时修改自己的信息，这时保存这些冗余信息是非常有必要的。

反范式优化也常用在 **数据仓库** 的设计中，因为数据仓库通常 **存储历史数据** ，对增删改的实时性要求不强，对历史数据的分析需求强。这时适当允许数据的冗余度，更方便进行数据分析。

我简单总结下数据仓库和数据库在使用上的区别：

1. 数据库设计的目的在于**捕获数据**，而数据仓库设计的目的在于**分析数据**；
2. 数据库对数据的**增删改实时性**要求强，需要存储在线的用户数据，而数据仓库存储的一般是**历史数据**；
3. 数据库设计需要**尽量避免冗余**，但为了提高查询效率也允许一定的**冗余度**，而数据仓库在设计上更偏向采用反范式设计。

### BCNF（巴斯范式）

人们在3NF的基础上进行了改进，提出了**巴斯范式**(BCNF) ，**也叫做巴斯-科德范式**(Boyce-Codd NormalForm)。BCNF被认为没有新的设计规范加入，只是对第三范式中设计规范要求更强，使得数据库冗余度更小。所以，称为是**修正的第三范式**，或**扩充的第三范式**，BCNF不被称为第四范式。

若一个关系达到了第三范式，并且它只有一个候选键，或者它的每个候选键都是单属性则该关系自然达到BC范式。

一般来说，一个数据库设计符合3NF或BCNF就可以了。

**1. 案例**

我们分析如下表的范式情况：

![image-20230128133953598](/image-20230128133953598.png)

在这个表中，一个仓库只有一个管理员，同时一个管理员也只管理一个仓库。我们先来梳理下这些属性之间的依赖关系。

仓库名决定了管理员，管理员也决定了仓库名，同时（仓库名，物品名）的属性集合可以决定数量这个属性。这样，我们就可以找到数据表的候选键。

**候选键** ：是（管理员，物品名）和（仓库名，物品名），然后我们从候选键中选择一个作为 主键 ，比如（仓库名，物品名）。

**主属性** ：包含在任一候选键中的属性，也就是仓库名，管理员和物品名。

**非主属性** ：数量这个属性。

**2. 是否符合三范式**

如何判断一张表的范式呢？我们需要根据范式的等级，从低到高来进行判断。

首先，数据表每个属性都是原子性的，符合 1NF 的要求；

其次，数据表中非主属性”数量“都与候选键全部依赖，（仓库名，物品名）决定数量，（管理员，物品名）决定数量。因此，数据表符合 2NF 的要求；

最后，数据表中的非主属性，不传递依赖于候选键。因此符合 3NF 的要求。

**3. 存在的问题**

既然数据表已经符合了 3NF 的要求，是不是就不存在问题了呢？我们来看下面的情况：

1. 增加一个仓库，但是还没有存放任何物品。根据数据表实体完整性的要求，主键不能有空值，因此会出现 **插入异常** ； 
2. 如果仓库更换了管理员，我们就可能会 **修改数据表中的多条记录**；
3. 如果仓库里的商品都卖空了，那么此时仓库名称和相应的管理员名称也会随之被删除。

你能看到，即便数据表符合 3NF 的要求，同样可能存在插入，更新和删除数据的异常情况。

**4.问题解决**

首先我们需要确认造成异常的原因：主属性仓库名对于候选键（管理员，物品名）是部分依赖的关系，这样就有可能导致上面的异常情况。因此引入BCNF，**它在 3NF 的基础上消除了主属性对候选键的部分依赖或者传递依赖关系**。

- 如果在关系R中，U为主键，A属性是主键的一个属性，若存在A -> Y，Y为主属性，则该关系不属于BCNF。

根据 BCNF 的要求，我们需要把仓库管理关系 warehouse_keeper 表拆分成下面这样：

```
仓库表 ：（仓库名，管理员）
库存表 ：（仓库名，物品名，数量）
```

这样就不存在主属性对于候选键的部分依赖或传递依赖，上面数据表的设计就符合 BCNF。

**再举例**：

有一个 学生导师表 ，其中包含字段：学生ID，专业，导师，专业GPA，这其中学生ID和专业是联合主键。

![image-20230128134017995](/image-20230128134017995.png)

这个表的设计满足三范式，但是这里存在另一个依赖关系，“专业”依赖于“导师”，也就是说每个导师只做一个专业方面的导师，只要知道了是哪个导师，我们自然就知道是哪个专业的了。

所以这个表的部分主键Major依赖于非主键属性Advisor，那么我们可以进行以下的调整，拆分成2个表：

学生导师表：

![image-20230128134041963](/image-20230128134041963.png)

导师表：

![image-20230128134050280](/image-20230128134050280.png)

### 第四范式

必须一对多，不能一对>2的多

**多值依赖的概念：**

- **多值依赖**即属性之间的一对多关系，记为K→→A。
- **函数依赖**事实上是单值依赖，所以不能表达属性值之间的一对多关系。
- **平凡的多值依赖**：全集U=K+A，一个k可以对应于多个A，即K→→A。 此时整个表就是一组一对多关系。
- **非平凡的多值依赖**：全集U=K+A+B，一个K可以对应于多个A，也可以对应于多个B，A与B互相独立，即K→→A，K→→B。整个表有多组一对多关系，且有：“一”部分是相同的属性集合，“多”部分是互相独立的属性集合。

第四范式即在满足巴斯科德范式(BCNF) 的基础上，消除非平凡且非函数依赖的多值依赖，**只保留了平凡的多值依赖**（即**把同一表内的多对多关系删除**）。

**举例1：**

职工表（职工编号，职工孩子姓名，职工选修课程）。

在这个表中，同一个职工可能会有多个职工孩子姓名。同样，同一个职工也可能会有多个职工选修课程，即这里存在着多值事实，不符合第四范式。

如果要符合第四范式，只需要将上表分为两个表，使它们只有一个多值事实。

例如： 

职工表一 (职工编号，职工孩子姓名)， 

职工表二 (职工编号，职工选修课程)，

两个表都只有一个多值事实，所以符合第四范式。

**举例2：**

比如我们建立课程、教师、教材的模型。我们规定，每门课程有对应的一组教师，每门课程也有对应的一组教材，一门课程使用的教材和教师没有关系。我们建立的关系表如下：

课程ID，教师ID，教材ID；这三列作为联合主键。

为了表述方便，我们用Name代替ID，这样更容易看懂：

![image-20230128134101268](/image-20230128134101268.png)

这个表除了主键，就没有其他字段了，所以肯定满足BC范式，但是却存在 **多值依赖** 导致的异常。

假如我们下学期想采用一本新的英版高数教材，但是还没确定具体哪个老师来教，那么我们就无法在这个表中维护Course高数和Book英版高数教材的的关系。

解决办法是我们把这个多值依赖的表拆解成2个表，分别建立关系。

课程-老师表：

![image-20230128134110396](/image-20230128134110396.png)

课程-教材表：

![image-20230128134122886](/image-20230128134122886.png)

### 第五范式，域键范式

除了第四范式外，我们还有更高级的第五范式（又称完美范式）和域键范式（DKNF）。

在满足第四范式（4NF）的基础上，消除不是由候选键所蕴含的连接依赖。**如果关系模式R中的每一个连接依赖均由R的候选键所隐含，则称此关系模式符合第五范式。**

函数依赖是多值依赖的一种特殊的情况，而多值依赖实际上是连接依赖的一种特殊情况。但连接依赖不像函数依赖和多值依赖可以由 **语义直接导出** ，而是在 **关系连接运算** 时才反映出来。存在连接依赖的关系模式仍可能遇到数据冗余及插入、修改、删除异常等问题。

第五范式处理的是 **无损连接问题** ，这个范式基本 **没有实际意义** ，因为无损连接很少出现，而且难以察觉。而域键范式试图定义一个 **终极范式** ，该范式考虑所有的依赖和约束类型，但是实用价值也是最小的，只存在理论研究中。

### 实战案例

商超进货系统中的进货单表进行剖析：

进货单表：

![image-20230128134135365](/image-20230128134135365.png)

> 在实际工作场景中，这种由于数据表结构设计不合理，而导致的数据重复的现象并不少见。往往是系统虽然能够运行，承载能力却很差，稍微有点流量，就会出现内存不足、CPU 使用率飙升的情况，甚至会导致整个项目失败。

#### 1NF

第一范式要求：**所有的字段都是基本数据字段，不可进一步拆分**。 这里需要确认，所有的列中，每个字段只包含一种数据。

这张表里，我们把“property”这一字段，拆分成“specification （规格）”和“unit（单位）”，这2个字段如下：

![image-20230128134151985](/image-20230128134151985.png)

#### 2NF

第二范式要求.在满足第一范式的基础上，“**还要满足数据表里的每-条数据记录，都是可唯一标识的。而且所有字段，都必须完全依赖主键，不能只依赖主键的一部分**”。

1. 确定主键
2. 确定主键后，判断哪些字段完全依赖主键，哪些职以来主键的一部分。把只依赖于主键一部分的字段拆分出去，形成新的数据表。

**进货单头表**

![image-20230128134209604](/image-20230128134209604.png)

**进货明细表**

![image-20230128134217209](/image-20230128134217209.png)

**商品信息表**

![image-20230128134230887](/image-20230128134230887.png)

现在3个表都满足了第二范式的要求。

1. 商品信息表中条码可能会重复，整张表中所有的字段都不能唯一标识表里的记录。因此我们必须给这个表加上一个主键，比如一个自增字段‘itemnumber’。并对‘进货明细表’、‘商品信息表’进行调整。

![image-20230128134247958](/image-20230128134247958.png)

![image-20230128134255678](/image-20230128134255678.png)

#### 3NF

进货单头表中，表单号和供应商编号与后面的供应商名称和仓库

供货商表

![image-20230128134304420](/image-20230128134304420.png)

进货单头表

![image-20230128134318172](/image-20230128134318172.png)

#### 反范式化：业务优先的原则

所谓的业务优先原则，就是指一切以业务需求为主,技术服务于业务。**完全按照理论的设计不一定就是最优， 还要根据实际情况来决定**。这里我们就来分析一下不同选择的利与弊。 

进货明细表中，总金额=数量*单价，以范式的准则来判断，是完全冗余的字段。但是实际业务需求中，可能出现打折、除不尽、省略等情况。因此，从业务需求的角度考虑不必拆分。

最终，进货单表被拆分为以下四个表：

![image-20230128134347119](/image-20230128134347119.png)

### ER模型

数据库设计是牵一发而动全身的。那有没有什么办法提前看到数据库的全貌呢?比如需要哪些数据表、数据表中应该有哪些字段，数据表与数据表之间有什么关系、通过什么字段进行连接，等等。这样我们才能进行整体的梳理和设计。

其实，ER模型就是一个这样的工具。 ER模型也叫作**实体关系模型**，是用来描述现实生活中客观存在的事物、事物的属性，以及事物之间关系的一种数据模型。**在开发基于数据库的信息系统的设计阶段，通常使用ER模型来描述信息需求和信息特性，帮助我们理清业务逻辑，从而设计出优秀的数据库。**

#### ER模型包括哪些元素？

**ER模型中有三个要素，分别是实体、属性和关系。**

**实体**，可以看做是数据对象，往往对应于现实生活中的真实存在的个体。在ER模型中，用**矩形**来表示。实体分为两类，分别是**强实体**和**弱实体**。

强实体是指不依赖于其他实体的实体；

弱实体是指对另一个实体有很强的依赖关系的实体。

**属性**，则是指实体的特性。比如超市的地址、联系电话、员工数等。在ER模型中用**椭圆形**来表示。

**关系**，则是指实体之间的联系。比如超市把商品卖给顾客,就是一-种超市与顾客之间的联系。在ER模型中用**菱形**来表示。

注意：实体和属性不容易区分。这里提供一个原则：我们要从系统整体的角度出发去看，**可以独立存在的是实体，不可再分的是属性**。也就是说，属性不能包含其他属性。

#### 关系的类型

在 ER 模型的 3 个要素中，关系又可以分为 3 种类型，分别是 **一对一、一对多、多对多**。

**一对一** ：指实体之间的关系是一一对应的，比如个人与身份证信息之间的关系就是一对一的关系。一个人只能有一个身份证信息，一个身份证信息也只属于一个人。

**一对多** ：指一边的实体通过关系，可以对应多个另外一边的实体。相反，另外一边的实体通过这个关系，则只能对应唯一的一边的实体。比如说，我们新建一个班级表，而每个班级都有多个学生，每个学生则对应一个班级，班级对学生就是一对多的关系。

**多对多** ：指关系两边的实体都可以通过关系对应多个对方的实体。比如在进货模块中，供货商与超市之间的关系就是多对多的关系，一个供货商可以给多个超市供货，一个超市也可以从多个供货商那里采购商品。再比如一个选课表，有许多科目，每个科目有很多学生选，而每个学生又可以选择多个科目，这就是多对多的关系。

#### 建模分析

ER 模型看起来比较麻烦，但是对我们把控项目整体非常重要。如果你只是开发一个小应用，或许简单设计几个表够用了，一旦要设计有一定规模的应用，在项目的初始阶段，建立完整的 ER 模型就非常关键了。开发应用项目的实质，其实就是 **建模** 。

我们设计的案例是 **电商业务** ，由于电商业务太过庞大且复杂，所以我们做了业务简化，比如针对SKU（StockKeepingUnit，库存量单位）和SPU（Standard Product Unit，标准化产品单元）的含义上，我们直接使用了SKU，并没有提及SPU的概念。本次电商业务设计总共有8个实体，如下所示。

- 地址实体
- 用户实体
- 购物车实体
- 评论实体
- 商品实体
- 商品分类实体
- 订单实体
- 订单详情实体

其中， **用户** 和 **商品分类** 是强实体，因为它们不需要依赖其他任何实体。而其他属于弱实体，因为它们虽然都可以独立存在，但是它们都依赖用户这个实体，因此都是弱实体。知道了这些要素，我们就可以给电商业务创建 ER 模型了，如图：

![image-20230128134402812](/image-20230128134402812.png)

在这个图中，地址和用户之间的添加关系，是一对多的关系，而商品和商品详情示一对1的关系，商品和订单是多对多的关系。 这个 ER 模型，包括了 8个实体之间的 8种关系。

（1）用户可以在电商平台添加多个地址；

（2）用户只能拥有一个购物车；

（3）用户可以生成多个订单；

（4）用户可以发表多条评论；

（5）一件商品可以有多条评论；

（6）每一个商品分类包含多种商品；

（7）一个订单可以包含多个商品，一个商品可以在多个订单里。

（8）订单中又包含多个订单详情，因为一个订单中可能包含不同种类的商品

#### ER模型的细化

有了这个 ER 模型，我们就可以从整体上 理解 电商的业务了。刚刚的 ER 模型展示了电商业务的框架，但是只包括了订单，地址，用户，购物车，评论，商品，商品分类和订单详情这八个实体，以及它们之间的关系，还不能对应到具体的表，以及表与表之间的关联。我们需要把 **属性加上** ，用 **椭圆** 来表示，这样我们得到的 ER 模型就更加完整了。

因此，我们需要进一步去设计一下这个 ER 模型的各个局部，也就是细化下电商的具体业务流程，然后把它们综合到一起，形成一个完整的 ER 模型。这样可以帮助我们理清数据库的设计思路。

接下来，我们再分析一下各个实体都有哪些属性，如下所示。

（1） **地址实体** 包括用户编号、省、市、地区、收件人、联系电话、是否是默认地址。

（2） **用户实体** 包括用户编号、用户名称、昵称、用户密码、手机号、邮箱、头像、用户级别。

（3） **购物车实体** 包括购物车编号、用户编号、商品编号、商品数量、图片文件url。

（4） **订单实体** 包括订单编号、收货人、收件人电话、总金额、用户编号、付款方式、送货地址、下单

时间。

（5） **订单详情实体** 包括订单详情编号、订单编号、商品名称、商品编号、商品数量。

（6） **商品实体** 包括商品编号、价格、商品名称、分类编号、是否销售，规格、颜色。

（7） **评论实体** 包括评论id、评论内容、评论时间、用户编号、商品编号

（8） **商品分类实体** 包括类别编号、类别名称、父类别编号

这样细分之后，我们就可以重新设计电商业务了，ER 模型如图：

![image-20230128134420505](/image-20230128134420505.png)

#### ER模型图转换成数据表

通过绘制 ER 模型，我们已经理清了业务逻辑，现在，我们就要进行非常重要的一步了：把绘制好的 ER模型，转换成具体的数据表，下面介绍下转换的原则：

（1）一个 **实体** 通常转换成一个 **数据表** ； 

（2）一个 **多对多的关系** ，通常也转换成一个 **数据表** ； 

（3）一个 **1 对 1** ，或者 **1 对多** 的关系，往往通过表的 **外键** 来表达，而不是设计一个新的数据表；

（4） **属性** 转换成表的 **字段** 。

下面结合前面的ER模型，具体讲解一下怎么运用这些转换的原则，把 ER 模型转换成具体的数据表，从而把抽象出来的数据模型，落实到具体的数据库设计当中。

##### 一个实体转换成一个数据表

**强实体转换成数据表：**

**用户实体**转换成用户表(user_info) 的代码如下所示。

```
CREATE TABLE `user_info`(
    `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '编号',
	`user_name` varchar (200) DEFAULT NULL COMMENT '用户名称',
	`nick_name` varchar (200) DEFAULT NULL COMMENT '用户呢称',
	`passwd` varchar (200) DEFAULT NULL COMMENT '用户密码' ,
	`phone_num` varchar (200) DEFAULT NULL COMMENT '手机号',
	`email` varchar(200) DEFAULT NULL COMMENT '邮箱',
	`head_img` varchar(200) DEFAULT NULL COMMENT '头像',
	`user_level` varchar(200) DEFAULT NULL COMMENT '用户级别',
PRIMARY KEY (`id`)
)ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8 COMMENT='用户表';
```

**商品分类实体**转换成商品分类表(base_category) ，由于商品分类可以有一级分类和二级分类，比如一级分类有家居、手机等等分类，二级分类可以根据手机的一级分类分为手机配件，运营商等，这里我们把商品分类实体规划为两张表，分别是**一级分类表**和**二级分类表**，之所以这么规划是因为一级分类和二级分类都是有限的，存储为两张表业务结构更加清晰。

```
#一级分类表
CREATE TABLE `base_category1` (
	`id` bigint(20) NOT NULL AUTO_ INCREMENT COMMENT '编号',
	`name` varchar(10) NOT NULL COMMENT ' 分类名称',
	 PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='一级分类表';

#二级分类表
CREATE TABLE `base_category2` (
	`id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '编号',
	`name` varchar(200) NOT NULL COMMENT '二级分类名称',
	`category1_id` bigint(20) DEFAULT NULL COMMENT '一级分类编号',
	PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='二级分类表';
```

那么如果规划为一张表呢，表结构如下所示。

```
CREATE TABLE `base_category` (
	`id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '编号',
	`name` varchar(200) NOT NULL COMMENT '分类名称',
	`category_parent_id` bigint(20) DEFAULT NULL COMMENT '父分类编号',
	PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='分类表';
```

如果这样分类的话，那么查询一级分类时候，就需要判断父分类编号是否为空，但是如果插入二级分类的时候也是空，就容易造成**业务数据混乱**。而且查询二级分类的时候IS NOT NULL条件是无法使用到索引的。同时，这样的设计也不符合第二范式(因为父分类编号并不依赖分类编号ID；因为父分类编号可以有很多数据为NULL)，所以需要进行表的拆分。因此无论是**业务需求**还是**数据库表的规范**来看都应该拆分为两张表。

**弱实体转换成数据表：**

**地址实体**转换成地址表(user_ address) ，如下所示。

```
CREATE TABLE `user_address`(
	`id` bigint(20) NOT NULL AUTO_ INCREMENT COMMENT ' 编号',
	`province`varchar( 500) DEFAULT NULL COMMENT ' 省',
	`city` varchar (500) DEFAULT NULL COMMENT ' 市',
	`user_address` varchar(500) DEFAULT NULL COMMENT '具体地址',
	`user_id` bigint(20) DEFAULT NULL COMMENT '用户id',
	`consignee` varchar(40) DEFAULT NULL COMMENT '收件人',
	`phone_num` varchar(40) DEFAULT NULL COMMENT '联系方式',
	`is_default` varchar(1) DEFAULT NULL COMMENT ' 是否是默认',
	PRIMARY KEY (`id`)
)ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='用户地址表';
```

**订单实体**转换成订单表(order_info) ，如下所示，实际业务中订单的信息会非常多，我们这里做了简化。

```
CREATE TABLE `order_info`(
	`id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '编号',
	`consignee` varchar(100) DEFAULT NULL COMMENT '收货人',
	`consignee_tel` varchar(20) DEFAULT NULL COMMENT '收件人电话',
	`total_amount` decimal(10,2) DEFAULT NULL COMMENT '总金额',
	`user_id` bigint(20) DEFAULT NULL COMMENT '用户id',
	`payment_way` varchar(20) DEFAULT NULL COMMENT '付款方式',
	`delivery_address` varchar(1000) DEFAULT NULL COMMENT '送货地址',
	`create_time` datetime DEFAULT NULL COMMENT '下单时间',
	PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='订单表';
```

**让单详情实体**转换成订单详情表(order_ detail)，如下所示。(用于体现多对多关系的， 见下节)

```
CREATE TABLE `order_detail`(
	`id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '订单详情编号',
	`order_id` bigint (20) DEFAULT NULL COMMENT '订单编号',
	`sku_id` bigint(20) DEFAULT NULL COMMENT 'sku_id',
	`sku_name` varchar(200) DEFAULT NULL COMMENT 'sku名称',
	`sku_num` varchar(200) DEFAULT NULL COMMENT' 购买个数',
	`create_time`datetime DEFAULT NULL COMMENT '操作时间',
	PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='订单明细表';
```

**购物车实体**转换成购物车表(cart_info) ，如下所示。

```
CREATE TABLE `cart_info` (
	`cart_id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '编号',
	`user_id` varchar (200) DEFAULT NULL COMMENT '用户id',
	`sku_id` bigint(20) DEFAULT NULL COMMENT 'skuid',
	`sku_num` int(11) DEFAULT NULL COMMENT '数量',
	`img_url`varchar (500) DEFAULT NULL COMMENT '图片文件',
	PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='购物车表';
```

**评论实体**转换成评论表(members) ，如下所示。

```
CREATE TABLE `sku_comments` (
	`comment_id` bigint(20) NOT NULL AUTO_ INCREMENT COMMENT '评论编号',
	`user_id` bigint(20) DEFAULT NULL COMMENT '用户编号',
	`sku_id` decimal(10, 0) DEFAULT NULL COMMENT '商品编号',
	`comment` varchar(2000) DEFAULT NULL COMMENT '评论内容',
	`create_time` datetime DEFAULT NULL COMMENT '评论时间',
	PRIMARY KEY (`id`) USING BTREE
) ENGINE= InnoDB AUTO_INCREMENT=45 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='商品评论表' ;
```

**商品实体**转换成商品表(members) ，如下所示。

```
CREATE TABLE `sku_info`(
 	`sku_id` Bigint(20) NOT NULL AUTO_ INCREMENT COMMENT '商品编号(itemID)',
	`prloei` decimal(10,0) DEFAULT NULL COMMENT '价格',
	`sku_name` varchar(200) DEFAULT NULL COMMENT 'sku名称',
	`sku_desa` varchar (2000) DEFAULT NULL COMMENT '商品 规格描述',
	`cotegory3_id` bigint(20) DEFAULT NULL COMMENT '三级分类id (冗余)',
	`color` varchar(2000) DEFAULT NULL COMMENT '颜色',
	`is_sale` tinyint(3) NOT NULL DEFAULT '0' COMMENT '是否销售(1:是0:否)',
PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=45 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='商品表';
```

##### 一个多对多的关系转换成一个数据表

这个ER模型中的多对多的关系有1个，即**商品**和**订单**之间的关系，同品类的商品可以出现在不同的订单中，不同的订单也可以包含同-类型的商品，所以它们之间的关系是多对多。针对这种情况需要设计一个独立的表来表示，这种表一般称为**中间表**。

我们可以设计一个独立的**订单详情表**，来代表商品和订单之间的包含关系。这个表关联到2个实体，分别是订单、商品。所以，表中必须要包括这2个实体转换成的表的主键。除此之外，我们还要包括该关系自有的属性：商品数量，商品下单价格以及商品名称。

```
#订单详情表
CREATE TABLE order `detail` (
	`id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '订单详情编号',
    `order_id` bigint(20) DEFAULT NULL COMMENT '订单编号',
	`sku_id` bigint(20) DEFAULT NULL COMMENT 'sku_id',
	`sku_name` varchar(200) DEFAULT NULL COMMENT 'sku名称',
    `sku_num` varchar(200) DEFAULT NULL COMMENT '购买个数',
	`create_time` datetime DEFAULT NULL COMMENT '操作时间',
    PRIMARY KEY (id、) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='订单明细表’;
```

##### 通过外键来表达一对多的关系

在上面的表的设计中，我们可以用外键来表达1对多的关系。比如在商品评论表sku_comments中，我们分别把user_id、sku_id定义成外键，以使用下面的语句设置外键。

```
CONSTRAINT fk_comment_user FOREIGN KEY (user_id) REFERENCES user_info (id),
CONSTRAINT fk_comment_sku FOREIGN KEY ( sku_id) REFERENCES sku_info (sku_id)
```

外键约束主要是在数据库层面上**保证数据的一致性** ，但是因为插入和更新数据需要检查外键，理论上性能会有所下降，对性能是负面的影响。

实际的项目，不建议使用外键，一方面是**降低开发的复杂度**(有外键的话主 从表类的操作必须先操作主表)，另外是有**外键在处理数据的时候非常麻烦**。在电商平台，由于**并发业务量比较大**，所以一般不设置外键，以免影响数据库性能。

在应用层面做数据的**一致性检查** ，本来就是一个正常的功能需求。如学生选课的场景，课程肯定不是输入的，而是通过下拉或查找等方式从系统中进行选取，就能够保证是合法的课程ID，因此就不需要靠数据库的外键来检查了。

##### 把属性转换成表的字段

在刚刚的设计中，我们也完成了把属性都转换成了表的字段，比如把商品属性转换成了商品信息表中的字段。

```
CREATE TABLE `sku_info` (
	`sku_id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '商品编号(itemID)',
	`price` decimal(10,0) DEFAULT NULL COMMENT '价格',
	`sku_name` varchar(200) DEFAULT NULL COMMENT 'sku名称',
	`sku_desc` varchar(2000) DEFAULT NULL COMMENT '商品规格描述',
	`category3_id` bigint(20) DEFAULT NULL COMMENT '三级分类id(冗余)',
	`color` varchar (2000) DEFAULT NULL COMMENT '颜色',
	`is_sale` tinyint(3) NOT NULL DEFAULT '0' COMMENT '是否销售(1:是0:否) ',
	PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=45 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='商品表' ;
```

到这里，我们通过创建**电商项目业务流程**的ER模型，再把ER模型转换成具体的数据表的过程，完成了利用ER模型设计电商项目数据库的工作。

其实，任何一个基于数据库的应用项目，都可以通过这种 **先建立 ER 模型** ，再 **转换成数据表** 的方式，完成数据库的设计工作。创建 ER 模型不是目的，目的是把业务逻辑梳理清楚，设计出优秀的数据库。我建议你不是为了建模而建模，要利用创建 ER 模型的过程来整理思路，这样创建 ER 模型才有意义。

![image-20230128134441397](/image-20230128134441397.png)

### 数据表的设计原则

综合以上内容，总结出数据表设计的一般原则："三少一多" 

1. **数据表的个数越少越好** 

   RDBMS的核心在于对实体和联系的定义，也就是E-R图(Entity Relationship Diagram)，数据表越少，证明实体和联系设计得越简洁，既方便理解又方便操作。

2. **数据表中的字段个数越少越好**

   字段个数越多，数据冗余的可能性越大。设置字段个数少的前提是各个字段相互独立，而不是某个字段的取值可以由其他字段计算出来。当然字段个数少是相对的，我们通常会在**数据冗余**和**检索效率**中进行平衡。

3. **数据表中联合主键的字段个数越少越好**

   设置主键是为了确定唯一性，当一个字段无法确定唯一性的时候， 就需要采用联合主键的方式(也就是用多个字段来定义一个主键)。联合主键中的字段越多，**占用的索引空间越大** ，不仅会加大理解难度，还会增加运行时间和索引空间，因此联合主键的字段个数越少越好。

4. **使用主键和外键越多越好**

   数据库的设计实际上就是定义各种表，以及各种字段之间的关系。这些关系越多，证明这些实体之间的冗余度越低，**利用度越高**。 这样做的好处在于不仅保证了数据表之间的**独立性**，还能提升相互之间的关联使用率。

“三少一多”原则的核心就是**简单可复用**。简单指的是用更少的表、更少的字段、更少的联合主键字段来完成数据表的设计。可复用则是通过主键、外键的使用来增强数据表之间的复用率。因为一个主键可以理解是一张表的代表。键设计得越多，证明它们之间的利用率越高。

> 注意：这个原则并不是绝对的，有时候我们需要牺牲数据的冗余度来换取数据处理的效率。

### 数据库对象编写的建议

#### 关于库

1. 【强制】库的名称必须控制在32个字符以内，只能使用英文字母、数字和下划线，建议以英文字 母开头。
2. 【强制】库名中英文 **一律小写** ，不同单词采用 下划线 **分割**。须见名知意。
3. 【强制】库的名称格式：业务系统名称_子系统名。
4. 【强制】库名禁止使用关键字（如type,order等）。
5. 【强制】创建数据库时必须 **显式指定字符集** ，并且字符集只能是utf8或者utf8mb4。

  创建数据库SQL举例：CREATE DATABASE crm_fund **DEFAULT CHARACTER SET 'utf8'** ; 

1. 【建议】对于程序连接数据库账号，遵循 **权限最小原则**

  使用数据库账号只能在一个DB下使用，不准跨库。程序使用的账号 **原则上不准有drop权限** 。

1. 【建议】临时库以 **tmp_** 为前缀，并以日期为后缀；

  				备份库以 **bak_** 为前缀，并以日期为后缀。

#### 关于表、列

1. 【强制】表和列的名称必须控制在32个字符以内，表名只能使用英文字母、数字和下划线，建议 以 **英文字母开头** 。
2. 【强制】 **表名、列名一律小写** ，不同单词采用下划线分割。须见名知意。
3. 【强制】表名要求有模块名强相关，同一模块的表名尽量使用 **统一前缀** 。比如：crm_fund_item 
4. 【强制】创建表时必须 **显式指定字符集** 为utf8或utf8mb4。 
5. 【强制】表名、列名禁止使用关键字（如type,order等）。
6. 【强制】创建表时必须 **显式指定表存储引擎** 类型。如无特殊需求，一律为InnoDB。
7. 【强制】建表必须有comment。  
8. 【强制】字段命名应尽可能使用表达实际含义的英文单词或 **缩写** 。如：公司 ID，不要使用 corporation_id, 而用corp_id 即可。
9. 【强制】布尔值类型的字段命名为 **is_描述** 。如member表上表示是否为enabled的会员的字段命名为 is_enabled。 
10. 【强制】禁止在数据库中存储图片、文件等大的二进制数据

通常文件很大，短时间内造成数据量快速增长，数据库进行数据库读取时，通常会进行大量的随  机IO操作，文件很大时，IO操作很耗时。通常存储于文件服务器，数据库只存储文件地址信息。

1. 【建议】建表时关于主键： **表必须有主键**

   1. 强制要求主键为id，类型为int或bigint，且为auto_increment 建议使用unsigned无符号型。
   2. 标识表里每一行主体的字段不要设为主键，建议设为其他字段如user_id，order_id等，并建立unique key索引。因为如果设为主键且主键值为随机插入，则会导致innodb内部页分裂和大量随机I/O，性能下降。

2. 【建议】核心表（如用户表）必须有行数据的 **创建时间字段** （create_time）和 **最后更新时间字段** （update_time），便于查问题。

3. 【建议】表中所有字段尽量都是 **NOT NULL** 属性，业务可以根据需要定义 **DEFAULT值** 。 因为使用NULL值会存在每一行都会占用额外存储空间、数据迁移容易出错、聚合函数计算结果偏差等问题。

4. 【建议】所有存储相同数据的 **列名和列类型必须一致** （一般作为关联列，如果查询时关联列类型不一致会自动进行数据类型隐式转换，会造成列上的索引失效，导致查询效率降低）。

5. 【建议】中间表（或临时表）用于保留中间结果集，名称以 **tmp_** 开头。

   备份表用于备份或抓取源表快照，名称以 **bak_** 开头。中间表和备份表定期清理。

6. 【示范】一个较为规范的建表语句：

   ```
   CREATE TABLE user_info
   (
       `id`                 int unsigned NOT NULL AUTO_INCREMENT COMMENT '自增主键',
       `user_id`            bigint(11)   NOT NULL COMMENT '用户id',
       `username`           varchar(45)  NOT NULL COMMENT '真实姓名',
       `email`              varchar(30)  NOT NULL COMMENT '用户邮箱',
       `nickname`           varchar(45)  NOT NULL COMMENT '昵称',
       `birthday`           date         NOT NULL COMMENT '生日',
       `sex`                tinyint(4)            DEFAULT '0' COMMENT '性别',
       `short_introduce`    varchar(150)          DEFAULT NULL COMMENT '一句话介绍自己，最多50个汉字',
       `user_resume`        varchar(300) NOT NULL COMMENT '用户提交的简历存放地址',
       `user_register_ip`   int          NOT NULL COMMENT '用户注册时的源ip',
       `create_time`        timestamp    NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
       `update_time`        timestamp    NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
       `user_review_status` tinyint      NOT NULL COMMENT '用户资料审核状态，1为通过，2为审核中，3为未 通过，4为还未提交审核',
       PRIMARY KEY (`id`),
       UNIQUE KEY `uniq_user_id` (`user_id`),
       KEY `idx_username` (`username`),
       KEY `idx_create_time_status` (`create_time`, `user_review_status`)
   ) ENGINE = InnoDB
     DEFAULT CHARSET = utf8 COMMENT ='网站用户基本信息';
   ```

   1. 【建议】创建表时，可以使用可视化工具。这样可以确保表、字段相关的约定都能设置上。

   实际上，我们通常很少自己写 DDL 语句，可以使用一些可视化工具来创建和操作数据库和数据表。

   可视化工具除了方便，还能直接帮我们将数据库的结构定义转化成 SQL 语言，方便数据库和数据表结构的导出和导入

#### 关于索引

1. 【强制】InnoDB表必须主键为id int/bigint auto_increment，且主键值 **禁止被更新** 。 
2. 【强制】InnoDB和MyISAM存储引擎表，索引类型必须为 **BTREE** （B+树）。
3. 【建议】主键的名称以 **pk_** 开头，唯一键以 **uni_** 或 **uk_** 开头，普通索引以 **idx_** 开头，一律 使用小写格式，以字段的名称或缩写作为后缀。
4. 【建议】多单词组成的columnname，取前几个单词首字母，加末单词组成column_name。如: sample 表 member_id 上的索引：idx_sample_mid。 
5. 【建议】单个表上的索引个数 **不能超过6个** 。 
6. 【建议】在建立索引时，多考虑建立 **联合索引** ，并把区分度最高的字段放在最前面。
7. 【建议】在多表 JOIN 的SQL里，保证被驱动表的连接列上有索引，这样JOIN 执行效率最高。
8. 【建议】建表或加索引时，保证表里互相不存在 **冗余索引** 。 比如：如果表里已经存在key(a,b)， 则key(a)为冗余索引，需要删除。

#### SQL编写

1. 【强制】程序端SELECT语句必须指定具体字段名称，禁止写成 *。 
2. 【建议】程序端insert语句指定具体字段名称，不要写成INSERT INTO t1 VALUES(…)。 
3. 【建议】除静态表或小表（100行以内），DML语句必须有WHERE条件，且使用索引查找。
4. 【建议】INSERT INTO…VALUES(XX),(XX),(XX).. 这里XX的值不要超过5000个。 值过多虽然上线很 快，但会引起主从同步延迟。
5. 【建议】SELECT语句不要使用UNION，推荐使用UNION ALL，并且UNION子句个数限制在5个以 内。
6. 【建议】线上环境，多表 JOIN 不要超过5个表。
7. 【建议】减少使用ORDER BY，和业务沟通能不排序就不排序，或将排序放到程序端去做。ORDER BY、GROUP BY、DISTINCT 这些语句较为耗费CPU，数据库的CPU资源是极其宝贵的。
8. 【建议】包含了ORDER BY、GROUP BY、DISTINCT 这些查询的语句，WHERE 条件过滤出来的结果集请保持在1000行以内，否则SQL会很慢。
9. 【建议】对单表的多次alter操作必须合并为一次

  对于超过100W行的大表进行alter table，必须经过DBA审核，并在业务低峰期执行，多个alter需整合在一起。 因为**alter table会产生 表锁** ，期间阻塞对于该表的所有写入，对于业务可能会产生极大影响。

1. 【建议】批量操作数据时，需要控制事务处理间隔时间，进行必要的sleep。 

2. 【建议】事务里包含SQL不超过5个。

   因为过长的事务会导致锁数据较久，MySQL内部缓存、连接消耗过多等问题。

3. 【建议】事务里更新语句尽量基于主键或UNIQUE KEY，如UPDATE… WHERE id=XX;

   否则会产生间隙锁，内部扩大锁定范围，导致系统性能下降，产生死锁。

### PowerDesigner的使用（自己看158）

#### 开始界面

#### 概念数据模型

##### Entity实体

##### 填充实体字段

##### 设置主标识符

##### 放大模型

##### 实体关系

#### 物理数据模型

#### 概念模型转为物理模型

#### 物理模型转为概念模型

#### 物理模型导出SQL语句

## 数据库其他调优策略

### 数据库调优的措施

#### 调优的目标

- 尽可能节省系统资源 ，以便系统可以提供更大负荷的服务。（吞吐量更大）
- 合理的结构设计和参数调整，以提高用户操作 响应的速度 。（响应速度更快）
- 减少系统的瓶颈，提高MySQL数据库整体的性能。

#### 如何定位调优问题

不过随着用户量的不断增加，以及应用程序复杂度的提升，我们很难用**“更快”**去定义数据库调优的目标，因为用户在不同时间段访问服务器遇到的瓶颈不同，比如双十一促销的时候会带来大规模的**并发访问**；还有用户在进行不同业务操作的时候，数据库的**事务处理**和**SQL查询**都会有所不同。因此我们还需要更加精细的定位，去确定调优的目标。

如何确定呢？一般情况下，有如下几种方式：

- **用户的反馈（主要）**

  用户是我们的服务对象，因此他们的反馈是最直接的。虽然他们不会直接提出技术建议，但是有些问题往往是用户第一时间发现的。我们要重视用户的反馈，找到和数据相关的问题。

- **日志分析（主要）**

  我们可以通过查看数据库日志和操作系统日志等方式找出异常情况，通过它们来定位遇到的问题。

- **服务器资源使用监控**

  通过监控服务器的CPU、内存、I/O 等使用情况，可以实时了解服务器的性能使用，与历史情况进行对比。

- **数据库内部状况监控**

  在数据库的监控中，**活动会话(Active Session) 监控**是一个重要的指标。通过它，你可以清楚地了解数据库当前是否处于非常繁忙的状态，是否存在SQL堆积等。

- **其它**

除了活动会话监控以外，我们也可以对 **事务** 、 **锁等待** 等进行监控，这些都可以帮助我们对数据库的运行状态有更全面的认识。

#### 调优的维度和步骤

我们需要调优的对象是整个数据库管理系统，它不仅包括 SQL 查询，还包括数据库的部署配置、架构等。从这个角度来说，我们思考的维度就不仅仅局限在 SQL 优化上了。通过如下的步骤我们进行梳理：

##### 选择合适的DBMS

如果对**事务性处理**以及**安全性要求高**的话，可以选择商业的数据库产品。这些数据库在事务处理和查询性能上都比较强，比如采用SQL Server、Oracle，那么**单表存储上亿条数据**是没有问题的。如果数据表设计得好，即使不采用**分库分表**的方式，查询效率也不差。

除此以外，你也可以采用开源的MySQL进行存储，它有很多存储引擎可以选择，如果进行事务处理的话可以选择InnoDB，非事务处理可以选择MyISAM。

NoSQL阵营包括**键值型数据库**、**文档型数据库**、 **搜索引擎**、 **列式存储**和**图形数据库**。这些数据库的优缺点和使用场景各有不同，比如列式存储数据库可以大幅度降低系统的I/O，适合于分布式文件系统，但如果数据需要频繁地增删改，那么列式存储就不太适用了。

**DBMS的选择关系到了后面的整个设计过程，所以第一步就是 要选择适合的DBMS**。如果已经确定好了DBMS，那么这步可以跳过。

##### 优化表设计

选择了DBMS之后，我们就需要进行表设计了。而数据表的设计方式也直接影响了后续的SQL查询语句。RDBMS中，每个对象都可以定义为一张表，表与表之间的关系代表了对象之间的关系。如果用的是MySQL，我们还可以根据不同表的使用需求，选择不同的存储引擎。除此以外，还有一些优化的原则可以参考：

1. 表结构要尽量**遵循三范式的原则**。这样可以让数据结构更加清晰规范，减少冗余字段，同时也减少了在更新，插入和删除数据时等异常情况的发生。
2. 如果**查询**应用比较多，尤其是需要进行**多表联查**的时候，可以采用**反范式**进行优化。反范式采用**空间换时间**的方式，通过增加冗余字段提高查询的效率。
3. **表字段的数据类型** 选择，关系到了查询效率的高低以及存储空间的大小。一般来说，如果字段可以采用数值类型就不要采用字符类型；字符长度要尽可能设计得短一些。针对字符类型来说，当确定字符长度固定时，就可以采用CHAR类型；当长度不固定时，通常采用VARCHAR类型。

数据表的结构设计很基础，也很关键。**好的表结构可以在业务发展和用户量增加的情况下依然发挥作用，不好的表结构设计会让数据表变得非常臃肿，查询效率也会降低。**

##### 优化逻辑查询

当我们建立好数据表之后，就可以对数据表进行增删改查的操作了。这时我们首先需要考虑的是逻辑查询优化。

SQL查询优化，可以分为**逻辑查询优化**和**物理查询优化**。逻辑查询优化就是通过改变SQL语句的内容让SQL执行效率更高效，采用的方式是对SQL语句进行等价变换,对查询进行重写。

SQL的查询重写包括了子查询优化、等价谓词重写、视图重写、条件简化、连接消除和嵌套连接消除等。

比如我们在讲解EXISTS子查询和IN子查询的时候，会根据**小表驱动大表**的原则选择适合的子查询。在WHERE子句中会尽量避免对字段进行函数运算，它们会让字段的索引失效。

举例：查询评论内容开头为abc的内容都有哪些，如果在WHERE子句中使用了函数，语句就会写成下面这样：

```
SELECT comment_id,comment_text，comment_time FROM product_comment WHERE
SUBSTRING(comment_text,1,3)= 'abc'
```

采用查询重写的方式进行等价替换:

```
SELECT comment_id, comment_text, comment_time FROM product_comment WHERE comment_text LIKE 'abc%'
```

##### 优化物理查询

物理查询优化是在确定了逻辑查询优化之后，采用物理优化技术(比如索引等)， 通过计算代价模型对各种可能的访问路径进行估算，从而找到执行方式中代价最小的作为执行计划。在这个部分中，我们需要掌握的重点是对索弓的创建和使用。

但索引不是万能的，我们需要根据实际情况来创建索引。那么都有哪些情况需要考虑呢？我们在前面几章中已经进行了细致的剖析。

SQL查询时需要对不同的数据表进行查询，因此在物理查询优化阶段也需要确定这些查询所采用的路径，具体的情况包括：

1. **单表扫描**：对于单表扫描来说，我们可以全表扫描所有的数据，也可以局部扫描。
2. **两张表的连接**：常用的连接方式包括了嵌套循环连接、HASH连接和合并连接。
3. **多张表的连接**：多张数据表进行连接的时候， **顺序**很重要，因为不同的连接路径查询的效率不同，搜索空间也会不同。我们在进行多表连接的时候，搜索空间可能会达到**很高的数据量级**，巨大的搜索空间显然会占用更多的资源，因此我们需要通过调整连接顺序，将搜索空间调整在一个可接受的范围内。

##### 使用Redis 或 Memcached 作为缓存

电商项目中，在购物车中利用redis+分布式集合的方式，在商品详情页面中可以采用redis做缓存

除了可以对SQL本身进行优化以外，我们还可以请外援提升查询的效率。

因为数据都是存放到数据库中，我们需要从数据库层中取出数据放到内存中进行业务逻辑的操作，当用户量增大的时候，如果频繁地进行数据查询，会消耗数据库的很多资源。如果我们将常用的数据直接放到内存中，就会大幅提升查询的效率。

键值存储数据库可以帮我们解决这个问题。

常用的键值存储数据库有Ridis和Memcached，它们都可以将数据存放到内存中。

从可靠性来说，**Redis 支持持久化**，可以让我们的数据保存在硬盘上，不过这样一来性能消耗也会比较大。 而Memcached仅仅是内存存储，不支持持久化。

从支持的数据类型来说，Redis比Memcached要多，它不仅支持key-value类型的数据，还支持List, Set, Hash等数据结构。当我们有持久化需求或者是更高级的数据处理需求的时候，就可以使用Redis。如果是简单的key-value存储，则可以使用Memcached。

**通常我们对于查询响应要求高的场景(响应时间短，吞吐量大)， 可以考虑内存数据库，毕竟术业有专攻。**传统的RDBMS都是将数据存储在硬盘上，而内存数据库则存放在内存中，查询起来要快得多。不过使用不同的工具，也增加了开发人员的使用成本。

##### 库级优化

库级优化是站在数据库的维度上进行的优化策略，比如控制一个库中的数据表数量。另外，单一的数据库总会遇到各种限制，不如取长补短，利用"外援"的方式。通过**主从架构**优化我们的读写策略，通过对数据库进行垂直或者水平切分，突破单一数据库或数据表的访问限制，提升查询的性能。

1、读写分离

如果读和写的业务量都很大，并且它们都在同一个数据库服务器中进行操作，那么数据库的性能就会出现瓶颈，这时为了提升系统的性能，优化用户体验，我们可以采用**读写分离**的方式降低主数据库的负载，比如用主数据库(master)完成写操作，用从数据库(slave) 完成读操作。

![image-20230128140945572](/image-20230128140945572.png)

![image-20230128140956472](/image-20230128140956472.png)

1、数据分片

对**数据库分库分表**。当数据量级达到千万级以上时，有时候我们需要把一个数据库切成多份，放到不同的数据库服务器上，减少对单一数据库服务器的访问压力。如果你使用的是MySQL，就可以使用MySQL自带的分区表功能，当然你也可以考虑自己做**垂直拆分(分库)、水平拆分 (分表)、垂直+水平拆分 (分库分表)**。

![image-20230128141004970](/image-20230128141004970.png)

![image-20230128141112740](/image-20230128141112740.png)

> 但需要注意的是，分拆在提升数据库性能的同时，也会增加维护和使用成本。

### 优化MySQL服务器

优化MySQL服务器主要从两个方面来优化，一方面是对**硬件**进行优化；另一方面是对MySQL**服务的参数**进行优化。这部分的内容需要较全面的知识，一般只有**专业的数据库管理员**才能进行这一类的优化。 对于可以定制参数的操作系统，也可以针对MySQL进行操作系统优化。

#### 优化服务器硬件

**服务器的硬件性能直接决定着MySQL数据库的性能。**硬件的性能瓶颈直接决定MySQL数据库的运行速度和效率。

针对性能瓶颈提高硬件配置,可以提高MySQL数据库查询、更新的速度。

1. **配置较大的内存**。足够大的内存是提高MySQL数据库性能的方法之一。内存的速度比磁盘I/O快得多，可以通过增加系统的**缓冲区容量**使数据在内存中停留的时间更长，**以减少磁盘I/O**。
2. **配置高速磁盘系统**，以减少读盘的等待时间，提高响应速度。磁盘的I/O能力，也就是它的寻道能力，目前的**SCSI**高速旋转的是7200转/分钟，这样的速度，一旦访问的用户量上去，磁盘的压力就会过大，如果是每天的网站pv (page view)在150w, 这样的一般的配置就无法满足这样的需求了。现在**SSD**盛行，在SSD上随机访问和顺序访问性能几乎差不多，使用SSD可以减少随机IO带来的性能损耗。（电脑用得时间长了，以为是CPU的问题，实际上装块固态就行了）
3. **合理分布磁盘I/O**，把磁盘I/O分散在多个设备上，以减少资源竞争，提高并行操作能力。
4. **配置多处理器**，MySQL是多线程的数据库，多处理器可同时执行多个线程。

#### 优化MySQL的参数

通过优化MySQL的参数可以提高资源利用率,从而达到提高MySQL服务器性能的目的。

MySQL服务的配置参数都在**my . cnf**或者**my . ini**文件的[mysqlq]组中。配置完参数以后，需要重新启动MySQL服务才会生效。

下面对几个对性能影响比较大的参数进行详细介绍。

- innodb_buffer_pool_size ：

  这个参数是Mysql数据库最重要的参数之一，表示InnoDB类型的 **表和索引的最大缓存** 。它不仅仅缓存 **索引数据** ，还会缓存 **表的数据** 。这个值越大，查询的速度就会越快。但是这个值太大会影响操作系统的性能。

- key_buffer_size ：

  表示 **索引缓冲区的大小** 。索引缓冲区是所有的 **线程共享** 。增加索引缓冲区可以得到更好处理的索引（对所有读和多重写）。当然，这个值不是越大越好，它的大小取决于内存的大小。如果这个值太大，就会导致操作系统频繁换页，也会降低系统性能。对于内存在 **4GB** 左右的服务器该参数可设置为 **256M 或 384M** 。

- table_cache ：

  表示 **同时打开的表的个数** 。这个值越大，能够同时打开的表的个数越多。物理内存越大，设置就越大。默认为2402，调到512-1024最佳。这个值不是越大越好，因为同时打开的表太多会影响操作系统的性能。

- query_cache_size ：

  表示 **查询缓冲区的大小** 。可以通过在MySQL控制台观察，如果Qcache_lowmem_prunes的值非常大，则表明经常出现缓冲不够的情况，就要增加Query_cache_size的值；如果Qcache_hits的值非常大，则表明查询缓冲使用非常频繁，如果该值较小反而会影响效率，那么可以考虑不用查询缓存；Qcache_free_blocks，如果该值非常大，则表明缓冲区中碎片很多。MySQL8.0之后失效。该参数需要和query_cache_type配合使用。

- query_cache_type：

  - query_cache_type的值是0时，**所有的查询都不使用查询缓存区**。但是query_cache_type=0并不会导致MySQL释放query_cache_size所配置的缓存区内存。
  - 当query_cache_type=1时，**所有的查询都将使用查询缓存区**，除非在查询语句中指定SQL_NO_CACHE ，如SELECT SQL_NO_CACHE * FROM tbl_name。 
  - 当query_cache_type=2时，**只有在查询语句中使用 SQL_CACHE 关键字**，查询才会使用查询缓存区。使用查询缓存区可以提高查询的速度，这种方式只适用于修改操作少且经常执行相同的查询操作的情况。

- sort_buffer_size ：

  表示每个 **需要进行排序的线程分配的缓冲区的大小** 。增加这个参数的值可以提高 **ORDER BY** 或 **GROUP BY** 操作的速度。默认数值是2 097 144字节（约2MB）。对于内存在4GB左右的服务器推荐设置为6-8M，如果有100个连接，那么实际分配的总共排序缓冲区大小为100 × 6 ＝600MB。 

- join_buffer_size = 8M ：

  表示 **联合查询操作所能使用的缓冲区大小** ，和sort_buffer_size一样，该参数对应的分配内存也是每个连接独享。

- read_buffer_size ：

  表示 **每个线程连续扫描时为扫描的每个表分配的缓冲区的大小（字节）** 。当线程从表中连续读取记录时需要用到这个缓冲区。SET SESSION read_buffer_size=n可以临时设置该参数的值。默认为64K，可以设置为4M。 

- innodb_flush_log_at_trx_commit ：

  表示 **何时将缓冲区的数据写入日志文件** ，并且将日志文件写入磁盘中。该参数对于innoDB引擎非常重要。该参数有3个值，分别为0、1和2。该参数的默认值为1。

  - 值为 0 时，表示 **每秒1次** 的频率将数据写入日志文件并将日志文件写入磁盘。每个事务的commit并不会触发前面的任何操作。该模式速度最快，但不太安全，mysqld进程的崩溃会导致上一秒钟所有事务数据的丢失。
  - 值为 1 时，表示 **每次提交事务时** 将数据写入日志文件并将日志文件写入磁盘进行同步。该模式是最安全的，但也是最慢的一种方式。因为每次事务提交或事务外的指令都需要把日志写入（flush）硬盘。
  - 值为 2 时，表示 **每次提交事务时** 将数据写入日志文件， **每隔1秒** 将日志文件写入磁盘。该模式速度较快，也比0安全，只有在操作系统崩溃或者系统断电的情况下，上一秒钟所有事务数据才可能丢失。

- innodb_log_buffer_size ：

  这是 InnoDB 存储引擎的 **事务日志所使用的缓冲区** 。为了提高性能，也是先将信息写入 Innodb Log Buffer 中，当满足 innodb_flush_log_trx_commit 参数所设置的相应条件（或者日志缓冲区写满）之后，才会将日志写到文件（或者同步到磁盘）中。

- max_connections ：

  表示 **允许连接到MySQL数据库的最大数量** ，默认值是 **151** 。如果状态变量connection_errors_max_connections 不为零，并且一直增长，则说明不断有连接请求因数据库连接数已达到允许最大值而失败，这是可以考虑增大max_connections 的值。在Linux 平台下，性能好的服务器，支持 500-1000 个连接不是难事，需要根据服务器性能进行评估设定。这个**连接数不是越大越好** ，因为这些连接会浪费内存的资源。过多的连接可能会导致MySQL服务器僵死。

- back_log ：

  用于 **控制MySQL监听TCP端口时设置的积压请求栈大小** 。如果MySql的连接数达到max_connections时，新来的请求将会被存在堆栈中，以等待某一连接释放资源，该堆栈的数量即back_log，如果等待连接的数量超过back_log，将不被授予连接资源，将会报错。5.6.6 版本之前默认值为 50 ， 之后的版本默认为 50 + （max_connections / 5）， 对于Linux系统推荐设置为小于512的整数，但最大不超过900。

  如果需要数据库在较短的时间内处理大量连接请求， 可以考虑适当增大back_log 的值。

- thread_cache_size ： 

  **线程池缓存线程数量的大小** ，当客户端断开连接后将当前线程缓存起来，当在接到新的连接请求时快速响应无需创建新的线程 。这尤其对那些使用短连接的应用程序来说可以极大的提高创建连接的效率。那么为了提高性能可以增大该参数的值。默认为60，可以设置为120。

  可以通过如下几个MySQL状态值来适当调整线程池的大小：

  ```
  mysql> show global status like 'Thread%';
  +-------------------+-------+
  | Variable_name 	| Value |
  +-------------------+-------+
  | Threads_cached 	| 2 	| 
  | Threads_connected | 1 	| 
  | Threads_created 	| 3 	|
  | Threads_running	| 2 	|
  +-------------------+-------+
  4 rows in set (0.01 sec)
  ```

  当 Threads_cached 越来越少，但 Threads_connected 始终不降，且 Threads_created 持续升高，可适当增加 thread_cache_size 的大小。

- wait_timeout ：

  指定 **一个请求的最大连接时间** ，对于4GB左右内存的服务器可以设置为5-10。 

- interactive_timeout ：

  表示服务器在关闭连接前等待行动的秒数。

这里给出一份my.cnf的参考配置：

```
port = 3306
serverid = 1
socket = /tmp/mysql.sock

#避免MySQL的外部锁定，减少出错几率增强稳定性。
skip-locking

#禁止MySQL对外部连接进行DNS解析，使用这一选项可以消除MySQL进行DNS解析的时间。但需要注意，如果开启该选项，则所有远程主机连接授权都要使用IP地址方式，否则MySQL将无法正常处理连接请求！
skip-name-resolve
back_log = 384
key_buffer_size = 256M
max_allowed_packet = 4M
thread_stack = 256K
table_cache = 128K
sort_buffer_size = 6M
read_buffer_size = 4M
read_rnd_buffer_size = 16M
join_buffer_size = 8M
myisam_sort_buffer_size = 64M
table_cache = 512
thread_cache_size = 64
query_cache_size = 64M
tmp_table_size = 256M
max_connections = 768
max_connect_errors = 10000000
wait_timeout = 10

#该参数取值为服务器逻辑CPU数量*2，在本例中，服务器有2颗物理CPU，而每颗物理CPU又支持H.T超线程，所以实际取值为4*2=8
thread_concurrency = 8

#开启该选项可以彻底关闭MySQL的TCP/IP连接方式，如果WEB服务器是以远程连接的方式访问MySQL数据库服务器则不要开启该选项！否则将无法正常连接！
skip-networking

table_cache = 1024

#默认为2M
innodb_additional_mem_pool_size = 4M

innodb_flush_log_at_trx_commit = 1

#默认为1M
innodb_log_buffer_size = 2M

#你的服务器CPU有几个就设置为几。建议用默认一般为8
innodb_thread_concurrency = 8

#默认为16M，调到64-256最挂
tmp_table_size = 64M

thread_cache_size = 120
query_cache_size=32M
```

很多情况还需要具体情况具体分析！

**举例**

下面是一个电商平台，类似京东或天猫这样的平台。商家购买服务，入住平台，开通之后，商家可以在系统中上架各种商品，客户通过手机App、微信小程序等渠道购买商品，商家接到订单以后安排快递送货。

**刚刚上线**的时候，系统运行状态良好。但是，随着入住的**商家不断增多**，使用系统的**用户量越来越多**，每天的订单数据达到了5万条以上。这个时候，系统开始出现问题，**CPU 使用率不断飙升**。终于，双十一或者618活动高峰的时候，CPU 使用率达到**99%**，这实际上就意味着，系统的计算资源已经耗尽，再也无法处理任何新的订单了。

换句话说，系统已经崩溃了。

这个时候，我们想到了对系统参数进行调整，因为参数的值决定了资源配置的方式和投放的程度。

为了解决这个问题，一共调整3个系统参数，分别是

- InnoDB_flush_log_at_trx_commit
- InnoDB_buffer_ pool_size
- InnoDB_buffer_pool_instances

下面我们就说一说调整这三个参数的原因是什么。

1. InnoDB_flush_log_at_trx_commit

   这个参数适用于InnoDB存储引擎，电商平台系统中的表用的存储引擎都是InnoDB。默认的值是1，意思是每次提交事务的时候，都把数据写入日志，并把日志写入磁盘。这样做的好处是数据安全性最佳，不足之处在于每次提交事务，都要进行磁盘写入的操作。在**大并发的场景下**，过于频繁的磁盘读写会导致CPU资源浪费，系统效率变低。

   这个参数的值还有2个可能的选项，分别是0和2。我们把这个参数的值改成了2。这样就不用每次提交事务的时候都启动磁盘读写了，在大并发的场景下，可以改善系统效率，降低CPU使用率。即便出现故障，损失的数据也比较小。

2. InnoDB_buffer_ pool_size

   这个参数的意思是，InnoDB 存储引擎使用**缓存来存储索引和数据**。这个值越大，可以加载到缓存区的索引和数据量就越多，需要的**磁盘读写就越少**。

   因为我们的MySQL服务器是数据库**专属服务器**，只用来运行MySQL数据库服务，没有其他应用了，而我们的计算机是64位机器，内存也有128G。于是我们把这个参数的值调整为64G。这样一来，磁盘读写次数可以大幅降低，我们就可以充分利用内存，释放出一些CPU的资源。

3. InnoDB_buffer_pool_instances

   这个参数可以将InnoDB的缓存区分成几个部分，这样可以提高系统的**并行处理能力**，因为可以允许多个进程同时处理不同部分的缓存区。

   我们把InnoDB_buffer_pool_instances 的值修改为64，意思就是把InnoDB的缓存区分成64个分区，这样就可以时有**多个进程**进行数据操作，CPU 的效率就高多了。修改好了系统参数的值，要重启MySQL数据库服务器。

> 总结一下就是遇到CPU资源不足的问题，可以从下面2个思路去解决。
>
> - 疏通拥堵路段，消除瓶颈，让等待的时间更短;
> - 开拓新的通道，增加并行处理能力。

### 优化数据库结构

一个好的**数据库设计方案**对于数据库的性能常常会起到事半功倍的效果。合理的数据库结构不仅可以使数据库占用更小的磁盘空间，而且能够使查询速度更快。数据库结构的设计需要考虑**数据冗余、查询和更新的速度、字段的数据类型**是否合理等多方面的内容。

#### 拆分表：冷热数据分离

拆分表的思路是，把1个包含很多字段的表拆分成2个或者多个相对较小的表。这样做的原因是，这些表中某些字段的操作频率很高**(热数据)**，经常要进行查询或者更新操作，而另外一些字段的使用频率却很低(**冷数据**)，**冷热数据分离**，可以减小表的宽度。如果放在一个表里面，每次查询都要读取大记录，会消耗较多的资源。

MySQL限制每个表最多存储**4096**列，并且每一行数据的大小不能超过**65535字节**。表越宽，把表装载进内存缓冲池时所占用的内存也就越大，也会消耗更多的IO。

**冷热数据分离的目的** :

①减少磁盘IO，保证热数据的内存缓存命中率。

②更有效的利用缓存，避免读入无用的冷数据。

**举例**1**：** **会员members表** 存储会员登录认证信息，该表中有很多字段，如id、姓名、密码、地址、电话、个人描述字段。其中地址、电话、个人描述等字段并不常用，可以将这些不常用的字段分解出另一个表。将这个表取名叫members_detail，表中有member_id、address、telephone、description等字段。这样就把会员表分成了两个表，分别为 **members表** 和 **members_detail表** 。

创建这两个表的SQL语句如下：

```
CREATE TABLE members(
    id              int(11) NOT NULL AUTO_INCREMENT,
    username        varchar(50)  DEFAULT NULL,
    password        varchar(50)  DEFAULT NULL,
    last_login_time datetime     DEFAULT NULL,
    last_login_ip   varchar(100) DEFAULT NULL,
    PRIMARY KEY (Id)
);#热数据

CREATE TABLE members_detail(
    Member_id   int(11) NOT NULL DEFAULT 0,
    address     varchar(255)     DEFAULT NULL,
    telephone   varchar(255)     DEFAULT NULL,
    description text
);#冷数据
```

如果需要查询会员的基本信息或详细信息，那么可以用会员的id来查询。如果需要将会员的基本信息和详细信息同时显示，那么可以将members表和members_detail表进行联合查询，查询语句如下：

```
SELECT * FROM members LEFT JOIN members_detail on members.id = members_detail.member_id;
```

通过这种分解可以提高表的查询效率。对于字段很多且有些字段使用不频繁的表，可以通过这种分解的方式来优化数据库的性能。

#### 增加中间表

对于需要经常联合查询的表，可以建立中间表以提高查询效率。通过建立中间表，**把需要经常联合查询的数据插入中间表中，然后将原来的联合查询改为对中间表的查询，以此来提高查询效率**。

首先，分析经常联合查询表中的字段;然后，使用这些字段建立一个中间表， 并将原来联合查询的表的数据插入中间表中；最后，使用中间表来进行查询。

**举例**1**：** **学生信息表** 和 **班级表** 的SQL语句如下：

```
CREATE TABLE `class`(
    `id`        INT(11) NOT NULL AUTO_INCREMENT,
    `className` VARCHAR(30) DEFAULT NULL,
    `address`   VARCHAR(40) DEFAULT NULL,
    `monitor`   INT     NULL,
    PRIMARY KEY (`id`)
) ENGINE = INNODB
  AUTO_INCREMENT = 1
  DEFAULT CHARSET = utf8;
  
CREATE TABLE `student`(
    `id`      INT(11) NOT NULL AUTO_INCREMENT,
    `stuno`   INT     NOT NULL,
    `name`    VARCHAR(20) DEFAULT NULL,
    `age`     INT(3)      DEFAULT NULL,
    `classId` INT(11)     DEFAULT NULL,
    PRIMARY KEY (`id`)
) ENGINE = INNODB
  AUTO_INCREMENT = 1
  DEFAULT CHARSET = utf8;
```

现在有一个模块需要经常查询带有学生名称（name）、学生所在班级名称（className）、学生班级班长（monitor）的学生信息。根据这种情况可以创建一个 **temp_student** 表。temp_student表中存储学生名称（stu_name）、学生所在班级名称（className）和学生班级班长（monitor）信息。创建表的语句。

如下：

```
CREATE TABLE `temp_student`(
    `id`        INT(11) NOT NULL AUTO_INCREMENT,
    `stu_name`  INT     NOT NULL,
    `className` VARCHAR(20) DEFAULT NULL,
    `monitor`   INT(3)      DEFAULT NULL,
    PRIMARY KEY (`id`)
) ENGINE = INNODB
  AUTO_INCREMENT = 1
  DEFAULT CHARSET = utf8;
```

接下来，从学生信息表和班级表中查询相关信息存储到临时表中：

```
insert into temp_student(stu_name, className, monitor)
select s.name, c.className, c.monitor
from student as s,
     class as c
where s.classId = c.id
```

以后，可以直接从temp_student表中查询学生名称、班级名称和班级班长，而不用每次都进行联合查询。这样可以提高数据库的查询速度。

> 如果用户信息修改了，是不是会导致temp_vip中的数据不一致的问题呢？如何同步数据呢？
>
> 方式1：清空数据->重新添加数据
>
> 方式2：使用视图

#### 增加冗余字段

设计数据库表时应尽量遵循范式理论的规约，尽可能减少冗余字段，让数据库设计看起来精致、优雅。但是，合理地加入冗余字段可以提高查询速度。

表的规范化程度越高，表与表之间的关系就越多，需要连接查询的情况也就越多。尤其在数据量大，而且需要频繁进行连接的时候，为了提升效率，我们也可以考虑增加冗余字段来减少连接。

这部分内容在《第11章_数据库的设计规范》章节中 **反范式化**小节 中具体展开讲解了。这里省略。

#### 优化数据类型

改进表的设计时，可以考虑优化字段的数据类型。这个问题在大家刚从事开发时基本不算是问题。但是，随着你的经验越来越丰富，参与的项目越来越大，数据量也越来越多的时候，你就不能只从系统稳定性的角度来思考问题了，还要考虑到系统整体的稳定性和效率。此时，**优先选择符合存储需要的最小的数据类型。**

列的**字段越大**，建立索引时所需要的**空间也就越大**，这样一页中所能存储的索引节点的**数量也就越少**，在遍历时所**需要的IO次数也就越**多，索引的**性能也就越差**。

具体来说:

**情况1：对整数类型数据进行优化。**

遇到整数类型的字段可以用 **INT 型** 。这样做的理由是，INT 型数据有足够大的取值范围，不用担心数据超出取值范围的问题。刚开始做项目的时候，首先要保证系统的稳定性，这样设计字段类型是可以的。但在数据量很大的时候，数据类型的定义，在很大程度上会影响到系统整体的执行效率。

对于 **非负型** 的数据（如自增ID、整型IP）来说，要优先使用无符号整型 **UNSIGNED** 来存储。因为无符号相对于有符号，同样的字节数，存储的数值范围更大。如tinyint有符号为-128-127，无符号为0-255，多出一倍的存储空间。

**情况2：既可以使用文本类型也可以使用整数类型的字段，要选择使用整数类型。**

跟文本类型数据相比，大整数往往占用 **更少的存储空间** ，因此，在存取和比对的时候，可以占用更少的内存空间。所以，在二者皆可用的情况下，尽量使用整数类型，这样可以提高查询的效率。如：将IP地址转换成整型数据。

**情况3：避免使用TEXT、BLOB数据类型**

MySQL**内存临时表**不支持TEXT、BLOB这样的大数据类型，如果查询中包含这样的数据，在排序等操作时，就不能使用内存临时表，必须使用**磁盘临时表**进行。并且对于这种数据，MySQL还是要进行**二次查询**，会使SQL性能变得很差，但是不是说一定不能使用这样的数据类型。

如果一定要使用，建议把BLOB或是TEXT列**分离到单独的扩展表**中，查询时一定不要使用select* ，而只需要取出必要的列，不需要TEXT列的数据时不要对该列进行查询。

**情况4：避免使用ENUM类型**

修改ENUM值需要使用ALTER语句。

ENUM类型的ORDER BY操作效率低，需要额外操作。使用TINYINT来代替ENUM类型。

**情况5：使用TIMESTAMP存储时间**

TIMESTAMP存储的时间范围1970-01-01 00:00:01 ~ 2038-01-19-03:14:07。

TIMESTAMP使用4字节， DATETIME使用8个字节，同时TIMESTAMP具有自动赋值以及自动更新的特性。

**情况6：用DECIMAL代替FLOAT和DOUBLE存储精确浮点数**

1. 非精准浮点: float，double
2. 精准浮点: decimal

Decimal类型为精准浮点数，在计算时不会丢失精度，尤其是财务相关的金融类数据。占用空间由定义的宽度决定，每4个字节可以存储9位数字，并且小数点要占用一个字节。 可用于存储比bigint更大的整型数据。

**总之，遇到数据量大的项目时，一定要在充分了解业务需求的前提下，合理优化数据类型，这样才能充分发挥资源的效率，使系统达到最优。** 

#### 优化插入记录的速度

插入记录时，影响插入速度的主要是索引、唯一性校验、一次插入记录条数等。 根据这些情况可以分别进行优化。这里我们分为MyISAM引擎和InnoDB存储引擎来讲。

1. MyISAM**引擎的表：**

**① 禁用索引**

对于非空表，插入记录时，MySQL会根据表的索引对插入的记录建立索引。如果插入大量数据，建立索引就会降低插入记录的速度。为了解决这种情况，可以在插入记录之前禁用索引，数据插入完毕后再开启索引。禁用索引的语句如下：

```
ALTER TABLE table_name DISABLE KEYS;
```

重新开启索引的语句如下:

```
ALTER TABLE table_name ENABLE KEYS;
```

若对于空表批量导入数据，则不需要进行此操作，因为MyISAM引擎的表是在导入数据之后才建立索引的。

**② 禁用唯一性检查**

插入数据时，MySQL会对插入的记录进行唯一性校验。 这种唯一性校验会降低插入记录的速度。 为了降低这种情况对查询速度的影响，可以在插入记录之前禁用唯一性检查， 等到记录插入完毕后再开启。 禁用唯一性检查的语句如下：

```
SET UNIQUE_CHECKS=0;
```

开启唯一性检查的语句如下：

```
SET UNIQUE_CHECKS=1;
```

**③ 使用批量插入**

插入多条记录时，可以使用一条INSERT语句插入一条记录，也可以使用一条INSERT语句插入多条记录。插入一条记录的INSERT语句情形如下:

```
insert into student values(1,'zhangsan',18,1);
insert into student values(2,'lisi',17,1); 
insert into student values(3,'wangwu',17,1); 
insert into student values(4,'zhaoliu',19,1);
```

使用一条INSERT语句插入多条记录的情形如下：

```
insert into student values
(1,'zhangsan',18,1), 
(2,'lisi',17,1), 
(3,'wangwu',17,1), 
(4,'zhaoliu',19,1);
```

第2种情形的插入速度要比第1种情形快。

**④ 使用**LOAD DATA INFILE **批量导入**

当需要批量导入数据时，如果能用LOAD DATA INFILE语句，就尽量使用。因为LOAD DATA INFILE语句导入数据的速度比INSERT语句快。

1. InnoDB**引擎的表：** 

**① 禁用唯一性检查**

插入数据之前执行set unique_checks=0 来禁止对唯一索引的检查，数据导入完成之后再运行set unique_checks=1。 这个和MyISAM引擎的使用方法一样。

**② 禁用外键检查**

插入数据之前执行禁止对外键的检查，数据插入完成之后再恢复对外键的检查。禁用外键检查的语句如下：

```
SET foreign_key_checks=0 ;
```

恢复对外键的检查语句如下：

```
SET foreign_key_checks=1 ;
```

**③ 禁止自动提交**

插入数据之前禁止事务的自动提交，数据导入完成之后，执行恢复自动提交操作。禁止自动提交的语句如下：

```
set autocommit=0;
```

恢复自动提交的语句如下:

```
set autocommit=1 ;
```

#### 使用非空约束

**在设计字段的时候，如果业务允许，建议尽量使用非空约束**。这样做的好处：

①进行比较和计算时，省去要对NULL值的字段判断是否为空的开销，提高存储效率。

②非空字段也容易创建索引。因为索引NULL列需要额外的空间来保存，所以要占用更多的空间。使用非空约束，就可以节省存储空间(每个字段1个bit)。

#### 分析表、检查表、优化表

MySQL提供了分析表、检查表和优化表的语句。**分析表**主要是分析关键字的分布，**检查表**主要是检查表是否存在错误，**优化表**主要是消除删除或者更新造成的空间浪费。

##### 分析表

MySQL中提供了ANALYZE TABLE语句分析表，ANALYZE TABLE语句的基本语法如下：

```
ANALYZE [LOCAL | NO_WRITE_TO_BINLOG] TABLE tbl_name[,tbl_name]…
```

默认的，MySQL服务会将 ANALYZE TABLE语句写到binlog中，以便在主从架构中，从服务能够同步数据。可以添加参数LOCAL 或者 NO_WRITE_TO_BINLOG取消将语句写到binlog中。

使用 **ANALYZE TABLE** 分析表的过程中，数据库系统会自动对表加一个 **只读锁** 。在分析期间，只能读取表中的记录，不能更新和插入记录。ANALYZE TABLE语句能够分析InnoDB和MyISAM类型的表，但是不能作用于视图。

ANALYZE TABLE分析后的统计结果会反应到 **cardinality** 的值，该值统计了表中某一键所在的列不重复的值的个数。**该值越接近表中的总行数，则在表连接查询或者索引查询时，就越优先被优化器选择使用。**也就是索引列的cardinality的值与表中数据的总条数差距越大，即使查询的时候使用了该索引作为查询条件，存储引擎实际查询的时候使用的概率就越小。下面通过例子来验证下。cardinality可以通过SHOW INDEX FROM 表名查看。

**举例**

```
#08-数据库的其他优化策略
CREATE TABLE `user1` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(255) DEFAULT NULL,
  `age` INT DEFAULT NULL,
  `sex` VARCHAR(255) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `idx_name` (`name`) USING BTREE
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb3;

#######
SET GLOBAL log_bin_trust_function_creators = 1;

DELIMITER //
CREATE FUNCTION  rand_num (from_num INT ,to_num INT) RETURNS INT(11)
BEGIN   
DECLARE i INT DEFAULT 0;  
SET i = FLOOR(from_num +RAND()*(to_num - from_num+1))   ;
RETURN i;  
END //
DELIMITER ;

###
DELIMITER //
CREATE PROCEDURE  insert_user( max_num INT )
BEGIN  
DECLARE i INT DEFAULT 0;   
 SET autocommit = 0;    
 REPEAT  
 SET i = i + 1;  
 INSERT INTO `user1` ( NAME,age,sex ) 
 VALUES ("atguigu",rand_num(1,20),"male");  
 UNTIL i = max_num  
 END REPEAT;  
 COMMIT; 
END //
DELIMITER;

##
CALL insert_user(1000);

SHOW INDEX FROM user1;

# cardinality（区分度）：越接近数据总数，说明这个字段上不同的值越多，越应该用到索引

SHOW INDEX FROM user1;# cardinality 1000 1

UPDATE user1 SET NAME = 'atguigu02' WHERE id = 2;
SHOW INDEX FROM user1;# cardinality 1000 1
#分析表
ANALYZE TABLE user1;
SHOW INDEX FROM user1;# cardinality 1000 2

UPDATE user1 SET NAME = 'atguigu03' WHERE id = 3;
SHOW INDEX FROM user1;# cardinality 1000 2
#分析表
ANALYZE TABLE user1;
SHOW INDEX FROM user1;# cardinality 1000 3
#分析表
ANALYZE TABLE user1;
```

![image-20230128141438197](/image-20230128141438197.png)

```
SHOW INDEX FROM user1;
```

![image-20230128141448808](/image-20230128141448808.png)

##### 检查表

MySQL中可以使用 **CHECK TABLE** 语句来检查表。CHECK TABLE语句能够检查InnoDB和MyISAM类型的表是否存在错误。CHECK TABLE语句在执行过程中也会给表加上 **只读锁** 。

对于MyISAM类型的表，CHECK TABLE语句还会更新关键字统计数据。而且，CHECK TABLE也可以检查视图是否有错误，比如在视图定义中被引用的表已不存在。该语句的基本语法如下：

```
CHECK TABLE tbl_name [, tbl_name] ... [option] ... 
option = {QUICK | FAST | MEDIUM | EXTENDED | CHANGED}
```

其中，tbl_name是表名；option参数有5个取值，分别是QUICK、FAST、MEDIUM、EXTENDED和 CHANGED。各个选项的意义分别是：

- **QUICK** ：不扫描行，不检查错误的连接。
- **FAST** ：只检查没有被正确关闭的表。
- **CHANGED** ：只检查上次检查后被更改的表和没有被正确关闭的表。
- **MEDIUM** ：扫描行，以验证被删除的连接是有效的。也可以计算各行的关键字校验和，并使用计算出的校验和验证这一点。
- **EXTENDED** ：对每行的所有关键字进行一个全面的关键字查找。这可以确保表是100%一致的，但是花的时间较长。

option只对MyISAM类型的表有效，对InnoDB类型的表无效。比如：

```
#检查表
CHECK TABLE user1;
```

![image-20230128141521633](/image-20230128141521633.png)

该语句对于检查的表可能会产生多行信息。**最后一行有一个状态的 Msg_type 值**，Msg_text 通常为 OK。如果得到的不是 OK，通常要对其进行修复；是 OK 说明表已经是最新的了。表已经是最新的，意味着存储引擎对这张表不必进行检查。

##### 优化表

**方式1：OPTIMIZE TABLE** 

MySQL中使用 **OPTIMIZE TABLE** 语句来优化表。但是，OPTILMIZE TABLE语句只能优化表中的**VARCHAR 、 BLOB 或 TEXT** 类型的字段。一个表使用了这些字段的数据类型，若已经 **删除** 了表的一大部分数据，或者已经对含有可变长度行的表（含有VARCHAR、BLOB或TEXT列的表）进行了很多 **更新** ，则应使用OPTIMIZE TABLE来重新利用未使用的空间，并整理数据文件的碎片。 

OPTIMIZE TABLE 语句对InnoDB和MyISAM类型的表都有效。该语句在执行过程中也会给表加上 只读锁 。

 OPTILMIZE TABLE语句的基本语法如下：

```
OPTIMIZE [LOCAL | NO_WRITE_TO_BINLOG] TABLE tbl_name [, tbl_name] ...
```

LOCAL | NO_WRITE_TO_BINLOG关键字的意义和分析表相同，都是指定不写入二进制日志。

```
CREATE TABLE t1(id INT,NAME VARCHAR(15)) ENGINE = MYISAM;
#优化表
OPTIMIZE TABLE t1;
```

![image-20230128141536751](/image-20230128141536751.png)

```
CREATE TABLE t2(id INT,NAME VARCHAR(15)) ENGINE = INNODB;
#优化表
OPTIMIZE TABLE t2;
```

![image-20230128141546756](/image-20230128141546756.png)

执行完毕，Msg_text显示

> 'numysql.SYS_APP_USER’, ‘optimize’, ‘note’, ‘Table does not support optimize, doing recreate + analyze instead’

原因是我服务器上的MySQL是InnoDB存储引擎。

到底优化了没有呢？看官网！

`https://dev.mysql.com/doc/refman/8.0/en/optimize-table.html `

优化的信息就是这样的。

**优化过程：**

在MyISAM中，是先分析这张表，然后会整理相关的MySQL datafile，之后回收未使用的空间；

在InnoDB中，回收空间是简单通过Alter table进行整理空间。

**在优化期间，MySQL会创建一个临时表，优化完成之后会删除原始表，然后会将临时表rename成为原始表。**

> 说明： 在多数的设置中，根本不需要运行OPTIMIZE TABLE。即使对可变长度的行进行了大量的更新，也不需要经常运行， 每周一次 或 每月一次 即可，并且只需要对 特定的表 运行。

**方式2：使用mysqlcheck命令**

```
mysqlcheck -0 DatabaseName TableName -u root -p******
```

mysqlcheck是Linux中的rompt，-o是代表0ptimize。

举例：优化所有的表

```
mysqlcheck -0 DatabaseName -u root -p****** 
#或
mysqlcheck -0 --all-databases -u root -p****** 
```

![image-20230128141601059](/image-20230128141601059.png)

#### 小结

上述这些方法都是有利有弊的。比如：

- 修改数据类型，节省存储空间的同时，你要考虑到数据不能超过取值范围；
- 增加冗余字段的时候，不要忘了确保数据一致性；
- 把大表拆分，也意味着你的查询会增加新的连接，从而增加额外的开销和运维的成本。

因此，你一定要**结合实际的业务需求进行权衡**。

### 大表优化

当MySQL单表记录数过大时，数据库的CRUD性能会明显下降，一些常见的优化措施如下:

#### 限定查询的范围

**禁止不带任何限制数据范围条件的查询语句。**比如：我们当用户在查询订单历史的时候，我们可以控制在一个月的范围内；

#### 读/写分离

经典的数据库拆分方案，主库负责写，从库负责读。

- 一主一从模式：

![image-20230128141642194](/image-20230128141642194.png)

- 双主双从模式：

![image-20230128141653260](/image-20230128141653260.png)

#### 垂直拆分

当数据量级达到 **千万级** 以上时，有时候我们需要把一个数据库切成多份，放到不同的数据库服务器上，减少对单一数据库服务器的访问压力。

![image-20230128141709756](/image-20230128141709756.png)

如果数据库中的数据表过多，可以采用**垂直分库**的方式，将关联的数据表部署在同一个数据库上。

如果数据表中的列过多，可以采用**垂直分表**的方式，将一张数据表分拆成多张数据表，把经常一起使用的列放到同一张表里。

![image-20230128141720377](/image-20230128141720377.png)

**垂直拆分的优点：** 可以使得列数据变小，在查询时减少读取的Block数，减少I/O次数。此外，垂直分区可以简化表的结构，易于维护。

**垂直拆分的缺点：** 主键会出现冗余，需要管理冗余列，并会引起 JOIN 操作。此外，垂直拆分会让事务变得更加复杂。

#### 水平拆分

- 尽量控制单表数据量的大小，建议控制在**1000万以内**。1000万并不是MySQL数据库的限制， 过大会造成修改表结构、备份、恢复都会有很大的问题。此时可以用**历史数据归档**(应用于日志数据) ，**水平分表**(应用于业务数据)等手段来控制数据量大小。
- 这里我们主要考虑业务数据的水平分表策略。将大的数据表按照**某个属性维度**分拆成不同的小表，每张小表保持相同的表结构。比如你可以按照年份来划分，把不同年份的数据放到不同的数据表中。2017 年、2018 年和2019年的数据就可以分别放到三E张数据表中。
- 水平分表仅是解决了单一表数据过大的问题，但由于表的数据还是在同一台机器上，其实对于提升MySQL并发能力没有什么意义，所以**水平拆分最好分库**，从而达到分布式的目的。

![image-20230128141733202](/image-20230128141733202.png)

水平拆分能够支持非常大的数据量存储，应用端改造也少，但**分片事务难以解决，跨节点Join性能较差**，逻辑复杂。《Java工程师修炼之道》的作者推荐**尽量不要对数据进行分片，因为拆分会带来逻辑、部署、运维的各种复杂度**，一般的数据表在优化得当的情况下支撑千万以下的数据量是没有太大问题的。

如果实在要分片，尽量选择客户端分片架构，这样可以减少一次和中间件的网络I/O。 

下面补充一下数据库分片的两种常见方案：

- **客户端代理： 分片逻辑在应用端，封装在jar包中，通过修改或者封装JDBC层来实现。** 当当网的**Sharding-JDBC** 、阿里的TDDL是两种比较常用的实现。
- **中间件代理： 在应用和数据中间加了一个代理层。分片逻辑统一维护在中间件服务中**。我们现在谈的 **Mycat** 、360的Atlas、网易的DDB等等都是这种架构的实现。

### 其他调优策略

#### 服务器语句超时处理

在MySQL 8.0中可以设置 **服务器语句超时的限制** ，单位可以达到 **毫秒级别** 。当中断的执行语句超过设置的毫秒数后，服务器将终止查询影响不大的事务或连接，然后将错误报给客户端。

设置服务器语句超时的限制，可以通过设置系统变量 MAX_EXECUTION_TIME 来实现。默认情况下，**MAX_EXECUTION_TIME**的值为0，代表没有时间限制。 例如：

```
SET GLOBAL MAX_EXECUTION_TIME=2000;
SET SESSION MAX_EXECUTION_TIME=2000; #指定该会话中SELECT语句的超时时间
```

#### 创建全局通用表空间

MySQL 8.0使用**CREATE TABLESPACE** 语句来创建一个**全局通用表空间**。全局表空间可以被所有的数据库的表共享，而且相比于独享表空间，**使用手动创建共享表空间可以节约元数据方面的内存**。可以在创建表的时候，指定属于哪个表空间，也可以对已有表进行表空间修改等。

下面创建名为atguigu1的共享表空间，SQL语句如下：

```
CREATE TABLESPACE atguigu1 ADD DATAFILE 'atguigu1.ibd' file_block_size=16k;
```

指定表空间，SQL语句如下：

```
CREATE TABLE test(id INT,NAME VARCHAR(10)) ENGINE=INNODB DEFAULT CHARSET utf8mb4 TABLESPACE atguigu1;
```

也可以通过ALTER TABLE语句指定表空间，SQL语句如下：

```
ALTER TABLE test TABLESPACE atguigu1;
```

删除表空间，SQL语句如下：

```
DROP TABLESPACE atguigu1;
```

![image-20230128141748612](/image-20230128141748612.png)

报错：表空间非空，先删除里面的表信息，SQL语句如下：

```
DROP TABLE test;
```

然后删除成功

#### MySQL8.0新特性：隐藏索引对调优的帮助

不可见索引的特性对于性能调试非常有用。在MySQL 8.0中，索引可以被“隐藏'和“显示”。**当一个索引被隐藏时，它不会被查询优化器所使用。**也就是说，管理员可以隐藏一个索引，然后观察对数据库的影响。如果数据库性能有所下降，就说明这个索引是有用的，于是将其“恢复显示’'即可；如果数据库性能看不出变化，就说明这个索引是多余的，可以删掉了。

需要注意的是当索引被隐藏时，它的内容仍然是和正常索引一样**实时更新**的。如果一个索引需要长期被隐藏，那么可以将其删除，因为索引的存在会影响插入、更新和删除的性能。

**数据表中的主键不能被设置为invisible**。（主键不能隐藏）

## 一条sql的查询

一条sql的查询大致分为三个阶段

+ 连接阶段：经过三次握手建立连接之后，Mysql服务器会对tcp传输过来的用户名和密码进行进行校验。主要校验的内容是查询mysql表中信息，查看用户名密码是否一致，若一致数据库会查出该用户的权限，再对用户进行权限校验。当然一个mysql服务器不只有一个用户访问，mysql有专门的TCP连接池，采用长连接模式复用TCP连接。
+ 解析与优化
  + sql接口
    + 接收用户的SQL命令，并且返回用户需要查询的结果
    + 会根据不同的sql触发不同的sql接口
  + 解析器
    + 语法分析：mysql根据你输入的sql语句判断你这条sql是要做什么的（select?update?insert?）
    + 语义分析：根据语法分析的结果判断你的sql对不对，不对会提示sql出错，每晚提的话，会生成一棵语法树
  + 优化器：
    + 逻辑优化：用另一种方式重写sql
    + 物理优化：索引
    + 选择最好的sql执行方式，生成执行计划
  + 查询缓存（5.7以后就没了）
    + 完全匹配才可以
    + 鸡肋，所以后续删除了
  + 执行器
    + 根据优化器提供的执行计划，执行sql语句
    + 同时执行器还会在sql执行的过程中操作操作redo log、bin log日志
+ 存储引擎
  + 先在buffer pool里面查找，如果没有的话，从索引的根开始查找到，找到对应的页之后，将页缓存到buffer pool中，在页中数据是以单向链表存储的，为了提高查询效率，mysql专门设计了页目录这个模块，通过对槽的二分查找快速找出数据所在的分组，然后对分组中的链表进行遍历，找到对应的数据。

# 15.事务篇

## 事务基础知识

### 数据库事务概述

事务是数据库区别于文件系统的重要特性之一，当我们有了事务就会让数据库始终保持一致性 ，同时我们还能通过事务的机制恢复到某个时间点，这样可以保证已提交到数据库的修改不会因为系统崩溃而丢失。

#### 存储引擎支持情况

SHOW ENGINES 命令来查看当前 MySQL 支持的存储引擎都有哪些，以及这些存储引擎是否支持事务。

![image-20230128144208660](/image-20230128144208660.png)

能看出在 MySQL 中，只有InnoDB 是支持事务的。

#### 基本概念

**事务**：一组逻辑操作单元，使数据从一种状态变换到另一种状态。

**事务处理的原则**：保证所有事务都作为 一个**工作单元** 来执行，即使出现了故障，都不能改变这种执行方式。当在一个事务中执行多个操作时，要么所有的事务都被提交**( commit )**，那么这些修改就 **永久** 地保存下来；要么数据库管理系统将 **放弃** 所作的所有 **修改** ，整个事务**回滚( rollback )**到最初状态。

```
#案例:AA用户给BB用户转账100
update account set money = money - 100 where name ='AA' ;
#服务器宕机
update account set money = money + 100 where name ='BB' ;
```

#### 事务的ACID

- **原子性（atomicity）：**

原子性是指事务是一个不可分割的工作单位，要么全部提交，要么全部失败回滚。

- **一致性（consistency）：**

（国内很多网站上对一致性的阐述有误，具体你可以参考 Wikipedia 对Consistency的阐述）

根据定义，一致性是指事务执行前后，数据从一个 **合法性状态** 变换到另外一个 **合法性状态** 。这种状态是 **语义**上 的而不是语法上的，跟具体的业务有关。

那什么是合法的数据状态呢？满足 **预定的约束** 的状态就叫做合法的状态。通俗一点，这状态是由你自己来定义的（比如满足现实世界中的约束）。满足这个状态，数据就是一致的，不满足这个状态，数据就是不一致的！如果事务中的某个操作失败了，系统就会自动撤销当前正在执行的事务，返回到事务操作之前的状态。

**举例1：**A账户有200元，转账300元出去，此时A账户余额为-100元。你自然就发现了此时数据是不一致的，为什么呢？因为你定义了一个状态，余额这列必须>= 0。

**举例2：**A账户200元，转账50元给B账户，A账户的钱扣了，但是B账户因为各种意外，余额并没有增加。你也知道此时数据是不一致的， 为什么呢？因为你定义了一个状态，要求A+B的总余额必须不变。

**举例3：**在数据表中我们将**姓名**字段设置为**唯一性约束** ，这时当事务进行提交或者事务发生回滚的时候，如果数据表中的姓名不唯一，就破坏了事务的一致性要求。

- **隔离型（isolation）：**

事务的隔离性是指一个事务的执行 **不能被其他事务干扰** ，即一个事务内部的操作及使用的数据对 **并发** 的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。

如果无法保证隔离性会怎么样？假设A账户有200元，B账户0元。A账户往B账户转账两次，每次金额为50元，分别在两个事务中执行。如果无法保证隔离性，会出现下面的情形：

```
UPDATE accounts SET money = money - 50 WHERE NAME = 'AA'; 

UPDATE accounts SET money = money + 50 WHERE NAME = 'BB';
```

![image-20220205111230646](file://C:/Users/%E5%B9%BB%E6%83%B3%E5%AE%B6%E6%87%92%E5%BE%97%E6%83%B3%E4%BA%86/AppData/Roaming/Typora/typora-user-images/image-20220205111230646.png?lastModify=1646465884)

- **持久性（durability）：**

持久性是指一个事务一旦被提交，它对数据库中数据的改变就是 **永久性的** ，接下来的其他操作和数据库故障不应该对其有任何影响。

持久性是通过 **事务日志** 来保证的。日志包括了 **重做日志** 和 **回滚日志** 。当我们通过事务对数据进行修改的时候，首先会将数据库的变化信息记录到重做日志中，然后再对数据库中对应的行进行修改。这样做的好处是，即使数据库系统崩溃，数据库重启后也能找到没有更新到数据库系统中的重做日志，重新执行，从而使事务具有持久性。

> 总结
>
> ACID是事务的四大特性，在这四个特性中，**原子性是基础，隔离性是手段，一致性是约束条件，而持久性是我们的目的**。
>
> 数据库事务，其实就是数据库设计者为了方便起见，把需要保证**原子性、隔离性、一致性**和**持久性**的一个或多个数据库操作称为一个事务。

#### 事务的状态

我们现在知道 **事务** 是一个抽象的概念，它其实对应着一个或多个数据库操作，MySQL根据这些操作所执行的不同阶段把 **事务** 大致划分成几个状态：

- **活动的（active）**

  事务对应的数据库操作正在执行过程中时，我们就说该事务处在 **活动的** 状态。

- **部分提交的（partially committed**）

  当事务中的最后一个操作执行完，但由于操作都在内存中执行，所造成的影响并 **没有刷新到磁盘**时，我们就说该事务处在 **部分提交的** 状态。

- **失败的（failed）**

  当事务处在 **活动的** 或者 **部分提交的** 状态时，可能遇到了某些错误（数据库自身的错误、操作系统错误或者直接断电等）而无法继续执行，或者人为的停止当前事务的执行，我们就说该事务处在 **失败的** 状态。

- **中止的（aborted）**

  如果事务执行了一部分而变为 **失败的** 状态，那么就需要把已经修改的事务中的操作还原到事务执行前的状态。换句话说，就是要撤销失败事务对当前数据库造成的影响。我们把这个撤销的过程称之为 **回滚** 。当 **回滚** 操作执行完毕时，也就是数据库恢复到了执行事务之前的状态，我们就说该事务处在了 **中止的** 状态。

  举例：

```
UPDATE accounts SET money = money - 50 WHERE NAME = 'AA'; 
UPDATE accounts SET money = money + 50 WHERE NAME = 'BB';
```

- **提交的（committed）**

  当一个处在 **部分提交的** 状态的事务将修改过的数据都 **同步到磁盘** 上之后，我们就可以说该事务处在了 **提交的** 状态。

一个基本的状态转换图如下所示：

![image-20220205112808066](file://C:/Users/%E5%B9%BB%E6%83%B3%E5%AE%B6%E6%87%92%E5%BE%97%E6%83%B3%E4%BA%86/AppData/Roaming/Typora/typora-user-images/image-20220205112808066.png?lastModify=1646465884)

图中可见，只有当事务处于**提交的**或者**中止的**状态时，一个事务的生命周期才算是结束了。

**对于已经提交的事务来说，该事务对数据库所做的修改将永久生效；**

**对于处于中止状态的事务，该事务对数据库所做的所有修改都会被回滚到没执行该事务之前的状态。**

### 如何使用事务

使用事务有两种方式，分别为 **显式事务** 和 **隐式事务** 。 

```
#1.事务的完成过程
#步骤1：开启事务
#步骤2：一系列的DML操作 
#....
#步骤3：事务结束的状态：提交的状态(COMMIT) 、 中止的状态(ROLLBACK)
```

#### 显式事务

```
#如何开启？ 使用关键字：start transaction  或 begin

# start transaction 后面可以跟：read only / read write (默认) / with consistent snapshot 

#保存点(savepoint)
```

**步骤1：** START TRANSACTION 或者 BEGIN ，作用是显式开启一个事务。

```
mysql> BEGIN; 
#或者 
mysql> START TRANSACTION;
```

START TRANSACTION 语句相较于 BEGIN 特别之处在于，后边能跟随几个 修饰符 ： 

① **READ ONLY ：**标识当前事务是一个 **只读事务** ，也就是属于该事务的数据库操作只能读取数据，而不能修改数据。

> 补充：只读事务中只是不允许修改那些其他事务也能访问到的表中的数据，对于**临时表**来说(我们使用CREATE TMEPORARY TABLE创建的表)，由于它们只能在当前会话中可见，所以只读事务其实也是可以对临时表进行增、删、改操作的。

② **READ WRITE ：**标识当前事务是一个 **读写事务** ，也就是属于该事务的数据库操作既可以读取数据，也可以修改数据。

③ **WITH CONSISTENT SNAPSHOT ：**启动一致性读。

**步骤2**：一系列事务中的操作（主要是DML，不含DDL）

**步骤3：**提交事务 或 中止事务（即回滚事务）

```
# 提交事务。当提交事务后，对数据库的修改是永久性的。 
mysql> COMMIT;
# 回滚事务。即撤销正在进行的所有没有提交的修改
mysql> ROLLBACK;

# 将事务回滚到某个保存点。
mysql> ROLLBACK TO [SAVEPOINT]
#保存点(savepoint)
```

关于savepoint的相关操作：

```
#在事务中创建保存点， 方便后续针对保存点进行回滚。一个事务中可以存在多个保存点。
SAVEPOINT保存点名称;

#删除某个保存点。
RELEASE SAVEPOINT保存点名称:
```

#### 隐式事务

MySQL中有一个系统变量 autocommit ：

```
mysql> SHOW VARIABLES LIKE 'autocommit'; #默认是ON
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| autocommit    | ON    | 
+---------------+-------+
1 row in set (0.01 sec)
```

默认情况下，如果我们不显式的使用**START TRANSACTION** 或者**BEGIN**语句开启一个事务，那么每一条语句都算是一个独立的事务，这种特性称之为事务的**自动提交**。也就是说，不以START TRANSACTION 或者BEGIN语句显式的开启一个事务，那么下边这两条语句就相当于放到两个独立的事务中去执行：

```
UPDATE account SET balance = balance - 10 WHERE id = 1; #此时这条DML操作是一个独立的事务

UPDATE account SET balance = balance + 10 WHERE id = 2; #此时这条DML操作是一个独立的事务
```

当然，如果我们想关闭这种 **自动提交** 的功能，可以使用下边两种方法之一：

- 显式的的使用 START TRANSACTION 或者 BEGIN 语句开启一个事务。这样在本次事务提交或者回滚前会暂时关闭掉自动提交的功能。
- 把系统变量 autocommit 的值设置为 OFF ，就像这样：

```
SET autocommit = OFF; 
#或
SET autocommit = 0;
```

这样的话，我们写入的多条语句就算是属于同一个事务了，直到我们显式的写出COMMIT语句来把这个事务提交掉，或者显式的写出ROLLBACK语句来把这个事务回滚掉。

```
#方式1：
SET autocommit = FALSE; #针对于DML操作是有效的，对DDL操作是无效的。

UPDATE account SET balance = balance - 10 WHERE id = 1;

UPDATE account SET balance = balance + 10 WHERE id = 2; 

COMMIT; #或rollback;

#方式2：我们在autocommit为true的情况下，使用start transaction 或begin开启事务，那么DML操作就不会自动提交数据

START TRANSACTION;

UPDATE account SET balance = balance - 10 WHERE id = 1;

UPDATE account SET balance = balance + 10 WHERE id = 2; 

COMMIT; #或rollback;
```

> 补充：Oracle 默认不自动提交，需要手写COMMIT命令，而MySQL默认自动提交。

#### 隐式提交数据的情况

- **数据定义语言(DDL)**

  数据库对象，指的就是**数据库、表、视图、存储过程**等结构。当我们使用**CREATE、ALTER、 DROP** 等语句去修改数据库对象时，就会隐式的提交前边语句所属于的事务。即：

  ```
  BEGIN;
  
  SELECT ... #事务中的一条语句
  UPDATE ... #事务中的一条语句
  ... #事务中的其它语句
  
  CREATE TABLE ... #此语句会隐 式的提交前边语句所属于的事务
  ```

- **隐式使用或修改mysql数据库中的表**

  当我们使用**ALTER USER、CREATE USER、DROP USER、GRANT 、RENAME USER、REVOKE 、SET PASSWORD**等语句时也会隐式的提交前边语句所属于的事务。

- **事务控制或关于锁定的语句**

  ① 当我们在一个事务还没提交或者回滚时就又使用 **START TRANSACTION** 或者 **BEGIN** 语句开启了另一个事务时，会 **隐式的提交** 上一个事务。即：

  ```
  BEGIN;
  
  SELECT ... #事务中的一条语句
  UPDATE ... #事务中的一条语句
  ... #事务中的其它语句
  
  BEGIN;#此语句会隐式的提交前面语句所属于的事务
  ```

  ② 当前的 **autocommit** 系统变量的值为 OFF ，我们手动把它调为 ON 时，也会 **隐式的提交** 前边语句所属的事务。

  ③ 使用 **LOCK TABLES 、 UNLOCK TABLES** 等关于锁定的语句也会 **隐式的提交** 前边语句所属的事务。

- **加载数据的语句**

  使用**LOAD DATA** 语句来批量往数据库中导入数据时，也会**隐式的提交**前边语句所属的事务。

- **关于MySQL复制的一些语句**

  使用**START SLAVE、 STOP SLAVE、 RESET SLAVE 、CHANGE MASTER TO**等语句时会**隐式的提交**前边语句所属的事务。

- **其它的一些语句**

  使用**ANALYZE TABLE、 CACHE INDEX、 CHECK TABLE、 FLUSH、 LOAD INDEX INTO CACHE 、OPTIMIZE TABLE、REPAIR TABLE、RESET** 等语句也会**隐式的提交**前边语句所属的事务。


#### 使用举例：提交和回滚

```
#情况1：
CREATE TABLE user3(NAME VARCHAR(15) PRIMARY KEY);

SELECT * FROM user3;

BEGIN;
INSERT INTO user3 VALUES('张三'); #此时不会自动提交数据
COMMIT;

BEGIN; #开启一个新的事务
INSERT INTO user3 VALUES('李四'); #此时不会自动提交数据
INSERT INTO user3 VALUES('李四'); #受主键的影响，不能添加成功
ROLLBACK;

SELECT * FROM user3;
```

![image-20230128150556155](/image-20230128150556155.png)

```
#情况2：
TRUNCATE TABLE user3;  #DDL操作会自动提交数据，不受autocommit变量的影响。

SELECT * FROM user3;

BEGIN;
INSERT INTO user3 VALUES('张三'); #此时不会自动提交数据
COMMIT;

INSERT INTO user3 VALUES('李四');# 默认情况下(即autocommit为true)，DML操作也会自动提交数据。
INSERT INTO user3 VALUES('李四'); #事务的失败的状态

ROLLBACK;

SELECT * FROM user3;
```

![image-20230128150605452](/image-20230128150605452.png)

```
#情况3：
TRUNCATE TABLE user3;
 
SELECT * FROM user3;

SELECT @@completion_type;
```

![image-20230128150614363](/image-20230128150614363.png)

```
#开启链式
SET @@completion_type = 1;

BEGIN;
INSERT INTO user3 VALUES('张三'); 
COMMIT;

SELECT * FROM user3;

INSERT INTO user3 VALUES('李四');
INSERT INTO user3 VALUES('李四'); 

ROLLBACK;

SELECT * FROM user3;
```

![image-20230128150628602](/image-20230128150628602.png)

这里我讲解下MySQL中`completion_type`参数的作用，实际上这个参数有3种可能：

1. `completion`=0，这是**默认情况**。当我们执行COMMIT的时候会提交事务，在执行下一个事务时，还需要使用`START TRANSACTION`或者`BEGIN`来开启。
2. `completion`=1，这种情况下， 当我们提交事务后，相当于执行了`COMMIT AND CHAIN`，也就是开启一个**链式事务**，即当我们提交事务之后会开启一个相同隔离级别的事务。
3. `completion`=2，这种情况下`COMMIT=COMMIT AND RELEASE` ，也就是当我们提交后，会自动与服务器断开连接。

> 当我们设置autocommit=0时，不论是否采用START TRANSACTION或者BEGIN的方式来开启事务，都需要用COMMIT进行提交，让事务生效，使用ROLLBACK对事务进行回滚。
>
> 当我们设置autocommit=1时，每条SQL语句都会自动进行提交。
>
> 不过这时，如果你采用START TRANSACTION或者BEGIN的方式来显式地开启事务，那么这个事务只有在COMMIT时才会生效，在ROLLBACK时才会回滚。

#### 使用举例：测试不支持事务的engine

```
#举例2：体会INNODB 和 MyISAM

CREATE TABLE test1(i INT) ENGINE = INNODB;

CREATE TABLE test2(i INT) ENGINE = MYISAM;

#针对于innodb表
BEGIN
INSERT INTO test1 VALUES (1);
ROLLBACK;

SELECT * FROM test1;
```

![image-20230128150726263](/image-20230128150726263.png)

```
#针对于myisam表:不支持事务
BEGIN
INSERT INTO test2 VALUES (1);
ROLLBACK;

SELECT * FROM test2;
```

![image-20230128150735208](/image-20230128150735208.png)

#### 使用举例：SAVEPOINT

```
#举例3：体会savepoint

CREATE TABLE user3(
    NAME VARCHAR(15),
    balance DECIMAL(10,2)
);

BEGIN
INSERT INTO user3(NAME,balance) VALUES('张三',1000);
COMMIT;

SELECT * FROM user3;
```

![image-20230128151545627](/image-20230128151545627.png)

```
BEGIN;
UPDATE user3 SET balance = balance - 100 WHERE NAME = '张三';
#900
UPDATE user3 SET balance = balance - 100 WHERE NAME = '张三';
#800

SAVEPOINT s1;#设置保存点

UPDATE user3 SET balance = balance + 1 WHERE NAME = '张三';
#801
ROLLBACK TO s1; #回滚到保存点
#800
SELECT * FROM user3;
```

![image-20230128151554323](/image-20230128151554323.png)

```
ROLLBACK; #回滚操作

SELECT * FROM user3;
```

![image-20230128151602536](/image-20230128151602536.png)

### 事务隔离级别

MySQL是一个 **客户端／服务器** 架构的软件，对于同一个服务器来说，可以有若干个客户端与之连接，每个客户端与服务器连接上之后，就可以称为一个会话（ **Session** ）。每个客户端都可以在自己的会话中向服务器发出请求语句，一个请求语句可能是某个事务的一部分，也就是对于服务器来说可能同时处理多个事务。事务有 **隔离性** 的特性，理论上在某个事务 **对某个数据进行访问** 时，其他事务应该进行**排队** ，当该事务提交之后，其他事务才可以继续访问这个数据。但是这样对 **性能影响太大** ，我们既想保持事务的隔离性，又想让服务器在处理访问同一数据的多个事务时 **性能尽量高些** ，那就看二者如何权衡取舍了。

#### 数据准备

我们需要创建一个表：

```
CREATE TABLE student ( 
	studentno INT, 
	name VARCHAR(20),
    class varchar(20),
    PRIMARY KEY (studentno) 
) Engine=InnoDB CHARSET=utf8;
```

然后向这个表里插入一条数据：

```
INSERT INTO student VALUES(1, '小谷', '1班');
```

现在表里的数据就是这样的：

```
mysql> select * from student; 
+-----------+--------+-------+
| studentno | name | class	 |
+-----------+--------+-------+
| 1 		| 小谷 	| 1班   | 
+-----------+--------+-------+
1 row in set (0.00 sec)
```

#### 数据并发问题

针对事务的隔离性和并发性，我们怎么做取舍呢？先看一下访问相同数据的事务在 **不保证串行执行** （就是执行完一个再执行另一个）的情况下可能会出现哪些问题：

1. **脏写（ Dirty Write ）**

对于两个事务 Session A、Session B，如果事务Session A **修改了** 另一个 **未提交** 事务Session B **修改过** 的数据，那就意味着发生了 **脏写**，示意图如下：

  ![image-20230128153409218](/image-20230128153409218.png)

  **A改了B刚改的**

Session A和Session B各开启了一个事务，Session B中的事务先将studentno列为1的记录的name列更新为'李四' ，然后Session A中的事务接着又把这条studentno列为1的记录的name列更新为'张三。**如果之后Session B中的事务进行了回滚，那么Session A中的更新也将不复存在**，这种现象就称之为脏写。这时Session A中的事务就没有效果了，明明把数据更新了，最后也提交事务了,最后看到的数据什么变化也没有。这里大家对事务的隔离级比较了解的话，会发现默认隔离级别下，上面SessionA中的更新语句会处于等待状态， 这里只是跟大家说明一下会出现这样现象。

1. **脏读（ Dirty Read ）**

对于两个事务 Session A、Session B，Session A **读取** 了已经被 Session B **更新** 但还 **没有被提交** 的字段。之后若 Session B **回滚** ，Session A **读取** 的内容就是 **临时且无效** 的。

![image-20230128153422212](/image-20230128153422212.png)

**A读到了B没提交的修改**

Session A和Session B各开启了一个事务，Session B中的事务先将studentno列为1的记录的name列更新为'张三'，然后Session A中的事务再去查询这条studentno为1的记录，如果读到列name的值为'张三'，而Session B中的事务稍后进行了回滚，那么Session A中的事务相当于读到了一个不存在的数据，这种现象就称之为 **脏读** 。 

1. **不可重复读（ Non-Repeatable Read ）**

对于两个事务Session A、Session B，Session A **读取** 了一个字段，然后 Session B **更新** 了该字段。 之后Session A **再次读取** 同一个字段， **值就不同** 了。那就意味着发生了不可重复读。

  ![image-20230128153445330](/image-20230128153445330.png)

  **A重复执行一个语句得到的是不同的结果，因为B在修改**

我们在Session B中提交了几个 **隐式事务** （注意是隐式事务，意味着语句结束事务就提交了），这些事务都修改了studentno列为1的记录的列name的值，每次事务提交之后，如果Session A中的事务都可以查看到最新的值，这种现象也被称之为 **不可重复读** 。 

1. **幻读（ Phantom ）**

对于两个事务Session A、Session B, Session A 从一个表中 **读取** 了一个字段, 然后 Session B 在该表中 **插 入** 了一些新的行。 之后, 如果 Session A **再次读取** 同一个表, 就会多出几行。那就意味着发生了幻读。

  ![image-20230128153506811](/image-20230128153506811.png)

  **B增加已一条数据后，A读不到，但是增加相同字段时显示已经存在。**

Session A中的事务先根据条件 studentno > 0这个条件查询表student，得到了name列值为'张三'的记录；之后Session B中提交了一个 **隐式事务** ，该事务向表student中插入了一条新记录；之后Session A中的事务再根据相同的条件 studentno > 0查询表student，得到的结果集中包含Session B中的事务新插入的那条记录，这种现象也被称之为 **幻读** 。我们把新插入的那些记录称之为 **幻影记录** 。

  **注意1：**

有的同学会有疑问，那如果SessionB中**删除**了一些符合 studentno > 0 的记录而不是插入新记录,那Session A之后再根据studentno > 0的条件读取的记录变少了，这种现象算不算幻读呢？**这种现象不属于幻读**，幻读强调的是一个事务按照某个相同条件多次读取记录时，后读取时读到了之前没有读到的记录。

  **注意2：**

那对于先前已经读到的记录，之后又读取不到这种情况，算啥呢？这相当于对每一条记录都发生了 **不可重复读**的现象。幻读只是重点强调了读取到了之前读取没有获取到的记录。

#### SQL中的四种隔离级别

上面介绍了几种并发事务执行过程中可能遇到的一些问题，这些问题有轻重缓急之分，我们给这些问题按照严重性来排一下序：

```
脏写 > 脏读 > 不可重复读 > 幻读
```

我们愿意舍弃一部分隔离性来换取一部分性能在这里就体现在：设立一些隔离级别，隔离级别越低，并发问题发生的就越多。 **SQL标准** 中设立了4个 **隔离级别** ： 

- **READ UNCOMMITTED** ：读未提交，在该隔离级别，所有事务都可以看到其他未提交事务的执行结果。不能避免脏读、不可重复读、幻读。
- **READ COMMITTED** ：读已提交，它满足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变。这是大多数数据库系统的默认隔离级别（但不是MySQL默认的）。可以避免脏读，但不可重复读、幻读问题仍然存在。
- **REPEATABLE READ** ：可重复读，事务A在读到一条数据之后，此时事务B对该数据进行了修改并提交，那么事务A再读该数据，读到的还是原来的内容。可以避免脏读、不可重复读，但幻读问题仍然存在。这是MySQL的默认隔离级别。
- **SERIALIZABLE** ：可串行化，确保事务可以从一个表中读取相同的行。在这个事务持续期间，禁止其他事务对该表执行插入、更新和删除操作。所有的并发问题都可以避免，但性能十分低下。能避免脏读、不可重复读和幻读。

**SQL标准** 中规定，针对不同的隔离级别，并发事务可以发生不同严重程度的问题，具体情况如下：

![image-20230128162231805](/image-20230128162231805.png)

**脏写 怎么没涉及到？因为脏写这个问题太严重了，不论是哪种隔离级别，都不允许脏写的情况发生。**

不同的隔离级别有不同的现象，并有不同的锁和并发机制，隔离级别越高，数据库的并发性能就越差，4种事务隔离级别与并发性能的关系如下：

![image-20230128162242569](/image-20230128162242569.png)

#### MySQL支持的四种事务隔离界别

不同的数据库厂商对SQL标准中规定的四种隔离级别支持不一样。比如，**Oracle 就只支持READ COMMITTED (默认隔离级别)和SERIALIZABLE隔离级别**。MySQL虽然支持4种隔离级别，但与SQL标准中所规定的各级隔离级别允许发生的问题却有些出入，MySQL在REPEATABLE READ隔离级别下，是可以禁止幻读问题的发生的，禁止幻读的原因我们在第16章讲解。

MySQL的默认隔离级别为REPEATABLE READ，我们可以手动修改一下事务的隔离级别。

```
# 查看隔离级别，MySQL 5.7.20的版本之前：
mysql>
SHOW VARIABLES LIKE 'tx_isolation';
+---------------+-----------------+
| Variable_name | Value           |
+---------------+-----------------+
| tx_isolation  | REPEATABLE-READ |
+---------------+-----------------+
1 row in set (0.00 sec)

# MySQL 5.7.20版本之后，引入transaction_isolation来替换tx_isolation

# 查看隔离级别，MySQL 5.7.20的版本及之后：
mysql> SHOW VARIABLES LIKE 'transaction_isolation';
+-----------------------+-----------------+
| Variable_name         | Value           |
+-----------------------+-----------------+
| transaction_isolation | REPEATABLE-READ |
+-----------------------+-----------------+
1 row in set(0.02 sec)

#或者不同MySQL版本中都可以使用的：
SELECT @@transaction_isolation;
```

#### 如何设置事务的隔离级别

**通过下面的语句修改事务的隔离级别：**

```
SET [GLOBAL|SESSION] TRANSACTION ISOLATION LEVEL 隔离级别; 
#其中，隔离级别格式： 
> READ UNCOMMITTED 
> READ COMMITTED 
> REPEATABLE READ 
> SERIALIZABLE
```

或者：（推荐）

```
SET [GLOBAL|SESSION] TRANSACTION_ISOLATION = '隔离级别' 
#其中，隔离级别格式： 
> READ-UNCOMMITTED 
> READ-COMMITTED 
> REPEATABLE-READ 
> SERIALIZABLE
```

**关于设置时使用GLOBAL或SESSION的影响：**

- 使用`GLOBAL` 关键字（在全局范围影响）：

  ```
  SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE; 
  #或
  SET GLOBAL TRANSACTION_ISOLATION = 'SERIALIZABLE';
  ```

  则：

  - 当前已经存在的会话无效
  - 只对执行完该语句之后产生的会话起作用

- 使用 `SESSION` 关键字（在会话范围影响）：

  ```
  SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE; 
  #或
  SET SESSION TRANSACTION_ISOLATION = 'SERIALIZABLE';
  ```

  则：

  - 对当前会话的所有后续的事务有效
  - 如果在事务之间执行，则对后续的事务有效
  - 该语句可以在已经开启的事务中间执行，但不会影响当前正在执行的事务

> 小结：
>
> 数据库规定了多种事务隔离级别，不同隔离级别对应不同的干扰程度，隔离级别越高，数据一致性就越好，但并发性越弱。

#### 不同隔离级别举例

**两个会话必须在相同的隔离级别下**

初始化数据：

```
TRUNCATE TABLE account;

INSERT INTO account VALUES(1,'张三','100'),(2,'李四','0');
```

表中的数据如下：

```
select * from account;
```

##### 演示1. 读未提交之脏读

**事务1读取到了事务2修改了但还未提交的数据。**

举例1：

事务1对数据进行修改，但还未提交

![image-20230128163327112](/image-20230128163327112.png)

但是事务2读取到了

![image-20230128163338553](/image-20230128163338553.png)

举例2：

事务1对数据进行修改，但未提交

张三给李四赚钱

![image-20230128163411439](/image-20230128163411439.png)

事务2操作数据时，因为事务1在对数据修改，所以要等待事务1提交，才能执行语句

![image-20230128163425413](/image-20230128163425413.png)

事务1回滚后（事务1对数据的操作无效了），事务2执行。

但是事务2并不知道事务1回滚了，因此还执行了一条语句

张三不转了，李四不知道，李四给张三转回去了

![image-20230128163525455](/image-20230128163525455.png)

但是因为脏读，就会出现以下情况

![image-20230128163539760](/image-20230128163539760.png)

![image-20230128164348321](/image-20230128164348321.png)

事务1和事务2的执行流程如下：

![image-20230128164358198](/image-20230128164358198.png)

##### 演示2：读已提交

避免了脏读：事务1读不到事务2修改了但还未提交的数据。

**未结束的事务1读取到了已经提交数据的事务2对数据的修改。**

事务1提交对数据的修改

![image-20230128164639416](/image-20230128164639416.png)

未提交的事务2读取到了事务1已经提交的数据

![image-20230128164652630](/image-20230128164652630.png)

事务2的角度看来就是：两次查询的数据不一致。（他并不知道事务1所做的修改）

![image-20230128164704036](/image-20230128164704036.png)

设置隔离级别为可重复读，事务的执行流程如下：

**未结束的事务1读取不到已经提交数据的事务2对数据的修改。**

![image-20230128164922679](/image-20230128164922679.png)

##### 演示4：幻读

**未提交的事务1读取到了已经提交的事务2 提交的新增数据。**

![image-20230128164934930](/image-20230128164934930.png)

事务一提交了新增的数据

![image-20230128164948182](/image-20230128164948182.png)

事务二虽然读取不到数据，但是在自己进行新增时，会报错数据已存在。

![image-20230128164959025](/image-20230128164959025.png)

发生了幻读。

这里要灵活的理解读取的意思，第一次select是读取,第二次的insert其实也属于隐式的读取，只不过是在mysql的机制中读取的，插入数据也是要先读取一下有没有主键冲突才能决定是否执行插入。

幻读，并不是说两次读取获取的结果集不同，幻读侧重的方面是某一次的select操作得到的结果所表征的数据状态无法支撑后续的业务操作。更为具体一些：select 某记录是否存在，不存在，准备插入此记录，但执行insert时发现此记录已存在，无法插入，此时就发生了幻读。

在RR隔离级别（默认）下，step1、 step2 是会正常执行的，step3 则会报错主键冲突，对于事务1的业务来说是执行失败的，这里事务1就是发生了幻读，因为事务1在step1中读取的数据状态并不能支撑后续的业务操作，事务1:“见鬼了，我刚才读到的结果应该可以支持我这样操作才对啊，为什么现在不可以”。事务1不敢相信的又执行了step4，发现和setp1读取的结果是一样的(RR下的MVCC机制)。此时， 幻读无疑已经发生，事务1无论读取多少次，都查不到id =3的记录，但它的确无法插入这条他通过读取来认定不存在的记录(此数据已被事务2插入)，对于事务1来说，它幻读了。

其实RR也是可以避免幻读的，通过对select操作手动加行**X锁(独占锁)** (SELECT ...  FOR UPDATE这也正是SERIALIZABLE隔离级别下会隐式为你做的事情)。同时，即便当前记录不存在，比如id=3是不存在的，当前事务也会获得一把记录锁(因为InnoDB的**行锁锁定的是索引**，故记录实体存在与否没关系，存在就加**行X锁**，不存在就加**间隙锁**)， 其他事务则无法插入此索引的记录，故杜绝了幻读。

在**SERIALIZABLE隔离级别**下，step1 执行时是会隐式的添加**行(X)锁/ gap(X)锁**的,从而step2会被阻塞，step3会正常执行，待事务1提交后，事务2才能继续执行(主键冲突执行失败)， 对于事务1来说业务是正确的，成功的阻塞扼杀了扰乱业务的事务2，对于事务1来说他前期读取的结果是可以支撑其后续业务的。

所以MySQL的幻读并非什么读取两次返回结果集不同，而是事务在插入事先检测不存在的记录时，惊奇的发现这些数据已经存在了，之前的检测读获取到的数据如同鬼影一般。

### 事务常见的分类

从事务理论的角度来看，可以把事务分为以下几种类型：

- 扁平事务（Flat Transactions）
- 带有保存点的扁平事务（Flat Transactions with Savepoints）
- 链事务（Chained Transactions）
- 嵌套事务（Nested Transactions）
- 分布式事务（Distributed Transactions）

下面分别介绍这几种类型：

1. **扁平事务**是事务类型中最简单的一种，但是在实际生产环境中,这可能是使用最频繁的事务，在扁平事务中，所有操作都处于同一层次，其由BEGIN WORK开始，由COMMIT WORK或ROLLBACK WORK结束，其间的操作是原子的，要么都执行，要么都回滚,因此,扁平事务是应用程序成为原子操作的基本组成模块。扁平事务虽然简单,但是在实际环境中使用最为频繁，也正因为其简单,使用频繁,故每个数据库系统都实现了对扁平事务的支持。扁平事务的主要限制是不能提交或者回滚事务的某一部分， 或分几个步骤提交。

   扁平事务一般有三种不同的结果：

   ①事务成功完成。在平常应用中约占所有事务的96%。

   ②应用程序要求停止事务。比如应用程序在捕获到异常时会回滚事务，约占事务的3%。

   ③外界因素强制终止事务。如连接超时或连接断开，约占所有事务的1%。

2. **带有保存点的扁平事务** 除了支持扁平事务支持的操作外，还允许在事务执行过程中回滚到同一事务中较早的一个状态。这是因为某些事务可能在执行过程中出现的错误并不会导致所有的操作都无效,放弃整个事务不合乎要求，开销太大。

   **保存点**(Savepoint)用来通知事务系统应该记住事务当前的状态，以便当之后发生错误时，事务能回到保存点当时的状态。对于扁平的事务来说，隐式的设置了一个保存点，然而在整个事务中，只有这一个保存点，因此,回滚只能会滚到事务开始时的状态。

3. **链事务** 是指一个事务由多个子事务链式组成，它可以被视为保存点模式的一个变种。带有保存点的扁平事务，当发生系统崩溃时，所有的保存点都将消失，这意味着当进行恢复时，事务需要从开始处重新执行，而不能从最近的一个保存点继续执行。链事务的思想 是:在提交一个事务时，释放不需要的数据对象，将必要的处理上下文隐式地传给下一个要开始的事务， 前一个子事务的提交操作和下一个子事务的开始操作合并成一个原子操作，这意味着下一个事务将看到上一个事务的结果,，好像在一个事务中进行一样。这样，**在提交子事务时就可以释放不需要的数据对象，而不必等到整个事务完成后才释放**。其工作方式如下:

   ![image-20220205134925993](file://C:/Users/%E5%B9%BB%E6%83%B3%E5%AE%B6%E6%87%92%E5%BE%97%E6%83%B3%E4%BA%86/AppData/Roaming/Typora/typora-user-images/image-20220205134925993.png?lastModify=1646465884)

   链事务与带有保存点的扁平事务的不同之处体现在：

   ①带有保存点的扁平事务能回滚到任意正确的保存点，而链事务中的回滚仅限于当前事务，即只能恢复到最近的一个保存点。

   ②对于锁的处理，两者也不相同，链事务在执行COMMIT后即释放了当前所持有的锁，而带有保存点的扁平事务不影响迄今为止所持有的锁。

4. **嵌套事务**是一个层次结构框架，由一个顶层事务(Top-Level Transaction) 控制着各个层次的事务，顶层事务之下嵌套的事务被称为子事务(Subtransaction) ，其控制着每一个局部的变换，子事务本身也可以是嵌套事务。因此，嵌套事务的层次结构可以看成是一棵树。

5. **分布式事务**通常是在一个分布式环境下运行的扁平事务，因此，需要根据数据所在位置访问网络中不同节点的数据库资源。例如，一个银行用户从招商银行的账户向工商银行的账户转账1000元，这里需要用到分布式事务，因为不能仅调用某一家银行的数据库就完成任务。

## MySQL事务日志

事务有4种特性：原子性、一致性、隔离性和持久性。那么事务的四种特性到底是基于什么机制实现呢？

- 事务的**隔离性**由 **锁机制** 实现。
- 而事务的原子性、一致性和持久性由事务的 redo 日志和undo 日志来保证。
  - REDO LOG 称为 **重做日志** ，提供再写入操作，恢复提交事务修改的页操作，用来保证事务的**持久性**。
  - UNDO LOG 称为 **回滚日志** ，回滚行记录到某个特定版本，用来保证事务的**原子性、一致性**。

有的DBA或许会认为 UNDO 是 REDO 的逆过程，其实不然。REDO 和UNDO都可以视为是一种**恢复操作**，但是：

- redo log：是**存储引擎层(innodb)生成的日志**，记录的是**"物理级别"**上的页修改操作，比如页号xxx、偏移量yyy写入了'zzz'数据。主要为了保证数据的可靠性；
- undo log：是**存储引擎层(innodb)生成的日志**，记录的是**逻辑操作**日志，比如对某一行数据进行了INSERT语句操作，那么undo log就记录一条与之相反的DELETE操作。主要用于**事务的回滚**（undo log记录的是每个修改操作的**逆操作**）和**一致性非锁定读**（undo log回滚行记录到某种特定的版本——MVCC，即多版本并发控制）。

### redo日志

InnoDB存储引擎是以**页为单位**来管理存储空间的。在真正访问页面之前，需要把在**磁盘上**的页缓存到内存中的Buffer Pool之后才可以访问。 所有的变更都必须**先更新缓冲池**中的数据，然后缓冲池中的**脏页**会以一定的频率被刷入磁盘（checkPoint机制），通过缓冲池来优化CPU和磁盘之间的鸿沟，这样就可以保证整体的性能不会下降太快。

**脏页**：**内存中改了，磁盘中还没改。**

#### 为什么需要REDO日志

一方面，缓冲池可以帮助我们消除CPU和磁盘之间的鸿沟，checkpoint机制可以保证数据的最终落盘，然而由于checkpoint **并不是每次变更的时候就触发**的，而是master线程隔一段时间去处理的。所以最坏的情况就是事务提交后，刚写完缓冲池，数据库宕机了，那么这段数据就是丢失的，无法恢复。

另一方面，事务包含 **持久性** 的特性，就是说对于一个已经提交的事务，在事务提交后即使系统发生了崩溃，这个事务对数据库中所做的更改也不能丢失。

那么如何保证这个持久性呢？ **一个简单的做法** ：在事务提交完成之前把该事务所修改的所有页面都刷新到磁盘，但是这个简单粗暴的做法有些问题。

- **修改量与刷新磁盘工作量严重不成比例**

  有时候我们仅仅修改了某个页面中的一个字节，但是我们知道在InnoDB中是以页为单位来进行磁盘lO的，也就是说我们在该事务提交时不得不将一个完整的页面从内存中刷新到磁盘 ，我们又知道一个页面默认是16KB大小，只修改一个字节就要刷新16KB的数据到磁盘上显然是太小题大做了。

- **随机IO刷新较慢**

  一个事务可能包含很多语句，即使是一条语句也可能修改许多页面，假如该事务修改的这些页面可能并不相邻，这就意味着在将某个事务修改的Buffer Pool中的页面**刷新到磁盘**时，需要进行很多的**随机IO**，随机IO比顺序IO要慢，尤其对于传统的机械硬盘来说。

**另一个解决的思路** ：我们只是想让已经提交了的事务对数据库中数据所做的修改永久生效，即使后来系统崩溃，在重启后也能把这种修改恢复出来。所以我们其实没有必要在每次事务提交时就把该事务在内存中修改过的全部页面刷新到磁盘，只需要把 **修改** 了哪些东西 **记录一下** 就好。比如，某个事务将系统表空间中 **第10号** 页面中偏移量为 **100 处**的那个字节的值 **1** 改成 **2** 。我们只需要记录一下：将第0号表空间的10号页面的偏移量为100处的值更新为 2 。

InnoDB引擎的事务采用了WAL技术（**Write- Ahead Logging**），这种技术的思想就是先写日志，再写磁盘，只有日志写入成功，才算事务提交成功，这里的日志就是redo log。当发生宕机且数据未刷到磁盘的时候，可以通过redo log来恢复，保证ACID中的D，这就是redo log的作用。

![image-20220205170518467](file://C:/Users/%E5%B9%BB%E6%83%B3%E5%AE%B6%E6%87%92%E5%BE%97%E6%83%B3%E4%BA%86/AppData/Roaming/Typora/typora-user-images/image-20220205170518467.png?lastModify=1646465884)

减少写盘数据量和频率，而且是顺序写入。

#### REDO日志的好处、特点 

##### 好处

- redo日志降低了刷盘频率
- redo日志占用的空间非常小

存储表空间ID、页号、偏移量以及需要更新的值，所需的存储空间是很小的，刷盘快。

##### 特点

- redo日志是顺序写入磁盘的

在执行事务的过程中，每执行一条语句， 就可能产生若干条redo日志，这些日志是按照**产生的顺序写入磁盘的**，也就是使用顺序IO，效率比随机IO快。

- 事务执行过程中，redo log不断记录

redo log跟bin log的区别：

redo log是**存储引擎层**产生的，而bin log是**数据库层**产生的。

假设一个事务， 对表做10万行的记录插入，在这个过程中，一直不断的往redo log顺序记录，而bin log不会记录，直到这个事务提交，才会一次写入到bin log文件中。

#### redo**的组成** 

Redo log可以简单分为以下两个部分：

- **重做日志的缓冲 (redo log buffer)** ，保存在内存中，是易失的。

在服务器启动时就向操作系统申请了一大片称之为redo log buffer的**连续内存**空间，翻译成中文就是redo日志缓冲区。这片内存空间被划分成若千个连续的**redo log block**。一个redo log block占用**512字节**大小。

![image-20230128172901653](/image-20230128172901653.png)

**参数设置：innodb_log_buffer_size：** 

redo log buffer 大小，默认 **16M** ，最大值是4096M，最小值为1M。

```
mysql> show variables like '%innodb_log_buffer_size%'; 
+------------------------+----------+
| Variable_name 		 | Value 	| 
+------------------------+----------+
| innodb_log_buffer_size | 16777216 | 
+------------------------+----------+
```

- **重做日志文件 (redo log file)** ，保存在硬盘中，是持久的。

RED0日志文件如图所示，其中的`ib_logfile0`和`ib_logfile1`即为REDO日志。

![image-20230128173729357](/image-20230128173729357.png)

#### redo的整体流程

以一个更新事务为例，redo log 流转过程，如下图所示：

![image-20230128173808633](/image-20230128173808633.png)

**第1步：**先将原始数据从磁盘中读入内存中来，修改数据的内存拷贝 

**第2步：**生成一条重做日志并写入redo log buffer，记录的是数据被修改后的值 

**第3步：**当事务commit时，将redo log buffer中的内容刷新到 redo log file，对 redo log file采用**追加写**的方式 

**第4步：**定期将内存中修改的数据刷新到磁盘中

> 体会：
>
> Write-Ahead Log(预先日志持久化)：在持久化一个数据页之前，先将内存中相应的日志页持久化。

#### redo log的刷盘策略

步骤3

redo log的写入并不是直接写入磁盘的，InnoDB引擎会在写redo log的时候先写redo log buffer，之后以 **一 定的频率** 刷入到真正的redo log file 中。这里的一定频率怎么看待呢？这就是我们要说的刷盘策略。

![image-20230128174358258](/image-20230128174358258.png)

注意，redo log buffer刷盘到redo log file的过程并不是真正的刷到磁盘中去，只是刷入到 **文件系统缓存** （page cache）中去（这是现代操作系统为了提高文件写入效率做的一个优化），真正的写入会交给系统自己来决定（比如page cache足够大了）。那么对于InnoDB来说就存在一个问题，如果交给系统来同步，同样如果系统宕机，那么数据也丢失了（虽然整个系统宕机的概率还是比较小的）。

针对这种情况，InnoDB给出 `innodb_flush_log_at_trx_commit` 参数，该参数控制 commit提交事务时，如何将 redo log buffer 中的日志刷新到 redo log file 中。它支持三种策略：

- `设置为0` ：表示每次事务提交时不进行刷盘操作。（系统默认master thread每隔1s进行一次重做日志的同步）
- `设置为1` ：表示每次事务提交时都将进行同步，刷盘操作（`默认值` ） 
- `设置为2` ：表示每次事务提交时都只把 redo log buffer 内容写入 page cache，不进行同步。由os自己决定什么时候同步到磁盘文件。

另外，InnoDB存储引擎有一个后台线程，每隔**1秒**，就会把**redo log buffer** 中的内容写到文件系统缓存（**page cache**），然后调用刷盘操作。

![image-20230128174702931](/image-20230128174702931.png)

也就是说，一个没有提交事务的`redo log`记录，也可能会刷盘。因为在事务执行过程`redo log`记录是会写入`redo log buffer`中，这些redo log记录会被**后台线程**刷盘。

![image-20230128174724531](/image-20230128174724531.png)

除了后台线程每秒1次的轮询操作，还有一种情况，当`redo log buffer` 占用的空间即将达到`innodb_log_buffer_size` (这个参数默认是16M)的一半的时候，后台线程会主动刷盘。

#### 不同刷盘策略演示

##### 流程图

![image-20230128175011983](/image-20230128175011983.png)

> 小结：`innodb_flush_log_at_trx_commit=1`
>
> 表示每次事务提交时都将进行同步，刷盘操作
>
> 为1时，只要事务提交成功，`redo log` 记录就一定在硬盘里， 不会有任何数据丢失。
>
> 如果事务执行期间`MySQL`挂了或宕机，这部分日志丢了，但是事务并没有提交，所以日志丢了也不会有损失。可以保证ACID的D，数据绝对不会丢失，但是`效率最差`的。
>
> 建议使用默认值，虽然操作系统宕机的概率理论小于数据库宕机的概率，但是一般既然使用了事务，那么数据的安全相对来说更重要些。

![image-20230128180908111](/image-20230128180908111.png)

> 小结`innodb_flush_log_at_trx_commit=2`
>
> 表示每次事务提交时都只把 redo log buffer 内容写入 page cache，不进行同步。由os自己决定什么时候同步到磁盘文件。
>
> 为2时，只要事务提交成功，`redo log buffer`中的内容只写入文件系统缓存（`page cache`）。
>
> 如果仅仅只是`MySQL`挂了不会有任何数据丢失，但是操作系统宕机可能会有1秒数据的丢失，这种情况下无法满足ACID中的D。但是数值2肯定是效率最高的。

![image-20230128180940741](/image-20230128180940741.png)

> 小结：`innodb_flush_log_at_trx_commit=0`
>
> 表示每次事务提交时不进行刷盘操作，让OS去存入缓存并刷盘。
>
> 为0时，master thread中每1秒进行一次重做日志的fsync操作，因此实例crash最多丢失1秒钟内的事务。（master thread是负责将缓冲池中的数据异步刷新到磁盘，保证数据的一致性）
>
> 数值0的话，是一种折中的做法，它的IO效率理论是高于1的，低于2的，这种策略也有丢失数据的风险，也无法保证D。

##### 举例

```
USE atguigudb3;

CREATE TABLE test_load(
a INT,
b CHAR(80)
)ENGINE=INNODB;

#创建存储过程，用于向test_load中添加数据
DELIMITER//
CREATE PROCEDURE p_load(COUNT INT UNSIGNED)
BEGIN
DECLARE s INT UNSIGNED DEFAULT 1;
DECLARE c CHAR(80)DEFAULT REPEAT('a',80);
WHILE s<=COUNT DO
INSERT INTO test_load SELECT NULL,c;
COMMIT;
SET s=s+1;
END WHILE;
END //
DELIMITER;

#测试1：
#innodb_flush_log_at_trx_commit = 1
#设置并查看：innodb_flush_log_at_trx_commit

SHOW VARIABLES LIKE 'innodb_flush_log_at_trx_commit';

#set GLOBAL innodb_flush_log_at_trx_commit = 1;

#调用存储过程
CALL p_load(30000); #1min 28sec

#测试2：
#innodb_flush_log_at_trx_commit = 0
TRUNCATE TABLE test_load;

SELECT COUNT(*) FROM test_load;

SET GLOBAL innodb_flush_log_at_trx_commit = 0;

SHOW VARIABLES LIKE 'innodb_flush_log_at_trx_commit';

#调用存储过程
CALL p_load(30000); #37.945 sec

#测试3：
#innodb_flush_log_at_trx_commit = 2
TRUNCATE TABLE test_load;

SELECT COUNT(*) FROM test_load;

SET GLOBAL innodb_flush_log_at_trx_commit = 2;

SHOW VARIABLES LIKE 'innodb_flush_log_at_trx_commit';

#调用存储过程
CALL p_load(30000); #45.173 sec
```

> 虽然用户可以通过设置参数innodb_flush_log_at_trx_commit为0或2来提高事务提交的性能，但需清楚，这种设置方法丧失了事务的ACID特性。

#### 写入redo log buffer 过程 （了解）

##### 补充概念：Mini-Transaction

MySQL把对底层页面中的一次原子访问的过程称之为一个**Mini-Transaction**，简称mtr，比如，向某个索引对应的B+树中插入一条记录的过程就是一个**Mini-Transaction**。一个所谓的mtr可以包含一组**redo**日志， 在进行崩溃恢复时这一组 **redo**日志作为一个不可分割的整体。

一个事务可以包含若干条语句，每一条语句其实是由若干个 **mtr** 组成，每一个 **mtr** 又可以包含若干条redo日志，画个图表示它们的关系就是这样：

![image-20230128181202905](/image-20230128181202905.png)

##### redo 日志写入log buffer

向**log buffer**中写入redo日志的过程是顺序的，也就是先往前边的block中写，当该block的空闲空间用完之后再往下一个block中写。当我们想往**log buffer**中写入redo日志时，第一个遇到的问题就是 应该写在哪个**block**的哪个偏移量处，所以**InnoDB**的设计者特意提供了一个称之为**buf_free**的全局变量，该变量指明后续写入的redo日志应该写入到**log buffer**中的哪个位置，如图所示:

![image-20230128181216098](/image-20230128181216098.png)

一个mtr执行过程中可能产生若干条redo日志，**这些redo日志是一个不可分割的组**，所以其实并不是每生成一条redo日志，就将其插入到log buffer中，而是每个mtr运行过程中产生的日志先暂时存到一个地方，当该mtr结束的时候，将过程中产生的一组redo日志再全部复制到log buffer中。我们现在假设有两个名为T1、T2 的事务，每个事务都包含2个mtr，我们给这几个mtr命名一下：

- 事务T1的两个mtr分别称为mtr_T1_1和mtr_T1_2。
- 事务T2的两个mtr分别称为mtr_T2_1和mtr_T2_2。

每个mtr都会产生一组redo日志，用示意图来描述一下这些mtr产生的日志情况：

![image-20230128181758059](/image-20230128181758059.png)

不同的事务可能是并发执行的，所以 T1 、 T2 之间的 mtr 可能是 **交替执行** 的。每当一个mtr执行完成时，伴随该mtr生成的一组redo日志就需要被复制到log buffer中，也就是说不同事务的mtr可能是交替写入log buffer的，我们画个示意图（为了美观，我们把一个mtr中产生的所有的redo日志当作一个整体来画）：

![image-20230128183124625](/image-20230128183124625.png)

有的mtr产生的redo日志量非常大，比如mtr_t1_2产生的redo日志占用空间较大，占用了3个block来存储。

##### redo log block的结构图

一个redo log block是由**日志头、日志体、 日志尾**组成。日志头占用12字节，日志尾占用8字节，所以一个block真正能存储的数据就是512-12-8=492字节。

> **为什么一个block设计成512字节?** 
>
> 这个和磁盘的扇区有关**机械磁盘默认的扇区就是512字节**，如果你要写入的数据大于512字节，那么要写入的扇区肯定不止一个，这时就要涉及到盘片的转动，找到下一个扇区，假设现在需要写入两个扇区A和B，如果扇区A写入成功，而扇区B写入失败，那么就会出现非原子性的写入，而如果每次只写入和扇区的大小一样的512字节，那么每次的写入都是原子性的。

![image-20230128183159853](/image-20230128183159853.png)

真正的redo日志都是存储到占用496字节大小的`log block body` 中，图中的`log block header`和`logblock trailer` 存储的是一些管理信息。 

![image-20230128183237729](/image-20230128183237729.png)

#### redo log file

##### 相关参数设置

- **innodb_log_group_home_dir ：**指定 redo log 文件组所在的路径，默认值为 `./` ，表示在数据库的数据目录下。MySQL的默认数据目录（`var/lib/mysql`）下默认有两个名为`ib_logfile0`和`ib_logfile1` 的文件，log buffer中的日志默认情况下就是刷新到这两个磁盘文件中。此redo日志文件位置还可以修改。

  ![image-20230128183246737](/image-20230128183246737.png)

- **innodb_log_files_in_group：**指明redo log file的个数，命名方式如：ib_logfile0，iblogfile1... iblogfilen。默认2个，最大100个。

```
mysql> show variables like 'innodb_log_files_in_group'; 
+---------------------------+-------+
| Variable_name 			| Value |
+---------------------------+-------+
| innodb_log_files_in_group | 2 	|
+---------------------------+-------+
#ib_logfile0 
#ib_logfile1
```

- **innodb_flush_log_at_trx_commit：**控制 redo log 刷新到磁盘的策略，默认为1。
- **innodb_log_file_size：**单个 redo log 文件设置大小，默认值为 48M 。最大值为512G，注意最大值指的是整个 redo log 系列文件之和，即（innodb_log_files_in_group * innodb_log_file_size ）不能大于最大值512G。

```
mysql> show variables like 'innodb_log_file_size'; 
+----------------------+----------+
| Variable_name 	   | Value 	  | 
+----------------------+----------+
| innodb_log_file_size | 50331648 | 
+----------------------+----------+
```

根据业务修改其大小，以便容纳较大的事务。编辑my.cnf文件并重启数据库生效，如下所示

```
[root@localhost ~]# vim /etc/my.cnf 
innodb_log_file_size=200M
```

##### 日志文件组

从上边的描述中可以看到，磁盘上的redo日志文件不只一个，而是以一个日志文件组的形式出现的。这些文件以ib_logfile[数字] （数字可以是0、1、2...） 的形式进行命名，每个的redo日志文件大小都是一样的。

在将redo日志写入日志文件组时，是从ib_logfile0开始写，如果ib_ logfile0 写满了，就接着ib_logfile1写。同理，ib_ logfile1写满了就去写ib_logfile2，依此类推。如果写到最后一个文件该咋办？那就重新转到ib logfile0 继续写，所以整个过程如下图所示：

![image-20230128183541822](/image-20230128183541822.png)

总共的redo日志文件大小其实就是：`innodb_log_file_size × innodb_log_files_in_group`。

采用循环使用的方式向redo日志文件组里写数据的话，会导致后写入的redo日志覆盖掉前边写的redo日志？当然！所以InnoDB的设计者提出了checkpoint的概念。

##### checkpoint

在整个日志文件组中还有两个重要的属性，分别是write pos、checkpoint

- write pos是当前记录的位置，一边写一边后移（缓存—>redo log日志中）
- checkpoint是当前要擦除的位置，也是往后推移（内存—>磁盘）

每次刷盘redo log记录到日志文件组中，write pos位置就会后移更新。每次MySQL加载日志文件组恢复数据时，会清空加载过的redo log记录，并把checkpoint后移更新。write pos和checkpoint之间的还空着的部分可以用来写入新的redo log记录。

![image-20230128183734385](/image-20230128183734385.png)

如果 write pos 追上 checkpoint ，表示**日志文件组满**了，这时候不能再写入新的 redo log记录，MySQL 得停下来，清空一些记录，把 checkpoint 推进一下。

![image-20230128183755923](/image-20230128183755923.png)

#### 小结

![image-20230128183848407](/image-20230128183848407.png)

### Undo日志

redo log是事务持久性的保证，undo log是事务原子性的保证。在事务中 **更新数据** 的 **前置操作** 其实是要先写入一个 **undo log** 。 

#### 如何理解Undo日志

事务需要保证 **原子性** ，也就是事务中的操作要么全部完成，要么什么也不做。但有时候事务执行到一半会出现一些情况，比如：

- 情况一：事务执行过程中可能遇到各种错误，比如 **服务器本身的错误 ， 操作系统错误** ，甚至是突然 **断电** 导致的错误。
- 情况二：程序员可以在事务执行过程中手动输入 **ROLLBACK** 语句结束当前事务的执行。

以上情况出现，我们需要把数据改回原先的样子，这个过程称之为 **回滚** ，这样就可以造成一个假象：这个事务看起来什么都没做，所以符合 **原子性** 要求。

每当我们要对一条记录做改动时(这里的**改动**可以指(**INSERT、DELETE、 UPDATE**) ，都需要"留一 手"——把回滚时所需的东西记下来。

MySQL把这些为了回滚而记录的这些内容称之为**撤销日志**或者回滚日志(即**undo log**) 。注意，由于查询操作(SELECT)并不会修改任何用户记录，所以在查询操作执行时，并**不需要记录**相应的undo日志。

此外，undo log会产生**redo log**，也就是undo log的产生会伴随着redo log的产生，这是因为undo log也需要持久性的保护。

#### Undo日志的作用

- **作用1：回滚数据**

  用户对undo日志可能有误解：undo用于将数据库物理地恢复到执行语句或事务之前的样子。但事实并非如此。undo是**逻辑日志**，因此只是将数据库逻辑地恢复到原来的样子。所有修改都被逻辑地取消了，但是数据结构和页本身在回滚之后可能大不相同。

- **作用2：MVCC** 

  undo的另一个作用是MVCC，即在InnoDB存储引擎中MVCC的实现是通过undo来完成。当用户读取一行记录时，若该记录已经被其他事务占用，当前事务可以通过undo读取之前的行版本信息，以此实现非锁定读取。

#### undo的存储结构

##### 回滚段与undo页

InnoDB对undo log的管理采用段的方式，也就是 **回滚段**（rollback segment） 。每个回滚段记录了1024 个 undo log segment ，而在每个undo log segment段中进行 **undo页** 的申请。

- 在 **InnoDB1.1版本之前** （不包括1.1版本），只有一个rollback segment，因此支持同时在线的事务限制为 **1024** 。虽然对绝大多数的应用来说都已经够用。
- 从1.1版本开始InnoDB支持最大 **128个rollback segment** ，故其支持同时在线的事务限制提高到了 **128\*1024**

```
mysql> show variables like 'innodb_undo_logs'; 
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| innodb_undo_logs | 128   | 
+------------------+-------+
```

虽然InnoDB1.1版本支持了128个rollback segment,但是这些rollback segment都存储于共享表空间ibdata中。从InnoDB1.2版本开始,可通过参数对rollback segment做进一步的设置 。

undo log 相关参数一般很少改动。

**undo页的重用**

当我们开启一个事务需要写undo log的时候，就得先去undo log segment中去找到一个空闲的位置，当有空位的时候，就去申请undo页，在这个申请到的undo页中进行undo log的写入。我们知道mysql默认一页的大小是16k。

为每一个事务分配一个页， 是非常浪费的(除非你的事务非常长)， 假设你的应用的TPS (每秒处理的事务数目)为1000， 那么1s就需要1000个页，大概需要16M的存储，1分钟大概需要1G的存储。如果照这样下去除非MySQL清理的非常勤快，否则随着时间的推移,磁盘空间会增长的非常快，而且很多空间都是浪费的。

于是undo页就被设计的可以**重用**了，当事务提交时，并不会立刻删除undo页。因为重用，所以这个undo页可能混杂着其他事务的undo log。undo log在commit后，会被放到一个**链表**中，然后判断undo页的使用空间是否**小于3/4**，如果小于3/4的话，则表示当前的undo页可以被重用，那么它就不会被回收,其他事务的undo log可以记录在当前undo页的后面。由于undo log是**离散的**，所以清理对应的磁盘空间时，效率不高。

##### 回滚段与事务

1. 每个事务只会使用一个回滚段，一个回滚段在同一时刻可能会服务于多个事务。
2. 当一个事务开始的时候，会制定一个回滚段，在事务进行的过程中，当数据被修改时，原始的数据会被复制到回滚段。
3. 在回滚段中，事务会不断填充盘区，直到事务结束或所有的空间被用完。如果当前的盘区不够用，事务会在段中请求扩展下一个盘区，如果所有已分配的盘区都被用完，事务会覆盖最初的盘区或者在回滚段允许的情况下扩展新的盘区来使用。
4. 回滚段存在于undo表空间中，在数据库中可以存在多个undo表空间，但同一时刻只能使用一个undo表空间。
5. 当事务提交时，InnoDB存储引擎会做以下两件事情：
   - 将undo log放入列表中，以供之后的purge操作
   - 判断undo log所在的页是否可以重用，若可以分配给下个事务使用

##### 回滚段中的数据分类

1. **未提交的回滚数据(uncommitted undo information)** 
   - 该数据所关联的事务并未提交，用于实现读一致性，所以该数据不能被其他事务的数据覆盖。
2. **已经提交但未过期的回滚数据(committed undo information)** 
   - 该数据关联的事务已经提交，但是仍受到undo retention参数的保持时间的影响。
3. **事务已经提交并过期的数据(expired undo information)**
   - 事务已经提交，而且数据保存时间已经超过undo retention参数指定的时间，属于已经过期的数据。当回滚段满了之后，会优先覆盖事务已经提交并过期的数据"。

#### undo的类型

在InnoDB存储引擎中，undo log分为：

- insert undo log 
  - insert undo log是指在insert操作中产生的undo log。因为insert操作的记录，只对事务本身可见，对其他事务不可见（这是事务隔离性的要求），故该undo log可以在事务提交后直接删除。不需要进行purge操作。
- update undo log 
  - update undo log记录的是对delete和update操作产生的undo log。该undo log可能需要提供MVCC机制，因此不能在事务提交时就进行删除。提交时放入undo log链表，等待purge线程进行最后的删除。

#### undo log的生命周期 

##### 简要生成过程

以下是undo+redo事务的简化过程

假设有2个数值，分别为A=1和B=2， 然后将A修改为3，B修改为4

```
1. start transaction;
2. 记录 A=1 到undo log;
3. update A = 3
4. 记录 A=3 到redo log;
5. 记录 B=2 到undo log; 
6. update B = 4
7. 记录B = 4到redo log;
8. 将redo log刷新到磁盘
9. commit
```

- 在1-8步骤的任意一步系统宕机，事务未提交，该事务就不会对磁盘上的数据做任何影响。
- 如果在8-9之间宕机，恢复之后可以选择回滚，也可以选择继续完成事务提交，因为此时redo log已经持久化。
- 若在9之后系统宕机，内存映射中变更的数据还来不及刷回磁盘，那么系统恢复之后，可以根据redo log把数据刷回磁盘。

**只有Buffer Pool的流程：**

![image-20230129130715522](/image-20230129130715522.png)

**有了Redo Log和Undo Log之后：**

![image-20230129131455006](/image-20230129131455006.png)

##### 详细生成过程

对于InnoDB引擎来说，每个行记录除了记录本身的数据之外，还有几个隐藏的列：

- DB_ROW_ID：如果没有为表显式的定义主键，并且表中也没有定义唯一索引，那么InnoDB会自动为表添加一个row_id的隐藏列作为主键。
- DB_TRX_ID：每个事务都会分配一个事务ID，当对某条记录发生变更时，就会将这个事务的事务ID写入trx_id中。
- DB_ROLL_PTR：回滚指针，本质上就是指向undo log的指针。

![image-20230129131802210](/image-20230129131802210.png)

**当我们执行INSERT时：**

```
begin; 
INSERT INTO user (name) VALUES ("tom");
```

插入的数据都会生成一条insert undo log，并且数据的回滚指针会指向它。undo log会记录undo log的序号、插入主键的列和值...，那么在进行rollback的时候，通过主键直接把对应的数据删除即可。

![image-20230129131830174](/image-20230129131830174.png)

**当我们执行UPDATE时：**

对于更新的操作会产生update undo log，并且会分更新主键的和不更新主键的，假设现在执行：

```
UPDATE user SET name="Sun" WHERE id=1 ;
```

![image-20230129131949145](/image-20230129131949145.png)

这时会把老的记录写入新的undo log，让回滚指针指向新的undo log，它的undo no是1，并且新的undo log会指向老的undo log (undo no=0)。

假设现在执行:

```
UPDATE user SET id=2 WHERE id=1;
```

![image-20230129132114745](/image-20230129132114745.png)

对于更新主键的操作，会先把原来的数据deletemark标识打开，这时并没有真正的删除数据，真正的删除会交给清理线程去判断，然后在后面插入一条新的数据，新的数据也会产生undo log，并且undo log的序号会递增。

可以发现每次对数据的变更都会产生一个undo log，当一条记录被变更多次时，那么就会产生多条undo log。undo log记录的是变更前的日志，并且每个undo log的序号是递增的，那么当要回滚的时候，按照序号**依次向前推**，就可以找到我们的原始数据了。

##### undo log是如何回滚的

以上面的例子来说，假设执行rollback，那么对应的流程应该是这样：

1. 通过undo no=3的日志把id=2的数据删除
2. 通过undo no=2的日志把id=1的数据的deletemark还原成0 
3. 通过undo no=1的日志把id=1的数据的name还原成Tom 
4. 通过undo no=0的日志把id=1的数据删除

##### undo log的删除

- 针对于insert undo log

因为insert操作的记录，只对事务本身可见，对其他事务不可见。故该undo log可以在事务提交后直接删除，不需要进行purge操作。

- 针对于update undo log 

该undo log可能需要提供MVCC机制，因此不能在事务提交时就进行删除。提交时放入undo log链表，等待purge线程进行最后的删除。

> 补充：
>
> purge线程两个主要作用是：**清理undo页**和**清除page里面带有Delete_Bit标识的数据行**。
>
> 在InnoDB中， 事务中的Delete操作实际上并不是真正的删除掉数据行，而是一种Delete Mark操作，在记录上标识Delete_Bit，而不删除记录。是一种“假删除”，只是做了个标记，真正的删除工作需要后台purge线程去完成。

#### 小结

![image-20230129133406185](/image-20230129133406185.png)

**undo log是逻辑日志，对事务回滚时，只是将数据库逻辑地恢复到原来的样子。**

**redo log是物理日志，记录的是数据页的物理变化，undo log不是redo log的逆过程。**

## 锁

事务的**隔离性**由这张所讲的**锁机制**来实现。

### 概述

**锁**是计算机协调多个进程或线程**并发访问某一资源**的机制。在程序开发中会存在多线程同步的问题，当多个线程并发访问某个数据的时候，尤其是针对一些敏感的数据(比如订单、金额等)，我们就需要保证这个数据在任何时刻**最多只有一个线程**在访问，保证数据的**完整性**和**一致性**。在开发过程中加锁是为了保证数据的一致性，这个思想在数据库领域中同样很重要。

在数据库中，除传统的计算资源(如CPU、 RAM、I/O等) 的争用以外，数据也是一种供许多用户共享的资源。 为保证数据的一致性，需要**对并发操作进行控制**，因此产生了**锁**。同时**锁机制**也为实现MySQL的各个隔离级别提供了保证。**锁冲突**也是影响数据库**并发访问性能**的一个重要因素。所以锁对数据库而言显得尤其重要，也更加复杂。

### MySQL并发事务访问相同记录

并发事务访问相同记录的情况大致可以划分为3种：

#### 读-读情况

**读-读** 情况，即并发事务相继 **读取相同的记录** 。读取操作本身不会对记录有任何影响，并不会引起什么问题，所以允许这种情况的发生。

#### 写-写情况

**写-写** 情况，即并发事务相继对相同的记录做出改动。

在这种情况下会发生 **脏写** 的问题，任何一种隔离级别都不允许这种问题的发生。所以在多个未提交事务相继对一条记录做改动时，需要让它们 **排队执行** ，这个排队的过程其实是通过 **锁** 来实现的。这个所谓的锁其实是一个 **内存中的结构** ，在事务执行前本来是没有锁的，也就是说一开始是没有 **锁结构** 和记录进行关联的，如图所示：

![image-20230129134903752](/image-20230129134903752.png)

当一个事务想对这条记录做改动时，首先会看看内存中有没有与这条记录关联的 **锁结构** ，当没有的时候就会在内存中生成一个 **锁结构** 与之关联。比如，事务 **T1** 要对这条记录做改动，就需要生成一个 **锁结构**与之关联：

![image-20230129134946081](/image-20230129134946081.png)

在**锁结构**里有很多信息，为了简化理解，只把两个比较重要的属性拿了出来：

- trx信息：代表这个锁结构是哪个事务生成的。
- is_waiting：代表当前事务是否在等待。

当事务**T1**改动了这条记录后，就生成了一个**锁结构**与该记录关联，因为之前没有别的事务为这条记录加锁，所以**is_waiting**属性就是**false**，我们把这个场景就称之为**获取锁成功**，或者加锁成功，然后就可以继续执行操作了。

在事务**T1**提交之前，另一个事务**T2**也想对该记录做改动，那么先看看有没有**锁结构**与这条记录关联，发现有一个锁结构与之关联后,然后也生成了一个锁结构与这条记录关联，不过锁结构的**is_ waiting**属性值为**true**，表示当前事务需要等待，我们把这个场景就称之为**获取锁失败**，或者**加锁失败**，图示：

![image-20230129135027714](/image-20230129135027714.png)

在事务T1提交之后，就会把该事务生成的**锁结构释放**掉，然后看看还有没有别的事务在等待获取锁，发现了事务T2还在等待获取锁，所以把事务**T2**对应的锁结构的**is_waiting**属性设置为**false**，然后把该事务对应的线程唤醒，让它继续执行，此时事务T2就算获取到锁了。效果图就是这样：

![image-20230129135048429](/image-20230129135048429.png)

小结几种说法：

- **不加锁**

  意思就是不需要在内存中生成对应的 锁结构 ，可以直接执行操作。

- **获取锁成功**，或者加锁成功

  意思就是在内存中生成了对应的 锁结构 ，而且锁结构的 is_waiting 属性为 false ，也就是事务可以继续执行操作。

- **获取锁失败**，或者加锁失败，或者没有获取到锁

  意思就是在内存中生成了对应的 锁结构 ，不过锁结构的 is_waiting 属性为 true ，也就是事务需要等待，不可以继续执行操作。

#### **读**-写或写-读情况

**读-写** 或 **写-读** ，即一个事务进行读取操作，另一个进行改动操作。这种情况下可能发生**脏读、不可重复读 、幻读**的问题。

各个数据库厂商对 **SQL标准** 的支持都可能不一样。比如MySQL在 **REPEATABLE READ** 隔离级别上就已经解决了 **幻读** 问题

#### 并发问题的解决方案

怎么解决 **脏读 、 不可重复读 、 幻读** 这些问题呢？其实有两种可选的解决方案：

- 方案一：读操作利用多版本并发控制（ **MVCC** ，下章讲解），写操作进行 **加锁** 。

  所谓的**MVCC**，就是生成一个 **ReadView**，通过ReadView找到符合条件的记录版本(历史版本由**undo日志**构建)。查询语句只能**读**到在生成ReadView之前**已提交事务所做的更改**，在生成ReadView之前未提交的事务或者之后才开启的事务所做的更改是看不到的。而**写操作**肯定针对的是**最新版本的记录**，读记录的历史版本和改动记录的最新版本本身并不冲突，也就是采用MVCC时，**读-写**操作并不冲突。

  > 普通的SELECT语句在READ COMMITTED和REPEATABLE READ隔离级别下会使用到MVCC读取记录。
  >
  > - 在 **READ COMMITTED** （读已提交）隔离级别下，一个事务在执行过程中每次执行SELECT操作时都会生成一个ReadView，ReadView的存在本身就保证了 **事务不可以读取到未提交的事务所做的更改** ，也就是避免了脏读现象；
  > - 在 **REPEATABLE READ** （可重复读）隔离级别下，一个事务在执行过程中只有 **第一次执行SELECT操作** 才会生成一个ReadView，之后的SELECT操作都 **复用** 这个ReadView，这样也就避免了不可重复读和幻读的问题。

- 方案二：读、写操作都采用 加锁 的方式。

  如果我们的一些业务场景不允许读取记录的旧版本，而是每次都必须去**读取记录的最新版本**。比如，在银行存款的事务中，你需要先把账户的余额读出来，然后将其加上本次存款的数额，最后再写到数据库中。在将账户余额读取出来后，就不想让别的事务再访问该余额，直到本次存款事务执行完成，其他事务才可以访问账户的余额。这样在读取记录的时候就需要对其进行**加锁**操作，这样也就意味着**读**操作和**写**操作也像**写-写**操作那样**排队**执行。

  **脏读**的产生是因为当前事务读取了另一个未提交事务写的一条记录，如果另一个事务在写记录的时候就给这条记录加锁，那么当前事务就无法继续读取该记录了，所以也就不会有脏读问题的产生了。

  **不可重复读**的产生是因为当前事务先读取一条记录，另外一个事务对该记录做了改动之后并提交之后，当前事务再次读取时会获得不同的值，如果在当前事务读取记录时就给该记录加锁，那么另一个事务就无法修改该记录，自然也不会发生不可重复读了。

  **幻读**问题的产生是因为当前事务读取了一个范围的记录， 然后另外的事务向该范围内插入了新记录，当前事务再次读取该范围的记录时发现了新插入的新记录。采用加锁的方式解决幻读问题就有一些麻烦, 因为当前事务在第一次读取记录时幻影记录并不存在，所以读取的时候加锁就有点尴尬(因为你并不知道给谁加锁)。

- 小结对比发现：

  - 采用 **MVCC** 方式的话， **读-写** 操作彼此并不冲突， **性能更高** 。
  - 采用 **加锁** 方式的话， **读-写** 操作彼此需要 **排队执行** ，影响性能。

一般情况下我们当然愿意采用 **MVCC** 来解决 **读-写** 操作并发执行的问题，但是业务在某些特殊情况下，要求必须采用 **加锁** 的方式执行。下面就讲解下MySQL中不同类别的锁。

### 锁的不同角度分类

![image-20230129135828405](/image-20230129135828405.png)

#### 从数据操作的类型划分：读锁、写锁 

对于数据库中并发事务的**读-读**情况并不会引起什么问题。对于**写-写**、**读-写** 或**写-读**这些情况可能会引起一些问题，需要使用**MVCC**或者**加锁**的方式来解决它们。在使用**加锁**的方式解决问题时，由于既要允许**读-读**情况不受影响，又要使**写-写**、**读-写**或**写-读**情况中的操作**相互阻塞**，所以MySQL实现一个由两种类型的锁组成的锁系统来解决。这两种类型的锁通常被称为**共享锁(Shared Lock, S Lock)和排他锁(Exclusive Lock, X Lock)，也叫读锁(readlock) 和写锁(write lock)。**

- **读锁** ：也称为 **共享锁** 、英文用 S 表示。针对同一份数据，多个事务的读操作可以同时进行而不会互相影响，相互不阻塞的。
- **写锁** ：也称为 **排他锁** 、英文用 X 表示。当前写操作没有完成前，它会阻断其他写锁和读锁。这样就能确保在给定的时间里，只有一个事务能执行写入，并防止其他用户读取正在写入的同一资源。

**需要注意的是对于 InnoDB 引擎来说，读锁和写锁可以加在表上，也可以加在行上。**

**举例**（行级读写锁）：如果一个事务T1已经获得了某个行 r 的读锁，那么此时另外的一个事务T2是可以去获得这个行 r 的读锁的，因为读取操作并没有改变行 r 的数据；但是，如果某个事务T3想获得行r的写锁，则它必须等待事务T1、T2释放掉行r 上的读锁才行。

总结：这里的兼容是指对同一-张表或记录的锁的兼容性情况。

![image-20230129140329410](/image-20230129140329410.png)

##### 锁定读

在采用**加锁**方式解决**脏读、不可重复读、 幻读**这些问题时，读取一条记录时需要获取该记录的**S锁**，其实是不严谨的，有时候需要在读取记录时就获取记录的**X锁**，来禁止别的事务读写该记录，为此MySQL提出了两种比较特殊的**SELECT**语句格式：

- 对读取的记录加S锁

  ```
  SELECT ...LOCK IN SHARE MODE;
  #或
  SELECT ... FOR SHARE#8.0新特性
  ```

  在普通的SELECT语句后边加`LOCK IN SHARE MODE`，如果当前事务执行了该语句，那么它会为读取到的记录加`S`锁，这样允许别的事务继续获取这些记录的S锁(比方说别的事 务也使用`SELECT ... LOCK IN SHARE MODE`语句来读取这些记录)，但是不能获取这些记录的X锁(比如使用 `SELECT.. FOR UPDATE`语句来读取这些记录，或者直接修改这些记录)。如果别的事务想要获取这些记录的`X锁`，那么它们会阻塞，直到当前事务提交之后将这些记录上的`S锁`释放掉。

- 对读取的记录加X锁

  ```
  SELECT ... FOR UPDATE;
  ```

  在普通的SELECT语句后边加`FOR UPDATE`，如果当前事务执行了该语句，那么它会为读取到的记录加X锁，这样既不允许别的事务获取这些记录的S锁(比方说别的事务使用`SELECT ... LOCK IN SHARE MODE`语句来读取这些记录)，也不允许获取这些记录的X锁(比如使用 `SELECT ... FOR UPDATE`语句来读取这些记录，或者直接修改这些记录)。如果别的事务想要获取这些记录的`S`锁或者`X`锁，那么它们会阻塞，直到当前事务提交之后将这些记录上的`X`锁释放掉。

**MySQL8.0新特性：**

在5.7及之前的版本，SELECT ... FOR UPDATE，如果获取不到锁，会一直等待，直到`innodb_lock_wait_timeout`超时。在8.0版本中，`SELECT ... FOR UPDATE`， `SELECT .. FOR SHARE`添加`NOWAIT、SKIP LOCKED` 语法，跳过锁等待，或者跳过锁定。

- 通过添加NOWAIT. SKIP LOCKED语法，能够立即返回。如果查询的行已经加锁：

  - 那么NOWAIT会立即报错返回

    ![image-20230129140412408](/image-20230129140412408.png)

  - 而SKIP LOCKED也会立即返回，只是返回的结果中不包含被锁定的行。

    ![image-20230129140424097](/image-20230129140424097.png)

##### 写操作

平时用到的写操作主要有三种：DELETE、UPDATE、INSERT：

DELETE、UPDATE是在给原有的数据加X锁；二IINSERT没有原本数据，因此采用一种隐式锁的方式保护事务。

- **DELETE**

  对一条记录做DELETE操作的过程其实是先在`B+`树中定位到这条记录的位置，然后获取这条记录的X锁，再执行`delete mark`操作。我们也可以把这个定位待删除记录在B+树中位置的过程看成是一个获取`X`锁的`锁定读`。

- **UPDATE**

  - **情况1：**未修改该记录的键值，并且被更新的列占用的**存储空间**在修改前后**未发生变化**。

    则先在B+树中定位到这条记录的位置，然后再获取一下记录的X锁，最后在原记录的位置进行修改操作。我们也可以把这个定位待修改记录在B+树中位置的过程看成是一个获取 **X锁**的**锁定读**。

  - **情况2：**未修改该记录的键值，并且**至少有一个**被更新的列占用的存储空间在修改前后**发生变化**。

    则先在B+树中定位到这条记录的位置，然后获取一下记录的X锁，将该记录彻底删除掉(就是把记录彻底移入垃圾链表)，最后再插入一条新记录。这个定位待修改记录在B+树中位置的过程看成是一个获取**X锁**的**锁定读**，新插入的记录由**INSERT**操作提供的**隐式锁**进行保护。

  - **情况3：**修改了该记录的键值，则相当于在原记录上做`DELETE`操作之后再来一次 INSERT操作，加锁操作就需要按照`DELETE`和`INSERT`的规则进行了。

- **INSERT**

  一般情况下，新插入一条记录的操作并不加锁，通过一种称之为**隐式锁**的结构来保护这条新插入的记录在本事务提交前不被别的事务访问。

#### 从数据操作的粒度划分：表级锁、页级锁、行锁

为了尽可能提高数据库的并发度，每次锁定的数据范围越小越好，理论上每次只锁定当前操作的数据的方案会得到最大的并发度，但是管理锁是很耗资源的事情（涉及获取、检查、释放锁等动作）。因此数据库系统需要在**高并发响应**和**系统性能**两方面进行平衡，这样就产生了“**锁粒度(Lock granularity)** ”的概念。

对一条记录加锁影响的也只是这条记录而已， 我们就说这个锁的粒度比较细；其实一个事务也可以在**表级别**进行加锁，自然就被称之为**表级锁**或者**表锁**，对一个表加锁影响整个表中的记录，我们就说这个锁的粒度比较粗。锁的粒度主要分为表级锁、页级锁和行锁。

##### 表锁

该锁会锁定整张表，它是MySQL中最基本的锁策略，**并不依赖于存储引擎**(不管你是MySQL的什么存储引擎，对于表锁的策略都是一样的)，并且表锁是**开销最小**的策略（因为粒度比较大）。由于表级锁一次会将整个表锁定，所以可以很好的**避免死锁**问题。当然，锁的粒度大所带来最大的负面影响就是出现锁资源争用的概率也会最高，导致**并发率大打折扣**。

###### 表级别的S锁、X锁

在对某个表执行SELECT、INSERT、DELETE、UPDATE语句时，InnoDB存储引擎是不会为这个表添加表级别的 **S锁** 或者 **X锁** 的。在对某个表执行一些诸如 **ALTER TABLE 、 DROP TABLE** 这类的 **DDL** 语句时，其他事务对这个表并发执行诸如SELECT、INSERT、DELETE、UPDATE的语句会发生阻塞。同理，某个事务中对某个表执行SELECT、INSERT、DELETE、UPDATE语句时，在其他会话中对这个表执行 **DDL** 语句也会发生阻塞。这个过程其实是通过在 **server层** 使用一种称之为 **元数据锁** （英文名： `Metadata Locks` ，简称 MDL ）结构来实现的。

**一般情况下，不会使用**InnoDB存储引擎提供的表级别的 S锁 和 X锁 。只会在一些特殊情况下，比方说 **崩溃恢复** 过程中用到。比如，在系统变量`autocommit`=0，`innodb_table_locks` = 1 时， `手动`获取InnoDB存储引擎提供的表t 的 S锁 或者 X锁 可以这么写：

- `LOCK TABLES t READ` ：InnoDB存储引擎会对表`t`加表级别的 `S锁`。 
- `LOCK TABLES t WRITE` ：InnoDB存储引擎会对表 `t` 加表级别的 `X锁`。

不过尽量避免在使用InnoDB存储引擎的表上使用 `LOCK TABLES` 这样的手动锁表语句，它们并不会提供什么额外的保护，只是会降低并发能力而已。InnoDB的厉害之处还是实现了更细粒度的 **行锁** ，关于InnoDB表级别的 S锁 和 X锁 大家了解一下就可以了。

**举例：**下面我们讲解MyISAM引擎下的表锁。

1. 创建表并添加数据。

   ![image-20230129142836994](/image-20230129142836994.png)

2. 查看表上加过的锁。

   ![image-20230129142944675](/image-20230129142944675.png)

   ![image-20230129142958209](/image-20230129142958209.png)

3. 手动加表锁命令

   ![image-20230129143032433](/image-20230129143032433.png)

4. 释放表锁

   ![image-20230129143047504](/image-20230129143047504.png)

5. 加读锁

   看结论

6. 加写锁

   看结论

总结：

**MyISAM存储引擎**在执行查询语句(SELECT) 前，会给涉及的所有表加读锁，在执行增删改操作前，会给涉及的表加写锁。

**InnoDB存储引擎**是不会为这个表添加表级别的**读锁**或者**写锁**的。

MySQL的表级锁有两种模式：（以MyISAM表进行操作的演示）

- 表共享读锁（Table Read Lock）
- 表独占写锁（Table Write Lock）

| 锁类型 | 自己可读 | 自己可写 | 自己可操作其他表 | 他人可读 | 他人可写 |
| ------ | -------- | -------- | ---------------- | -------- | -------- |
| 读锁   | 是       | 否       | 否               | 是       | 否，等待 |
| 写锁   | 是       | 是       | 否               | 否，等待 | 否，等待 |

###### 意向锁 （intention lock） 

InnoDB 支持 **多粒度锁（multiple granularity locking）** ，**它允许 行级锁 与 表级锁 共存**，而意向锁就是其中的一种 **表锁** 。

1. 意向锁的存在是为了**协调行锁和表锁的关系**，支持多粒度(表锁与行锁)的锁并存。
2. 意向锁是一种**不与行级锁冲突表级锁**，这一点非常重要。
3. 表明“**某个事务正在某些行持有了锁**或**该事务准备去持有锁**”。

意向锁分为两种：

- **意向共享锁**（intention shared lock, IS）：事务有意向对表中的某些行加共享锁（S锁）

  ```
  -- 事务要获取某些行的 S 锁，必须先获得表的 IS 锁。 
  SELECT column FROM table ... LOCK IN SHARE MODE;
  ```

- **意向排他锁**（intention exclusive lock, IX）：事务有意向对表中的某些行加排他锁（X锁）

  ```
  -- 事务要获取某些行的 X 锁，必须先获得表的 IX 锁。 
  SELECT column FROM table ... FOR UPDATE;
  ```

即：意向锁是由存储引擎 **自己维护的** ，用户无法手动操作意向锁，在为数据行加共享 / 排他锁之前，InooDB 会先获取该数据行 **所在数据表的对应意向锁** 。

**1. 意向锁要解决的问题** 

现在有两个事务，分别是T1和T2，其中T2试图在该表级别上应用共享或排它锁，如果没有意向锁存在，那么T2就需要去检查各个页或行是否存在锁；如果存在意向锁，那么此时就会受到由T1控制的**表级别意向锁的阻塞**。T2在锁定该表前不必检查各个页或行锁，而只需检查表上的意向锁。简单来说就是给更大一级别的空间示意里面是否已经上过锁。

在数据表的场景中，**如果我们给某一行数据加上了排它锁，数据库会自动给更大一级的空间，比如数据页或数据表加上意向锁，告诉其他人这个数据页或数据表已经有人上过排它锁了**，这样当其他人想要获取数据表排它锁的时候，只需要了解是否有人已经获取了这个数据表的意向排他锁即可。

- 如果事务想要获得数据表中某些记录的**共享锁**，就需要在数据表上**添加意向共享锁**。
- 如果事务想要获得数据表中某些记录的**排他锁**，就需要在数据表上**添加意向排他锁**。

这时，意向锁会告诉其他事务已经有人锁定了表中的某些记录。

**InnoDB会在行锁/表锁建立的同时，在表锁/页锁上新增一个意向锁。当其他事务要操作该行/表时，会先去和表/页上与意向锁进行兼容互斥的判断，就不用一条一条查了（节省了查询时间，提高了效率）。**

**意向锁之间的兼容互斥性：**

|                | 意向共享锁(IS) | 意向排它锁(IX) |
| -------------- | -------------- | -------------- |
| 意向共享锁(IS) | 兼容           | 兼容           |
| 意向排它锁(IX) | 兼容           | 兼容           |

**意向锁和普通锁的兼容互斥性：**

|           | 意向共享锁(IS) | 意向排它锁(IX) |
| --------- | -------------- | -------------- |
| 共享锁(S) | 兼容           | 互斥           |
| 排它锁(X) | 互斥           | 互斥           |

**意向锁的并发性**

意向锁不会与行级的共享 / 排他锁互斥！正因为如此，意向锁并不会影响到多个事务对不同数据行加排他锁时的并发性。（不然我们直接用普通的表锁就行了）

**从上面的案例可以得到如下结论：**

1. InnoDB 支持 **多粒度锁** ，特定场景下，行级锁可以与表级锁共存。
2. 意向锁之间互不排斥，但除了 IS 与 S 兼容外， **意向锁会与 共享锁 / 排他锁 互斥** 。 
3. IX，IS是表级锁，不会和行级的X，S锁发生冲突。只会和表级的X，S发生冲突。
4. 意向锁在保证并发性的前提下，实现了 **行锁和表锁共存** 且 **满足事务隔离性** 的要求。

###### 自增锁（AUTO -INC锁）

在使用MySQL过程中，我们可以为表的某个列添加 AUTO_INCREMENT 属性。举例：

```
CREATE TABLE `teacher`(
    `id`   int          NOT NULL AUTO_INCREMENT,
    `name` varchar(255) NOT NULL,
    PRIMARY KEY (`id`)
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4
  COLLATE = utf8mb4_0900_ai_ci;
```

由于这个表的id字段声明了AUTO_INCREMENT，意味着在书写插入语句时不需要为其赋值，SQL语句修改如下所示。

```
INSERT INTO `teacher` (name) VALUES ('zhangsan'), ('lisi');
```

上边的插入语句并没有为id列显式赋值，所以系统会自动为它赋上递增的值，结果如下所示。

```
mysql> select * from teacher; 
+----+----------+
| id | name 	| 
+----+----------+
| 1 | zhangsan  |
| 2 | lisi 		| 
+----+----------+
2 rows in set (0.00 sec)
```

现在我们看到的上面插入数据只是一种简单的插入模式，所有插入数据的方式总共分为三类，分别是**“ Simple inserts ”，“ Bulk inserts ”**和**“ Mixed-mode inserts ”**。

1. **“Simple inserts” （简单插入）**

可以 **预先确定要插入的行数** （当语句被初始处理时）的语句。包括没有嵌套子查询的单行和多行`INSERT...VALUES()`和 `REPLACE` 语句。比如我们上面举的例子就属于该类插入，已经确定要插入的行数。

1. **“Bulk inserts” （批量插入）**

**事先不知道要插入的行数** （和所需自动递增值的数量）的语句。比如`INSERT ... SELECT` ， `REPLACE ... SELECT` 和 `LOAD DATA` 语句，但不包括纯INSERT。 InnoDB在每处理一行，为AUTO_INCREMENT列分配一个新值。

1. **“Mixed-mode inserts” （混合模式插入）**

这些是“Simple inserts”语句但是指定部分新行的自动递增值。例如 `INSERT INTO teacher (id,name) VALUES (1,'a'), (NULL,'b'), (5,'c'), (NULL,'d');` 只是指定了部分id的值。另一种类型的“混合模式插入”是 `INSERT ... ON DUPLICATE KEY UPDATE` 。

对于上面数据插入的案例，MySQL中采用了**自增锁**的方式来实现，**AUTO_INC锁是当向使用含有AUTO_INCREMENT列的表中插入数据时需要获取的一种特殊的表级锁**，在执行插入语句时就在表级别加一个AUTO_INC锁，然后为每条待插入记录的AUTO_INCREMENT修饰的列分配递增的值，在该语句执行结束后，再把AUTO_INC锁释放掉。**一个事务在持有AUTO_INC锁的过程中，其他事务的插入语句都要被阻塞**，可以保证一个语句中分配的递增值是连续的。也正因为此，其并发性显然并不高，**当我们向一个有AUTO_INCREMENT关键字的主键插入值的时候，每条语句都要对这个表锁进行竞争**，这样的并发潜力其实是很低下的，所以innodb通过`innodb_autoinc_lock_mode`的不同取值来提供不同的锁定机制，来显著提高SQL语句的可伸缩性和性能。

innodb_autoinc_lock_mode有三种取值，分别对应与不同锁定模式：

1. `innodb_autoinc_lock_mode = 0(“传统”锁定模式)`

   在此锁定模式下，所有类型的insert语句都会获得一个特殊的表级AUTO-INC锁，用于插入具有AUTO_INCREMENT列的表。这种模式其实就如我们上面的例子，即每当执行insert的时候，都会得到一个表级锁(AUTO-INC锁)，使得语句中生成的auto_increment为顺序，且在binlog中重放的时候，可以保证master与slave中数据的auto_increment是相同的。因为是表级锁，当在同一时间多个事务中执行insert的时候，对于AUTO-INC锁的争夺会 **限制并发** 能力。

2. `innodb_autoinc_lock_mode = 1(“连续”锁定模式)`

   在 MySQL 8.0 之前，连续锁定模式是 **默认** 的。

   在这个模式下，“bulk inserts”仍然使用AUTO-INC表级锁，并保持到语句结束。这适用于所有INSERT ... SELECT，REPLACE ... SELECT LOAD DATA语句。同一时刻只有一个语句可以持有AUTO-INC锁。

   对于“Simple inserts”（要插入的行数事先已知），则通过在 **mutex（轻量锁）** 的控制下获得所需数量的自动递增值来避免表级AUTO-INC锁， 它只在分配过程的持续时间内保持，而不是直到语句完成。不使用表级AUTO-INC锁，除非AUTO-INC锁由另一个事务保持。如果另一个事务保持AUTO-INC锁，则“Simple inserts”等待AUTO-INC锁，如同它是一个“bulk inserts”。

3. `innodb_autoinc_lock_mode = 2(“交错”锁定模式)`

   从 MySQL 8.0 开始，交错锁模式是 **默认** 设置。

   在这种锁定模式下，所有类INSERT语句都不会使用表级AUTO_INC锁，并且可以同时执行多个语句。这是最快和最可扩展的锁定模式，但是当使用基于语句的复制或恢复方案时，**从二进制日志重播SQL语句时，这是不安全的**。

   在此锁定模式下，自动递增值 **保证** 在所有并发执行的所有类型的insert语句中是 **唯一** 且 **单调递增** 的。但是，由于多个语句可以同时生成数字（即，跨语句交叉编号），**为任何给定语句插入的行生成的值可能不是连续的**。

如果执行的语句是“simple inserts”，其中要插入的行数已提前知道，除了“Mixed-mode inserts"之外，为单个语句生成的数字不会有间隙。然而，当执行“bulk inserts”时，在由任何给定语句分配的自动递增值中可能存在间隙。

###### 元数据锁（MDL锁）

MySQL5.5引入了meta data lock，简称MDL锁，属于表锁范畴。MDL 的作用是，保证读写的正确性。比如，如果一个查询正在遍历一个表中的数据，而执行期间另一个线程对这个 **表结构做变更** ，增加了一列，那么查询线程拿到的结果跟表结构对不上，肯定是不行的。

因此，**当对一个表做增删改查操作的时候，加 MDL 读锁；当要对表做结构变更操作的时候，加 MDL 写锁**。

读锁之间不互斥，因此你可以有多个线程同时对一张表增删改查。读写锁之间、写锁之间是互斥的，用来保证变更表结构操作的安全性，解决了DML和DDL操作之间的一致性问题。**不需要显式使用**，在访问一个表的时候会被自动加上。

读表

![image-20230129150217613](/image-20230129150217613.png)

在另一个会话中改表结构

![image-20230129150233199](/image-20230129150233199.png)

进程等待

查询当前的锁：

![image-20230129151006086](/image-20230129151006086.png)

##### InnoDB中的行锁

行锁(Row Lock)也称为记录锁，顾名思义，就是锁住某一行(某条记录row)。需要的注意的是，MySQL 服务器层并没有实现行锁机制，**行级锁只在存储引擎层实现**。

- **优点：**锁定力度小，发生**锁冲突概率低**，可以实现的**并发度高**。
- **缺点：**对于**锁的开销比较大**，加锁会比较慢，容易出现**死锁**情况。

InnoDB与MyISAM的最大不同有两点：一是支持事务（TRANSACTION）；二是采用了行级锁。

**举例**

创建表

![image-20230129151024092](/image-20230129151024092.png)

###### 记录锁（Record Locks）

记录锁也就是仅仅把一条记录锁上，官方的类型名称为：`LOCK_REC_NOT_GAP` 。比如我们把id值为8的那条记录加一个记录锁的示意图如图所示。仅仅是锁住了id值为8的记录，对周围的数据没有影响。

![image-20230129151143850](/image-20230129151143850.png)

举例如下：

![image-20230129151323020](/image-20230129151323020.png)

记录锁是有S锁和X锁之分的，称之为 **S型记录锁** 和 **X型记录锁** 。

- 当一个事务获取了一条记录的S型记录锁后，其他事务也可以继续获取该记录的S型记录锁，但不可以继续获取X型记录锁；
- 当一个事务获取了一条记录的X型记录锁后，其他事务既不可以继续获取该记录的S型记录锁，也不可以继续获取X型记录锁。

**和表锁一样，只不过锁的对象是行**

###### 间隙锁（Gap Locks）

**MySQL** 在 **REPEATABLE READ** 隔离级别下是可以解决幻读问题的，解决方案有两种，可以使用 **MVCC** 方案解决，也可以采用 加锁 方案解决。但是在使用加锁方案解决时有个大问题，就是事务在第一次执行读取操作时，那些幻影记录尚不存在，我们无法给这些 **幻影记录** 加上 **记录锁** 。InnoDB提出了一种称之为**Gap Locks** 的锁，官方的类型名称为： **LOCK_GAP** ，我们可以简称为 **gap锁** 。比如，把id值为8的那条记录加一个gap锁的示意图如下。

![image-20230129152821774](/image-20230129152821774.png)

图中id值为8的记录加了gap锁，意味着 **不允许别的事务在id值为8的记录前边的间隙插入新记录** ，其实就是id列的值(3, 8)这个区间的新记录是不允许立即插入的。比如，有另外一个事务再想插入一条id值为4的新记录，它定位到该条新记录的下一条记录的id值为8，而这条记录上又有一个gap锁，所以就会阻塞插入操作，直到拥有这个gap锁的事务提交了之后，id列的值在区间(3, 8)中的新记录才可以被插入。

**gap锁的提出仅仅是为了防止插入幻影记录而提出的。**虽然**有共享gap锁**和**独占gap锁**这样的说法，但是它们起到的作用是相同的。而且如果对一条记录加了gap锁（不论是共享gap锁还是独占gap锁），并不会限制其他事务对这条记录加记录锁或者继续加gap锁。

**举例1**

2加锁，1-3之间不能加数据

4/5/6/7加锁，3-8之间不能加数据

. . .

注意，给一条记录加了**gap锁**只是**不允许**其他事务往这条记录前边的间隙**插入新记录**，那对于最后一条记录之后的间隙，也就是student表中id值为**20**的记录之后的间隙该咋办呢？也就是说给哪条记录加**gap锁**才能阻止其他事务插入**id**值在(20，+∞) 这个区间的新记录呢?这时候我们在讲数据页时绍的两条伪记录派上用场了：

- **Infimum**记录，表示该页面中最小的记录。
- **Supremum**记录，表示该页面中最大的记录。

为了实现阻止其他事务插入id值在(20, +∞)这个区间的新记录，我们可以给索引中的最后一条记录，也就是id值为20的那条记录所在页面的Supremum记录加上一个gap锁， 如图所示。

![image-20230129153909877](/image-20230129153909877.png)

检测：

```
SELECT * FROM performance_schema.data_locks\G
```

![image-20230129153927944](/image-20230129153927944.png)

**举例2——死锁**

间隙锁的引入，可能会导致同样的语句锁住更大的范围，这其实是影响了并发度的。

session1：1（S锁） 4

![image-20230129154010086](/image-20230129154010086.png)

session2：2（X锁）3

![image-20230129153940903](/image-20230129153940903.png)

session1先锁，session2后锁，session2要修改数据但是被session1锁住了，session1要修改数据被session2锁住了

session2报错了，session1成功了

![image-20230129154024779](/image-20230129154024779.png)

原因：报错的回滚了，死锁解除了，这边的就成功了

###### 临键锁（Next-Key ocks）

有时候我们既想 **锁住某条记录** ，又想**阻止**其他事务在该记录前边的 **间隙插入新记录** ，所以InnoDB就提出了一种称之为 **Next-Key Locks** 的锁，官方的类型名称为： **LOCK_ORDINARY** ，我们也可以简称为**next-key锁** 。Next-Key Locks是在存储引擎 **innodb** 、事务级别在 **可重复读** 的情况下使用的数据库锁，innodb默认的锁就是Next-Key locks。比如，我们把id值为8的哪条记录加一个next-key锁的示意图，如下：

![image-20230129154103691](/image-20230129154103691.png)

**next-key锁**的本质就是一个**记录锁**和一个**gap锁**的合体，它既能保护该条记录，又能阻止别的事务将新记录插入被保护记录前边的**间隙**。

```
begin; 
select * from student where id <=8 and id > 3 for update;
```

![image-20230129154307837](/image-20230129154307837.png)

###### 插入意向锁（Insert Intention Locks）

我们说一个事务在 **插入** 一条记录时需要判断一下插入位置是不是被别的事务加了 `gap锁` （ `next-key锁`也包含 `gap锁` ），如果有的话，插入操作需要等待，直到拥有 gap锁 的那个事务提交。但是**InnoDB规定事务在等待的时候也需要在内存中生成一个锁结构**，表明有事务想在某个 `间隙` 中 `插入` 新记录，但是现在在等待。InnoDB就把这种类型的锁命名为 `Insert Intention Locks` ，官方的类型名称为：`LOCK_INSERT_INTENTION` ，我们称为 `插入意向锁` 。插入意向锁是一种 `Gap锁` ，不是意向锁，在insert操作时产生。

创建一个间隙锁

![image-20230129154505090](/image-20230129154505090.png)

用另一个会话开启事务，在间隙内插入数据时，该语句会进入等待。

![image-20230129154528907](/image-20230129154528907.png)

再用一个会话开启事务，在间隙内插入数据时，该语句也会进入等待。

![image-20230129154611249](/image-20230129154611249.png)

此时，间隙锁释放后，11就可以插入，12会在11插入后插入。（两个插入语句之间不会相互阻塞）

插入意向锁是在插入一条记录行前，由 **INSERT 操作产生的一种间隙锁** 。该锁用以表示插入意向，当多个事务在同一区间(gap) 插入位置不同的多条数据时，事务之间不需要互相等待。假设存在两条值分别为4和7的记录，两个不同的事务分别试图插入值为5和6的两条记录，每个事务在获取插入行上独占的(排他)锁前，都会获取(4, 7)之间的间隙锁，但是因为数据行之间并**不冲突**，所以两个事务之间并不会产生冲突(阻塞等待)。

总结来说，插入意向锁的特性可以分成两部分：

1. 插入意向锁是一种**特殊的间隙锁**——间隙锁可以锁定开区间内的部分记录。
2. 插入意向锁之间**互不排斥**，所以即使多个事务在同一区间插入多条记录,只要记录本身(主键、唯一索引)不冲突,那么事务之间就不会出现冲突等待。

注意，虽然插入意向锁中含有意向锁三个字,但是它并不属于意向锁而属于间隙锁，因为意向锁是表锁而插入意向锁是**行锁**。

比如，把id值为8的那条记录加一个插入意向锁的示意图如下：

![image-20230129154655648](/image-20230129154655648.png)

比如，现在T1为id值为8的记录加了一个gap锁，然后T2和T3分别想向student表中插入id值分别为4、5的两条记录，所以现在为id值为8的记录加的锁的示意图就如下所示：

![image-20230129154710526](/image-20230129154710526.png)

事实上**插入意向锁并不会阻止别的事务继续获取该记录上任何类型的锁**。

##### 页锁

页锁就是在 **页的粒度** 上进行锁定，锁定的数据资源比行锁要多，因为一个页中可以有多个行记录。当我们使用页锁的时候，会出现数据浪费的现象，但这样的浪费最多也就是一个页上的数据行。**页锁的开销介于表锁和行锁之间，会出现死锁。锁定粒度介于表锁和行锁之间，并发度一般。**

每个层级的锁数量是有限制的，因为锁会占用内存空间， **锁空间的大小是有限的** 。当某个层级的锁数量超过了这个层级的阈值时，就会进行 **锁升级** 。锁升级就是用更大粒度的锁替代多个更小粒度的锁，比如InnoDB 中行锁升级为表锁，这样做的好处是占用的锁空间降低了，但同时数据的并发度也下降了。

#### 从对待锁的态度划分：乐观锁、悲观锁

从对待锁的态度来看锁的话，可以将锁分成乐观锁和悲观锁，从名字中也可以看出这两种锁是两种看待**数据并发的思维方式** 。需要注意的是，乐观锁和悲观锁并不是锁，而是锁的 **设计思想** 。 

##### 悲观锁

悲观锁是一种思想，顾名思义，就是很悲观，对数据被其他事务的修改持保守态度，会通过数据库自身的锁机制来实现，从而保证数据操作的排它性。

悲观锁总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会 **阻塞** 直到它拿到锁（**共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程**）。比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁，当其他线程想要访问数据时，都需要阻塞挂起。Java中 **synchronized** 和 **ReentrantLock** 等独占锁就是悲观锁思想的实现。

注意：**select ... for update语句执行过程中所有扫描的行都会被锁上，因此在MySQL中用悲观锁必须确定使用了索引，而不是全表扫描，否则将会把整个表锁住。**

悲观锁不适用的场景较多，它存在一些不足，因为悲观锁大多数情况下依靠数据库的锁机制来实现，以保证程序的并发访问性，同时这样对数据库性能开销影响也很大，特别是**长事务**而言，这样的**开销往往无法承受**，这时就需要乐观锁。

##### 乐观锁

乐观锁认为对同一数据的并发操作不会总发生，属于小概率事件，不用每次都对数据上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，也就是**不采用数据库自身的锁机制，而是通过程序来实现**。在程序上，我们可以采用 `版本号机制` 或者 `CAS机制` 实现。**乐观锁适用于多读的应用类型，这样可以提高吞吐量。**在Java中`java.util.concurrent.atomic` 包下的原子变量类就是使用了乐观锁的一种实现方式：CAS实现的。

1. **乐观锁的版本号机制**

在表中设计一个 `版本字段 version` ，第一次读的时候，会获取 version 字段的取值。然后对数据进行更新或删除操作时，会执行 `UPDATE ... SET version=version+1 WHERE version=version` 。此时如果已经有事务对这条数据进行了更改，修改就不会成功。

这种方式类似我们熟悉的SVN、CVS 版本管理系统，当我们修改了代码进行提交时，首先会检查当前版本号与服务器上的版本号是否一致，如果一致就可以直接提交，如果不一致就需要更新服务器上的最新代码，然后再进行提交。

1. **乐观锁的时间戳机制**

时间戳和版本号机制一样，也是在更新提交的时候，将当前数据的时间戳和更新之前取得的时间戳进行比较，如果两者一致则更新成功，否则就是版本冲突。

你能看到乐观锁就是程序员自己控制数据并发操作的权限，基本是通过给数据行增加一个戳（版本号或者时间戳），从而证明当前拿到的数据是否最新。

##### 两种锁的使用场景

从这两种锁的设计思想中，我们总结一下乐观锁和悲观锁的适用场景：

1. **乐观锁** 适合 **读操作多** 的场景，相对来说写的操作比较少。它的优点在于 **程序实现 ， 不存在死锁问题**，不过适用场景也会相对乐观，因为它阻止不了除了程序以外的数据库操作。
2. **悲观锁** 适合 **写操作多** 的场景，因为写的操作具有 **排它性** 。采用悲观锁的方式，可以在数据库层面阻止其他事务对该数据的操作权限，防止 `读 - 写` 和 `写 - 写` 的冲突。

![image-20230129155925613](/image-20230129155925613.png)

#### 按加锁的方式划分：显式锁、隐式锁 

##### 隐式锁

一个事务在执行`INSERT`操作时，如果即将插入的`间隙`已经被其他事务加了`gap锁`，那么本次`INSERT`操作会阻塞，并且当前事务会在该间隙上加一个插入意向锁，否则一般情况下INSERT操作是不加锁的。那如果一个事务首先插入了一条记录(此时并没有在内存生产与该记录关联的锁结构)，然后另一个事务: 

- 立即使用`SELECT LOCK IN SHARE MODE` 语句读取这条记录，也就是要获取这条记录的S锁，或者使用`SELECT ... FOR UPDATE` 语句读取这条记录，也就是要获取这条记录的X锁，怎么办?

  如果允许这种情况的发生，那么可能产生**脏读**问题。

- 立即修改这条记录，也就是要获取这条记录的**X锁**，怎么办？

  如果允许这种情况的发生，那么可能产生**脏写**问题。

这时候我们前边提过的**事务id**又要起作用了。我们把聚簇索引和二级索引中的记录分开看一下:

**情景一：**对于聚簇索引记录来说，有一个 **trx_id** 隐藏列，该隐藏列记录着最后改动该记录的 **事务 id** 。那么如果在当前事务中新插入一条聚簇索引记录后，该记录的 **trx_id** 隐藏列代表的的就是当前事务的 **事务id** ，如果事务2此时想对该记录添加 **S锁** 或者 **X锁** 时，首先会看一下该记录的**trx_id** 隐藏列代表的事务是否是当前的活跃事务，如果是的话，那么就帮助当前事务创建一个 **X 锁** （也就是为当前事务创建一个锁结构， **is_waiting 属性是 false** ），然后事务2自己进入等待状态（也就是为自己也创建一个锁结构， **is_waiting 属性是 true** ）。

**情景二：**对于二级索引记录来说，本身并没有 **trx_id 隐藏列**，但是在二级索引页面的 **Page Header** 部分有一个 **PAGE_MAX_TRX_ID** 属性，该属性代表对该页面做改动的最大的 **事务id** ，如果 **PAGE_MAX_TRX_ID** 属性值小于当前最小的活跃 **事务id** ，那么说明对该页面做修改的事务都已经提交了，否则就需要在页面中定位到对应的二级索引记录，然后回表找到它对应的聚簇索引记录，然后再重复 **情景一** 的做法。

即：一个事务对新插入的记录可以不显式的加锁(生成一个锁结构) ，但是由于**事务id**的存在，相当于加了一个隐式锁。别的事务在对这条记录加S锁或者X锁时，由于**隐式锁**的存在，会先帮助当前事务生成一个锁结构，然后自己再生成一个锁结构后进入等待状态。隐式锁是一种延迟加锁的机制，从而来减少加锁的数量。

隐式锁在实际内存对象中并不含有这个锁信息。只有当产生锁等待时，隐式锁转化为显式锁。

InnoDB的insert操作，对插入的记录不加锁，但是此时如果另一个线程进行当前读，类似以下的用例，session 2 会锁等待session 1，那么这是如何实现的呢?

**session 1:** 

```
mysql> begin;
Query OK, 0 rows affected (0.00 sec) 

mysql> insert INTO student VALUES(34,"周八","二班"); 
Query OK, 1 row affected (0.00 sec)
```

**session 2:**

```
mysql> begin; 
Query OK, 0 rows affected (0.00 sec) 

mysql> select * from student lock in share mode; #执行完，当前事务被阻塞
```

执行下述语句，输出结果：

```
SELECT * FROM performance_schema,data__lock_waits\G;
```

![image-20230129163241363](/image-20230129163241363.png)

在执行session2之前，查询不出锁；执行后，隐式锁变为显示锁

**隐式锁的逻辑过程如下：**

A. InnoDB的每条记录中都一个隐含的trx_id字段，这个字段存在于聚簇索引的B+Tree中。

B. 在操作一条记录前，首先根据记录中的trx_id检查该事务是否是活动的事务(未提交或回滚)。如果是活动的事务，首先将 **隐式锁** 转换为 **显式锁** (就是为该事务添加一个锁)。 

C. 检查是否有锁冲突，如果有冲突，创建锁，并设置为waiting状态。如果没有冲突不加锁，跳到E。

D. 等待加锁成功，被唤醒，或者超时。

E. 写数据，并将自己的trx_id写入trx_id字段。

##### 显式锁

通过特定的语句进行加锁，我们一般称之为显示加锁，例如：

显示加共享锁：

```
select .... lock in share mode
```

显示加排它锁：

```
select .... for updat
```

#### 其它锁之：全局锁

全局锁就是对 **整个数据库实例** 加锁。当你需要让整个库处于 **只读状态** 的时候，可以使用这个命令，之后他线程的以下语句会被阻塞：数据更新语句（数据的增删改）、数据定义语句（包括建表、修改表结构等）和更新类事务的提交语句。全局锁的典型使用 **场景** 是：做 **全库逻辑备份** 。

全局锁的命令：

```
Flush tables with read lock
```

#### 其它锁之：死锁

##### 概述

两个事务都持有对方需要的锁，并且在等待对象释放，并且双方都不会释放自己的锁。死锁示例：

**举例1**

![image-20230129163658241](/image-20230129163658241.png)

**举例2**

```
#事务1
update account set balance = balance - 100 where name = 'A'; #操作1
update account set balance = balance + 100 where name = 'B'; #操作3

#事务2
update account set balance = balance - 100 where name = 'B'; #操作2 
update account set balance = balance + 100 where name = 'A'; #操作4
```

##### 产生死锁的必要条件

1. 两个或者两个以上事务
2. 每个事务都已经持有锁并且申请新的锁
3. 锁资源同时只能被同一个事务持有或者不兼容
4. 事务之间因为持有锁和申请锁导致彼此循环等待

> 死锁的关键在于：两个(或以上)的Session加锁的顺序不一致。

##### 如何处理死锁

**方式一：等待，直到超时（默认50秒）**

![image-20230129163716404](/image-20230129163716404.png)

**方式二：使用死锁检测进行死锁处理**

方式1检测死锁太过被动，innodb还提供了`wait-for-graph`算法来主动进行死锁检测，每当加锁请求无法立即满足需要并进入等待时，`wait-for-graph`算法都会被触发。

这是一种较为`主动的死锁检测机制`，要求数据库保存`锁的信息链表`和`事务等待链表`两部分信息。

![image-20230129163732037](/image-20230129163732037.png)

基于这两个信息，可以绘制wait-for-graph（等待图）

![image-20230129163743501](/image-20230129163743501.png)

> 死锁检测的原理是构建一个以事务为顶点、锁为边的有向图，判断有向图是否存在环，存在即有死锁。

一旦检测到回路、有死锁，这时候InnoDB存储引擎会选择`回滚undo量最小的事务`，让其他事务继续执行( innodb_ deadlock_ detect=on 表示开启这个逻辑)。

**缺点**：每个新的被阻塞的线程，都要判断是不是由于自己的加入导致了死锁，这个操作时间复杂度是O(n)。 如果100个并发线程同时更新同一行，意味着要检测100* 100 = 1万次，1万个线程就会有1千万次检测。

**如何解决？**

- 方式1：关闭死锁检测，但意味着可能会出现大量的超时，会导致业务有损。

- 方式2：控制并发访问的数量。比如在中间件中实现对于相同行的更新，在进入引擎之前排队，这样在InnoDB内部就不会有大量的死锁检测工作。

  这个并发控制要做在 **数据库服务端** 。如果你有中间件，可以考虑在 **中间件实现** ；甚至有能力修改MySQL源码的人，也可以做在MySQL里面。基本思路就是，对于相同行的更新，在进入引擎之前排队，这样在InnoDB内部就不会有大量的死锁检测工作了。

  ### 锁的内存结构

**进一步思路**

可以考虑通过将一行改成逻辑上的多行来减少**锁冲突**。

比如，连锁超市账户总额的记录，可以考虑放到多条记录上。账户总额等于这多个记录的值的总和。（直接在总部的数据库中操作，锁冲突会释放频繁，可以先在各自的店中对好账目，在规定的时间分批上传，以减少数据库中的所冲突）

##### 如何避免死锁

- **合理设计索引**，使业务SQL尽可能通过索引|定位更少的行，减少锁竞争。
- **调整业务逻辑SQL执行顺序**，避免update/delete长时间持有锁的SQL在事务前面。
- **避免大事务**，尽量将大事务拆成多个小事务来处理，小事务缩短锁定资源的时间，发生锁冲突的几率也更小。
- 在并发比较高的系统中，**不要显式加锁**，特别是在事务里显式加锁。如select ... for update语句，如果是在事务里运行了start transaction或设置了autocommit等于0，那么就会锁定所查找到的记录。
- **降低隔离级别**。如果业务允许，将隔离级别调低也是较好的选择，比如将隔离级别从RR调整为RC，可以避免掉很多因为gap锁造成的死锁。

### 锁的内存结构

我们前边说对一条记录加锁的本质就是在内存中创建一个**锁结构**与之关联，那么是不是一个事务对多条记录加锁，就要创建多个**锁结构**呢？比如：

```
#事务T1
SELECT * FROM user LOCK IN SHARE MODE ;
```

理论上创建多个**锁结构**没问题，但是如果一个事务要获取10000条记录的锁，生成10000个锁结构也太崩溃了！所以决定在对不同记录加锁时，如果符合下边这些条件的记录会放到一个**锁结构**中。

- 在同一个事务中进行加锁操作
- 被加锁的记录在同一个页面中
- 加锁的类型是一样的
- 等待状态是一样的

**InnoDB** 存储引擎中的 **锁结构** 如下：

![image-20230129165342609](/image-20230129165342609.png)

结构解析：

1. **锁所在的事务信息** ：

   不论是 **表锁** 还是 **行锁** ，都是在事务执行过程中生成的，哪个事务生成了这个 **锁结构** ，这里就记录这个事务的信息。

   此 **锁所在的事务信息** 在内存结构中只是一个指针，通过指针可以找到内存中关于该事务的更多信息，比方说事务id等。

2. **索引信息** ：

   对于 **行锁** 来说，需要记录一下加锁的记录是属于哪个索引的。这里也是一个指针。

3. **表锁／行锁信息** ： 

   **表锁结构** 和 **行锁结构** 在这个位置的内容是不同的：

   - 表锁：

     记载着是对哪个表加的锁，还有其他的一些信息。

   - 行锁：      

   - 记载了三个重要的信息：

     - `Space ID` ：记录所在表空间。

     - `Page Number` ：记录所在页号。

     - `n_bits` ：对于行锁来说，一条记录就对应着一个比特位，一个页面中包含很多记录，用不同的比特位来区分到底是哪一条记录加了锁。为此在行锁结构的末尾放置了一堆比特位，这个`n_bits` 属性代表使用了多少比特位。

       > n_bits的值一般都比页面中记录条数多一些。主要是为了之后在页面中插入了新记录后也不至于重新分配锁结构

4. **type_mode** ：

   这是一个32位的数，被分成了 `lock_mode` 、`lock_type` 和 `rec_lock_type` 三个部分，如图所示：

   ![image-20230129165400132](/image-20230129165400132.png)

   - 锁的模式（ `lock_mode` ），占用低4位，可选的值如下：

     - `LOCK_IS` （十进制的 0 ）：表示共享意向锁，也就是 `IS锁` 。
     - `LOCK_IX` （十进制的 1 ）：表示独占意向锁，也就是`IX锁` 。 
     - `LOCK_S` （十进制的 2 ）：表示共享锁，也就是 `S锁`。 
     - `LOCK_X` （十进制的 3 ）：表示独占锁，也就是`X锁` 。 
     - `LOCK_AUTO_INC` （十进制的 4 ）：表示 `AUTO-INC锁`。 

     在InnoDB存储引擎中，LOCK_IS，LOCK_IX，LOCK_AUTO_INC都算是表级锁的模式，LOCK_S和 LOCK_X既可以算是表级锁的模式，也可以是行级锁的模式。

   - 锁的类型（ `lock_type` ），占用第5～8位，不过现阶段只有第5位和第6位被使用：

     - `LOCK_TABLE` （十进制的 `16` ），也就是当第5个比特位置为1时，表示表级锁。
     - `LOCK_REC` （十进制的 `32` ），也就是当第6个比特位置为1时，表示行级锁。

   - 行锁的具体类型（`rec_lock_type` ），使用其余的位来表示。只有在 lock_type 的值为

     - `LOCK_ORDINARY` （十进制的 0 ）：表示 next-key锁 。 
     - `LOCK_GAP` （十进制的 512 ）：也就是当第10个比特位置为1时，表示 gap锁 。 
     - `LOCK_REC_NOT_GAP` （十进制的 1024 ）：也就是当第11个比特位置为1时，表示正经 记录 锁 。
     - `LOCK_INSERT_INTENTION` （十进制的 2048 ）：也就是当第12个比特位置为1时，表示插入意向锁。其他的类型：还有一些不常用的类型我们就不多说了。

- `is_waiting` 属性呢？基于内存空间的节省，所以把 `is_waiting` 属性放到了 `type_mode` 这个32位的数字中：
  - `LOCK_WAIT` （十进制的 256 ） ：当第9个比特位置为 1 时，表示 `is_waiting` 为 `true` ，也就是当前事务尚未获取到锁，处在等待状态；当这个比特位为 0 时，表示 `is_waiting` 为 `false` ，也就是当前事务获取锁成功。

1. **其他信息** ：

   为了更好的管理系统运行过程中生成的各种锁结构而设计了各种**哈希表**和**链表**。

2. **一堆比特位** ：

   如果是 **行锁结构** 的话，在该结构末尾还放置了一堆比特位，比特位的数量是由上边提到的 `n_bits` 属性表示的。

   InnoDB数据页中的每条记录在 `记录头信息` 中都包含一个`heap_no` 属性，伪记录 `Infimum` 的 `heap_no` 值为 0 ， `Supremum` 的 `heap_no` 值为 1 ，之后每插入一条记录，`heap_no` 值就增1。 

   `锁结构` 最后的一堆比特位就对应着一个页面中的记录，一个比特位映射一个 `heap_no` ，即一个比特位映射到页内的一条记录。

### 锁监控

关于MySQL锁的监控，我们一般可以通过检查 InnoDB_row_lock 等状态变量来分析系统上的行锁的争夺情况

```
mysql> show status like 'innodb_row_lock%'; 
+-------------------------------+-------+
| Variable_name	 				| Value | 
+-------------------------------+-------+
| Innodb_row_lock_current_waits | 0 	| #正在等待的锁的数量
| Innodb_row_lock_time 			| 0 	| #系统启动锁等待的总时间
| Innodb_row_lock_time_avg 		| 0 	| #平均所等待的时间
| Innodb_row_lock_time_max 		| 0		| #最长等待时间
| Innodb_row_lock_waits 		| 0 	| #锁等待的次数
+-------------------------------+-------+
5 rows in set (0.01 sec)
```

对各个状态量的说明如下：

- Innodb_row_lock_current_waits：当前正在等待锁定的数量；
- Innodb_row_lock_time ：从系统启动到现在锁定总时间长度；（等待总时长）
- Innodb_row_lock_time_avg ：每次等待所花平均时间；（等待平均时长）
- Innodb_row_lock_time_max：从系统启动到现在等待最常的一次所花的时间；
- Innodb_row_lock_waits ：系统启动后到现在总共等待的次数；（等待总次数）

对于这5个状态变量，比较重要的3个见上面（橙色）。

尤其是当等待次数很高，而且每次等待时长也不小的时候，我们就需要分析系统中为什么会有如此多的等待，然后根据分析结果着手指定优化计划。

**其他监控方法：**

MySQL把事务和锁的信息记录在了`information_schema` 库中，涉及到的三张表分别是`INNODB_TRX`、 `INNODB_LOCKS` 和 `INNODB_LOCK_WAITS` 。 

`MySQL5.7及之前` ，可以通过information_schema.INNODB_LOCKS查看事务的锁情况，但只能看到阻塞事务的锁；如果事务并未被阻塞，则在该表中看不到该事务的锁情况。

MySQL8.0删除了information_schema.INNODB_LOCKS，添加了 `performance_schema.data_locks` ，可以通过performance_schema.data_locks查看事务的锁情况，和MySQL5.7及之前不同，performance_schema.data_locks不但可以看到阻塞该事务的锁，还可以看到该事务所持有的锁。

同时，information_schema.INNODB_LOCK_WAITS也被`performance_schema.data_lock_waits` 所代替。

我们模拟一个锁等待的场景，以下是从这三张表收集的信息锁等待场景，我们依然使用记录锁中的案例，当事务2进行等待时，查询情况如下：

（1）查询正在被锁阻塞的sql语句。

```
SELECT * FROM information_schema.INNODB_TRX\G;
```

重要属性代表含义已在上述中标注。

（2）查询锁等待情况

```

```

（3）查询锁的情况

```

```

从锁的情况可以看出来，两个事务分别获取了IX锁，我们从意向锁章节可以知道，IX锁互相时兼容的。所以这里不会等待，但是事务1同样持有X锁，此时事务2也要去同一行记录获取X锁，他们之间不兼容，导致等待的情况发生。

## 多版本并发控制（183-186）

### MVCC

MVCC （Multiversion Concurrency Control），多版本并发控制。顾名思义，MVCC 是通过数据行的多个版本管理来实现数据库的 **并发控制** 。这项技术使得在InnoDB的事务隔离级别下执行 **一致性读** 操作有了保证。换言之，就是为了查询一些正在被另一个事务更新的行，并且可以看到它们被更新之前的值，这样在做查询的时候就不用等待另一个事务释放锁。

MVCC没有正式的标准，在不同的DBMS中MVCC的实现方式可能是不同的，也不是皮鞭使用的（大家可以参考DBMS文档）。这里讲解InnoDB中的MVCC的实现机制。

### 快照读与当前读

MVCC在MySQL InnoDB中的实现主要是为了提高数据库并发性能，用更好的方式去处理 `读-写冲突` ，做到即使有读写冲突时，也能做到 **不加锁** ， **非阻塞并发读** ，而这个读指的就是 **快照读** , 而非 **当前读** 。当前读实际上是一种加锁的操作，是悲观锁的实现。而MVCC本质是采用乐观锁思想的一种方式。

**读——快照读，写——当前读**

#### 快照读

快照读又叫**一致性读**，**读取的是快照数据**。**不加锁的简单的 SELECT 都属于快照读**，即不加锁的非阻塞读；比如这样：

```
SELECT * FROM player WHERE ...
```

之所以出现快照读的情况，是基于提高并发性能的考虑，快照读的实现是基于MVCC，它在很多情况下，避免了加锁操作，降低了开销。

既然是基于多版本，那么快照读可能读到的并不一定是数据的最新版本，而有可能是之前的历史版本。

快照读的前提是隔离级别不是串行级别，串行级别下的快照读会退化成当前读。

#### 当前读

**当前读读取的是记录的最新版本**（最新数据，而不是历史版本的数据），读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。加锁的 SELECT，或者对数据进行增删改都会进行当前读。比如：

```
SELECT * FROM student LOCK IN SHARE MODE; # 共享锁 
SELECT * FROM student FOR UPDATE; # 排他锁 
INSERT INTO student values ... # 排他锁
DELETE FROM student WHERE ... # 排他锁 
UPDATE student SET ... # 排他锁
```

### 复习

#### 再谈隔离级别

我们知道事务有 4 个隔离级别，可能存在三种并发问题：

![image-20230129165425600](/image-20230129165425600.png)

在MySQL中，默认的隔离级别是可重复读，可以解决脏读和不可重复读的问题，如果仅从定义的角度来看，它并不能解决幻读问题。如果我们想要解决幻读问题，就需要采用串行化的方式，也就是将隔离级别提升到最高，但这样一来就会大幅降低数据库的事务并发能力。

**MVCC可以不采用锁机制，而是通过乐观锁的方式来解决不可重复读和幻读问题**！它可以在大多数情况下替代行级锁，降低系统的开销

![image-20230129165438096](/image-20230129165438096.png)

#### 隐藏字段、Undo Log版本链

回顾一下undo日志的版本链，对于使用 `InnoDB` 存储引擎的表来说，它的聚簇索引记录中都包含两个必要的隐藏列。

- `trx_id` ：每次一个事务对某条聚簇索引记录进行改动时，都会把该事务的 **事务id** 赋值给**trx_id** 隐藏列。
- `roll_pointer` ：每次对某条聚簇索引记录进行改动时，都会把旧的版本写入到 **undo日志** 中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息。

**举例**

student表数据如下：

![image-20230129213151020](/image-20230129213151020.png)

假设插入该记录事务id为8，那么此刻该条记录的示意图如图所示：

![image-20230129165449364](/image-20230129165449364.png)

> insert undo只在事务回滚时起作用，当事务提交后，该类型的undo日志就没用了，它占用的Undo Log Segment也会被系统回收（也就是该undo日志占用的Undo页面链表要么被重用，要么被释放）。

假设之后两个事务id分别为 10 、 20 的事务对这条记录进行 UPDATE 操作，操作流程如下：

![image-20230129165502016](/image-20230129165502016.png)

> 能不能在两个事务中交叉更新同一条记录呢？不能！这不就是一个事务修改了另一个未提交事务修改过的数据（脏写）
>
> InnoDB使用锁来保证不会有脏写情况的发生，也就是在第一个事务更新了某条记录后，就会给这条记录加锁，另一个食物再次更新时就需要等待第一个事务提交了，八锁释放之后才可以继续更新。

每次对记录进行改动，都会记录一条**undo日志**，每条undo日志也都有一个 **roll_pointer** 属性（ **INSERT** 操作对应的undo日志没有该属性，因为该记录并没有更早的版本），可以将这些 **undo日志**都连起来，串成一个链表：

![image-20230129165516885](/image-20230129165516885.png)

对该记录每次更新后，都会将旧值放到一条 **undo日志** 中，就算是该记录的一个旧版本，随着更新次数的增多，所有的版本都会被 **roll_pointer** 属性连接成一个链表，我们把这个链表称之为 **版本链** ，版本链的头节点就是当前记录最新的值。

每个版本中还包含生成该版本时对应的 **事务id** 。

### MVCC实现原理之ReadView

MVCC 的实现依赖于：**隐藏字段、Undo Log、Read View。**

#### 什么是ReadView

在MVCC机制中，多个事务对同一个行记录进行更新会产生多个历史快照，这些历史快照保存在Undo Log里。如果一个事务想要查询这个行记录，需要读取哪个版本的行记录呢？这时就需要用到ReadView了，它帮我们解决了行的可见性问题。

**ReadView就是事务在使用MVCC机制进行快照读操作时产生的读视图**。当事务启动时，会生成数据库系统当前的一个快照。InnoDB 为每个事务构造了一个数组， 用来记录并维护系统当前**活跃事务**的ID (“活跃’ 指的就是，启动了但还没提交)。

#### 设计思路

使用 **READ UNCOMMITTED** 隔离级别的事务，由于可以读到未提交事务修改过的记录，所以直接读取记录的最新版本就好了。

使用 **SERIALIZABLE** 隔离级别的事务，InnoDB规定使用加锁的方式来访问记录。

使用 **READ COMMITTED** 和 **REPEATABLE READ** 隔离级别的事务，都必须保证读到 **已经提交了的** 事务修改过的记录。假如另一个事务已经修改了记录但是尚未提交，是不能直接读取最新版本的记录的，核心问题就是需要判断一下版本链中的哪个版本是当前事务可见的，这是ReadView要解决的主要问题。

这个ReadView中主要包含4个比较重要的内容，分别如下：

1. `creator_trx_id` ，创建这个 Read View 的事务 ID。

> 说明：只有在对表中的记录做改动时（执行INSERT、DELETE、UPDATE这些语句时）才会为事务分配事务id，否则在一个只读事务中的事务id值都默认为0。

1. `trx_ids` ，表示在生成ReadView时当前系统中活跃的读写事务的 **事务id列表** 。 
2. `up_limit_id` ，活跃的事务中最小的事务 ID。
3. `low_limit_id` ，表示生成ReadView时系统中应该分配给下一个事务的 `id`值。low_limit_id 是系统最大的事务id值，这里要注意是系统中的事务id，需要区别于正在活跃的事务ID。

> 注意：low_limit_id并不是trx_ids中的最大值，事务id是递增分配的。比如，现在有id为1， 2，3这三个事务，之后id为3的事务提交了。那么一个新的读事务在生成ReadView时，trx_ids就包括1和2，up_limit_id的值就是1，low_limit_id的值就是4。

**举例**

trx_ ids为tpx2、trx3、 trx5 和trx8的集合，系统的最大事务ID (low_ limit_id) 为trx8+1 (如果之前没有其他的新增事务)，活跃的最小事务ID (up_ limit _id)为trx2。

![image-20230129165528890](/image-20230129165528890.png)

#### ReadView的规则

有了这个ReadView，这样在访问某条记录时，只需要按照下边的步骤判断记录的某个版本是否可见。

- 如果被访问版本的trx_id属性值与`ReadView中的 creator_trx_id` 值相同，意味着当前事务在访问它自己修改过的记录，所以该版本可以被当前事务访问。
- 如果被访问版本的trx_id属性值小于ReadView中的 `up_limit_id` 值，表明生成该版本的事务在当前事务生成ReadView前已经提交，所以该版本可以被当前事务访问。
- 如果被访问版本的trx_id属性值大于或等于ReadView中的 `low_limit_id` 值，表明生成该版本的事务在当前事务生成ReadView后才开启，所以该版本不可以被当前事务访问。
- 如果被访问版本的trx_id属性值在ReadView的 `up_limit_id` 和 `low_limit_id` 之间，那就需要判断一下trx_id属性值是不是在 `trx_ids` 列表中。
  - 如果在，说明创建ReadView时生成该版本的事务还是活跃的，该版本不可以被访问。
  - 如果不在，说明创建ReadView时生成该版本的事务已经被提交，该版本可以被访问。

#### MVCC整体操作流程

了解了这些概念之后，我们来看下当查询一条记录的时候，系统如何通过MVCC找到它：

1. 首先获取事务自己的版本号，也就是事务 ID； 
2. 获取 ReadView；
3. 查询得到的数据，然后与 ReadView 中的事务版本号进行比较；
4. 如果不符合 ReadView 规则，就需要从 Undo Log 中获取历史快照；
5. 最后返回符合规则的数据。

如果某个版本的数据对当前事务不可见的话，那就顺着版本链找到下一个版本的数据，继续按照上边的步骤判断可见性，依此类推，直到版本链中的最后-个版本。如果最后一个版本也不可见的话，那么就意味着该条记录对该事务完全不可见，查询结果就不包含该记录。

> InnoDB中，MVCC是通过Undo Log + Read View进行数据读取，Undo Log保存了历史快照，而Read View规则帮我们判断当前版本的数据是否可见。

在隔离级别为**读已提交**（Read Committed）时，一个事务中的每一次 SELECT 查询都会重新获取一次Read View。

如表所示：

![image-20230129165545320](/image-20230129165545320.png)

> 注意，此时同样的查询语句都会重新获取一次 Read View，这时如果 Read View 不同，就可能产生不可重复读或者幻读的情况。

当隔离级别为**可重复读**的时候，就避免了不可重复读，这是因为一个事务只在第一次 SELECT 的时候会获取一次 Read View，而后面所有的 SELECT 都会复用这个 Read View，如下表所示：

![image-20230129165558736](/image-20230129165558736.png)

### 举例说明

假设现在student表中插入一条由 事务id 为8 的事务插入的一条记录：

```
mysql> select * from student;
+--------+--------+-------+
|	id	 | name	  |	class |
+--------+--------+-------+
|	1	 | 张三	 |  一班  |
+--------+--------+-------+
```

MVCC稚嫩挂在READ COMMITTED 和 REPEATABLE READ两个隔离级别下工作。接下来看一下`READ COMMITTED` 和 `REPEATABLE READ` 所谓的生成ReadView 的实际不同到底不同在哪里。

#### READ COMMITTED隔离级别下 

**READ COMMITTED ：每次读取数据前都生成一个ReadView。**

现在有两个`事务id` 分别为 `10 、 20` 的事务在执行：

```
# Transaction 10 
BEGIN; 
UPDATE student SET name="李四" WHERE id=1; 
UPDATE student SET name="王五" WHERE id=1; 

# Transaction 20 
BEGIN;
# 更新了一些别的表的记录 
...
```

此刻，表student 中 id 为 1 的记录得到的版本链表如下所示：

![image-20230129165617223](/image-20230129165617223.png)

假设现在有一个使用 READ COMMITTED 隔离级别的事务开始执行：

```
# 使用READ COMMITTED隔离级别的事务 
BEGIN;

# SELECT1：Transaction 10、20未提交
SELECT * FROM student WHERE id = 1; # 得到的列name的值为'张三'
```

这个SELECT1的执行过程如下：

步骤1：在执行SELECT语句时会先生成一个ReadView，ReadView 的trx_ids列表的内容就是[10, 20]，up_limit_id为10，low_limit_id为21，creator_trx_id为0。

步骤2：从版本链中挑选可见的记录，从图中看出，最新版本的列name的内容是‘王五' ，该版本的trx_id值为10，在trx_ids列表内，所以不符合可见性要求，根据roll_pointer跳到下一个版本。

步骤3：下一个版本的列name的内容是‘李四'，该版本的trx_id值也为10，也在trx_ids列表内，所以也不符合要求，继续跳到下一个版本。

步骤4：下一个版本的列name的内容是' 张三'，该版本的trx_id值为8，小于ReadView中的up_limit_id值10，所以这个版本是符合要求的，最后返回给用户的版本就是这条列name为'张三'的记录。

之后，我们把 事务id 为 10 的事务提交一下：

```
# Transaction 10 
BEGIN; 

UPDATE student SET name="李四" WHERE id=1; 
UPDATE student SET name="王五" WHERE id=1; 

COMMIT;
```

然后再到 事务id 为 20 的事务中更新一下表 student 中 id 为 1 的记录：

```
# Transaction 20 
BEGIN; 

# 更新了一些别的表的记录 
... 
UPDATE student SET name="钱七" WHERE id=1;
UPDATE student SET name="宋八" WHERE id=1;
```

此刻，表student中 id 为 1 的记录的版本链就长这样：

![image-20230129165630590](/image-20230129165630590.png)

然后再到刚才使用 READ COMMITTED 隔离级别的事务中继续查找这个 id 为 1 的记录，如下：

```
# 使用READ COMMITTED隔离级别的事务
BEGIN; 

# SELECT1：Transaction 10、20均未提交 
SELECT * FROM student WHERE id = 1; 
# 得到的列name的值为'张三'

# SELECT2：Transaction 10提交，Transaction 20未提交 
SELECT * FROM student WHERE id = 1; 
# 得到的列name的值为'王五'
```

这个SELECT2的执行过程如下：

步骤1：在执行SELECT语句时会又会单独生成一个ReadView，该ReadView的trx_ids列表的内容就是[20] ，up_limit_id为20，low_limit_id为21，creator_trx_id为0。

步骤2：从版本链中挑选可见的记录，从图中看出，最新版本的列name的内容是‘宋八'，该版本的trx_id值为20，在trx_ids列表内，所以不符合可见性要求，根据roll_pointer跳到下一个版本。

步骤3：下一个版本的列name的内容是‘钱七'，该版本的trx_id值为20，也在trx_ids列表内，所以也不符合要求，继续跳到下一个版本。

步骤4：下一个版本的列name的内容是‘王五'，该版本的trx_id值为10，小于ReadView中的up_limit_id值20，所以这个版本是符合要求的，最后返回给用户的版本就是这条列name为' 王五'的记录。

以此类推，如果之后事务id为20的记录也提交了，再次在使用READ COMMITTED 隔离级别的事务中查询表student中id值为1的记录时，得到的结果就是‘宋八'了，具体流程我们就不分析了。

> 强调：使用READ COMMITTED隔离级别的事务在每次查询开始时都会生成一个独立的ReadView。

#### REPEATABLE READ隔离级别下

使用 REPEATABLE READ 隔离级别的事务来说，只会在第一次执行查询语句时生成**一个 ReadView** ，之后的查询就不会重复生成了。

比如，系统里有两个 事务id 分别为 10 、 20 的事务在执行：

```
# Transaction 10 
BEGIN;
UPDATE student SET name="李四" WHERE id=1;
UPDATE student SET name="王五" WHERE id=1; 

# Transaction 20 
BEGIN;
# 更新了一些别的表的记录
...
```

此刻，表student 中 id 为 1 的记录得到的版本链表如下所示：

![image-20230129165650327](/image-20230129165650327.png)

假设现在有一个使用 REPEATABLE READ 隔离级别的事务开始执行：

```
# 使用REPEATABLE READ隔离级别的事务 
BEGIN; 

# SELECT1：Transaction 10、20未提交 
SELECT * FROM student WHERE id = 1; # 得到的列name的值为'张三'
```

步骤1：在执行SELECT语句时会先生成一个ReadView，ReadView 的trx_ids列表的内容就是[10, 20]，up_limit_id为10，low_limit_id为21，creator_trx_id为0。

步骤2：从版本链中挑选可见的记录，从图中看出，最新版本的列name的内容是‘王五' ，该版本的trx_id值为10，在trx_ids列表内，所以不符合可见性要求，根据roll_pointer跳到下一个版本。

步骤3：下一个版本的列name的内容是‘李四'，该版本的trx_id值也为10，也在trx_ids列表内，所以也不符合要求，继续跳到下一个版本。

步骤4：下一个版本的列name的内容是' 张三'，该版本的trx_id值为8，小于ReadView中的up_limit_id值10，所以这个版本是符合要求的，最后返回给用户的版本就是这条列name为'张三'的记录。

（和READ COMMITTED一样）

之后，我们把 事务id 为 10 的事务提交一下，就像这样：

```
# Transaction 10 
BEGIN; 

UPDATE student SET name="李四" WHERE id=1; 
UPDATE student SET name="王五" WHERE id=1; 

COMMIT;
```

然后再到 事务id 为 20 的事务中更新一下表 student 中 id 为 1 的记录：

```
# Transaction 20 
BEGIN; 

# 更新了一些别的表的记录 
... 
UPDATE student SET name="钱七" WHERE id=1;
UPDATE student SET name="宋八" WHERE id=1;
```

此刻，表student 中 id 为 1 的记录的版本链长这样：

![image-20230129165703658](/image-20230129165703658.png)

然后再到刚才使用 REPEATABLE READ 隔离级别的事务中继续查找这个 id 为 1 的记录，如下：

```
# 使用REPEATABLE READ隔离级别的事务 
BEGIN;

# SELECT1：Transaction 10、20均未提交 
SELECT * FROM student WHERE id = 1; # 得到的列name的值为'张三' 

# SELECT2：Transaction 10提交，Transaction 20未提交 
SELECT * FROM student WHERE id = 1; # 得到的列name的值仍为'张三
```

步骤1：因为当前事务的隔离级别为REPEATABLE READ， 而之前在执行SELECT1时已经生成过ReadView了，所以此时直接复用之前的ReadView,之前的ReadView的trx_ids列表的内容就是[10，20]， up_limit_id为10，low_limit_id为21，creator_trx_id为0。

步骤2：然后从版本链中挑选可见的记录，从图中可以看出，最新版本的列name的内容是' 宋八'，该版本的trx_id值为20，在trx_ids列表内，所以不符合可见性要求，根据roll_pointer跳到下一个版本。

步骤3：下一个版本的列name的内容是‘钱七'， 该版本的trx_ id值为20，也在trx_ids列表内，所以也不符合要求，继续跳到下一个版本。

步骤4：下一个版本的列name的内容是'王五'，该版本的trx_ id值为10，而trx_ids列表中是包含值为10的事务id的，所以该版本也不符合要求，同理下一个列name的内容是' 李四'的版本也不符合要求。继续跳到下一个版本。

步骤5：下一个版本的列name的内容是' 张三'，该版本的trx_id值为80，小于ReadView中的up_limit_id值10，所以这个版本是符合要求的，最后返回给用户的版本就是这条列c为'张三'的记录。

两次SELECT查询得到的结果是重复的，记录的列c值都是'张三'，这就是可重复读的含义。如果我们之后再把事务id为20的记录提交了，然后再到刚才使用REPEATABLE READ 隔离级别的事务中继续查找这个id为1的记录，得到的结果还是'张三'，

#### 如何解决幻读

接下来说明InnoDB 是如何解决幻读的。

假设现在表 student 中只有一条数据，数据内容中，主键 id=1，隐藏的trx_id=10，它的 undo log 如下图所示。

![image-20230129165714705](/image-20230129165714705.png)

假设现在有事务 A 和事务 B 并发执行， 事务 A 的事务 id 为 20 ， 事务 B 的事务 id 为 30 。

步骤1：事务 A 开始第一次查询数据，查询的 SQL 语句如下。

```
select * from student where id >= 1;
```

在开始查询之前，MySQL 会为事务 A 产生一个 ReadView，此时 ReadView 的内容如下： trx_ids= [20,30] ， up_limit_id=20 ， low_limit_id=31 ， reator_trx_id=20 。

由于此时表 student 中只有一条数据，且符合 where id>=1 条件，因此会查询出来。然后根据 ReadView机制，发现该行数据的trx_id=10，小于事务 A 的 ReadView 里 up_limit_id，这表示这条数据是事务 A 开启之前，其他事务就已经提交了的数据，因此事务 A 可以读取到。

结论：事务 A 的第一次查询，能读取到一条数据，id=1。

步骤2：接着事务 B(trx_id=30)，往表 student 中新插入两条数据，并提交事务。

```
insert into student(id,name) values(2,'李四'); 
insert into student(id,name) values(3,'王五');
```

此时表student 中就有三条数据了，对应的 undo 如下图所示：

![image-20230129165733580](/image-20230129165733580.png)

步骤3：接着事务 A 开启第二次查询，根据可重复读隔离级别的规则，此时事务 A 并不会再重新生成ReadView。此时表 student 中的 3 条数据都满足 where id>=1 的条件，因此会先查出来。然后根据ReadView 机制，判断每条数据是不是都可以被事务 A 看到。

1）首先 id=1 的这条数据，前面已经说过了，可以被事务 A 看到。

2）然后是 id=2 的数据，它的 trx_id=30，此时事务 A 发现，这个值处于 up_limit_id 和 low_limit_id 之间，因此还需要再判断 30 是否处于 trx_ids 数组内。由于事务 A 的 trx_ids=[20,30]，因此在数组内，这表示 id=2 的这条数据是与事务 A 在同一时刻启动的其他事务提交的，所以这条数据不能让事务 A 看到。

3）同理，id=3 的这条数据，trx_id 也为 30，因此也不能被事务 A 看见。

![image-20230129165751407](/image-20230129165751407.png)

结论：最终事务 A 的第二次查询，只能查询出 id=1 的这条数据。这和事务 A 的第一次查询的结果是一样的，因此没有出现幻读现象，所以说在 MySQL 的可重复读隔离级别下，不存在幻读问题。

### 总结 

这里介绍了 MVCC 在 **READ COMMITTD 、 REPEATABLE READ** 这两种隔离级别的事务在执行快照读操作时访问记录的版本链的过程。这样使不同事务的 读-写 、 写-读 操作并发执行，从而提升系统性能。

核心点在于 ReadView 的原理， READ COMMITTD 、 REPEATABLE READ 这两个隔离级别的一个很大不同就是生成ReadView的时机不同：

- READ COMMITTD 在每一次进行普通SELECT操作前都会生成一个ReadView 
- REPEATABLE READ 只在第一次进行普通SELECT操作前生成一个ReadView，之后的查询操作都重复使用这个ReadView就好了。

> 说明：我们之前说执行DELETE语句或者更新主键的UPDATE语句并不会立即把对应的记录完全从页面中删除，而是执行一个所谓的delete marK操作，相当于只是对记录打上了一个删除标志位，这主要就是为MVCC服务的。

通过MVCC我们可以解决：

1. **读写之间阻塞的问题**。 通过MVCC可以让读写互相不阻塞，即读不阻塞写，写不阻塞读，这样就可以提升事务并发处理能力。
2. **降低了死锁的概率**。这是因为MVCC采用了乐观锁的方式，读取数据时并不需要加锁，对于写操作，也只锁定必要的行。
3. **解决快照读的问题** 。当我们查询数据库在某个时间点的快照时，只能看到这个时间点之前事务提交更新的结果，而不能看到这个时间点之后事务提交的更新结果。

# 16.日志与备份

## 其他数据库日志

对于线上数据库应用系统，突然遭遇**数据库宕机**怎么办？在这种情况下，**定位宕机的原因**就非常关键。我们可以查看数据库的**错误日志**。因为日志中记录了数据库运行中的诊断信息，包括了错误、警告和注释等信息。比如：从日志中发现某个连接中的SQL操作发生了死循环，导致内存不足,被系统强行终止了。明确了原因，处理起来也就轻松了，系统很快就恢复了运行。

除了发现错误，日志在数据复制、数据恢复、操作审计，以及确保数据的永久性和一致性等方面，都有着不可替代的作用。

**千万不要小看日志。**很多看似奇怪的问题，答案往往就藏在日志里。很多情况下，只有通过查看日志才能发现问题的原因，真正解决问题。所以，一定要学会查看日志，养成检查日志的习惯,对提升你的数据库应用开发能力至关重要。

### MySQL支持的日志

#### 日志类型

MySQL有不同类型的日志文件，用来存储不同类型的日志，分为 **二进制日志 、 错误日志 、 通用查询日志 和 慢查询日志** ，这也是常用的4种。MySQL 8又新增两种支持的日志： **中继日志 和 数据定义语句日志** 。使用这些日志文件，可以查看MySQL内部发生的事情。

**这6类日志分别为：**

- **慢查询日志**：记录所有执行时间超过long_query_time的所有查询，方便我们对查询进行优化。
- **通用查询日志**：记录所有连接的起始时间和终止时间，以及连接发送给数据库服务器的所有指令，对我们复原操作的实际场景、发现问题，甚至是对数据库操作的审计都有很大的帮助。
- **错误日志**：记录MySQL服务的启动、运行或停止MySQL服务时出现的问题，方便我们了解服务器的状态，从而对服务器进行维护。
- **二进制日志**：记录所有更改数据的语句，可以用于主从服务器之间的数据同步，以及服务器遇到故障时数据的无损失恢复。
- **中继日志**：用于主从服务器架构中，从服务器用来存放主服务器二进制日志内容的一个中间文件。从服务器通过读取中继日志的内容，来同步主服务器上的操作。
- **数据定义语句日志**：记录数据定义语句执行的元数据操

除二进制日志外，其他日志都是 **文本文件** 。默认情况下，所有日志创建于 **MySQL数据目录** 中。

#### 日志的弊端

- 日志功能会 **降低MySQL数据库的性能** 。
- 日志会 **占用大量的磁盘空间** 。

### 慢查询日志

前面章节《第09章_性能分析工具的使用》已经详细讲述。

### 通用查询日志

通用查询日志用来 **记录用户的所有操作** ，包括启动和关闭MySQL服务、所有用户的连接开始时间和截止时间、发给 MySQL 数据库服务器的所有 SQL 指令等。当我们的数据发生异常时，**查看通用查询日志，还原操作时的具体场景**，可以帮助我们准确定位问题。

#### 问题场景

在电商系统中，购买商品并且使用微信支付完成以后，却发现支付中心的记录并没有新增，此时用户再次使用支付宝支付，就会出现**重复支付**的问题。但是当去数据库中查询数据的时候，会发现只有一条记录存在。 那么此时给到的现象就是只有一条支付记录，但是用户却支付了两次。

我们对系统进行了仔细检查,没有发现数据问题，因为用户编号和订单编号以及第三方流水号都是对的。可是用户确实支付了两次,这个时候，我们想到了检查通用查询日志，看看当天到底发生了什么。

查看之后，发现: 1月1日下午2点,用户使用微信支付完以后，但是由于网络故障,I支付中心没有及时收到微信支付的回调通知，导致当时没有写入数据。1月1日下午2点30，用户又使用支付宝支付，此时记录更新到支付中心。1月1日晚上9点,微信的回调通知过来了,但是支付中心已经存在了支付宝的记录,所以只能覆盖记录了。

由于网络的原因导致了重复支付。至于解决问题的方案就很多了，这里省略。

可以看到通用查询日志可以帮助我们了解操作发生的具体时间和操作的细节，对找出异常发生的原因极其关键。

#### 查看当前状态

```
mysql> SHOW VARIABLES LIKE '%general%'; 
+------------------+------------------------------+
| Variable_name    | Value 						  |
+------------------+------------------------------+
| general_log 	   | OFF 						  | #通用查询日志处于关闭状态 
| general_log_file | /var/lib/mysql/atguigu01.log | #通用查询日志文件的名称是atguigu01.log 
+------------------+------------------------------+
2 rows in set (0.03 sec)
```

#### 启动日志

**方式1：永久性方式**

修改my.cnf或者my.ini配置文件来设置。在[mysqld]组下加入log选项，并重启MySQL服务。格式如下：

```
[mysqld] 
general_log=ON 
general_log_file=[path[filename]] #日志文件所在目录路径，filename为日志文件名
```

如果不指定目录和文件名，通用查询日志将默认存储在MySQL数据目录中的hostname.log文件中，hostname表示主机名。

**方式2：临时性方式**

```
SET GLOBAL general_log=on; # 开启通用查询日志
SET GLOBAL general_log_file=’path/filename’; # 设置日志文件保存位置
```

对应的，关闭操作SQL命令如下：

```
SET GLOBAL general_log=off; # 关闭通用查询日志
```

查看设置后情况：

```
SHOW VARIABLES LIKE 'general_log%';
```

#### 查看日志

通用查询日志是以 文本文件 的形式存储在文件系统中的，可以使用 文本编辑器 直接打开日志文件。每台MySQL服务器的通用查询日志内容是不同的。

- 在Windows操作系统中，使用文本文件查看器； 
- 在Linux系统中，可以使用vi工具或者gedit工具查看；
- 在Mac OSX系统中，可以使用文本文件查看器或者vi等工具查看。

从 `SHOW VARIABLES LIKE 'general_log%';` 结果中可以看到通用查询日志的位置。

```
vim atguigu08.log
```

![image-20230129165906577](/image-20230129165906577.png)

在通用查询日志里面，我们可以清楚地看到，什么时候开启了新的客户端登陆数据库，登录之后做了什么 SQL 操作，针对的是哪个数据表等信息。

#### 停止日志

**方式1：永久性方式**

修改 `my.cnf` 或者 `my.ini` 文件，把[mysqld]组下的 `general_log` 值设置为 `OFF` 或者把general_log一项注释掉。修改保存后，再 `重启MySQL服务` ，即可生效。 举例1：

```
[mysqld] 
general_log=OFF
```

举例2：

```
[mysqld] 
#general_log=ON
```

**方式2：临时性方式**

使用SET语句停止MySQL通用查询日志功能：

```
SET GLOBAL general_log=off;
```

查询通用日志功能：

```
SHOW VARIABLES LIKE 'general_log%';
```

#### 删除\刷新日志

如果数据的使用非常频繁，那么通用查询日志会占用服务器非常大的磁盘空间。数据管理员可以删除很长时间之前的查询日志，以保证MySQL服务器上的硬盘空间。

**手动删除文件**

```
SHOW VARIABLES LIKE 'general_log%';
```

可以看出，通用查询日志的目录默认为MySQL数据目录。在该目录下手动删除通用查询日志atguigu01.log。

删除前先将日志文件复制一份（方便演示）

![image-20230129165936047](/image-20230129165936047.png)

使用如下命令重新生成查询日志文件，具体命令如下。刷新MySQL数据目录，发现创建了新的日志文件。前提一定要开启通用日志。

```
mysqladmin -uroot -p flush-logs
```

如果希望备份就的通用查询日志，就必须将就的日志文件复制出来或者改名，然后执行上面的mysqladmin命令。正确的流程如下：

```
cd mysql-data-directory #输入自己的通用日志文件所在目录
mv mysq1. general.log mysql.general.log.old #指名就的文件名以及新的文件名
mysqladmin -uroot -p flush-logs 
```

### 错误日志

错误日志记录了MySQL服务器启动、停止运行的时间，以及系统启动、运行和停止过程中的诊断信息，包括**错误**、**警告**和**提示**等。

通过错误日志可以查看系统的运行状态，便于即时发现故障、修复故障。如果MySQL服务**出现异常**，错误日志是发现问题、解决故障的**首选**。

#### 启动日志

在MySQL数据库中，错误日志功能是 **默认开启** 的。而且，**错误日志** 无法被禁止 。

默认情况下，错误日志存储在MySQL数据库的数据文件夹下，名称默认为 `mysqld.log` （Linux系统）或`hostname.err` （mac系统）。如果需要制定文件名，则需要在my.cnf或者my.ini中做如下配置：

```
[mysqld] 
log-error=[path/[filename]] #path为日志文件所在的目录路径，filename为日志文件名
```

修改配置项后，需要重启MySQL服务以生效。

#### 查看日志

MySQL错误日志是以文本文件形式存储的，可以使用文本编辑器直接查看。

查询错误日志的存储路径：

![image-20230129170044424](/image-20230129170044424.png)

执行结果中可以看到错误日志文件是mysqld.log，位于MySQL默认的数据目录下。

![image-20220209144359775](/image-20230129170012194.png)

可以看到，错误日志文件中记录了服务器启动的时间，以及存储引擎InnoDB启动和停止的时间等。我们在做初始化时候生成的数据库初始密码也是记录在error.log中。

#### 删除\刷新日志

对于很久以前的错误日志，数据库管理员查看这些错误日志的可能性不大，可以将这些错误日志删除，以保证MySQL服务器上的 **硬盘空间** 。MySQL的错误日志是以文本文件的形式存储在文件系统中的，可以**直接删除** 。

- 第一步（方式一）：删除操作

  ```
  rm -f /var/lib/mysql/mysqld.log
  ```

  在运行状态下删除错误日志问价年后，MySQL并不会自动创建日志文件。

- 第一步（方式二）：重命名文件

  ```
  mv /var/log/mysql/mysqld.log /var/log/mysql/mysqld.log.old
  ```

- 第二步：重建日志

  ```
  mysqladmin -u root -p flush-logs
  ```

可能会报错：

```
[root@atguigu01 log]# mysqladmin -uroot -p flush-logs 
Enter password: 
mysqladmin: refresh failed; error: 'Could not open file '/var/log/mysqld.log' for error logging.'
```

官网提示：

![image-20230129170112395](/image-20230129170112395.png)

补充操作：

```
install -omysql -gmysql -m0644 /dev/null /var/log/mysqld.log
```

![image-20230129170130616](/image-20230129170130616.png)

`flush-logs`指令操作：

- MYSQL 5.5.7以前的版本，flush-logs将错误日志文件重命名为filename.err_old, 并创建新的日志文件。
- 从MySQL 5.5.7开始，flush-logs只是重新打开日志文件，**并不做日志备份和创建的操作**。
- 如果日志文件不存在，MySQL启动或者执行flush-logs时会自动创建新的日志文件。重新创建错误日志，大小为0字节。

### 二进制日志

binlog可以说是MySQL中比较 **重要** 的日志了，在日常开发及运维过程中，经常会遇到。

binlog即binary log，二进制日志文件，也叫作变更日志（update log）。它记录了数据库所有执行的**DDL** 和 **DML** 等数据库更新事件的语句，但是不包含没有修改任何数据的语句（如数据查询语句select、show等）。

它以**事务形式**记录并保存在**二级制文件**中。通过这些信息，我们可以再现数据更新操作的全过程。

> 如果想要记录所有语句（例如：为了识别有问题的查询），需要使用通用查询日志。

binlog主要应用场景：

- 一是用于 **数据恢复**，如果MySQL数据库意外停止，可以通过二进制日志文件来查看用户执行了哪些操作，对数据库服务器文件做了哪些修改，然后根据二进制日志文件中的记录来恢复数据库服务器。
- 二是用于 **数据复制** ，由于日志的延续性和时效性，master把它的二进制日志传递给slaves来达到master-slave数据一致的目的。

可以说MySQL数据库的**数据备份、主备、主主、主从**都离不开binlog，需要依靠binlog来同步数据，保证数据一致性。

![image-20230129170146443](/image-20230129170146443.png)

#### 查看默认情况

查看记录二进制日志是否开启：在MySQL8中默认情况下，二进制文件是开启的。

![image-20230129170156544](/image-20230129170156544.png)

#### 日志参数设置

**方式**1**：永久性方式**

修改MySQL的 my.cnf 或 my.ini 文件可以设置二进制日志的相关参数：

```
[mysqld] 
#启用二进制日志 
log-bin=atguigu-bin
binlog_expire_logs_seconds=600
max_binlog_size=100M
```

> 提示:
>
> 1. log-bin=mysql-bin #打开日志(主机需要打开)，这个mysql-bin也可以自定义，这里也可以加上路径，如: /home/www/mysql_bin_log/mysql-bin
> 2. binlog_ expire_ logs_ seconds：此参数控制二进制日志文件保留的时长，单位是秒，默认2592000 30天 -- 14400 4小时; 86400 1天; 259200 3天；
> 3. max_ binlog_ size：控制单个二进制日志大小，当前日志文件大小超过此变量时，执行切换动作。此参数的**最大和默认值是1GB**，该设置并**不能严格控制Binlog的大小**，尤其是Binlog比较靠近最大值而又遇到一个比较大事务时，为了保证事务的完整性，可能不做切换日志的动作，只能将该事务的所有SQL都记录进当前日志，直到事务结束。一般情况下可采取默认值。

重新启动MySQL服务，查询二进制日志的信息，执行结果：

![image-20230129170214450](/image-20230129170214450.png)

**设置带文件夹的bin-log日志存放目录**

如果想改变日志文件的目录和名称，可以对my.cnf或my.ini中的log_bin参数修改如下：

```
[mysqld] 
log-bin="/var/lib/mysql/binlog/atguigu-bin"
```

注意：新建的文件夹需要使用mysql用户，使用下面的命令即可。

```
chown -R -v mysql:mysql binlog
```

> 提示：
>
> 数据库文件最好不要与日志文件放在同一个磁盘上！这样，当数据库文件所在的磁盘发生故障时，可以使用日志文件恢复数据。

**方式2：临时性方式**

如果不希望通过修改配置文件并重启的方式设置二进制日志的话，还可以使用如下指令，需要注意的是在mysql8中只有 **会话级别** 的设置，没有了global级别的设置。

```
# global
级别 mysql> set global sql_log_bin=0; 
ERROR 1228 (HY000): Variable 'sql_log_bin' is a SESSION variable and can`t be used 
with SET GLOBAL 

# session级别
mysql> SET sql_log_bin=0; 
Query OK, 0 rows affected (0.01 秒)
```

#### 查看日志

当MySQL创建二进制日志文件时，先创建一个以“filename”为名称、以“.index”为后缀的文件，再创建一个以“filename”为名称、以“.000001”为后缀的文件。

MySQL服务 **重新启动一次** ，以“.000001”为后缀的文件就会增加一个，并且后缀名按1递增。即日志文件的个数与MySQL服务启动的次数相同；如果日志长度超过了 **max_binlog_size** 的上限（默认是1GB），就 会创建一个新的日志文件。

查看当前的二进制日志文件列表及大小。指令如下：

![image-20230129170236862](/image-20230129170236862.png)

所有对数据库的修改都会记录在binglog中。但binlog是二 进制文件，无法直接查看，想要更直观的观测它就要借 助**mysqlbinlog**命令工具了。指令如下：在查看执行，先执行一条SQL语句，如下

```
update student set name='张三_back' where id=1;
```

开始查看binlog

```
mysqlbinlog "/var/lib/mysql/binlog/atguigu-bin.000002"
```

![image-20220209212738503](/image-20230129170301626.png)

执行结果可以看到，这是一-个简单的日志文件，日志中记录了用户的一些操作，这里并没有出现具体的SQL语 句，这是因为binlog关键字后面的内容是经过编码后的**二进制日志**。

这里一个update语句包含如下事件

- Query 事件负责开始一个事务(BEGIN)
- Table_map事件负责映射需要的表
- Update_ rows事件负责写入数据
- Xid事件负责结束事务

下面命令将行事件以 伪SQL的形式 表现出来

```
mysqlbinlog -v "/var/lib/mysql/binlog/atguigu-bin.000002"
```

![image-20230129170317167](/image-20230129170317167.png)

前面的命令同时显示binlog格式的语句，使用如下命令不显示它

```
mysqlbinlog -v --base64-output=DECODE-ROWS "/var/lib/mysql/binlog/atguigu-bin.000002"
```

关于mysqlbinlog工具的使用技巧还有很多，例如只解析对某个库的操作或者某个时间段内的操作等。简单分享几个常用的语句，更多操作可以参考官方文档。

```
# 可查看参数帮助 
mysqlbinlog --no-defaults --help 

# 查看最后100行 
mysqlbinlog --no-defaults --base64-output=decode-rows -vv atguigu-bin.000002 |tail -100 

# 根据position查找
mysqlbinlog --no-defaults --base64-output=decode-rows -vv atguigu-bin.000002 |grep -A 20 '4939002'
```

上面这种办法读取出binlog日志的全文内容比较多，不容易分辨查看到pos点信息，下面介绍一种更为方便的查询命令：

```
mysql> show binlog events [IN 'log_name'] [FROM pos] [LIMIT [offset,] row_count];
```

- **IN 'log_name'** ：指定要查询的binlog文件名（不指定就是第一个binlog文件）　
- **FROM pos** ：指定从哪个pos起始点开始查起（不指定就是从整个文件首个pos点开始算）
- **LIMIT [offset]** ：偏移量(不指定就是0)
- **row_count** ：查询总条数（不指定就是所有行）

![image-20230129170332719](/image-20230129170332719.png)

![image-20230129170346209](/image-20230129170346209.png)

![image-20230129170359264](/image-20230129170359264.png)

上面我们讲了这么多都是基于binlog的默认格式，binlog格式查看

![image-20230129170411554](/image-20230129170411554.png)

除此之外，binlog还有2种格式，分别是**Statement**和**Mixed** 

- **Statement**

  每一条会修改数据的sql都会记录在binlog中。

  优点：**不需要记录每一行的变化**，减少了binlog日志量，节约了IO，提高性能。

- **Row** 

  5.1.5版本的MySQL才开始支持row level 的复制，它不记录sql语句上下文相关信息，仅保存哪条记录被修改。

  优点：row level 的日志内容**会非常清楚的记录下每一行数据修改的细节**。而且不会出现某些特定情况下的存储过程，或function，以及trigger的调用和触发无法被正确复制的问题。

- **Mixed** 

  从5.1.8版本开始，MySQL提供了Mixed格式，实际上就是Statement与Row的结合。详细情况，下章讲解。

#### 使用日志恢复数据

如果MySQL服务器启用了二进制日志，在数据库出现意外丢失数据时，可以使用MySQLbinlog工具从指定的时间点开始(例如，最后一次备份)直到现在或另一个指定的时间点的日志中恢复数据。

mysqlbinlog恢复数据的语法如下：

```
mysqlbinlog [option] filename|mysql –uuser -ppass;
```

这个命令可以这样理解：使用mysqlbinlog命令来读取filename中的内容，然后使用mysql命令将这些内容恢复到数据库中。

- **filename** ：是日志文件名。
- **option** ：可选项，比较重要的两对option参数是--start-date、--stop-date 和 --start-position、-- stop-position。 
  - **--start-date 和 --stop-date** ：可以指定恢复数据库的起始时间点和结束时间点。
  - **--start-position和--stop-position** ：可以指定恢复数据的开始位置和结束位置。

> 注意：使用mysqlbinlog命令进行恢复操作时，必须是编号小的先恢复，例如atguigu-bin.000001必须在atguigu-bin.000002之前恢复。

**举例**

![image-20230129170434287](/image-20230129170434287.png)

将表中的后三条数据“误删”

![image-20230129170445825](/image-20230129170445825.png)

**恢复步骤：**

1. 创建一个日志文件

![image-20230129170501737](/image-20230129170501737.png)

1. 读取日志（02）文件

   ```
   show binlog events in ‘atguigu-bin.000002’
   ```

   找到对应的插入语句后，开始恢复

   ![image-20230129170520906](/image-20230129170520906.png)

2. 恢复插入的数据

   插入的操作时三个事务，而且三个事务是连在一起的，所以我们可以直接恢复三个事务，开始的pos是884，结束的pos是1729

   ```
   /usr/bin/mysqlbinlog --start-position=884 --stop-position=1729 --database=atguigu14
   /var/lib/mysql/binlog/atguigu-bin.000003 | /usr/bin/mysql -uroot -pabc123（密码） -V atguigu14（数据库名称）
   ```

   ![image-20220209225634559](/image-20230129170536324.png)

   结果如下：

   ![image-20230129170553304](/image-20230129170553304.png)

   成功！

   其他删除、修改语句操作步骤一致。

3. 当然，也可以使用日期恢复，命令格式如下：

   ```
   /usr/bin/mysqlbinlog --start-datetime="2022-01-05 15:39:22" --stop-datetime="2022-01-05 15:40:19” --database=atguigu14 /var/lib/mysql/binlog/atguigu-bin.000005 | /usr /bin/mysq1 -uroot -pabc123 -v atguigu14 
   ```

   日期可以根据binlog日志详情查看，如下所示。可能出现一个事务执行时间过短，那么就是统一的时间，一秒内执行完成，此时我们找到下一个事务的开始时间即可，多计算一些时间就可以了（下一个事务开始之前的时间段都可以）。 本次事务的开始时间是220105 15:39:22,结束时间设置为220105 15:40:19 。

   先查询出时间范围，确定好时间范围后，在执行语句。

   ![image-20230129170608178](/image-20230129170608178.png)

   ![image-20220209231738809](/image-20230129170625398.png)

#### 删除二进制日志

MySQL的二进制文件可以配置自动删除，同时MySQL也提供了安全的手动删除二进制文件的方法。`PURGE MASTER LOGS` 只删除指定部分的二进制日志文件， `RESET MASTER` 删除所有的二进制日志文件。具体如下：

1. **PURGE MASTER LOGS：删除指定日志文件**

   PURGE MASTER LOGS语法如下：

   ```
   PURGE {MASTER | BINARY} LOGS TO ‘指定日志文件名’ 
   PURGE {MASTER | BINARY} LOGS BEFORE ‘指定日期’
   ```

**举例**

使用PURGE MASTER LOGS语句删除2020年10月25号前创建的所有日志文件。具体步骤如下：

1. 显示二进制日志文件列表

```
SHOW BINARY LOGS;
```

1. 执行mysqlbinlog命令查看进制日志文件inlo.00005的内容

```
mysqlbinlog --no-defaults ”/var/lib/mysql/binlog/atguigu-bin.00005”
```

结果可以看出20220105为日志创建的时间，即2022年1月05日.

1. 使用PURGE MASTER LOGS语句删除2022年1月05日前创建的所有日志文件

```
PURGE MASTER LOGs before "20220105" ;
```

1. 显示二进制日志文件列表

```
SHOW BINARY LOGS;
```

2022年01月05号之前的二进制日志文件都已经被删除，最后一个没有删除， 是因为当前在用，还未记录最后的时间，所以未被删除。

1. **RESET MASTER:删除所有的二进制日志文件**

使用`RESET MASTER` 语句，清空所有的binlog日志。MySQL会重新创建二进制文件，新的日志文件扩展名将重新从000001开始编号。**慎用**！

**举例**：使用RESET MASTER语句删除所有日志文件。

(1)重启MySQL服务若干次,执行SHOW语句显示二进制日志文件列表。

```
SHOW BINARY LOGS ;
```

(2)执行RESET MASTER语句，删除所有日志文件

```
RESET MASTER；
```

执行完该语句后，原来的所有二进制日志已经全部被删除。

#### 其他场景

二进制日志可以通过数据库的 **全量备份** 和二进制日志中保存的 **增量信息** ，完成数据库的 **无损失恢复** 。但是，如果遇到数据量大、数据库和数据表很多（比如分库分表的应用）的场景，用二进制日志进行数据恢复，是很有挑战性的，因为起止位置不容易管理。

在这种情况下，一个有效的解决办法是 **配置主从数据库服务器** ，甚至是 **一主多从** 的架构，把二进制日志文件的内容通过中继日志，同步到从数据库服务器中，这样就可以有效避免数据库故障导致的数据异常等问题。

### 再谈二进制日志

#### 写入机制

binlog的写入时机也非常简单，事务执行过程中，先把日志写到 `binlog cache` ，事务提交的时候，再把binlog cache写到binlog文件中。因为一个事务的binlog不能被拆开，无论这个事务多大，也要确保一次性写入，所以系统会给每个线程分配一个块内存作为binlog cache。

我们可以通过`binlog_cache_size`参数控制单个线程binlog cache大小，如果存储内容超过了这个参数，就要暂存到磁盘(Swap) 。binlog日志刷盘流程如下:

![image-20230129170646414](/image-20230129170646414.png)

> 上图的**write**，是指把日志写入到文件系统的 page cache，并没有把数据持久化到磁盘，所以速度比较快
>
> 上图的**fsync**，才是将数据持久化到磁盘的操作

write和fsync的时机，可以由参数 `sync_binlog` 控制，默认是 `0` 。为0的时候，表示每次提交事务都只write，由系统自行判断什么时候执行fsync。虽然性能得到提升，但是机器宕机，page cache里面的binglog 会丢失。如下图：

![image-20230129170701649](/image-20230129170701649.png)

为了安全起见，可以设置为 1 ，表示每次提交事务都会执行fsync，就如同**redo log 刷盘流程**一样。

最后还有一种折中方式，可以设置为N(N>1)，表示每次提交事务都write，但累积N个事务后才fsync。

![image-20230129170716217](/image-20230129170716217.png)

在出现IO瓶颈的场景里，将sync_binlog设置成一个比较大的值，可以提升性能。同样的，如果机器宕机，会丢失最近N个事务的binlog日志。

#### binlog和redolog对比

- redo log 它是 **物理日志** ，记录内容是“在某个数据页上做了什么修改”，属于 InnoDB 存储引擎层产生的。
- 而 binlog 是 **逻辑日志** ，记录内容是语句的原始逻辑，类似于“给 ID=2 这一行的 c 字段加 1”，属于MySQL Server 层。
- 虽然它们都属于持久化的保证，但是则重点不同。
  - redo log让InnoDB存储引擎拥有了崩溃恢复能力。
  - binlog 保证了MySQL集群架构的数据一致性。

#### 两阶段提交

在执行更新语句过程，会记录redo log与binlog两块日志，以基本的事务为单位，redo log在事务执行过程中可以不断写入，而binlog只有在提交事务时才写入，所以redo log与binlog的 **写入时机** 不一样。

![image-20230129170742864](/image-20230129170742864.png)

**redo log与binlog两份日志之间的逻辑不一致，会出现什么问题？**

以update语句为例，假设`id=2`的记录，字段`c`值是`0`，把字段`c`值更新成`1` ，SQL语句为update T set c=1 where id=2。

假设执行过程中写完redo log日志后，binlog日志写期间发生了异常，会出现什么情况呢?

![image-20230129170755864](/image-20230129170755864.png)

由于binlog没写完就异常，这时候binlog里面没有对应的修改记录。因此，之后用binlog日志恢复数据时，就会少这一次更新， 恢复出来的这一行c值是0，而原库因为redo log日志恢复，这一行c值是1，最终数据不一致。

![image-20230129170812146](/image-20230129170812146.png)

为了解决两份日志之间的逻辑一致问题，InnoDB存储引擎使用**两阶段提交**方案。原理很简单，将redo log的写入拆成了两个步骤prepare和commit，这就是**两阶段提交**。

![image-20230129170829653](/image-20230129170829653.png)

使用两阶段提交后，写入binlog时发生异常也不会有影响，因为**MySQL根据redo log日志恢复数据时，发现redo log还处于prepare阶段，并且没有对应的binlog日志，就会回滚该事务。**

![image-20220209100704823](/image-20230129170907768.png)

另一个场景，redo log设置commit阶段发生异常，那会不会回滚事务呢？

![image-20220209100731491](/image-20230129170921895.png)

**并不会回滚事务，它会执行上图框住的逻辑，虽然redo log是处于prepare阶段，但是能通过事务id找到对应的binlog日志，所以MySQL认为是完整的，就会提交事务恢复数据。**

### 中继日志

#### 介绍

**中继日志只在主从服务器架构的从服务器上存在**。从服务器为了与主服务器保持一致，要从主服务器读取二进制日志的内容，并且把读取到的信息写入 **本地的日志文件** 中，这个从服务器本地的日志文件就叫**中继日志** 。然后，从服务器读取中继日志，并根据中继日志的内容对从服务器的数据进行更新，完成主从服务器的 **数据同步** 。

搭建好主从服务器之后，中继日志默认会保存在从服务器的数据目录下。

文件名的格式是： **从服务器名 -relay-bin.序号** 。中继日志还有一个索引文件： **从服务器名 -relay- bin.index** ，用来定位当前正在使用的中继日志。

#### 查看中继日志

中继日志与二进制日志的格式相同，可以用 **mysqlbinlog** 工具进行查看。下面是中继日志的一个片段：

![image-20230129170938781](/image-20230129170938781.png)

这一段的意思是，主服务器（“server id 1”）对表 atguigu.test 进行了 2 步操作：

```
定位到表 atguigu.test 编号是 91 的记录，日志位置是 832; 

删除编号是 91 的记录，日志位置是 872。
```

#### 恢复的经典错误

如果从服务器宕机，有的时候为了系统恢复，要重装操作系统，这样就可能会导致你的 **服务器名称** 与之前 **不同** 。而中继日志里是 **包含从服务器名** 的。在这种情况下，就可能导致你恢复从服务器的时候，无法从宕机前的中继日志里读取数据，以为是日志文件损坏了，其实是名称不对了。

解决的方法也很简单，**把从服务器的名称改回之前的名称**。

## 主从复制

### 主从复制概述

#### 如何提升数据库的并发能力

在实际工作中，我们常常将**Redis** 作为缓存与**MySQL** 配合来使用，当有请求的时候，首先会从缓存中进行查找，如果存在就直接取出。如果不存在再访问数据库，这样就**提升了读取的效率**，也减少了对后端数据库的**访问压力**。Redis 的缓存架构是**高并发架构**中非常重要的一环。

![image-20230129170954151](/image-20230129170954151.png)

此外，一般应用对数据库而言都是“ **读多写少** ”，也就说对数据库读取数据的压力比较大，有一个思路就是采用数据库集群的方案，做 **主从架构** 、进行 **读写分离** ，这样同样可以提升数据库的并发处理能力。但并不是所有的应用都需要对数据库进行主从架构的设置，毕竟设置架构本身是有成本的。

如果我们的目的在于提升数据库高并发访问的效率，那么首先考虑的是如何 **优化SQL和索引** ，这种方式简单有效；其次才是采用 **缓存的策略** ，比如使用 Redis将热点数据保存在内存数据库中，提升读取的效率；最后才是对数据库采用 **主从架构** ，进行读写分离。

按照上面的方式进行优化，使用和维护成本是由低到高的。

#### 主从复制的作用

主从同步设计不仅可以提高数据库的吞吐量，还有以下 3 个方面的作用。

**第1个作用：读写分离。**

![image-20230129171007175](/image-20230129171007175.png)

其中一个是Master主库，负责写入数据，我们称之为：写库。

其它都是Slave从库，负责读取数据，我们称之为：读库。

当主库进行更新的时候，会自动将数据复制到从库中，而我们在客户端读取数据的时候，会从从库中进行读取。

面对“**读多写少**”的需求，采用读写分离的方式，可以实现**更高的并发访问**。同时，我们还能对从服务器进行**负载均衡**，让不同的读请求按照策略均匀地分发到不同的从服务器上，让**读取更加顺畅**。读取顺畅的另一个原因，就是**减少了锁表的影响**，比如我们让主库负责写，当主库出现写锁的时候，不会影响到从库进行SELECT的读取。

**第2个作用就是数据备份。**我们通过主从复制将主库上的数据复制到了从库上，相当于是一种热备份机制，也就是在主库正常运行的情况下进行的备份，不会影响到服务。

**第3个作用是具有高可用性**。数据备份实际上是一种冗余的机制， 通过这种冗余的方式可以换取数据库的高可用性，也就是当服务器出现 **故障** 或 **宕机** 的情况下，可以 **切换** 到从服务器上，保证服务的正常运行。

关于高可用性的程度，我们可以用一个指标衡量，即正常可用时间/全年时间。比如要达到全年9.99999的时间都可用，就意味着系统在一年中的不可用时间不得超过`365*24*60* (1-99.999%) =5.256分钟`(含系统崩溃的时间、日常维护操作导致的停机时间等)， 其他时间都需要保持可用的状态。

实际上，更高的高可用性，意味着需要付出更高的成本代价。在现实中我们需要结合业务需求和成本来进行选择。

### 主从复制的原理

**Slave** 会从 **Master** 读取 **binlog** 来进行数据同步。

#### 原理剖析

**三个线程**

实际上主从同步的原理就是基于 binlog 进行数据同步的。在主从复制过程中，会基于 3 个线程 来操作，一个主库线程，两个从库线程。

![image-20230129171023381](/image-20230129171023381.png)

**二进制日志转储线程** （Binlog dump thread）是一个主库线程。当从库线程连接的时候， 主库可以将二进制日志发送给从库，当主库读取事件（Event）的时候，会在 Binlog 上 加锁 ，读取完成之后，再将锁释放掉。

**从库 I/O 线程** 会连接到主库，向主库发送请求更新 Binlog。这时从库的 I/O 线程就可以读取到主库的二进制日志转储线程发送的 Binlog 更新部分，并且拷贝到本地的中继日志 （Relay log）。

**从库 SQL 线程** 会读取从库中的中继日志，并且执行日志中的事件，将从库中的数据与主库保持同步。

![image-20230129171033279](/image-20230129171033279.png)

> 注意：
>
> 不是所有版本的MySQL都默认开启服务器的二进制日志。在进行主从同步的时候，我们需要先检查服务器是否已经开启了二进制日志。
>
> 除非特殊指定，默认情况下从服务器会执行所有主服务器中保存的事件。也可以通过配置，使从服务器执行特定的事件。

**复制三步骤**

步骤1： **Master** 将写操作记录到二进制日志（ binlog ）。这些记录叫做**二进制日志事件**；

步骤2： **Slave** 将 **Maste**r 的binary log events拷贝到它的中继日志（ **relay log** ）；

步骤3： **Slave** 重做中继日志中的事件，将改变应用到自己的数据库中。 MySQL复制是异步的且串行化的，而且重启后从 **接入点** 开始复制。

**复制的问题**

复制的最大问题： **延时** 

#### 复制的基本原则

- 每个 **Slave** 只有一个 **Master**
- 每个 **Slave** 只能有一个唯一的服务器ID
- 每个 **Master** 可以有多个 **Slave**

### 一主一从架构搭建

一台 **主机** 用于处理所有 **写请求** ，一台 **从机** 负责所有 **读请求** ，架构图如下：

![image-20230129171045571](/image-20230129171045571.png)

#### 准备工作

1、准备 2台 **CentOS** 虚拟机

2、每台虚拟机上需要安装好MySQL (可以是MySQL8.0 )

说明：前面我们讲过如何克隆一台CentOS。大家可以在一台CentOS上安装好MySQL，进而通过克隆的方式复制出1台包含MySQL的虚拟机。

注意：克隆的方式需要修改新克隆出来主机的：**① MAC地址 ② hostname ③ IP 地址 ④ UUID 。**

##### 克隆过程：

vmware中的完整克隆是基于指定的虚拟机克隆出相同的一份出来，不必再安装。

但是我们要保证几个地方不能一样，一个是主机名称（hostname），一个是虚拟网卡设备mac地址，还有就是是ip地址和UUID。

所以我们在克隆后要对这四个地方进行修改。

1. 首先进行完全克隆，注意（要克隆的虚拟机在克隆前是需要处于关闭状态）

   选择要克隆的虚拟机右键，选择管理，然后选择克隆

   ![image-20230129171117832](/image-20230129171117832.png)

   然后直到这一步选择完整克隆，注意：链接克隆是指在一些资源上两个虚拟机会共用，这里不做详细介绍，完整克隆是完全独立出来的一个新虚拟机。

   ![image-20230129171130765](/image-20230129171130765.png)

   然后下一步，虚拟机名称输入你想要的名称就完成了。

   ![image-20230129171144995](/image-20230129171144995.png)

   注意，克隆完之后所有信息与原虚拟机一样，所以下面我们进行一些信息的修改

2. 开机前修改mac地址（注意：如果是动态生成IP地址，请在启动前先启动被克隆的虚拟机，以保证原来虚拟机的ip不会变，否则原来虚拟机ip会变，克隆后的虚拟机ip是原来的虚拟机ip）

   点击编辑虚拟机设置，选择网络适配器，点击右下角高级然后下边就是mac地址，这个mac地址和被克隆的是一样的，我们点击生成，重新生成一个新的：

   ![image-20230129171202443](/image-20230129171202443.png)

   然后mac地址就修改完了。

3. 开机后修改主机名称**(这步不用改也可以)**

   修改主机名可能不同linux版本不同，修改方法也不同。centos7就是 `vim /etc/hostname` 命令来编辑主机名。

   需要重启。（reboot）

4. 修改IP地址 

   此处需要注意的是：如果虚拟机使用的是动态ip分配，那么不需要更改ip，如果想改为静态ip，请修改：

   vim /etc/sysconfig/network-scripts/ifcfg-ens33 

   ![image-20230129171215695](/image-20230129171215695.png)

5. 修改UUID

   同样在修改IP地址的路径中修改即可

   ![image-20230129171234228](/image-20230129171234228.png)

   systemctl restart network

这里以centos为例

此外，克隆的方式生成的虚拟机（包含MySQL Server），则克隆的虚拟机MySQL Server的UUID相同，必须修改，否则在有些场景会报错。比如： show slave status\G ，报如下的错误：

```
Last_IO_Error: Fatal error: The slave I/O thread stops because master and slave have equal MySQL server UUIDs; these UUIDs must be different for replication to work.
```

**修改MySQL Server 的UUID方式**：

```
vim /var/lib/mysql/auto.cnf 
systemctl restart mysqld
```

![image-20230129171245354](/image-20230129171245354.png)

#### 主机配置文件

建议mysql版本一致且后台以服务运行，主从所有配置项都配置在 **[mysqld]** 节点下，且都是小写字母。

具体参数配置如下：

- 必选

  ```
  #[必须]主服务器唯一ID
  server-id=1
  
  #[必须]启用二进制日志,指名路径。比如：自己本地的路径/log/mysqlbin
  log-bin=atguigu-bin
  ```

- 可选

  ```
  #[可选] 0（默认）表示读写（主机），1表示只读（从机）
  read-only=0 
  
  #设置日志文件保留的时长，单位是秒 
  binlog_expire_logs_seconds=6000 
  #控制单个二进制日志大小。此参数的最大和默认值是1GB 
  max_binlog_size=200M 
  
  #[可选]设置不要复制的数据库
  binlog-ignore-db=test 
  #[可选]设置需要复制的数据库,默认全部记录。比如：binlog-do-db=atguigu_master_slave
  binlog-do-db=需要复制的主数据库名字
  
  #[可选]设置binlog格式 
  binlog_format=STATEMENT
  ```

重启后台mysql服务，使配置生效。

> 注意：
>
> 先搭建完主从复制，在创建数据库
>
> MySQL主从复制起始时，从机不继承主机数据。

**binlog格式设置：**

**① STATEMENT模式 （基于SQL语句的复制(statement-based replication, SBR)）**

```
binlog_format=STATEMENT
```

每一条会修改数据的sql语句会记录到binlog中。这是默认的binlog格式。

- SBR 的优点：
  - 历史悠久，技术成熟
  - 不需要记录每一行的变化，**减少了binlog日志量，文件较小**
  - binlog中包含了所有数据库更改信息，可以据此来审核数据库的安全等情况
  - binlog可以用于实时的还原，而不仅仅用于复制
  - 主从版本可以不一样，从服务器版本可以比主服务器版本高
- SBR 的缺点：
  - **不是所有的UPDATE语句都能被复制，尤其是包含不确定操作的时候**
- 使用以下函数的语句也无法被复制：LOAD_FILE()、UUID()、USER()、FOUND_ROWS()、SYSDATE() (除非启动时启用了 --sysdate-is-now 选项) 
  - INSERT ... SELECT 会产生比 RBR 更多的行级锁
  - 复制需要进行全表扫描(WHERE 语句中没有使用到索引)的 UPDATE 时，需要比 RBR 请求更多的行级锁
  - 对于有 AUTO_INCREMENT 字段的 InnoDB表而言，INSERT 语句会阻塞其他 INSERT 语句
  - 对于一些复杂的语句，在从服务器上的耗资源情况会更严重，而 RBR 模式下，只会对那个发生变化的记录产生影响
  - 执行复杂语句如果出错的话，会消耗更多资源
  - 数据表必须几乎和主服务器保持一致才行，否则可能会导致复制出错

**② ROW模式（基于行的复制(row-based replication, RBR)）**

```
binlog_format=ROW
```

5.1.5版本的MySQL才开始支持，不记录每条sql语句的上下文信息，仅记录哪条数据被修改了，修改成什么样了。

- RBR 的优点：
  - 任何情况都可以被复制，这对复制来说是最 **安全可靠** 的。（比如：不会出现某些特定情况下的存储过程、function、trigger的调用和触发无法被正确复制的问题）
  - 多数情况下，从服务器上的表如果有主键的话，复制就会快了很多
  - 复制以下几种语句时的行锁更少：INSERT ... SELECT、包含 AUTO_INCREMENT 字段的 INSERT、没有附带条件或者并没有修改很多记录的 UPDATE 或 DELETE 语句
  - 执行 INSERT，UPDATE，DELETE 语句时锁更少
  - 从服务器上采用 **多线程** 来执行复制成为可能
- RBR 的缺点：
  - binlog 大了很多
  - 复杂的回滚时 binlog 中会包含大量的数据
  - 主服务器上执行 UPDATE 语句时，所有发生变化的记录都会写到 binlog 中，而 SBR 只会写一次，这会导致频繁发生 binlog 的并发写问题
  - 无法从 binlog 中看到都复制了些什么语句

**③ MIXED模式（混合模式复制(mixed-based replication, MBR)）**

```
binlog_format=MIXED
```

从5.1.8版本开始，MySQL提供了Mixed格式，实际上就是Statement与Row的结合。

在Mixed模式下，**一般的语句修改使用statment格式保存binlog。如一些函数，statement无法完成主从复制的操作，则采用row格式保存binlog。** 

MySQL会根据执行的每一条具体的sql语句来区分对待记录的日志形式，也就是在Statement和Row之间选择一种。

#### 从机配置文件

要求主从所有配置项都配置在 my.cnf 的 [mysqld] 栏位下，且都是小写字母。

- 必选

  ```
  #[必须]从服务器唯一ID 
  server-id=2
  ```

- 可选

  ```
  #[可选]启用中继日志 
  relay-log=mysql-relay
  ```

重启后台mysql服务，使配置生效。

> 注意：主从机都关闭防火墙
>
> service iptables stop #CentOS 6
>
> systemctl stop firewalld.service #CentOS 7

#### 主机：建立账户并授权

```
#在主机MySQL里执行授权主从复制的命令

GRANT REPLICATION SLAVE ON *.* TO 'slave1'@'从机器数据库IP' IDENTIFIED BY 'abc123'; #5.5,5.7
```

**注意：如果使用的是MySQL8，需要如下的方式建立账户，并授权slave**：

```
CREATE USER 'slave1'@'%' IDENTIFIED BY '123456'; 

GRANT REPLICATION SLAVE ON *.* TO 'slave1'@'%'; 

#此语句必须执行。否则见下面。
ALTER USER 'slave1'@'%' IDENTIFIED WITH mysql_native_password BY '123456';#5.7和8.0的加密不同

flush privileges;
```

> 注意：在从机执行show slave status\G时报错：
>
> Last_10_Error: error connecting to master 'slave1@192.168.1.150:3306' -retry-time: 60 retries: 1 message: Authentication plugin 'caching_sha2_password' reported error: Authentication requires secure connection.

查询Master的状态，并记录下File和Position的值。

```
show master status;
```

![image-20230129171303932](/image-20230129171303932.png)

- 记录下File和Position的值

> 注意：执行完此步骤后**不要再操作主服务器**MySQL，防止主服务器状态值变化。

#### 从机：配置需要复制的主机

**步骤**1**：**从机上复制主机的命令

```
CHANGE MASTER TO
MASTER_HOST='主机的IP地址',
MASTER_USER='主机用户名',
MASTER_PASSWORD='主机用户名的密码', 
MASTER_LOG_FILE='mysql-bin.具体数字',
MASTER_LOG_POS=具体值;
```

举例：

```
CHANGE MASTER TO 
MASTER_HOST='192.168.1.150',MASTER_USER='slave1',MASTER_PASSWORD='123456',MASTER_LOG_F ILE='atguigu-bin.000007',MASTER_LOG_POS=154;
```

![image-20230129171317556](/image-20230129171317556.png)

之前做过主从复制/从机开着，就会报下面的错误

![image-20230129171327214](/image-20230129171327214.png)

**步骤2：**

```
#启动slave同步 
START SLAVE;
```

如果报错：

之前做过主从复制/从机开着，就会报下面的错误

![image-20230129171341161](/image-20230129171341161.png)

可以执行如下操作，删除之前的relay_log信息。然后重新执行 CHANGE MASTER TO ...语句即可。

```
mysql> reset slave; #删除SLAVE数据库的relaylog日志文件，并重新启用新的relaylog文件
```

接着，查看同步状态：

```
SHOW SLAVE STATUS\G;
```

![image-20230129171353661](/image-20230129171353661.png)

> 上面两个参数都是Yes，则说明主从配置成功！

显式如下的情况，就是不正确的。可能错误的原因有：

```
1. 网络不通 
2. 账户密码错误 
3. 防火墙 
4. mysql配置文件问题
5. 连接服务器时语法 
6. 主服务器mysql权限
```

![image-20230129171408567](/image-20230129171408567.png)

#### 测试

主机新建库、新建表、insert记录，从机复制

```
CREATE DATABASE atguigu_master_slave; 
CREATE TABLE mytbl(id INT,NAME VARCHAR(16)); 
INSERT INTO mytbl VALUES(1, 'zhang3'); 
INSERT INTO mytbl VALUES(2,@@hostname);
```

#### 停主从同步

- 停止主从同步命令：

  ```
  stop slave;
  ```

- 如何重新配置主从

  如果停止从服务器复制功能，再使用需要重新配置主从。否则会报错如下：

   ![image-20230129171425621](/image-20230129171425621.png)

  重新配置主从，需要在从机上执行：

  ```
  stop slave; 
  
  reset master; #删除Master中所有的binglog文件，并将日志索引文件清空，重新开始所有新的日志文件(慎用)
  ```

#### 后续

**搭建主从复制：双主双从**

![image-20230129171438627](/image-20230129171438627.png)

![image-20230129171448276](/image-20230129171448276.png)

### 同步数据一致性问题

**主从同步的要求：**

- 读库和写库的数据一致(最终一致)；
- 写数据必须写到写库；
- 读数据必须到读库(不一定)； 

#### 理解主从延迟问题

进行主从同步的内容是二进制日志，它是一个文件，在进行 **网络传输** 的过程中就一定会 **存在主从延迟**（比如 500ms），这样就可能造成用户在从库上读取的数据不是最新的数据，也就是主从同步中的 **数据不一致性** 问题。

**举例**：导致主从延迟的时间点主要包括以下三个：

1. 主库A执行完成一个事务，写入binlog，我们把这个时刻记为T1；
2. 之后传给从库B，我们把从库B接收完这个binlog的时刻记为T2；
3. 从库B执行完成这个事务，我们把这个时刻记为T3。

#### 主从延迟问题原因

在网络正常的时候，日志从主库传给从库所需的时间是很短的，即T2-T1的值是非常小的。即，网络正常情况下，主备延迟的主要来源是备库接收完binlog和执行完这个事务之间的时间差。

**主备延迟最直接的表现是，从库消费中继日志（**relay log**）的速度，比主库生产**binlog**的速度要慢。**造成原因：

1、从库的机器性能比主库要差

2、从库的压力大

3、大事务的执行

**举例1**：一次性用delete语句删除太多数据

结论：后续再删除数据的时候，要控制每个事务删除的数据量，分成多次删除。

**举例2**：一次性用insert...select插入太多数据

**举例3**：大表DDL

比如在主库对一张500W的表添加一个字段耗费了10分钟，那么从节点上也会耗费10分钟。

#### 如何减少主从延迟

若想要减少主从延迟的时间，可以采取下面的办法：

1. 降低多线程大事务并发的概率，优化业务逻辑
2. 优化SQL，避免慢SQL， **减少批量操作** ，建议写脚本以update-sleep这样的形式完成。
3. **提高从库机器的配置** ，减少主库写binlog和从库读binlog的效率差。
4. 尽量采用 **短的链路** ，也就是主库和从库服务器的距离尽量要短，提升端口带宽，减少binlog传输的网络延时。
5. 实时性要求的业务读强制走主库，从库只做灾备，备份。

#### 如何解决一致性问题

如果操作的数据存储在同一个数据库中，那么对数据进行更新的时候，可以对记录加写锁，这样在读取的时候就不会发生数据不一致的情况。但这时从库的作用就是 **备份** ，并没有起到 **读写分离** ，分担主库**读压力** 的作用。

![image-20230129171505204](/image-20230129171505204.png)

读写分离情况下，解决主从同步中数据不一致的问题， 就是解决主从之间 **数据复制方式** 的问题，如果按照数据一致性 **从弱到强** 来进行划分，有以下 3 种复制方式。

##### 方法一：异步复制

异步模式就是客户端提交COMMIT之后不需要等从库返回任何结果，而是直接将结果返回给客户端，这样做的好处是不会影响主库写的效率，但可能会存在主库宕机，而Binlog还没有同步到从库的情况，也就是此时的主库和从库数据不一致。这时候从从库中选择一个作为新主， 那么新主则可能缺少原来主服务器中已提交的事务。所以，这种复制模式下的数据一致性 是最弱的。

![image-20230129171516208](/image-20230129171516208.png)

##### 方法二：半同步复制

MySQL5.5版本之后开始支持半同步复制的方式。原理是在客户端提交COMMIT之后不直接将结果返回给客户端，而是等待至少有一个从库接收到了Binlog，并且写入到中继日志中，再返回给客户端。

这样做的好处就是提高了数据的一致性，当然相比于异步复制来说，至少多增加了一个网络连接的延迟，降低了主库写的效率。

在MySQL5.7版本中还增加了一个`rpl_semi_sync_master_wait_for_slave_count`参数，可以对应答的从库数量进行设置，默认为1，也就是说只要有1个从库进行了响应，就可以返回给客户端。如果将这个参数调大，可以提升数据一致性的强度，但也会增加主库等待从库响应的时间。

![image-20230129171531687](/image-20230129171531687.png)

##### 组复制

异步复制和半同步复制都无法最终保证数据的一致性问题，半同步复制是通过判断从库响应的个数来决定是否返回给客户端，虽然数据一致性相比于异步复制有提升，但仍然无法满足对数据一致性要求高的场景，比如金融领域。MGR 很好地弥补了这两种复制模式的不足。

组复制技术，简称 MGR（MySQL Group Replication）。是 MySQL 在 5.7.17 版本中推出的一种新的数据复制技术，这种复制技术是基于 Paxos 协议的状态机复制。

**MGR是如何工作的**

首先我们将多个节点共同组成一个复制组， 在**执行读写(RW)事务**的时候，需要通过一致性协议层(Consensus层)的同意，也就是读写事务想要进行提交,必须要经过组里“大多数人”(对应Node节点)的同意，大多数指的是同意的节点数量需要大于(N/2+1) ，这样才可以进行提交，而不是原发起方一个说了算。 而针对**只读(RO)事务**则不需要经过组内同意，直接COMMIT即可。

在一个复制组内有多个节点组成，它们各自维护了自己的数据副本，并且在一致性协议层实现了原子消息和全局有序消息，从而保证组内数据的一致性。

![image-20230129171546372](/image-20230129171546372.png)

MGR 将 MySQL 带入了数据强一致性的时代，是一个划时代的创新，其中一个重要的原因就是MGR 是基于 Paxos 协议的。Paxos 算法是由 2013 年的图灵奖获得者 Leslie Lamport 于 1990 年提出的，有关这个算法的决策机制可以搜一下。事实上，Paxos 算法提出来之后就作为 **分布式一致性算法** 被广泛应用，比如Apache 的 ZooKeeper 也是基于 Paxos 实现的。

### 知识延伸

在主从架构的配置中，如果想要采取读写分离的策略，我们可以 **自己编写程序** ，也可以通过 **第三方的中间件** 来实现。

- 自己编写程序的好处就在于比较自主，我们可以自己判断哪些查询在从库上来执行，针对实时性要求高的需求，我们还可以考虑哪些查询可以在主库上执行。同时，程序直接连接数据库，减少了中间件层，相当于减少了性能损耗。

- 采用中间件的方法有很明显的优势， **功能强大** ， **使用简单** 。但因为在客户端和数据库之间增加了中间件层会有一些 **性能损耗** ，同时商业中间件也是有使用成本的。我们也可以考虑采取一些优秀的开源工具。

  ![image-20230129171609882](/image-20230129171609882.png)

① **Cobar** 属于阿里B2B事业群，始于2008年，在阿里服役3年多，接管3000+个MySQL数据库的schema,集群日处理在线SQL请求50亿次以上。由于Cobar发起人的离职，Cobar停止维护。

② **Mycat** 是开源社区在阿里cobar基础上进行二次开发，解决了cobar存在的问题，并且加入了许多新的功能在其中。青出于蓝而胜于蓝。

③ **OneProxy** 基于MySQL官方的proxy思想利用c语言进行开发的，OneProxy是一款商业 收费 的中间件。舍弃了一些功能，专注在 性能和稳定性上 。 

④ **kingshard** 由小团队用go语言开发，还需要发展，需要不断完善。

⑤ **Vitess** 是Youtube生产在使用，架构很复杂。不支持MySQL原生协议，使用 需要大量改造成 本 。 

⑥ **Atlas** 是360团队基于mysql proxy改写，功能还需完善，高并发下不稳定。

⑦ **MaxScale** 是mariadb（MySQL原作者维护的一个版本） 研发的中间件

⑧ **MySQLRoute** 是MySQL官方Oracle公司发布的中间件

![image-20230129171627664](/image-20230129171627664.png)

![image-20230129171643832](/image-20230129171643832.png)

主备切换：

![image-20230129171655082](/image-20230129171655082.png)

- 主动切换
- 被动切换
- 如何判断主库出问题了？如何解决过程中的数据不一致性问题？

## 数据库备份与恢复

在任何数据库环境中，总会有**不确定的意外**情况发生，比如例外的停电、计算机系统中的各种软硬件故障、人为破坏、管理员误操作等是不可避免的，这些情况可能会导致**数据的丢失、服务器瘫痪**等严重的后果。存在多个服务器时，会出现主从服务器之间的**数据同步问题**。

为了有效防止数据丢失,并将损失降到最低，应**定期**对MySQL数据库服务器做**备份**。如果数据库中的数据丢失或者出现错误，可以使用备份的数据**进行恢复**。主从服务器之间的数据同步问题可以通过复制功能实现。

### 物理备份与逻辑备份

**物理备份**：备份数据文件，转储数据库物理文件到某一目录。物理备份**回复速度比较快，但占用空间比较小**，MySQL中可以用 **xtrabackup** 工具来进行物理备份。

**逻辑备份**：对数据库对象利用工具进行导出工作，汇总入备份文件内。逻辑备份**恢复速度慢，但占用空间小，更灵活**。MySQL 中常用的逻辑备份工具为 **mysqldump** 。逻辑备份就是 **备份sql语句** ，在恢复的时候执行备份的sql语句实现数据库数据的重现

### mysqldump实现逻辑备份

mysqldump是MySQL提供的一个非常有用的数据库备份工具。

#### 备份一个数据库

mysqldump命令执行时，可以将数据库备份成一个 **文本文件**，该文件中实际上包含多个`CREATE`和`INSERT`语句，使用这些语句可以重新创建表和插入数据。

- 查出需要备份的表的结构，在文本文件中生成一个CREATE语句
- 将表中的所有记录转换成一条INSERT语句。

**基本语法：**

```
mysqldump –u 用户名称 –h 主机名称 –p密码 待备份的数据库名称[tbname, [tbname...]]> 备份文件名称.sql
```

> 说明：
>
> 备份的文件并非一定要求后缀名为.sql，例如后缀名为.txt的文件也是可以的。

举例：使用root用户备份atguigu数据库：

```
mysqldump -uroot -p atguigu>atguigu.sql #备份文件存储在当前目录下
mysqldump -uroot -p atguigudb1 > /var/lib/mysql/atguigu.sql
```

![image-20230129171710117](/image-20230129171710117.png)

备份文件剖析：

开头将系统变量赋值给用户变量

结尾将用户变量赋值回系统变量

![image-20220211195407365](/image-20230129171731433.png)

![image-20230129171749281](/image-20230129171749281.png)

- 后面的DROP语句、CREATE语句和INSERT语句都是还原时使用的。 例如，“DROP TABLE IF EXISTS 'student"语句用来判断数据库中是否还有名为student的表，如果存在，就删除这个表；CREATE语句用来创建student的表；INSERT语句用来还原数据。
- 备份文件开始的一些语句以数字开头。这些数字代表了MySQL版本号，告诉我们这些语句只有在制定的MySQL版本或者比该版本高的情况下才能执行。例如，40101表明这些语句只有在MySQL版本号为4.01.01或者更高的条件下才可以被执行。文件的最后记录了备份的时间。

#### 备份全部数据库

若想用mysqldump备份整个实例，可以使用 **--all-databases** 或 **-A** 参数：

```
mysqldump -uroot -pxxxxxx --all-databases > all_database.sql 

mysqldump -uroot -pxxxxxx -A > all_database.sql
```

![image-20230129171801569](/image-20230129171801569.png)

#### 备份部分数据库

使用 -**-databases** 或 **-B** 参数了，该参数后面跟数据库名称，多个数据库间用空格隔开。如果指定databases参数，备份文件中会存在创建数据库的语句，如果不指定参数，则不存在。语法如下：

```
mysqldump –u user –h host –p --databases [数据库的名称1 [数据库的名称2...]] > 备份文件名 称.sql
```

举例：

```
mysqldump -uroot -p --databases atguigu atguigu12 >two_database.sql
```

或

```
mysqldump -uroot -p -B atguigu atguigu12 > two_database.sql
```

![image-20230129171812211](/image-20230129171812211.png)

#### 备份部分表

比如，在表变更前做个备份。语法如下：

```
mysqldump –u user –h host –p 数据库的名称 [表名1 [表名2...]] > 备份文件名称.sql
```

举例：备份atguigu数据库下的book表 

```
mysqldump -uroot -p atguigu book> book.sql
```

book.sql文件内容如下

表的创建，表的添加，表的数据

![image-20230129171832249](/image-20230129171832249.png)

![image-20230129171849021](/image-20230129171849021.png)

可以看到，book文件和备份的库文件类似。不同的是，book文件只包含book表的DROP、CREATE和 INSERT语句。

备份多张表使用下面的命令，比如备份book和account表：

```
#备份多张表 
mysqldump -uroot -p atguigu book account > 2_tables_bak.sql
```

#### 备份单表的部分数据

有些时候一张表的数据量很大，我们只需要部分数据。这时就可以使用 --where 选项了。where后面附带需要满足的条件。

举例：备份student表中id小于10的数据：

```
mysqldump -uroot -p atguigu student --where="id < 15 " > student_part_id10_low_bak.sql
```

内容如下所示，insert语句只有id小于15的部分

![image-20230129171904912](/image-20230129171904912.png)

#### 排除某些表的备份

如果我们想备份某个库，但是某些表数据量很大或者与业务关联不大，这个时候可以考虑排除掉这些表，同样的，选项 --ignore-table 可以完成这个功能。

```
mysqldump -uroot -p atguigu --ignore-table=atguigu.student > no_stu_bak.sql
```

通过如下指定判定文件中没有student表结构：

```
grep "student" no_stu_bak.sql
```

#### 只备份结构或只备份数据

只备份结构的话可以使用 **--no-data** 简写为 **-d** 选项；只备份数据可以使用 **--no-create-info** 简写为 **-t** 选项。

- 只备份结构

```
mysqldump -uroot -p atguigu --no-data > atguigu_no_data_bak.sql 
#使用grep命令，没有找到insert相关语句，表示没有数据备份。 
[root@node1 ~]# grep "INSERT" atguigu_no_data_bak.sql 
[root@node1 ~]#
```

![image-20230129171920831](/image-20230129171920831.png)

![image-20230129171932579](/image-20230129171932579.png)

- 只备份数据

```
mysqldump -uroot -p atguigu --no-create-info > atguigu_no_create_info_bak.sql 
#使用grep命令，没有找到create相关语句，表示没有数据结构。 
[root@node1 ~]# grep "CREATE" atguigu_no_create_info_bak.sql 
[root@node1 ~]#
```

![image-20230129171944069](/image-20230129171944069.png)

#### 备份中包含存储过程、函数、事件

mysqldump备份默认是不包含存储过程，自定义函数及事件的。可以使用 --routines 或 -R 选项来备份存储过程及函数，使用 --events 或 -E 参数来备份事件。

举例：备份整个atguigu库，包含存储过程及事件：

- 使用下面的SQL可以查看当前库有哪些存储过程或者函数

![image-20230129171958920](/image-20230129171958920.png)

下面备份atguigu库的数据，函数以及存储过程。

```
mysqldump -uroot -p -R -E --databases atguigu > fun_atguigu_bak.sql
```

查询备份文件中是否存在函数，如下所示，可以看到确实包含了函数。

![image-20230129172013914](/image-20230129172013914.png)

![image-20230129172030141](/image-20230129172030141.png)

#### mysqldump常用选项

mysqldump其他常用选项如下：

```
--add-drop-database：在每个CREATE DATABASE语句前添加DROP DATABASE语句。
--add-drop-tables：在每个CREATE TABLE语句前添加DROP TABLE语句。
--add-locking：用LOCK TABLES和UNLOCK TABLES语句引用每个表转储。重载转储文件时插入得更快。
--all-database, -A：转储所有数据库中的所有表。与使用--database选项相同，在命令行中命名所有数据库。
--comment[=0|1]：如果设置为0，禁止转储文件中的其他信息，例如程序版本、服务器版本和主机。--skip- comments与--comments=0的结果相同。默认值为1，即包括额外信息。
--compact：产生少量输出。该选项禁用注释并启用--skip-add-drop-tables、--no-set-names、--skip- disable-keys和--skip-add-locking选项。
--compatible=name：产生与其他数据库系统或旧的MySQL服务器更兼容的输出，值可以为ansi、MySQL323、 MySQL40、postgresql、oracle、mssql、db2、maxdb、no_key_options、no_table_options或者 no_field_options。
--complete_insert, -c：使用包括列名的完整的INSERT语句。
--debug[=debug_options], -#[debug_options]：写调试日志。
--delete，-D：导入文本文件前清空表。
--default-character-set=charset：使用charsets默认字符集。如果没有指定，就使用utf8。
--delete--master-logs：在主复制服务器上，完成转储操作后删除二进制日志。该选项自动启用-master- data。
--extended-insert，-e：使用包括几个VALUES列表的多行INSERT语法。这样使得转储文件更小，重载文件时可 以加速插入。
--flush-logs，-F：开始转储前刷新MySQL服务器日志文件。该选项要求RELOAD权限。
--force，-f：在表转储过程中，即使出现SQL错误也继续。
--lock-all-tables，-x：对所有数据库中的所有表加锁。在整体转储过程中通过全局锁定来实现。该选项自动关 闭--single-transaction和--lock-tables。
--lock-tables，-l：开始转储前锁定所有表。用READ LOCAL锁定表以允许并行插入MyISAM表。对于事务表（例 如InnoDB和BDB），--single-transaction是一个更好的选项，因为它根本不需要锁定表。
--no-create-db，-n：该选项禁用CREATE DATABASE /*!32312 IF NOT EXIST*/db_name语句，如果给出- -database或--all-database选项，就包含到输出中。
--no-create-info，-t：只导出数据，而不添加CREATE TABLE语句。
--no-data，-d：不写表的任何行信息，只转储表的结构。
--opt：该选项是速记，它可以快速进行转储操作并产生一个能很快装入MySQL服务器的转储文件。该选项默认开启， 但可以用--skip-opt禁用。
--password[=password]，-p[password]：当连接服务器时使用的密码。
-port=port_num，-P port_num：用于连接的TCP/IP端口号。
--protocol={TCP|SOCKET|PIPE|MEMORY}：使用的连接协议。
--replace，-r –replace和--ignore：控制替换或复制唯一键值已有记录的输入记录的处理。如果指定--
replace，新行替换有相同的唯一键值的已有行；如果指定--ignore，复制已有的唯一键值的输入行被跳过。如果不 指定这两个选项，当发现一个复制键值时会出现一个错误，并且忽视文本文件的剩余部分。
--silent，-s：沉默模式。只有出现错误时才输出。
--socket=path，-S path：当连接localhost时使用的套接字文件（为默认主机）。
--user=user_name，-u user_name：当连接服务器时MySQL使用的用户名。
--verbose，-v：冗长模式，打印出程序操作的详细信息。
--xml，-X：产生XML输出
```

运行帮助命令 mysqldump --help ，可以获得特定版本的完整选项列表。

> 提示 如果运行mysqldump没有--quick或--opt选项，mysqldump在转储结果前将整个结果集装入内存。如果转储大数据库可能会出现问题，该选项默认启用，但可以用--skip-opt禁用。如果使用最新版本的mysqldump程序备份数据，并用于恢复到比较旧版本的MySQL服务器中，则不要使用--opt 或-e选项。

### mysql命令恢复数据

使用mysqldump命令将数据库中的数据备份成一个文本文件。 需要恢复时，可以使用**mysql命令**来恢复备份的数据。

mysql命令可以执行备份文件中的**CREATE语句**和**INSERT语句**。通过CREATE语句来创建数据库和表。通过INSERT语句来插入备份的数据。

基本语法：

```
mysql –u root –p [dbname] < backup.sql
```

#### 单库备份中恢复单库

使用root用户，将之前练习中备份的atguigu.sql文件中的备份导入数据库中，命令如下：

如果备份文件中包含了创建数据库的语句，则恢复的时候不需要指定数据库名称，如下所示

```
mysql -uroot -p < atguigu.sql
```

否则需要指定数据库名称，如下所示

```
mysql -uroot -p atguigu4< atguigu.sql
```

![image-20230129172046366](/image-20230129172046366.png)

#### 全量备份恢复

如果我们现在有昨天的全量备份，现在想整个恢复，则可以这样操作：

```
mysql –u root –p < all.sql
mysql -uroot -pxxxxxx < all.sql
```

执行完后，MySQL数据库中就已经恢复了all.sql文件中的所有数据库。

> 补充：
>
> 如果使用-all-databases参数备份了所有的数据库，那么恢复时不需要指定数据库。对应的sql文件包含有CREATE DATABASE语句，可通过该语句创建数据库。创建数据库后，可以执行sql文件中的USE语句选择数据库，再创建表并插入记录。

#### 从全量备份中恢复单库

可能有这样的需求，比如说我们只想恢复某一个库，但是我们有的是整个实例的备份，这个时候我们可以从全量备份中分离出单个库的备份。

举例：

```
sed -n '/^-- Current Database: `atguigu`/,/^-- Current Database: `/p' all_database.sql > atguigu.sql 

#分离完成后我们再导入atguigu.sql即可恢复单个库
```

![image-20230129172127301](/image-20230129172127301.png)

得重启一下才能看到

#### 从单库备份中恢复单表

这个需求还是比较常见的。比如说我们知道哪个表误操作了，那么就可以用单表恢复的方式来恢复。

举例：我们有atguigu整库的备份，但是由于class表误操作，需要单独恢复出这张表。

```
cat atguigu.sql | sed -e '/./{H;$!d;}' -e 'x;/CREATE TABLE `class`/!d;q' > class_structure.sql 
cat atguigu.sql | grep --ignore-case 'insert into `class`' > class_data.sql 
#用shell语法分离出创建表的语句及插入数据的语句后 再依次导出即可完成恢复

use atguigu; 
mysql> source class_structure.sql; 
Query OK, 0 rows affected, 1 warning (0.00 sec) 

mysql> source class_data.sql; 
Query OK, 1 row affected (0.01 sec)
```

![image-20230129172145260](/image-20230129172145260.png)

### 物理备份：直接复制整个数据库

直接将MySQL中的数据库文件复制出来。这种方法最简单，速度也最快。MySQL的数据库目录位置不一定相同：

- 在Windows平台下，MySQL 8.0存放数据库的目录通常默认为 “ **C:\ProgramData\MySQL\MySQL Server 8.0\Data** ”或者其他用户自定义目录；
- 在Linux平台下，数据库目录位置通常为/var/lib/mysql/； 在
- MAC OSX平台下，数据库目录位置通常为“/usr/local/mysql/data”

但为了保证备份的一致性。需要保证：

- 方式1：备份前，将服务器停止。

- 方式2：备份前，对相关表执行 **FLUSH TABLES WITH READ LOCK** 操作。这样当复制数据库目录中的文件时，允许其他客户继续查询表。同时，FLUSH TABLES语句来确保开始备份前将所有激活的索引页写入硬盘。

  ![image-20230129172200931](/image-20230129172200931.png)

这种方式方便、快速，但不是最好的备份方法，因为实际情况可能 **不允许停止MySQL服务器** 或者 **锁住表** ，而且这种方法**对InnoDB存储引擎** 的表不适用。对于MyISAM存储引擎的表，这样备份和还原很方便，但是还原时最好是相同版本的MySQL数据库，否则可能会存在文件类型不同的情况。

注意，物理备份完毕后，执行 **UNLOCK TABLES** 来结算其他客户对表的修改行为。

> 说明： 在MySQL版本号中，第一个数字表示主版本号，主版本号相同的MySQL数据库文件格式相同。

此外，还可以考虑使用相关工具实现备份。比如， **MySQLhotcopy** 工具。MySQLhotcopy是一个Perl脚本，它使用LOCK TABLES、FLUSH TABLES和cp或scp来快速备份数据库。它是备份数据库或单个表最快的途径，但它只能运行在数据库目录所在的机器上，并且只能备份**MyISAM**类型的表。多用于mysql5.5之前。

### 物理恢复：直接复制到数据库目录

**步骤：**

1. 演示删除备份的数据库中指定表的数据
2. 将备份的数据库数据拷贝到数据目录下，并重启MySQL服务器
3. 查询相关表的数据是否恢复。需要使用下面的 **chown** 操作。

**要求：**

- 必须确保备份数据的数据库和待恢复的数据库服务器的主版本号相同。
  - 因为只有MySQL数据库主版本号相同时，才能保证这两个MySQL数据库文件类型是相同的。
- 这种方式对 **MyISAM类型的表比较有效** ，对于InnoDB类型的表则不可用。
  - 因为InnoDB表的表空间不能直接复制。
- 在Linux操作系统下，复制到数据库目录后，一定要将数据库的用户和组变成mysql，命令如下：
  - 不执行的话，读取不到（权限不够）
  - ![image-20230129172212912](/image-20230129172212912.png)

```
chown -R mysql.mysql /var/lib/mysql/dbname
```

其中，两个mysql分别表示组和用户；“-R”参数可以改变文件夹下的所有子文件的用户和组；“dbname”参数表示数据库目录。

> 提示 Linux操作系统下的权限设置非常严格。通常情况下，MySQL数据库只有root用户和mysql用户组下的mysql用户才可以访问，因此将数据库目录复制到指定文件夹后，一定要使用chown命令将文件夹的用户组变为mysql，将用户变为mysql。

### 表的导出和导入

#### 表的导出

##### 使用SELECT…INTO OUTFILE导出文本文件

在MySQL中，可以使用SELECT…INTO OUTFILE语句将表的内容导出成一个文本文件。

举例：使用SELECT…INTO OUTFILE将atguigu数据库中account表中的记录导出到文本文件。 

（1）选择数据库atguigu，并查询account表，执行结果如下所示。

![image-20230129172227245](/image-20230129172227245.png)

（2）mysql默认对导出的目录有权限限制，也就是说使用命令行进行导出的时候，需要指定目录进行操 作。

查询secure_file_priv值：

![image-20230129172241528](/image-20230129172241528.png)

参数secure_ file_ .priv的可选值和作用分别是：

- 如果设置为empty，表示不限制文件生成的位置，这是不安全的设置；
- 如果设置为一个表示路径的字符串，就要求生成的文件只能放在这个指定的目录，或者它的子目录； 
- 如果设置为NULL，就表示禁止在这个MySQL实例上执行select ... into outfile操作。

（3）上面结果中显示，secure_file_priv变量的值为/var/lib/mysql-files/，导出目录设置为该目录，SQL语句如下。

```
SELECT * FROM account INTO OUTFILE "/var/lib/mysql-files/account.txt";
```

（4）查看 /var/lib/mysql-files/account.txt`文件。

```
1 张三 90 
2 李四 100 
3 王五 0
```

##### 使用mysqldump命令导出文本文件

举例1：使用mysqldump命令将将atguigu数据库中account表中的记录导出到文本文件：

```
mysqldump -uroot -p -T "/var/lib/mysql-files/" atguigu account
```

mysqldump命令执行完毕后，在指定的目录/var/lib/mysql-files/下生成了account.sql和account.txt文件。

打开account.sql文件，其内容包含创建account表的CREATE语句。

![image-20230129172255689](/image-20230129172255689.png)

打开account.txt文件，其内容只包含account表中的数据。

```
[root@node1 mysql-files]# cat account.txt 
1 张三 90 
2 李四 100 
3 王五 0
```

举例2：使用mysqldump将atguigu数据库中的account表导出到文本文件，使用FIELDS选项，要求字段之间使用逗号“，”间隔，所有字符类型字段值用双引号括起来：

```
mysqldump -uroot -p -T "/var/lib/mysql-files/" atguigu account --fields-terminated- by=',' --fields-optionally-enclosed-by='\"'
```

语句mysqldump语句执行成功之后，指定目录下会出现两个文件account.sql和account.txt。

打开account.sql文件，其内容包含创建account表的CREATE语句。

![image-20230129172311849](/image-20230129172311849.png)

打开account.txt文件，其内容包含创建account表的数据。从文件中可以看出，字段之间用逗号隔开，字符类型的值被双引号括起来。

```
[root@node1 mysql-files]# cat account.txt 
1,"张三",90 
2,"李四",100 
3,"王五",0
```

##### 使用mysql命令导出文本文件

举例1：使用mysql语句导出atguigu数据中account表中的记录到文本文件：

```
mysql -uroot -p --execute="SELECT * FROM account;" atguigu> "/var/lib/mysqlfiles/account.txt"
```

打开account.txt文件，其内容包含创建account表的数据。

```
[root@node1 mysql-files]# cat account.txt 
id	name	balance 
1 	张三 		90 
2 	李四 		100 
3 	王五 		0
```

举例2：将atguigu数据库account表中的记录导出到文本文件，使用--veritcal参数将该条件记录分为多行显示：

```
mysql -uroot -p --vertical --execute="SELECT * FROM account;" atguigu > "/var/lib/mysql-files/account_1.txt"
```

打开account_1.txt文件，其内容包含创建account表的数据。

![image-20230129172329503](/image-20230129172329503.png)

举例3：将atguigu数据库account表中的记录导出到xml文件，使用--xml参数，具体语句如下。

```
mysql -uroot -p --xml --execute="SELECT * FROM account;" atguigu>"/var/lib/mysqlfiles/account_3.xml"
```

![image-20230129172344869](/image-20230129172344869.png)

说明：如果要将表数据导出到html文件中，可以使用 --html 选项。然后可以使用浏览器打开。

#### 表的导入

##### 使用LOAD DATA INFILE方式导入文本文件

**举例**1**：**

使用SELECT...INTO OUTFILE将atguigu数据库中account表的记录导出到文本文件

```
SELECT * FROM atguigu.account INTO OUTFILE '/var/lib/mysql-files/account_0.txt';
```

删除account表中的数据：

```
DELETE FROM atguigu.account;
```

从文本文件account.txt中恢复数据：

```
LOAD DATA INFILE '/var/lib/mysql-files/account_0.txt' INTO TABLE atguigu.account;
```

查询account表中的数据：

```
mysql> select * from account; 
+----+--------+---------+
| id | name   | balance | 
+----+--------+---------+
| 1  | 张三 	 | 90 	   | 
| 2  | 李四 	 | 100     | 
| 3  | 王五    | 0 	   | 
+----+--------+---------+
3 rows in set (0.00 sec)
```

举例2： 选择数据库atguigu，使用SELECT…INTO OUTFILE将atguigu数据库account表中的记录导出到文本文件，使用FIELDS选项和LINES选项，要求字段之间使用逗号"，"间隔，所有字段值用双引号括起来：

```
SELECT * FROM atguigu.account INTO OUTFILE '/var/lib/mysql-files/account_1.txt' FIELDS TERMINATED BY ',' ENCLOSED BY '\"';
```

删除account表中的数据：

```
DELETE FROM atguigu.account;
```

从/var/lib/mysql-files/account.txt中导入数据到account表中：

```
LOAD DATA INFILE '/var/lib/mysql-files/account_1.txt' INTO TABLE atguigu.account FIELDS TERMINATED BY ',' ENCLOSED BY '\"';
```

查询account表中的数据，具体SQL如下：

```
select * from account; mysql> 
select * from account;
+----+--------+---------+
| id | name   | balance | 
+----+--------+---------+
| 1  | 张三    | 90 	   | 
| 2  | 李四    | 100 	   | 
| 3  | 王五    | 0 	   |
+----+--------+---------+
3 rows in set (0.00 sec)
```

##### 使用mysqlimport方式导入文本文件

举例：

导出文件account.txt，字段之间使用逗号"，"间隔，字段值用双引号括起来：

```
SELECT * FROM atguigu.account INTO OUTFILE '/var/lib/mysql-files/account.txt' FIELDS TERMINATED BY ',' ENCLOSED BY '\"';
```

删除account表中的数据：

```
DELETE FROM atguigu.account;
```

使用mysqlimport命令将account.txt文件内容导入到数据库atguigu的account表中：

```
mysqlimport -uroot -p atguigu '/var/lib/mysql-files/account.txt' --fields-terminated- by=',' --fields-optionally-enclosed-by='\"'
```

查询account表中的数据：

```
select * from account; mysql> 
select * from account; 
+----+--------+---------+
| id | name   | balance | 
+----+--------+---------+
| 1  | 张三    | 90      |
| 2  | 李四    | 100     |
| 3  | 王五    | 0       |
+----+--------+---------+
3 rows in set (0.00 sec)
```

### 数据库迁移

#### 概述

数据迁移（data migration）是指选择、准备、提取和转换数据，并**将数据从一个计算机存储系统永久地传输到另一个计算机存储系统的过程**。此外， **验证迁移数据的完整性** 和 **退役原来旧的数据存储** ，也被认为是整个数据迁移过程的一部分。

数据库迁移的原因是多样的，包括服务器或存储设备更换、维护或升级，应用程序迁移，网站集成，灾难恢复和数据中心迁移。

根据不同的需求可能要采取不同的迁移方案，但总体来讲，MySQL 数据迁移方案大致可以分为 **物理迁移** 和 **逻辑迁移** 两类。通常以尽可能 **自动化** 的方式执行，从而将人力资源从繁琐的任务中解放出来。

#### 迁移方案

- 物理迁移

物理迁移适用于大数据量下的整体迁移。使用物理迁移方案的优点是比较快速，但需要停机迁移并且要求 MySQL 版本及配置必须和原服务器相同，也可能引起未知问题。

物理迁移包括拷贝数据文件和使用 XtraBackup 备份工具两种。

不同服务器之间可以采用物理迁移，我们可以在新的服务器上安装好同版本的数据库软件，创建好相同目录，建议配置文件也要和原数据库相同，然后从原数据库方拷贝来数据文件及日志文件，配置好文件组权限，之后在新服务器这边使用 mysqld 命令启动数据库。

- 逻辑迁移

逻辑迁移适用范围更广，无论是 部分迁移 还是 全量迁移 ，都可以使用逻辑迁移。逻辑迁移中使用最多的就是通过 mysqldump 等备份工具。

#### 迁移注意点

1. **相同版本的数据库之间迁移注意点**

   指的是在主版本号相同的MySQL数据库之间进行数据库移动。

   方式1： 因为迁移前后MySQL数据库的 主版本号相同 ，所以可以通过复制数据库目录来实现数据库迁移，但是物理迁移方式只适用于MyISAM引擎的表。对于InnoDB表，不能用直接复制文件的方式备份数据库。

   方式2： 最常见和最安全的方式是使用 mysqldump命令 导出数据，然后在目标数据库服务器中使用MySQL命令导入。

   举例：

   ```
   #host1的机器中备份所有数据库,并将数据库迁移到名为host2的机器上 
   mysqldump –h host1 –uroot –p –-all-databases| 
   mysql –h host2 –uroot –p
   ```

   在上述语句中，“|”符号表示管道，其作用是将mysqldump备份的文件给mysql命令；“--all-databases”表示要迁移所有的数据库。通过这种方式可以直接实现迁移。

2. **不同版本的数据库之间迁移注意点**

例如，原来很多服务器使用5.7版本的MySQL数据库，在8.0版本推出来以后，改进了5.7版本的很多缺陷，因此需要把数据库升级到8.0版本。

旧版本与新版本的MySQL可能使用不同的默认字符集，例如有的旧版本中使用latin1作为默认字符集，而最新版本的MySQL默认字符集为utf8mb4。如果数据库中有中文数据，那么迁移过程中需要对 默认字符集 进行修改 ，不然可能无法正常显示数据。

高版本的MySQL数据库通常都会 兼容低版本 ，因此可以从低版本的MySQL数据库迁移到高版本的MySQL数据库。

3. **不同数据库之间迁移注意点**

不同数据库之间迁移是指从其他类型的数据库迁移到MySQL数据库，或者从MySQL数据库迁移到其他类型的数据库。这种迁移没有普适的解决方法。

迁移之前，需要了解不同数据库的架构， 比较它们之间的差异 。不同数据库中定义相同类型的数据的 关 键字可能会不同 。例如，MySQL中日期字段分为DATE和TIME两种，而ORACLE日期字段只有DATE；SQL Server数据库中有ntext、Image等数据类型，MySQL数据库没有这些数据类型；MySQL支持的ENUM和SET类型，这些SQL Server数据库不支持。

另外，数据库厂商并没有完全按照SQL标准来设计数据库系统，导致不同的数据库系统的 SQL语句 有差别。例如，微软的SQL Server软件使用的是T-SQL语句，T-SQL中包含了非标准的SQL语句，不能和MySQL 的SQL语句兼容。

不同类型数据库之间的差异造成了互相 迁移的困难 ，这些差异其实是商业公司故意造成的技术壁垒。但是不同类型的数据库之间的迁移并 不是完全不可能 。例如，可以使用 MyODBC 实现MySQL和SQL Server之间的迁移。MySQL官方提供的工具 MySQL Migration Toolkit 也可以在不同数据之间进行数据迁移。MySQL迁移到Oracle时，需要使用mysqldump命令导出sql文件，然后， 手动更改 sql文件中的CREATE语  句。

#### 迁移小结

![image-20230129172402002](/image-20230129172402002.png)

### 删库了不跑路，能干点啥？

#### delete：误删行

经验之谈：

1. 恢复数据比较安全的做法，是恢复出一个备份 ，或者找一个从库作为临时库 ，在这个临时库上执行这些操作，然后再将确认过的临时库的数据，恢复回主库。如果直接修改主库，可能导致对数据的二次破坏 。
2. 当然，针对预防误删数据的问题，建议如下：
3. 把 sql_safe_updates 参数设置为 on 。这样一来，如果我们忘记在delete或者update语句中写where条件，或者where条件里面没有包含索引字段的话，这条语句的执行就会报错。

```
> 如果确定要把一个小表的数据全部删掉，在设置了sql_safe_updates=on情况下，可以在delete语句中加上where条件，比如where id>=0。
```

1. 代码上线前，必须经过 SQL审计 。

#### truncate/drop：误删库/表

方案：

这种情况下，要想恢复数据，就需要使用 全量备份 ，加 增量日志 的方式了。这个方案要求线上有定期的全量备份，并且实时备份binlog。

在这两个条件都具备的情况下，假如有人中午12点误删了一个库，恢复数据的流程如下：

1. 取最近一次 全量备份 ，假设这个库是一天一备，上次备份是当天 凌晨2点 ；
2. 用备份恢复出一个 临时库 ；
3. 从日志备份里面，取出凌晨2点之后的日志；
4. 把这些日志，除了误删除数据的语句外，全部应用到临时库。

#### 演示复制备库

如果有 非常核心 的业务，不允许太长的恢复时间，可以考虑搭建延迟复制的备库。一般的主备复制结构存在的问题是，如果主库上有个表被误删了，这个命令很快也会被发给所有从库，进而导致所有从库的数据表也都一起被误删了。

延迟复制的备库是一种特殊的备库，通过 CHANGE MASTER TO MASTER_DELAY = N 命令，可以指定这个备库持续保持跟主库有 N秒的延迟 。比如你把N设置为3600，这就代表了如果主库上有数据被误删了，并且在1小时内发现了这个误操作命令，这个命令就还没有在这个延迟复制的备库执行。这时候到这个备库上执行stop slave，再通过之前介绍的方法，跳过误操作命令，就可以恢复出需要的数据。

#### 预防误删库/表的方法

1. 账号分离 。这样做的目的是，避免写错命令。比如：

- 只给业务开发同学DML权限，而不给truncate/drop权限。而如果业务开发人员有DDL需求的话，可以通过开发管理系统得到支持。
- 即使是DBA团队成员，日常也都规定只使用 只读账号 ，必要的时候才使用有更新权限的账号。

1. 制定操作规范 。比如：

- 在删除数据表之前，必须先 对表做改名 操作。然后，观察一段时间，确保对业务无影响以后再删除这张表。
- 改表名的时候，要求给表名加固定的后缀（比加 _to_be_deleted )，然后删除表的动作必须通过管理系统执行。并且，管理系统删除表的时候，只能删除固定后缀的表。

#### rm：误删MySQL实例

对于一个有高可用机制的MySQL集群来说，不用担心 rm删除数据 了。只是删掉了其中某一个节点的数据的话，HA系统就会开始工作，选出一个新的主库，从而保证整个集群的正常工作。我们要做的就是在这个节点上把数据恢复回来，再接入整个集群。

### 附录：MySQL常用命令

#### mysql

该mysql不是指mysql服务，而是指mysql的客户端工具。

语法 ：

```
mysql [options] [database]
```

##### 连接选项

```
#参数 ：

-u, --user=name 指定用户名
-p, --password[=name] 指定密码
-h, --host=name 指定服务器IP或域名
-P, --port=# 指定连接端口 

#示例 ：

mysql -h 127.0.0.1 -P 3306 -u root -p

mysql -h127.0.0.1 -P3306 -uroot -p密码
```

##### 执行选项

```
-e, --execute=name 执行SQL语句并退出
```

此选项可以在Mysql客户端执行SQL语句，而不用连接到MySQL数据库再执行，对于一些批处理脚本，这种方式尤其方便。

```
#示例： 
mysql -uroot -p db01 -e "select * from tb_book";
```

#### mysqldamin

mysqladmin 是一个执行管理操作的客户端程序。可以用它来检查服务器的配置和当前状态、创建并删除数据库等。

可以通过 ： mysqladmin --help 指令查看帮助文档

```
#示例 ：
mysqladmin -uroot -p create 'test01'; 
mysqladmin -uroot -p drop 'test01'; 
mysqladmin -uroot -p verson;
```

#### mysqlbinlog

由于服务器生成的二进制日志文件以二进制格式保存，所以如果想要检查这些文本的文本格式，就会使用到mysqlbinlog 日志管理工具。

语法 ：

```
mysqlbinlog [options] log-files1 log-files2 ... 
#选项：

-d, --database=name : 指定数据库名称，只列出指定的数据库相关操作。
-o, --offset=# : 忽略掉日志中的前n行命令。
-r,--result-file=name : 将输出的文本格式日志输出到指定文件。
-s, --short-form : 显示简单格式， 省略掉一些信息。
--start-datatime=date1 --stop-datetime=date2 : 指定日期间隔内的所有日志。
--start-position=pos1 --stop-position=pos2 : 指定位置间隔内的所有日志。
```

#### mysqldump

mysqldump 客户端工具用来备份数据库或在不同数据库之间进行数据迁移。备份内容包含创建表，及插入表的SQL语句。

语法 ：

```
mysqldump [options] db_name [tables] 
mysqldump [options] --database/-B db1 [db2 db3...] 
mysqldump [options] --all-databases/-A
```

##### 连接选项

```
#参数 ：
-u, --user=name 指定用户名
-p, --password[=name] 指定密码
-h, --host=name 指定服务器IP或域名
-P, --port=# 指定连接端口
```

##### 输出内容选项

```
#参数：

--add-drop-database 在每个数据库创建语句前加上 Drop database 语句
--add-drop-table 在每个表创建语句前加上 Drop table 语句 , 默认开启 ; 不开启 (--skip-add-drop-table)
-n, --no-create-db 不包含数据库的创建语句
-t, --no-create-info 不包含数据表的创建语句
-d --no-data 不包含数据
-T, --tab=name 自动生成两个文件：一个.sql文件，创建表结构的语句； 一个.txt文件，数据文件，相当于select into outfile

#示例 ：
mysqldump -uroot -p db01 tb_book --add-drop-database --add-drop-table > a
mysqldump -uroot -p -T /tmp test city
```

#### mysqllimport/source

mysqlimport 是客户端数据导入工具，用来导入mysqldump 加 -T 参数后导出的文本文件。

语法：

```
mysqlimport [options] db_name textfile1 [textfile2...]
```

示例：

```
mysqlimport -uroot -p test /tmp/city.txt
```

如果需要导入sql文件,可以使用mysql中的source 指令 : 

```
source /root/tb_book.sql
```

#### mysqlshow

mysqlshow 客户端对象查找工具，用来很快地查找存在哪些数据库、数据库中的表、表中的列或者索引。

语法：

```
mysqlshow [options] [db_name [table_name [col_name]]]
```

参数：

```
--count 显示数据库及表的统计信息（数据库，表 均可以不指定）
-i 显示指定数据库或者指定表的状态信息
```

实例：

# 补充

## SQL语句执行过程

### 问题引入

![image-20230129172444825](/image-20230129172444825.png)

![image-20230129172504536](/image-20230129172504536.png)

**Where和Having的对比**

**区别1：WHERE 可以直接使用表中的字段作为筛选条件，但不能使用分组中的计算函数作为筛选条件；HAVING 必须要与 GROUP BY 配合使用，可以把分组计算的函数和分组字段作为筛选条件。**

这决定了，在需要对数据进行分组统计的时候，HAVING 可以完成 WHERE 不能完成的任务。这是因为，在查询语法结构中，WHERE 在 GROUP BY 之前，所以无法对分组结果进行筛选。HAVING 在 GROUP BY 之后，可以使用分组字段和分组中的计算函数，对分组的结果集进行筛选，这个功能是 WHERE 无法完成的。另外，WHERE排除的记录不再包括在分组中。

**区别2：如果需要通过连接从关联表中获取需要的数据，WHERE 是先筛选后连接，而 HAVING 是先连接后筛选。** 这一点，就决定了在关联查询中，WHERE 比 HAVING 更高效。因为 WHERE 可以先筛选，用一个筛选后的较小数据集和关联表进行连接，这样占用的资源比较少，执行效率也比较高。HAVING 则需要先把结果集准备好，也就是用未被筛选的数据集进行关联，然后对这个大的数据集进行筛选，这样占用的资源就比较多，执行效率也较低。

![image-20230129172523490](/image-20230129172523490.png)

问：区别2中，having和where的优劣是如何决定的呢？

### 查询的结构

```
#方式1(sql92)： 
SELECT ...,....,...
FROM ...,...,....
WHERE 多表的连接条件 
AND 不包含组函数的过滤条件 
GROUP BY ...,... 
HAVING 包含组函数的过滤条件
ORDER BY ... ASC/DESC
LIMIT ...,...

#方式2(sql99)：
SELECT ...,....,...
FROM ...
JOIN ... 
ON 多表的连接条件
JOIN ... 
ON ... 
WHERE 不包含组函数的过滤条件 
AND/OR 不包含组函数的过滤条件 
GROUP BY ...,... 
HAVING 包含组函数的过滤条件
ORDER BY ... ASC/DESC
LIMIT ...,...

#其中： 
#（1）from：从哪些表中筛选
#（2）on：关联多表查询时，去除笛卡尔积
#（3）where：从表中筛选的条件
#（4）group by：分组依据
#（5）having：在统计结果中再次筛选
#（6）order by：排序
#（7）limit：分页
```

### SELECT执行顺序

![image-20230129172541425](/image-20230129172541425.png)

1. **关键字的顺序是不能颠倒的：**

```
SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ... LIMIT...
```

1. **SELECT 语句的执行顺序**（在 MySQL 和 Oracle 中，SELECT 执行顺序基本相同）：

```
FROM -> WHERE -> GROUP BY -> HAVING -> SELECT 的字段 -> DISTINCT -> ORDER BY -> LIMIT
```

![image-20230129172603929](/image-20230129172603929.png)

比如你写了一个 SQL 语句，那么它的关键字顺序和执行顺序是下面这样的：

```
SELECT DISTINCT player_id, player_name, count(*) as num # 顺序 5 
FROM player JOIN team ON player.team_id = team.team_id # 顺序 1 
WHERE height > 1.80 # 顺序 2
GROUP BY player.team_id # 顺序 3 
HAVING num > 2 # 顺序 4 
ORDER BY num DESC # 顺序 6 
LIMIT 2 # 顺序 7
```

在 SELECT 语句执行这些步骤的时候，每个步骤都会产生一个 **虚拟表** ，然后将这个虚拟表传入下一个步骤中作为输入。需要注意的是，这些步骤隐含在 SQ 的执行过程中，对于我们来说是不可见的。

答：where的优先级比having高，且WHERE不包含组函数的过滤条件，过滤数据时也比having过滤的快，having包含组函数的过滤条件。

### SQL的执行原理

SELECT 是先执行 FROM 这一步的。在这个阶段，如果是多张表联查，还会经历下面的几个步骤：

1. 首先先通过 CROSS JOIN 求笛卡尔积，相当于得到虚拟表 vt（virtual table）1-1；
2. 通过 ON 进行筛选，在虚拟表 vt1-1 的基础上进行筛选，得到虚拟表 vt1-2；
3. 添加外部行。如果我们使用的是左连接、右链接或者全连接，就会涉及到外部行，也就是在虚拟表 vt1-2 的基础上增加外部行，得到虚拟表 vt1-3。

当然如果我们操作的是两张以上的表，还会重复上面的步骤，直到所有表都被处理完为止。这个过程得到是我们的原始数据。

然后进入第三步和第四步，也就是 GROUP 和 HAVING 阶段 。在这个阶段中，实际上是在虚拟表 vt2 的基础上进行分组和分组过滤，得到中间的虚拟表 vt3 和 vt4 。

当我们完成了条件筛选部分之后，就可以筛选表中提取的字段，也就是进入到 SELECT 和 DISTINCT 阶段 。

首先在 SELECT 阶段会提取想要的字段，然后在 DISTINCT 阶段过滤掉重复的行，分别得到中间的虚拟表vt5-1 和 vt5-2 。

当我们提取了想要的字段数据之后，就可以按照指定的字段进行排序，也就是 ORDER BY 阶段 ，得到虚拟表 vt6 。

最后在 vt6 的基础上，取出指定行的记录，也就是 LIMIT 阶段 ，得到最终的结果，对应的是虚拟表vt7 。

当然我们在写 SELECT 语句的时候，不一定存在所有的关键字，相应的阶段就会省略。

同时因为 SQL 是一门类似英语的结构化查询语言，所以我们在写 SELECT 语句的时候，还要注意相应的关键字顺序，**所谓底层运行的原理，就是我们刚才讲到的执行顺序**。

压测工具

并发压测

## MySQL的数据类型

### MySQL中的数据类型

![image-20230129172626497](/image-20230129172626497.png)

常见数据类型的属性，如下：

![image-20230129172637460](/image-20230129172637460.png)

字符集举例

```
#1.关于属性：character set name

SHOW VARIABLES LIKE 'character_%';

#创建数据库时指名字符集
CREATE DATABASE IF NOT EXISTS dbtest12 CHARACTER SET 'utf8';

SHOW CREATE DATABASE dbtest12;

#创建表的时候，指名表的字符集
CREATE TABLE temp(
id INT
) CHARACTER SET 'utf8';

SHOW CREATE TABLE temp;

#创建表，指名表中的字段时，可以指定字段的字符集
CREATE TABLE temp1(
id INT,
NAME VARCHAR(15) CHARACTER SET 'gbk'

);
SHOW CREATE TABLE temp1;
```

### 整形类型

#### 类型介绍

整数类型一共有 5 种，包括 TINYINT、SMALLINT、MEDIUMINT、INT（INTEGER）和 BIGINT。

它们的区别如下表所示：

![image-20230129172654061](/image-20230129172654061.png)

```
#2.整型数据类型

USE dbtest12;

CREATE TABLE test_int1(
f1 TINYINT,
f2 SMALLINT,
f3 MEDIUMINT,
f4 INTEGER,
f5 BIGINT

);

DESC test_int1;

INSERT INTO test_int1(f1)
VALUES(12),(-12),(-128),(127);

SELECT * FROM test_int1;

#Out of range value for column 'f1' at row 1
INSERT INTO test_int1(f1)
VALUES(128);

CREATE TABLE test_int2(
f1 INT,
f2 INT(5),
f3 INT(5) ZEROFILL  #① 显示宽度为5。当insert的值不足5位时，使用0填充。 ②当使用ZEROFILL时，自动会添加UNSIGNED
)

INSERT INTO test_int2(f1,f2)
VALUES(123,123),(123456,123456);

SELECT * FROM test_int2;

INSERT INTO test_int2(f3)
VALUES(123),(123456);

SHOW CREATE TABLE test_int2;


CREATE TABLE test_int3(
f1 INT UNSIGNED
);

DESC test_int3;

INSERT INTO test_int3
VALUES(2412321);

#Out of range value for column 'f1' at row 1
INSERT INTO test_int3
VALUES(4294967296);
```

#### 可选属性

**整数类型的可选属性有三个：**

- M

  - 表示显示宽度，M的取值范围是(0, 255)。

  - 如果设置了显示宽度，那么插入的数据宽度超过显示宽度限制，会不会截断或插入失败？

  - 答案：不会对插入的数据有任何影响，还是按照类型的实际宽度进行保存，即，显示宽度与类型可以存储的值范围无关 。**从MySQL 8.0.17开始，整数数据类型不推荐使用显示宽度属性。**

  - ```
    CREATE TABLE test_int2(
    f1 INT,
    f2 INT(5),
    f3 INT(5) ZEROFILL  
    #① 显示宽度为5。当insert的值不足5位时，使用0填充。 
    #②当使用ZEROFILL时，自动会添加UNSIGNED
    )
    ```

- UNSIGNED

  - 无符号类型（非负），所有的整数类型都有一个可选的属性UNSIGNED（无符号属性），无符号整数类型的最小取值为0。所以，如果需要在MySQL数据库中保存非负整数值时，可以将整数类型设置为无符号类型。
  - int类型默认显示宽度为int(11)，无符号int类型默认显示宽度为int(10)。 

- ZEROFILL

  - 0填充,（如果某列是ZEROFILL，那么MySQL会自动为当前列添加UNSIGNED属性），如果指定了ZEROFILL只是表示不够M位时，用0在左边填充，如果超过M位，只要不超过数据存储范围即可。

    原来，在 int(M) 中，M 的值跟 int(M) 所占多少存储空间并无任何关系。 int(3)、int(4)、int(8) 在磁盘上都是占用 4 bytes 的存储空间。也就是说，int(M)，**必须和UNSIGNED ZEROFILL一起使用才有意义。**如果整数值超过M位，就按照实际位数存储。只是无须再用字符 0 进行填充。

#### 适用场景

**TINYINT** ：一般用于枚举数据，比如系统设定取值范围很小且固定的场景。

**SMALLINT** ：可以用于较小范围的统计数据，比如统计工厂的固定资产库存数量等。

**MEDIUMINT** ：用于较大整数的计算，比如车站每日的客流量等。

**INT、INTEGER** ：取值范围足够大，一般情况下不用考虑超限问题，用得最多。比如商品编号。

**BIGINT** ：只有当你处理特别巨大的整数时才会用到。比如双十一的交易量、大型门户网站点击量、证券公司衍生产品持仓等。

#### 如何选择？

在评估用哪种整数类型的时候，你需要考虑 **存储空间** 和 **可靠性** 的平衡问题：一方 面，用占用字节数少的整数类型可以节省存储空间；另一方面，要是为了节省存储空间， 使用的整数类型取值范围太小，一旦遇到超出取值范围的情况，就可能引起 **系统错误** ，影响可靠性。

举个例子，商品编号采用的数据类型是 INT。原因就在于，客户门店中流通的商品种类较多，而且，每天都有旧商品下架，新商品上架，这样不断迭代，日积月累。

如果使用 SMALLINT 类型，虽然占用字节数比 INT 类型的整数少，但是却不能保证数据不会超出范围65535。相反，使用 INT，就能确保有足够大的取值范围，不用担心数据超出范围影响可靠性的问题。

你要注意的是，在实际工作中，**系统故障产生的成本远远超过增加几个字段存储空间所产生的成本。**因此，我建议你首先确保数据不会超过取值范围，在这个前提之下，再去考虑如何节省存储空间。

### 浮点类型

#### 类型介绍

浮点数和定点数类型的特点是可以 处理小数 ，你可以把整数看成小数的一个特例。因此，浮点数和定点数的使用场景，比整数大多了。 MySQL支持的浮点数类型，分别是 FLOAT、DOUBLE、REAL。 

- **FLOAT 表示单精度浮点数；**

- **DOUBLE 表示双精度浮点数；**

- REAL默认就是 DOUBLE。如果你把 SQL 模式设定为启用“REAL_AS_FLOAT ”，那 么，MySQL 就认为REAL 是 FLOAT。如果要启用“REAL_AS_FLOAT”，可以通过以下 SQL 语句实现：

  ```
  SET sql_mode = “REAL_AS_FLOAT”;
  ```

**问题**1**：**FLOAT 和 DOUBLE 这两种数据类型的区别是啥呢？

FLOAT 占用字节数少，取值范围小；DOUBLE 占用字节数多，取值范围也大。

**问题**2**：**为什么浮点数类型的无符号数取值范围，只相当于有符号数取值范围的一半，也就是只相当于有符号数取值范围大于等于零的部分呢？

MySQL 存储浮点数的格式为： **符号(S) 、 尾数(M) 和 阶码(E)** 。因此，无论有没有符号，MySQL 的浮点数都会存储表示符号的部分。因此， 所谓的无符号数取值范围，其实就是有符号数取值范围大于等于零的部分。

#### 数据精度说明

对于浮点类型，在MySQL中单精度值使用 4 个字节，双精度值使用 8 个字节。

#### 精度误差说明

浮点数类型有个缺陷，就是不精准。

为什么会存在这样的误差呢？问题还是出在 MySQL 对浮点类型数据的存储方式上。

MySQL 用 4 个字节存储 FLOAT 类型数据，用 8 个字节来存储 DOUBLE 类型数据。无论哪个，都是采用二进制的方式来进行存储的。比如 9.625，用二进制来表达，就是 1001.101，或者表达成 1.001101×2^3。如果尾数不是 0 或 5（比如 9.624），你就无法用一个二进制数来精确表达。进而，就只好在取值允许的范围内进行四舍五入。

在编程中，如果用到浮点数，要特别注意误差问题，**因为浮点数是不准确的，所以我们要避免使用“=”来判断两个数是否相等**。同时，在一些对精确度要求较高的项目中，千万不要使用浮点数，不然会导致结果错误，甚至是造成不可挽回的损失。那么，MySQL 有没有精准的数据类型呢？当然有，这就是定点数类型： **DECIMAL** 。

```
#3.浮点类型
CREATE TABLE test_double1(
f1 FLOAT,
f2 FLOAT(5,2),
f3 DOUBLE,
f4 DOUBLE(5,2)
);

DESC test_double1;

INSERT INTO test_double1(f1,f2)
VALUES(123.45,123.45);

SELECT * FROM test_double1;

INSERT INTO test_double1(f3,f4)
VALUES(123.45,123.456); #存在四舍五入

#Out of range value for column 'f4' at row 1
INSERT INTO test_double1(f3,f4)
VALUES(123.45,1234.456);

#Out of range value for column 'f4' at row 1
INSERT INTO test_double1(f3,f4)
VALUES(123.45,999.995);

#测试FLOAT和DOUBLE的精度问题
CREATE TABLE test_double2(
f1 DOUBLE
);

INSERT INTO test_double2
VALUES(0.47),(0.44),(0.19);

SELECT SUM(f1)
FROM test_double2;

SELECT SUM(f1) = 1.1,1.1 = 1.1
FROM test_double2;
```

### 定点数类型

#### 类型介绍

- MySQL中的定点数类型只有 DECIMAL 一种类型。

  ![image-20230129172726233](/image-20230129172726233.png)

  使用 DECIMAL(M,D) 的方式表示高精度小数。其中，M被称为精度，D被称为标度。0<=M<=65， 0<=D<=30，D<M。例如，定义DECIMAL（5,2）的类型，表示该列取值范围是-999.99~999.99。

- **DECIMAL(M,D)的最大取值范围与DOUBLE类型一样，但是有效的数据范围是由M和D决定的。**但是M占用存储空间M+2个字节，所以在一些对精度要求不高的场景下数，浮点数表达的数值范围可以更大一些。

- 定点数在MySQL内部是以 **字符串** 的形式进行存储，这就决定了它一定是精准的。

- **浮点数** vs **定点数**

  - **浮点数**相对于定点数的优点是在长度一定的情况下，浮点类型取值范围大，但是不精准，适用于需要取值范围大，又可以容忍微小误差的科学计算场景（比如**计算化学、分子建模、流体动力学**等）
  - **定点数**类型取值范围相对小，但是精准，没有误差，适合于对精度要求极高的场景 （比如涉及**金额计算**的场景）

```
#4. 定点数类型
CREATE TABLE test_decimal1(
f1 DECIMAL,
f2 DECIMAL(5,2)
);

DESC test_decimal1;

INSERT INTO test_decimal1(f1)
VALUES(123),(123.45);

SELECT * FROM test_decimal1;

INSERT INTO test_decimal1(f2)
VALUES(999.99);

INSERT INTO test_decimal1(f2)
VALUES(67.567);#存在四色五入

#Out of range value for column 'f2' at row 1
INSERT INTO test_decimal1(f2)
VALUES(1267.567);

#Out of range value for column 'f2' at row 1
INSERT INTO test_decimal1(f2)
VALUES(999.995);

#演示DECIMAL替换DOUBLE，体现精度
ALTER TABLE test_double2
MODIFY f1 DECIMAL(5,2);

DESC test_double2;

SELECT SUM(f1)
FROM test_double2;

SELECT SUM(f1) = 1.1,1.1 = 1.1
FROM test_double2;
```

#### 开发中经验

“由于 DECIMAL 数据类型的精准性，在我们的项目中，除了极少数（比如商品编号）用到整数类型外，其他的数值都用的是 DECIMAL，原因就是这个项目所处的零售行业，要求精准，一分钱也不能差。 ” 

——来自某项目经理

### 位类型

BIT类型中存储的是**二进制值**，类似010110。 

![image-20230129172744506](/image-20230129172744506.png)

BIT类型，如果没有指定(M)，默认是1位。这个1位，表示只能存1位的二进制值。这里(M)是表示二进制的位数，位数最小值为1，最大值为64。

```
#5. 位类型：BIT
CREATE TABLE test_bit1(
f1 BIT, 	#只能存2^1-1位的二进制值
f2 BIT(5),	#只能存2^5-1位的二进制值
f3 BIT(64)	#只能存2^64-1位的二进制值
);

DESC test_bit1;

INSERT INTO test_bit1(f1)
VALUES(0),(1);

SELECT *
FROM test_bit1;

#Data too long for column 'f1' at row 1
INSERT INTO test_bit1(f1)
VALUES(2);

INSERT INTO test_bit1(f2)
VALUES(31);

#Data too long for column 'f2' at row 1
INSERT INTO test_bit1(f2)
VALUES(32);#32加不进去，最多加31
#以上都是以16进制的形式存储在数据库

#BIN()展示2进制，HEX()展示16进制
SELECT BIN(f1),BIN(f2),HEX(f1),HEX(f2)
FROM test_bit1;

#此时+0以后，可以以十进制的方式显示数据
SELECT f1 + 0, f2 + 0
FROM test_bit1;
```

### 日期与时间类型

日期与时间是重要的信息，在我们的系统中，几乎所有的数据表都用得到。原因是客户需要知道数据的时间标签，从而进行数据查询、统计和处理。

MySQL有多种表示日期和时间的数据类型，不同的版本可能有所差异，MySQL8.0版本支持的日期和时间类型主要有：YEAR类型、TIME类型、DATE类型、DATETIME类型和TIMESTAMP类型。

- **YEAR** 类型通常用来表示年
- **DATE** 类型通常用来表示年、月、日
- **TIME** 类型通常用来表示时、分、秒
- **DATETIME** 类型通常用来表示年、月、日、时、分、秒
- **TIMESTAMP** 类型通常用来表示带时区的年、月、日、时、分、秒

![image-20230129172806387](/image-20230129172806387.png)

可以看到，不同数据类型表示的时间内容不同、取值范围不同，而且占用的字节数也不一样，你要根据实际需要灵活选取。

为什么时间类型 TIME 的取值范围不是 -23:59:59～23:59:59 呢？原因是 MySQL 设计的 TIME 类型，不光表示一天之内的时间，而且可以用来表示一个时间间隔，这个时间间隔可以超过 24 小时。

#### YEAR

YEAR类型用来表示年份，在所有的日期时间类型中所占用的存储空间最小，只需要 **1个字节** 的存储空间。

在MySQL中，YEAR有以下几种存储格式：

- 以4位字符串或数字格式表示YEAR类型，其格式为YYYY，最小值为1901，最大值为2155。 
- 以2位字符串格式表示YEAR类型，最小值为00，最大值为99。
  - 当取值为01到69时，表示2001到2069；
  - 当取值为70到99时，表示1970到1999；
  - 当取值整数的0或00添加的话，那么是0000年；
  - 当取值是日期/字符串的'0'添加的话，是2000年。

**从MySQL5.5.27开始，2位格式的YEAR已经不推荐使用**。YEAR默认格式就是“YYYY”，没必要写成YEAR(4)， 从MySQL 8.0.19开始，不推荐使用指定显示宽度的YEAR(4)数据类型。

```
#6.1 YEAR类型
CREATE TABLE test_year(
f1 YEAR,
f2 YEAR(4)
);

DESC test_year;

INSERT INTO test_year(f1)
VALUES('2021'),(2022);

SELECT * FROM test_year;

INSERT INTO test_year(f1)
VALUES ('2155');

#Out of range value for column 'f1' at row 1
INSERT INTO test_year(f1)
VALUES ('2156');

INSERT INTO test_year(f1)
VALUES ('69'),('70');

INSERT INTO test_year(f1)
VALUES (0),('00');
```

#### DATE

DATE类型表示日期，没有时间部分，格式为 YYYY-MM-DD ，其中，YYYY表示年份，MM表示月份，DD表示日期。需要 3个字节 的存储空间。在向DATE类型的字段插入数据时，同样需要满足一定的格式条件。

以 YYYY-MM-DD 格式或者 YYYYMMDD 格式表示的字符串日期，其最小取值为1000-01-01，最大取值为9999-12-03。YYYYMMDD格式会被转化为YYYY-MM-DD格式。

以 YY-MM-DD 格式或者 YYMMDD 格式表示的字符串日期，此格式中，年份为两位数值或字符串满足YEAR类型的格式条件为：当年份取值为00到69时，会被转化为2000到2069；当年份取值为70到99时，会被转化为1970到1999。

使用 CURRENT_DATE() 或者 NOW() 函数，会插入当前系统的日期。

```
#6.2 DATE类型

CREATE TABLE test_date1(
f1 DATE
);

DESC test_date1;

INSERT INTO test_date1
VALUES ('2020-10-01'), ('20201001'),(20201001);

INSERT INTO test_date1
VALUES ('00-01-01'), ('000101'), ('69-10-01'), ('691001'), ('70-01-01'), ('700101'), ('99-01-01'), ('990101');

INSERT INTO test_date1
VALUES (000301), (690301), (700301), (990301); #存在隐式转换

INSERT INTO test_date1
VALUES (CURDATE()),(CURRENT_DATE()),(NOW());

SELECT *
FROM test_date1;
```

#### TIME

TIME类型用来表示时间，不包含日期部分。在MySQL中，需要 3个字节 的存储空间来存储TIME类型的数据，可以使用“HH:MM:SS”格式来表示TIME类型，其中，HH表示小时，MM表示分钟，SS表示秒。

在MySQL中，向TIME类型的字段插入数据时，也可以使用几种不同的格式。

1. 可以使用带有冒号的字符串，比如**' D HH:MM:SS' 、' HH:MM:SS '、' HH:MM '、' D HH:MM '、' D HH '或' SS '**格式，都能被正 确地插入TIME类型的字段中。其中D表示天，其最小值为0，最大值为34。如果使用带有D格式的字符串插入TIME类型的字段时，D会被转化为小时，计算格式为D*24+HH。当使用带有冒号并且不带D的字符串表示时间时，表示当天的时间，比如12:10表示12:10:00，而不是00:12:10。
2. 可以使用不带有冒号的字符串或者数字，格式为' HHMMSS '或者 HHMMSS 。如果插入一个不合法的字符串或者数字，MySQL在存储数据时，会将其自动转化为00:00:00进行存储。比如1210，MySQL会将最右边的两位解析成秒，表示00:12:10，而不是12:10:00。
3. 使用 CURRENT_TIME() 或者 NOW() ，会插入当前系统的时间。

```
#6.3 TIME类型
CREATE TABLE test_time1(
f1 TIME
);

DESC test_time1;

INSERT INTO test_time1
VALUES('2 12:30:29'), ('12:35:29'), ('12:40'), ('2 12:40'),('1 05'), ('45');

INSERT INTO test_time1
VALUES ('123520'), (124011),(1210);

INSERT INTO test_time1
VALUES (NOW()), (CURRENT_TIME()),(CURTIME());

SELECT *
FROM test_time1;
```

#### DATETIME

DATETIME类型在所有的日期时间类型中占用的存储空间最大，总共需要 **8** 个字节的存储空间。在格式上为DATE类型和TIME类型的组合，可以表示为 **YYYY-MM-DD HH:MM:SS** ，其中YYYY表示年份，MM表示月份，DD表示日期，HH表示小时，MM表示分钟，SS表示秒。

在向DATETIME类型的字段插入数据时，同样需要满足一定的格式条件。

- 以 **YYYY-MM-DD HH:MM:SS** 格式或者 **YYYYMMDDHHMMSS** 格式的字符串插入DATETIME类型的字段时，最小值为1000-01-01 00:00:00，最大值为9999-12-03 23:59:59。 
  - 以YYYYMMDDHHMMSS格式的数字插入DATETIME类型的字段时，会被转化为YYYY-MM-DD HH:MM:SS格式。
- 以 **YY-MM-DD HH:MM:SS** 格式或者 **YYMMDDHHMMSS** 格式的字符串插入DATETIME类型的字段时，两位数的年份规则符合YEAR类型的规则，00到69表示2000到2069；70到99表示1970到1999。
- 使用函数 **CURRENT_TIMESTAMP()** 和 **NOW()** ，可以向DATETIME类型的字段插入系统的当前日期和时间。

```
#6.4 DATETIME类型
CREATE TABLE test_datetime1(
dt DATETIME
);

INSERT INTO test_datetime1
VALUES ('2021-01-01 06:50:30'), ('20210101065030');

INSERT INTO test_datetime1
VALUES ('99-01-01 00:00:00'), ('990101000000'), ('20-01-01 00:00:00'), ('200101000000');

INSERT INTO test_datetime1
VALUES (20200101000000), (200101000000), (19990101000000), (990101000000);
 
INSERT INTO test_datetime1
VALUES (CURRENT_TIMESTAMP()), (NOW()),(SYSDATE());

SELECT *
FROM test_datetime1;
```

#### TIMESRAMP

TIMESTAMP类型也可以表示日期时间，其显示格式与DATETIME类型相同，都是 **YYYY-MM-DD HH:MM:SS** ，需要**4**个字节的存储空间。但是TIMESTAMP存储的时间范围比DATETIME要小很多，只能存储“1970-01-01 00:00:01 UTC”到“2038-01-19 03:14:07 UTC”之间的时间。其中，UTC表示世界统一时间，也叫作世界标准时间。

- **存储数据的时候需要对当前时间所在的时区进行转换，查询数据的时候再将时间转换回当前的时区。因此，使用TIMESTAMP存储的同一个时间值，在不同的时区查询时会显示不同的时间。**

向TIMESTAMP类型的字段插入数据时，当插入的数据格式满足YY-MM-DD HH:MM:SS和YYMMDDHHMMSS时，两位数值的年份同样符合YEAR类型的规则条件，只不过表示的时间范围要小很多。

如果向TIMESTAMP类型的字段插入的时间超出了TIMESTAMP类型的范围，则MySQL会抛出错误信息。

```
#6.5 TIMESTAMP类型
CREATE TABLE test_timestamp1(
ts TIMESTAMP
);

INSERT INTO test_timestamp1
VALUES ('1999-01-01 03:04:50'), ('19990101030405'), ('99-01-01 03:04:05'), ('990101030405');

INSERT INTO test_timestamp1
VALUES ('2020@01@01@00@00@00'), ('20@01@01@00@00@00');

INSERT INTO test_timestamp1
VALUES (CURRENT_TIMESTAMP()), (NOW());

#Incorrect datetime value
INSERT INTO test_timestamp1
VALUES ('2038-01-20 03:14:07');

SELECT *
FROM test_timestamp1;
```

**TIMESTAMP和DATETIME的区别：**

- TIMESTAMP存储空间比较小，表示的日期时间范围也比较小
- 底层存储方式不同，TIMESTAMP底层存储的是毫秒值，距离1970-1-1 0:0:0 0毫秒的毫秒值。
- 两个日期比较大小或日期计算时，TIMESTAMP更方便、更快。
- TIMESTAMP和时区有关。TIMESTAMP会根据用户的时区不同，显示不同的结果。而DATETIME则只能反映出插入时当地的时区，其他时区的人查看数据必然会有误差的。

```
#对比DATETIME 和 TIMESTAMP
CREATE TABLE temp_time(
d1 DATETIME,
d2 TIMESTAMP
);

INSERT INTO temp_time VALUES('2021-9-2 14:45:52','2021-9-2 14:45:52');

INSERT INTO temp_time VALUES(NOW(),NOW());

SELECT * FROM temp_time;

#修改当前的时区
SET time_zone = '+9:00';

SELECT * FROM temp_time;
```

#### 开发中经验

用得最多的日期时间类型，就是 **DATETIME** 。虽然 MySQL 也支持 YEAR（年）、 TIME（时间）、DATE（日期），以及 TIMESTAMP 类型，但是在实际项目中，**尽量用 DATETIME 类型**。**因为这个数据类型包括了完整的日期和时间信息，取值范围也最大，使用起来比较方便。**毕竟，如果日期时间信息分散在好几个字段，很不容易记，而且查询的时候，SQL 语句也会更加复杂。

此外，一般存注册时间、商品发布时间等，不建议使用DATETIME存储，而是使用 **时间戳** ，因为DATETIME虽然直观，但不便于计算。

```
mysql> SELECT UNIX_TIMESTAMP();
+------------------+
| UNIX_TIMESTAMP() |
+------------------+
| 1635932762 	   | 
+------------------+
1 row in set (0.00 sec)
```

### 文本字符串类型

在实际的项目中，我们还经常遇到一种数据，就是字符串数据。

MySQL中，文本字符串总体上分为 **CHAR 、 VARCHAR 、 TINYTEXT 、 TEXT 、 MEDIUMTEXT 、 LONGTEXT 、 ENUM 、 SET** 等类型。

![image-20230129172825261](/image-20230129172825261.png)

#### CHAR和VARCHAR

CHAR和VARCHAR类型都可以存储比较短的字符串。

![image-20230129172840023](/image-20230129172840023.png)

**CHAR类型：**

- CHAR(M) 类型一般需要预先定义字符串长度。如果不指定(M)，则表示长度默认是1个字符。
- 如果保存时，数据的实际长度比CHAR类型声明的长度小，则会在 **右侧填充** 空格以达到指定的长度。当MySQL检索CHAR类型的数据时，CHAR类型的字段会去除尾部的空格。
- 定义CHAR类型字段时，声明的字段长度即为CHAR类型字段所占的存储空间的字节数。

```
#7.1 CHAR类型
CREATE TABLE test_char1(
	c1 CHAR,
	c2 CHAR(5)
);

DESC test_char1;

INSERT INTO test_char1(c1)
VALUES('a');

#Data too long for column 'c1' at row 1
INSERT INTO test_char1(c1)
VALUES('ab');

INSERT INTO test_char1(c2)
VALUES('ab');

INSERT INTO test_char1(c2)
VALUES('hello');

INSERT INTO test_char1(c2)
VALUES('尚');

INSERT INTO test_char1(c2)
VALUES('硅谷');

INSERT INTO test_char1(c2)
VALUES('尚硅谷教育');

#Data too long for column 'c2' at row 1
INSERT INTO test_char1(c2)
VALUES('尚硅谷IT教育');

SELECT * FROM test_char1;

SELECT CONCAT(c2,'***')
FROM test_char1;

INSERT INTO test_char1(c2)
VALUES('ab  ');

SELECT CHAR_LENGTH(c2)
FROM test_char1;
```

**VARCHAR类型：**

- VARCHAR(M) 定义时， 必须指定长度M，否则报错。
- MySQL4.0版本以下，varchar(20)：指的是20字节，如果存放UTF8汉字时，只能存6个（每个汉字3字节） ；MySQL5.0版本以上，varchar(20)：指的是20字符。
- 检索VARCHAR类型的字段数据时，会保留数据尾部的空格。VARCHAR类型的字段所占用的存储空间为字符串实际长度加1个字节。

```
#7.2 VARCHAR类型
CREATE TABLE test_varchar1(
NAME VARCHAR  #错误
);

#Column length too big for column 'name' (max = 21845); use BLOB or TEXT instead
CREATE TABLE test_varchar2(
NAME VARCHAR(65535)
);

CREATE TABLE test_varchar3(
NAME VARCHAR(5)
);

INSERT INTO test_varchar3
VALUES('尚硅谷'),('尚硅谷教育');

#Data too long for column 'NAME' at row 1
INSERT INTO test_varchar3
VALUES('尚硅谷IT教育');
```

**哪些情况使用 CHAR 或 VARCHAR 更好**

![image-20230129172857369](/image-20230129172857369.png)

情况1：存储很短的信息。比如门牌号码101，201……这样很短的信息应该用char，因为varchar还要占个byte用于存储信息长度，本来打算节约存储的，结果得不偿失。

情况2：固定长度的。比如使用uuid作为主键，那用char应该更合适。因为他固定长度，varchar动态根据长度的特性就消失了，而且还要占个长度信息。

情况3：十分频繁改变的column。因为varchar每次存储都要有额外的计算，得到长度等工作，如果一个非常频繁改变的，那就要有很多的精力用于计算，而这些对于char来说是不需要的。

情况4：具体存储引擎中的情况：

- **MyISAM** 数据存储引擎和数据列：MyISAM数据表，最好使用固定长度(CHAR)的数据列代替可变长度(VARCHAR)的数据列。这样使得整个表静态化，从而使 数据检索更快 ，用空间换时间。
- MEMORY 存储引擎和数据列：MEMORY数据表目前都使用固定长度的数据行存储，因此无论使用CHAR或VARCHAR列都没有关系，两者都是作为CHAR类型处理的。
- **InnoDB** 存储引擎，建议使用VARCHAR类型。因为对于InnoDB数据表，内部的行存储格式并没有区分固定长度和可变长度列（所有数据行都使用指向数据列值的头指针），而且**主要影响性能的因素是数据行使用的存储总量**，由于char平均占用的空间多于varchar，所以除了简短并且固定长度的，其他考虑varchar。这样节省空间，对磁盘I/O和数据存储总量比较好。 

#### TEXT

在MySQL中，TEXT用来保存文本类型的字符串，总共包含4种类型，分别为TINYTEXT、TEXT、 MEDIUMTEXT 和 LONGTEXT 类型。

在向TEXT类型的字段保存和查询数据时，系统自动按照实际长度存储，不需要预先定义长度。这一点和VARCHAR类型相同。

每种TEXT类型保存的数据长度和所占用的存储空间不同，如下：

![image-20230129172908948](/image-20230129172908948.png)

**由于实际存储的长度不确定，MySQL 不允许 TEXT 类型的字段做主键**。遇到这种情况，你只能采用

CHAR(M)，或者 VARCHAR(M)。

```
#7.3 TEXT类型
CREATE TABLE test_text(
tx TEXT
);

INSERT INTO test_text
VALUES('atguigu   ');

SELECT CHAR_LENGTH(tx)
FROM test_text; #10
```

说明在保存和查询数据时，并没有删除TEXT类型的数据尾部的空格。

**开发中经验：**

TEXT文本类型，可以存比较大的文本段，搜索速度稍慢，因此如果不是特别大的内容，建议使用CHAR， VARCHAR来代替。还有TEXT类型不用加默认值，加了也没用。而且text和blob类型的数据删除后容易导致“空洞”，使得文件碎片比较多，所以频繁使用的表不建议包含TEXT类型字段，建议单独分出去，单独用一个表。

### ENUM类型

ENUM类型也叫作枚举类型，ENUM类型的取值范围需要在定义字段时进行指定。设置字段值时，ENUM类型只允许从成员中选取单个值，不能一次选取多个值。

![image-20230129172926682](/image-20230129172926682.png)

- 当ENUM类型包含1～255个成员时，需要1个字节的存储空间；
- 当ENUM类型包含256～65535个成员时，需要2个字节的存储空间。
- ENUM类型的成员个数的上限为65535个。

```
#8. ENUM类型

CREATE TABLE test_enum(
season ENUM('春','夏','秋','冬','unknow')
);

INSERT INTO test_enum
VALUES('春'),('秋');

SELECT * FROM test_enum;

#Data truncated for column 'season' at row 1
INSERT INTO test_enum
VALUES('春,秋');
#Data truncated for column 'season' at row 1
INSERT INTO test_enum
VALUES('人');

INSERT INTO test_enum
VALUES('unknow');

#忽略大小写的
INSERT INTO test_enum
VALUES('UNKNOW');

#可以使用索引进行枚举元素的调用
INSERT INTO test_enum
VALUES(1),('3');

# 没有限制非空的情况下，可以添加null值
INSERT INTO test_enum
VALUES (NULL);
```

### SET类型

SET表示一个字符串对象，可以包含0个或多个成员，但成员个数的上限为 64 。设置字段值时，可以取取值范围内的 0 个或多个值。

当SET类型包含的成员个数不同时，其所占用的存储空间也是不同的，具体如下：

![image-20230129172939614](/image-20230129172939614.png)

SET类型在存储数据时成员个数越多，其占用的存储空间越大。注意：SET类型在选取成员时，可以一次选择多个成员，这一点与ENUM类型不同。

```
#9. SET类型
CREATE TABLE test_set(
s SET ('A', 'B', 'C')
);

INSERT INTO test_set (s) VALUES ('A'), ('A,B');

#插入重复的SET类型成员时，MySQL会自动删除重复的成员
INSERT INTO test_set (s) VALUES ('A,B,C,A');

#向SET类型的字段插入SET成员中不存在的值时，MySQL会抛出错误。
INSERT INTO test_set (s) VALUES ('A,B,C,D');

SELECT *
FROM test_set;


CREATE TABLE temp_mul(
gender ENUM('男','女'),
hobby SET('吃饭','睡觉','打豆豆','写代码')
);

INSERT INTO temp_mul
VALUES('男','睡觉,打豆豆');

SELECT * 
FROM temp_mul;

#Data truncated for column 'gender' at row 1
INSERT INTO temp_mul
VALUES('男,女','睡觉,打豆豆');
```

### 二进制字符串类型

MySQL中的二进制字符串类型主要存储一些二进制数据，比如可以存储图片、音频和视频等二进制数据。

MySQL中支持的二进制字符串类型主要包括BINARY、VARBINARY、TINYBLOB、BLOB、MEDIUMBLOB 和 LONGBLOB类型。

#### BINARY**与**VARBINARY**类型** 

BINARY和VARBINARY类似于CHAR和VARCHAR，只是它们存储的是二进制字符串。

![image-20230129172957140](/image-20230129172957140.png)

```
#10.1 BINARY 与 VARBINARY类型
CREATE TABLE test_binary1(
f1 BINARY,
f2 BINARY(3),
#f3 VARBINARY,
f4 VARBINARY(10)
);

DESC test_binary1;

INSERT INTO test_binary1(f1,f2)
VALUES('a','abc');

SELECT * FROM test_binary1;

#Data too long for column 'f1' at row 1
INSERT INTO test_binary1(f1)
VALUES('ab');

INSERT INTO test_binary1(f2,f4)
VALUES('ab','ab');

SELECT LENGTH(f2),LENGTH(f4)
FROM test_binary1;
```

#### BLOB类型

BLOB是一个 二进制大对象 ，可以容纳可变数量的数据。

MySQL中的BLOB类型包括TINYBLOB、BLOB、MEDIUMBLOB和LONGBLOB 4种类型，它们可容纳值的最大长度不同。可以存储一个二进制的大对象，比如 图片 、 音频 和 视频 等。

需要注意的是，在实际工作中，往往不会在MySQL数据库中使用BLOB类型存储大对象数据，通常会将图片、音频和视频文件存储到 服务器的磁盘上 ，并将图片、音频和视频的访问路径存储到MySQL中。

![image-20230129173025002](/image-20230129173025002.png)

```
#10.2 Blob类型
CREATE TABLE test_blob1(
id INT,
img MEDIUMBLOB
);

INSERT INTO test_blob1(id)
VALUES (1001);

SELECT *
FROM test_blob1;
```

**TEXT和BLOB的使用注意事项：**

在使用text和blob字段类型时要注意以下几点，以便更好的发挥数据库的性能。

① BLOB和TEXT值也会引起自己的一些问题，特别是执行了大量的删除或更新操作的时候。删除这种值会在数据表中留下很大的" 空洞 "，以后填入这些"**空洞**"的记录可能长度不同。为了提高性能，建议定期使用 OPTIMIZE TABLE 功能对这类表进行 **碎片整理** 。 

② 如果需要对大文本字段进行模糊查询，MySQL 提供了 **前缀索引** 。但是仍然要在不必要的时候避免检索大型的BLOB或TEXT值。例如，SELECT * 查询就不是很好的想法，除非你能够确定作为约束条件的WHERE子句只会找到所需要的数据行。否则，你可能毫无目的地在网络上传输大量的值。

③ 把BLOB或TEXT列 **分离到单独的表** 中。在某些环境中，如果把这些数据列移动到第二张数据表中，可以让你把原数据表中的数据列转换为固定长度的数据行格式，那么它就是有意义的。这会 **减少主表中的 碎片** ，使你得到固定长度数据行的性能优势。它还使你在主数据表上运行 SELECT * 查询的时候不会通过网络传输大量的BLOB或TEXT值。

### JSON类型

JSON（JavaScript Object Notation）是一种轻量级的 数据交换格式 。简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。它易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

**JSON 可以将 JavaScript 对象中表示的一组数据转换为字符串，然后就可以在网络或者程序之间轻松地传递这个字符串，并在需要的时候将它还原为各编程语言所支持的数据格式。**

在MySQL 5.7中，就已经支持JSON数据类型。在MySQL 8.x版本中，JSON类型提供了可以进行自动验证的JSON文档和优化的存储结构，使得在MySQL中存储和读取JSON类型的数据更加方便和高效。 创建数据表，表中包含一个JSON类型的字段 js 。

```
#11. JSON类型
CREATE TABLE test_json(
js json

);

INSERT INTO test_json (js) 
VALUES ('{"name":"songhk", "age":18, "address":{"province":"beijing", "city":"beijing"}}');


SELECT * FROM test_json;

SELECT js -> '$.name' AS NAME,js -> '$.age' AS age ,js -> '$.address.province' AS province, js -> '$.address.city' AS city
FROM test_json;
```

### 空间类型

MySQL 空间类型扩展支持地理特征的生成、存储和分析。这里的地理特征表示世界上具有位置的任何东西，可以是一个实体，例如一座山；可以是空间，例如一座办公楼；也可以是一个可定义的位置，例如一个十字路口等等。MySQL中使用 Geometry（几何） 来表示所有地理特征。Geometry指一个点或点的集合，代表世界上任何具有位置的事物。

MySQL的空间数据类型（Spatial Data Type）对应于OpenGIS类，包括单值类型：GEOMETRY、POINT、 LINESTRING、POLYGON以及集合类型：MULTIPOINT、MULTILINESTRING、MULTIPOLYGON、 GEOMETRYCOLLECTION 。

- Geometry是所有空间集合类型的基类，其他类型如POINT、LINESTRING、POLYGON都是Geometry的子类。
  - Point，点
  - LineString，线
  - Polygon，多边形

下面展示几种常见的几何图形元素：

![image-20230129173043536](/image-20230129173043536.png)

下面展示的是多个同类或异类几何图形元素的组合：

![image-20230129173055325](/image-20230129173055325.png)

### 小结选择建议

在定义数据类型时，如果确定是 整数 ，就用 **INT** ； 如果是 小数 ，一定用定点数类型**DECIMAL(M,D)** ； 如果是日期与时间，就用 **DATETIME** 。

这样做的好处是，首先确保你的系统不会因为数据类型定义出错。不过，凡事都是有两面的，可靠性好，并不意味着高效。比如，TEXT 虽然使用方便，但是效率不如 CHAR(M) 和 VARCHAR(M)。

关于字符串的选择，建议参考如下阿里巴巴的《Java开发手册》规范：

**阿里巴巴《Java开发手册》之MySQL数据库：**

- 任何字段如果为**非负数**，必须是 **UNSIGNED** 
- 【 强制 】**小数类型为 DECIMAL**，禁止使用 FLOAT 和 DOUBLE。
  - 说明：在存储的时候，FLOAT 和 DOUBLE 都存在精度损失的问题，很可能在比较值的时候，得到不正确的结果。如果存储的数据范围**超过 DECIMAL 的范围**，建议**将数据拆成整数和小数并分开存储**。
- 【 强制 】**如果存储的字符串长度几乎相等，使用 CHAR 定长字符串类型**。
- 【 强制 】**VARCHAR 是可变长字符串，不预先分配存储空间，长度不要超过 5000。如果存储长度大于此值，定义字段类型为 TEXT，独立出来一张表，用主键来对应，避免影响其它字段索引效率**。

### 约束
