# Mysql数据库

## 一、数据库相关概念

1. 持久化：把数据保存到可掉电存储设备中以供日后使用
2. DB：数据库，即存储数据的仓库，保存了一系列有组织的数据
3. DBMS：数据库管理系统，是一种操作和管理数据库的大型软件，用于建立、使用和维护数据库，对数据库进行统一管理和控制。
4. SQL：结构化查询语言，专门用来与数据库通信的语言
4. MySQL是关系型数据库管理系统

## 二、RDBMS与非RDBMS

1. RDBMS：关系型数据库，实质就是把复杂的数据结构归结为简单的二元关系（即二维表格形式），关系型数据库以行和列的形式存储数据

   优势：可以使用SQL语句进行复杂的查询，同时也支持事务，提高了安全性

2. 非RDBMS：非关系型数据库，基于键值对存储数据，不需要经过SQL层解析，性能非常高。

## 三、关系型数据库设计规则

### 3.1表，记录，字段

* E-R(entity-relationshil，实体-联系)模型中三个主要概念：实体集、属性、联系集
* 一个实体集对应一个表。一个实体对应数据库表中的一行，即一条记录。一个属性对应数据库表中的一列，即字段

ORM(Object Relationl Mapping)思想:对象关系映射

数据库中一个表  -----  java中的一个类

表中一条数据     -----  类中一个对象

表中一列            -----  类中一个字段、属性

### 3.2 表的关联关系

四种关系：一对一、一对多、多对多、自我引用

1. 一对一关系

   举例：设计学生表：学号、姓名、手机号码等

   拆分为两个表，两个表的记录时一一对应的关系

   两种建表原则：外键用于保持数据的一致性

   * 外键唯一：主表的主键和从表的外键，形成主外表关系，外键唯一
   * 外键是主键：主表的主键和从表的主键，形成主外键的关系

2. 一对多关系

   一对多建表原则：在从表(多方)创建一个字段，字段作为外键指向主表的主键

   ![](https://picture-1310712259.cos.ap-nanjing.myqcloud.com/%E4%B8%80%E5%AF%B9%E5%A4%9A.png)

3. 多对多的关系

   要表示多对多的关系，必须创建第三个表，该表通常称为联接表，他将多对多的关系划分为两个一对多的关系，这两个表的主键都插入到第三个表中

   ![](https://picture-1310712259.cos.ap-nanjing.myqcloud.com/%E5%A4%9A%E5%AF%B9%E5%A4%9A.png)

4. 自我引用

   ![](https://picture-1310712259.cos.ap-nanjing.myqcloud.com/%E8%87%AA%E6%88%91%E5%BC%95%E7%94%A8.png)

## 四、基本的Select语句

### 1、SQL分类

* DDL(Data Definition Languages)：数据定义语言，这些语句定义数据库、表、视图、索引，可以用来创建、删除、修改数据库和数据表的结构，主要关键字create、drop、alter
* DML(Data Manipulation Language)：数据操作语言，用于增删改查，主要关键字insert、delete、update、select
* DCL(Data Control Language)：数据控制语言，用于定义数据库、表、字段、用户的访问权限和安全级别，关键字grant、revoke、commit

### 2、SQL语言规范

* 每条命令以；或  \g 或 \G 结束
* 关键字不可缩写和分行
* 标点符号成对出现

注意：MySQL在windows环境下大小写不敏感，在Linux环境下大小写敏感

统一书写规范：

* 数据库名、表名、表别名、字段名、字段别名等都小写
* SQL关键字、函数名、绑定变量等都大写
* 当字段名与关键字名冲突时(尽量避免)，使用着重号引起来  `

命名规则：

* 只能包含A-Z，a - z, 0 - 9，_共63个字符
* 数据库名、表名、字段名等对象名中间不要包含空格

### 3、注释

单行注释：#注释文字(MySQL特有的方式) 

单行注释：-- 注释文字(--后面必须包含一个空格。) 

多行注释：/* 注释文字 */ 

### 4、Select语法

#### 4.1 Select

```sql
SELECT 1; #没有任何子句 
SELECT 9/2; #没有任何子句
```

![](https://picture-1310712259.cos.ap-nanjing.myqcloud.com/1.png)

#### 4.2 SELECT .... FROM 

* 语法

  ```sql
  SELECT 标识选择哪些列 
  FROM 标识从哪个表中选择 
  ```

* 选择全部列

  ```sql
  SELECT * 
  FROM departments;
  ```

  ![](https://picture-1310712259.cos.ap-nanjing.myqcloud.com/2.png)

* 选择特定列

  ```sql
  SELECT department_id, location_id 
  FROM departments;
  ```

#### 4.2 列的别名

1. 作用：重命名一个列便于计算

2. 实现方式：紧跟列名，或者在列名和别名之间加入关键字AS，AS可以省略。别名使用双引号，可以在别名中包含空格或特殊字符并区分大小写

3. 举例

   ```sql
   SELECT last_name AS name, commission_pct comm 
   FROM employees;
   ```

   ![](https://picture-1310712259.cos.ap-nanjing.myqcloud.com/3.png)

   ```sql
   SELECT last_name "Name", salary*12 "Annual Salary" 
   FROM employees;
   ```

   ![](https://picture-1310712259.cos.ap-nanjing.myqcloud.com/4.png)

#### 4.3 去除重复行

默认情况下，查询会返回全部行，包括重复行

在SELECT语句中使用关键字DISTINCE去除重复行

```sql
SELECT DISTINCT department_id 
FROM employees;
```

针对于：

```sql
SELECT DISTINCT department_id,salary 
FROM employees;
```

有两点注意：

1. DISTINCT 需要放到所有列名的前面，如果写成 SELECT salary, DISTINCT department_id FROM employees 会报错
2. DISTINCT 其实是对后面所有列名的组合进行去重

#### 4.4 空值参与运算

所有运算符或列值遇到null值，运算的结果都为null

```sql
SELECT employee_id,salary,commission_pct, 12 * salary * (1 + commission_pct) "annual_sal" 
FROM employees;
```

注意，在 MySQL 里面， 空值不等于空字符串。一个空字符串的长度是 0，而一个空值的长度是空。而且，在 MySQL 里面，空值是占用空间的。

#### 4.5着重号

我们需要保证表中的字段、表名等没有和保留字、数据库系统或常用方法冲突。如果真的相同，在SQL语句中使用一对``（着重号）引起来。

```sql
SELECT * FROM `ORDER`
```

#### 4.6 查询常数

SELECT 查询还可以对常数进行查询。对的，就是在 SELECT 查询结果中增加一列固定的常数列。这列的取值是我们指定的，而不是从数据表中动态取出的。

比如说，我们想对 employees 数据表中的员工姓名进行查询，同时增加一列字段 corporation ，这个字段固定值为“尚硅谷”，可以这样写：

```sql
SELECT '尚硅谷' as corporation, last_name FROM employees;
```

则列名为corporation，里面的值全为尚硅谷

#### 4.7 显示表结构

使用DESCRIBE 或 DESC 命令，表示表结构

```sql
DESCRIBE employees; 
或
DESC employees;
```

![](https://picture-1310712259.cos.ap-nanjing.myqcloud.com/5.png)

其中，各个字段的含义分别解释如下：

Field：表示字段名称。

Type：表示字段类型，这里 barcode、goodsname 是文本型的，price 是整数类型的。

Null：表示该列是否可以存储NULL值。

Key：表示该列是否已编制索引。PRI表示该列是表主键的一部分；UNI表示该列是UNIQUE索引的一

部分；MUL表示在列中某个给定值允许出现多次。

Default：表示该列是否有默认值，如果有，那么值是多少。

Extra：表示可以获取的与给定列有关的附加信息，例如AUTO_INCREMENT等。

### 5. 过滤数据

语法

```sql
SELECT 字段1,字段2 
FROM 表名 
WHERE 过滤条件
```

* 使用where子句，将不满足条件的行过滤掉
* **where子句紧随from子句**

```sql
SELECT employee_id, last_name, job_id, department_id 
FROM employees 
WHERE department_id = 90 ;
```

![](https://picture-1310712259.cos.ap-nanjing.myqcloud.com/6.png)



111
