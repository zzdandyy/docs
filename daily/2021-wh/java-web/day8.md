---
description: '2021-02-01'
---

# day8

## 完成情况

{% hint style="success" %}
每一天都学习并记录
{% endhint %}

{% hint style="warning" %}
每天看至少3个leetcode的题目
{% endhint %}

## 学习内容

### 数据库

DataBase DB

登录：mysql -u -p

退出：exit

#### SQL

Structured Query Language

操作所有的关系型数据库

#### 语法

不区分大小写，但是关键字建议使用大写

建议使用 \# 注释

**DDL**

（操作数据库和表）

**数据库**

CREATE 创建

* create database test; 创建数据库test
* create database test if not exists;  如果test数据库不存在，创建
* create database test character set gbk; 创建数据库test,指定字符集为gbk

RETRIEVE 查询

* show databases; 查看所有的数据库
* show create database mysql;查看创建mysql数据库的语句，查看字符集等等

UPDATE 修改

* alter database test character set utf8; 修改数据库test的字符集为utf8

DELETE 删除

* drop database test; 删除数据库test
* drop database test if exists; 如果数据库test存在，删除

USE 使用

* use test; 使用数据库test
* select database\(\); 查看当前正在使用的数据库

**表**

CREATE 创建

* 数据类型

  int 整数类型

  double\(4,2\) 小数类型，长度为4，小数部分为2

  date 日期，只包含年月日，yyyy-MM-dd

  datetime 日期，包含年月日时分秒,yyyy-MM-dd HH:mm:ss

  timestamp 时间戳类型，包含年月日时分秒,yyyy-MM-dd HH:mm:ss，如果不给字段赋值，即默认为系统当前时间

  varchar\(5\) 字符串，长度最长为5的字符串

* create table student\(

  列名1 数据类型，

  列名2 数据类型，

  列名3 数据类型，

  \)；

* 复制表

  create table stu like student;

RETRIEVE 查询

* show tables; 查看当前数据库的所有表
* desc func; 查看func表的结构

UPDATE 修改

* alter table student reanme to stu; 修改表名
* alter table student character set utf8; 修改字符集
* alter table student add 列名 数据类型； 添加列
* alter table student change 列名 新列名 新数据库类型； 修改列名，数据类型

  alter table student modify 列名 新数据类型； 修改某列数据类型

* alter table student drop 列名; 删除列

DELETE 删除

* drop table test; 删除表test
* drop table test if exists; 如果表test存在，删除

**DML**

增删改表中的数据

* 添加数据

  insert into 表名\(列名1,列名2\) values\(值1,值2\)

* 删除数据

  delete from 表名 where 条件

* 删除所有数据

  delete from 表名 但是不推荐使用这个，效率低

  truncate table student 删除表并创建一个一模一样的空表

* update 表名 set 列名1=值1，列名2=值2 where 条件

**DQL**

基础查询 `from`

```sql
SELECT
    1; -- 测试数据库连接

SELECT
    *  -- 查看students表的所有内容
FROM
    students; 

SELECT 
    name -- 查看classes表的name列
FROM
    classes; 

SELECT
    score points  -- 查询时列可以起别名
FROM
    students; 

SELECT DISTINCT -- distinct 去除重复数据
    gender 
FROM
    students; 

SELECT NAME
    id,
    score,
    id + IFNULL( score, 0 ) myscore  -- 起别名，IFNULL（如果是Null，就替换为0）
FROM
    students;
```

条件查询 `where`…`and`…`or`…`in (_,_)`…`between and` `like`

```sql
SELECT
    *  -- 所有列
FROM
    students -- 
WHERE
    score >= 80;  -- 成绩大于等于80
```

```sql
SELECT
    * 
FROM
    students 
WHERE
    gender = 'B'; -- 性别为男的所有学生信息
```

```sql
SELECT
    * 
FROM
    students 
WHERE
    class_id <> 2;  -- class_id 不等于2的所有学生信息
```

```sql
SELECT
    * 
FROM
    students 
WHERE
    score >= 80 
    AND gender = 'G'; -- 成绩大于等于80的女生
```

```sql
SELECT
    * 
FROM
    students 
WHERE
    score >= 80 
    OR gender = 'B';  -- 成绩大于等于80的学生或者男生
```

```sql
SELECT 
    name  -- 姓名
FROM
    students 
WHERE
    ( score < 80 OR score > 90 ) 
    AND gender = 'B';  -- 成绩在80到90中之间的男生的名字
```

```sql
SELECT 
    *
FROM
    students 
WHERE
    score IN(88,90,92);  -- 成绩是是88或90或92的学生
```

```sql
SELECT
    * 
FROM
    students 
WHERE
    score LIKE '9%'; -- 成绩以 9 开头
```

```sql
SELECT
    * 
FROM
    students 
WHERE
    name LIKE '小_'; -- 名字以小开头且名字为两个字
```

```sql
SELECT
    * 
FROM
    students 
WHERE
    name NOT LIKE '小%'; -- 姓名不以 小 开头
```

排序 `order by` `desc`

```sql
SELECT
    * 
FROM
    students 
ORDER BY
    score;  -- 默认升序排序
```

```sql
SELECT
    * 
FROM
    students 
ORDER BY
    score DESC;  -- 降序排序
```

```sql
SELECT
    * 
FROM
    students 
ORDER BY
    class_id,    -- 先根据class_id升序排序
    score DESC;  -- 再根据score降序排序
```

```sql
SELECT
    id,     -- 学号
    NAME,   -- 姓名    
    gender, -- 性别
    score   -- 成绩
FROM
    students 
WHERE
    class_id = 1  -- 一班
ORDER BY
    score;  成绩排序
```

分页查询 `limit _ offset _`

```sql
SELECT
    * 
FROM
    students 
ORDER BY
    class_id,
    score 
    LIMIT 0,3; -- 每页3条数据，查询第1页
```

聚合查询 `count(*)` `sum` `avg` `max` `min` `group by`

```sql
SELECT
    count(*) num  -- 查询行数  重命名为num
FROM
    students;
```

```sql
SELECT
    count(*) num 
FROM
    students 
WHERE
    gender = 'B';  -- 男生的人数
```

```sql
SELECT
    avg( score ) average -- 查询平均数  重命名为average
FROM
    students 
WHERE
    gender = 'G';-- 女生的平均成绩
```

```sql
SELECT
    class_id,
    count(*) num 
FROM
    students 
GROUP BY
    class_id;-- 各班的人数
```

```sql
SELECT
    class_id,
    gender,
    count(*) num 
FROM
    students 
GROUP BY
    class_id,
    gender;  -- 各班男生女生的人数
```

```sql
SELECT
    class_id,
    avg( score ) average 
FROM
    students 
GROUP BY
    class_id;  -- 各班的平均分
```

```sql
SELECT
    SUM(score)
FROM
    students 
GROUP BY
    class_id;   -- 各班的总分
```

```sql
SELECT
    MAX(score)  -- 成绩的最大值
FROM
    students
```

```sql
SELECT
    MIN(score)  -- 成绩的最小值
FROM
    students
```

约束

* 对表中的数据进行限定，保证数据的准确性，完整性，有效性
* 主键约束：primary key

  一张表只能有一个字段为主键

  非空且唯一

  表中记录的唯一标识

  ```sql
  CREATE TABLE stu (
      id INT PRIMARY KEY, -- 添加主键
      NAME VARCHAR
  );

  ALTER TABLE stu DROP PRIMARY KEY; -- 删除主键

  ALTER TABLE stu MODIFY id INT  PRIMARY KEY;  -- 创建表之后添加主键约束
  ```

  自动增长 auto\_increment

  ```sql
  CREATE TABLE stu ( id INT PRIMARY KEY AUTO_INCREMENT, NAME VARCHAR ( 20 ) );
  ```

* 非空约束：not null

  ```sql
  CREATE TABLE stu ( id INT, NAME VARCHAR ( 20 ) NOT NULL ); -- 创建表时添加非空约束

  ALTER TABLE stu MODIFY NAME VARCHAR ( 20 ) NOT NULL;  -- 创建表之后添加非空约束

  ALTER TABLE stu MODIFY NAME VARCHAR ( 20 );  -- 删除非空约束
  ```

* 唯一约束：unique

  ```sql
  CREATE TABLE stu ( id INT, phone_number VARCHAR ( 11 ) UNIQUE ); -- 创建表时添加唯一约束

  ALTER TABLE stu MODIFY phone_number VARCHAR ( 11 ) UNIQUE; -- 创建表之后添加唯一约束

  ALTER TABLE stu DROP INDEX phone_number -- 删除唯一约束
  ```

* 外键约束：foreign key

  ```sql
  CREATE TABLE emp( -- 创建emp表
      id INT PRIMARY KEY AUTO_INCREMENT,
      NAME VARCHAR(30),
      age INT,
      dep_name VARCHAR(30), -- 部门名称
      dep_location VARCHAR(30) -- 部门所在地
  );

  INSERT INtO emp (NAME,age,dep_name,dep_location) VALUES ("张三",20,"研发部","广州");
  INSERT INtO emp (NAME,age,dep_name,dep_location) VALUES ("李四",21,"研发部","广州");
  INSERT INtO emp (NAME,age,dep_name,dep_location) VALUES ("王五",20,"研发部","广州");
  INSERT INtO emp (NAME,age,dep_name,dep_location) VALUES ("老王",20,"销售部","深圳");
  INSERT INtO emp (NAME,age,dep_name,dep_location) VALUES ("大王",22,"销售部","深圳");
  INSERT INtO emp (NAME,age,dep_name,dep_location) VALUES ("小王",18,"销售部","深圳");

  -- 发现数据有冗余

  SELECT * FROM emp;
  ```

  ```sql
  CONSTRAINT emp_dept_fk FOREIGN KEY (dep_id) REFERENCES department(id) -- 添加外键

  ALTER TABLE employee DROP FOREIGN KEY emp_dept_fk; -- 删除外键

  ON UPDATE CASCADE  -- 级联更新
  ON DELETE CASCADE  -- 级联删除  -- 谨慎使用
  ```

* 数据库设计

  一点一

  一对多，多对一

  多对多

  范式 要求满足第三范式

多表查询

```sql
select * from students，classes;   //返回列数为两个表列数乘积
```

连接查询 `inner join` `right outer join` `left outer join` `full outer join`

```sql
SELECT
    s.id,
    s.name,
    c.name class,
    s.gender,
    s.score 
FROM
    students s
    INNER JOIN classes c ON s.class_id = c.id;
```

**DCL**

进行授权，创建等等

**数据库的备份和还原**

备份

mysqldump -u用户名 -p密码 数据库名 &gt; 保存的路径

还原

source 文件路径

**事务**

要么同时成功，要么同时失败

