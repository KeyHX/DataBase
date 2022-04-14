# Mysql基础篇

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
SELECT '尚硅谷' as corporation, last_name 
FROM employees;
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

## 五、运算符

### 1、算术运算符

+，-，*，/(或DIV)，%(MOD)

1. 对于加减

   一个整数类型的值对整数进行加法和减法操作，结果还是一个整数；

   一个整数类型的值对浮点数进行加法和减法操作，结果是一个浮点数；

   在Java中，+的左右两边如果有字符串，那么表示字符串的拼接。但是在MySQL中+只表示数值相加。如果遇到非数值类型，先尝试转成数值，如果转失败，就按0计算。（补充：MySQL中字符串拼接要使用字符串函数CONCAT()实现）

2. 对于乘除

   一个数除以整数后，不管是否能除尽，结果都为一个浮点数；

   一个数除以另一个数，除不尽时，结果为一个浮点数，并保留到小数点后4位；

   在数学运算中，0不能用作除数，在MySQL中，一个数除以0为NULL。 

### 2、比较运算符

比较运算符用来对表达式左边的操作数和右边的操作数进行比较，**比较的结果为真则返回1，比较的结果为假则返回0**，其他情况则返回NULL。

=，<=>(安全等于)，<>(!=)(不等于)，<，<=，>，>=

1.  =

   * 如果等号两边的值、字符串或表达式都为字符串，则MySQL会按照字符串进行比较，其比较的

     是每个字符串中字符的ANSI编码是否相等。

   * 如果等号两边的值一个是整数，另一个是字符串，则MySQL会将字符串转化为数字进行比较。

   * **如果等号两边的值、字符串或表达式中有一个为NULL，则比较结果为NULL。**

2.  <=>

    安全等于运算符（<=>）与等于运算符（=）的作用是相似的， 唯一的区别 是‘<=>’可以用来对NULL进行判断。在两个操作数均为NULL时，其返回值为1，而不为NULL；当一个操作数为NULL时，其返回值为0，而不为NULL。

3. 不等于运算符

   不等于运算符（<>和!=）用于判断两边的数字、字符串或者表达式的值是否不相等，如果不相等则返回1，相等则返回0。不等于运算符不能判断NULL值。如果两边的值有任意一个为NULL，或两边都为NULL，则结果为NULL。

### 3、非符号类型运算符

![](https://picture-1310712259.cos.ap-nanjing.myqcloud.com/9.png)

对于LIKE运算符

LIKE运算符通常使用如下通配符：

**“%”：匹配0个或多个字符。** 

**“_”：只能匹配一个字符。**

比如：

```sql
SELECT first_name 
FROM employees 
WHERE first_name LIKE 'S%';

SELECT last_name 
FROM employees 
WHERE last_name LIKE '_o%';
```

回避特殊符号的：**使用转义符**。\就是转义字符

```sql
SELECT job_id 
FROM jobs 
WHERE job_id LIKE ‘IT\_%‘;
```

如果使用\表示转义，要省略ESCAPE。如果不是\，则要加上ESCAPE。 

```sql
SELECT job_id 
FROM jobs 
WHERE job_id LIKE ‘IT$_%‘ escape ‘$‘;
#用escape标注 $就是转义字符
```

### 4、逻辑运算符

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/10.png)

**OR可以和AND一起使用，但是在使用时要注意两者的优先级，由于AND的优先级高于OR，因此先对AND两边的操作数进行操作，再与OR中的操作数结合。**

当有null的时候，他们的返回值全是null

### 5、位运算符

位运算符是在二进制数上进行计算的运算符。位运算符会先将操作数变成二进制数，然后进行位运算，最后将计算结果从二进制变回十进制数。

&，|，^，~(取反），>>，<<

按位右移运算符将给定的值的二进制数的所有位右移指定的位数。右移指定的位数后，右边低位的数值被移出并丢弃，左边高位空出的位置用0补齐。

## 六、排序与分页

### 1、排序数据

#### 1.1 排序规则

使用 ORDER BY 子句排序
	ASC（ascend）: 升序       (默认)
	DESC（descend）:降序
**ORDER BY 子句在SELECT语句的结尾。**

#### 1.2 单列排序

```sql
SELECT last_name, job_id, department_id, hire_date
FROM employees
ORDER BY hire_date ;

SELECT last_name, job_id, department_id, hire_date
FROM employees
ORDER BY hire_date DESC ;

SELECT employee_id, last_name, salary*12 annsal
FROM employees
ORDER BY annsal;
```

#### 1.3 多列排序

```sql
SELECT last_name, department_id, salary
FROM employees
ORDER BY department_id, salary DESC;
```

可以使用不在SELECT列表中的列排序。

在对多列进行排序的时候，首先排序的第一列必须有相同的列值，才会对第二列进行排序。如果第
一列数据中所有值都是唯一的，将不再对第二列进行排序。

### 2、分页

#### 2.1 分页原理

所谓分页显示，就是将数据库中的结果集，一段一段显示出来需要的条件。

#### 2.2分页规则

MySQL中使用 LIMIT 实现分页

格式：

```
LIMIT [位置偏移量,] 行数
```

第一个“位置偏移量”参数指示MySQL从哪一行开始显示（位偏移量就是index，开始的第一个索引），是一个可选参数，如果不指定“位置偏移量”，将会从表中的第一条记录开始（第一条记录的位置偏移量是0，第二条记录的位置偏移量是1，以此类推）；第二个参数“行数”指示返回的记录条数。

举例：

```sql
--前10条记录：
SELECT * FROM 表名 LIMIT 0,10;
或者
SELECT * FROM 表名 LIMIT 10;
--第11至20条记录：
SELECT * FROM 表名 LIMIT 10,10;
--第21至30条记录：
SELECT * FROM 表名 LIMIT 20,10;
```

MySQL 8.0中可以使用“LIMIT 3 OFFSET 4”，意思是获取从第5条记录开始后面的3条记录，和“LIMIT 4,3;”返回的结果相同。

分页显式公式：（当前页数-1）*每页条数，每页条数

```sql
SELECT * FROM table
LIMIT(PageNo - 1)*PageSize,PageSize;
```

**注意：LIMIT 子句必须放在整个SELECT语句的最后！**

## 七、多表查询

多表查询的前提条件：这些一起查询的表之间有关系(一对一、一对多)，他们之间有关键的字段

### 1、笛卡尔积(交叉连接)

笛卡尔乘积是一个数学运算。假设我有两个集合 X 和 Y，那么 X 和 Y 的笛卡尔积就是 X 和 Y 的所有可能组合，也就是第一个对象来自于 X，第二个对象来自于 Y 的所有可能。组合的个数XY即为两个集合中元素个数的乘积数。

比如下面就会出现笛卡尔积：

```sql
SELECT last_name, department_name
FROM employees, departments;
```

笛卡尔积也是交叉连接，在SQL99中交叉连接是CROSS JOIN

避免笛卡尔积，可以在where后面加入有效的连接条件

```sql
SELECT table1.column, table2.column
FROM table1, table2
WHERE table1.column1 = table2.column2; #连接条件
```

### 2、多表查询的分类

#### 2.1 等值连接 VS 非等值连接

1. 等值连接

   ![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/3.png)

```sql
SELECT employees.employee_id, employees.last_name,
	 	employees.department_id, departments.department_id,
		departments.location_id
FROM employees, departments
WHERE employees.department_id = departments.department_id;
```

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/4.png)

注意：

* 在多个表中具有相同的列时，必须在列名之前加上表名的前缀

  ```sql
  SELECT employees.last_name, departments.department_name,employees.department_id
  FROM employees, departments
  WHERE employees.department_id = departments.department_id;
  ```

* 使用表的别名可以简化查询，使用别名作为前缀可以提高查询的效率（使用的时候建议加上别名）

  ```sql
  SELECT e.employee_id, e.last_name, e.department_id,
  d.department_id, d.location_id
  FROM employees e , departments d
  WHERE e.department_id = d.department_id;
  ```

  一旦我们使用了表的别名，则在查询字段和过滤条件中只能使用别名，不能出现原有的表名

* 结论：在连接n个表的时候，至少需要n - 1个连接条件

  ```sql
  SELECT employee_id,last_name,department_name,city
  FROM employees e,departments d,locations l
  WHERE e.`department_id` = d.`department_id`
  AND d.location_id = l.location_id;
  ```

  

2. 非等值连接

   ```sql
   SELECT e.last_name, e.salary, j.grade_level
   FROM employees e, job_grades j
   WHERE e.salary BETWEEN j.lowest_sal AND j.highest_sal;
   ```

#### 2.2 自连接 vs 非自连接

1. 自连接

   使用的是同一张表，只是用取别名的方式虚拟称两张表以代表不同的意义

   ```sql
   #练习：查询员工id，员工姓名，管理者的id和姓名
   SELECT emp.employee_id,emp.last_name,mgr.employee_id,mgr.last_name
   FROM employees emp,employees mgr
   WHERE emp.manager_id = mgr.employee_id;
   ```

2. 非自连接

   2.1 节都是非自连接

#### 2.3 内连接 vs 外连接

1. 内连接：合并具有同一列的两个以上的表的行，结果集中不包含一个表与另一个表不匹配的行

   ```sql
   SELECT employee_id,last_name
   FROM employees e,departments d
   WHERE e.`department_id` = d.`department_id`;
   ```

2.  外连接：合并具有同一列的两个以上的表的行，结果集中除了包含一个表与另一个表匹配的行，还查询到左表或右表中不匹配的行

   左外连接：两个表在连接过程中除了返回满足连接条件的行以外还返回左表中不满足条件的行

   右外连接：两个表在连接过程中除了返回满足连接条件的行以外还返回右表中不满足条件的行

   上面的语法是SQL92实现的内连接，MySQL不支持SQL92实现外连接

   主要讲SQL99实现多表查询

   * SQL99语法实现内连接：

   语法：

   ```sql
   SELECT 字段列表
   FROM A表 INNER JOIN B表
   ON 关联条件
   WHERE 等其他子句;
   ```

   举例：

   ```sql
   #SQL99语法实现内连接
   SELECT last_name,department_name
   FROM employees e INNER JOIN departments d #inner可以省略
   ON e.department_id = d.department_id;
   
   SELECT last_name,department_name,city
   FROM employees e JOIN departments d
   ON e.department_id = d.department_id
   JOIN lication l
   ON d.location_id = l.location_id
   ```

   

   * SQL99语法实现外连接

   ```sql
   #查询 所有（关键字，有所有说明是外连接） 员工的last_name,department_name信息
   #左外连接
   #语法：
   SELECT 字段列表
   FROM A表 LEFT OUTER JOIN B表
   ON 关联条件
   WHERE 等其他子句;
   #举例：
   SELECT last_name,department_name
   FROM employees e LEFT OUTER JOIN departments d 
   ON e.department_id = d.department_id;
   
   #右外连接
   #语法：
   SELECT 字段列表
   FROM A表 RIGHT OUTER JOIN B表
   ON 关联条件
   WHERE 等其他子句;
   #举例
   SELECT last_name,department_name
   FROM employees e RIGHT OUTER JOIN departments d 
   ON e.department_id = d.department_id;
   ```

   * 满外连接 = 左右表匹配的数据 + 左表没有匹配到的数据 + 右表没有匹配到的数据

     ```sql
     #满外连接：:mysql不支持full outer join
     SELECT last_name,department_name
     FROM employees e FULL OUTER JOIN departments d 
     ON e.department_id = d.department_id;
     ```

   

### 3、UNION的使用

作用：合并查询结果 利用UNION关键字，可以给出多条SELECT语句，并将它们的结果组合成单个结果集。合并时，两个表对应的列数和数据类型必须相同，并且相互对应。各个SELECT语句之间使用UNION或UNION ALL关键字分隔。

语法格式：

```sql
SELECT column,... FROM table1
UNION [ALL]
SELECT column,... FROM table2
```

UNION  与 UNION ALL 的区别

UNION 操作符返回两个查询的结果集的并集，去除重复记录

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/5.png)



UNION ALL操作符返回两个查询的结果集的并集。对于两个结果集的重复部分，不去重。

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/6.png)

举例：

```sql
#方式1
SELECT * FROM employees WHERE email LIKE '%a%' OR department_id>90;

#方式2
SELECT * FROM employees WHERE email LIKE '%a%'
UNION
SELECT * FROM employees WHERE department_id>90;
```

### 4、7种SQL JOINS的实现

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/7.png)

```sql
#中图：内连接
SELECT employee_id,department_name
FROM employees e INNER JOIN departments d
ON e.department_id = d.department_id;

#左上图：左外连接
SELECT employee_id,department_name
FROM employees e LEFT OUTER JOIN departments d
ON e.department_id = d.department_id;

#右上图：右外连接
SELECT employee_id,department_name
FROM employees e RIGHT OUTER JOIN departments d
ON e.department_id = d.department_id;

#左中图：
SELECT employee_id,department_name,d.department_id 
FROM employees e LEFT OUTER JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;#找B中为null的，但B中没有null的，所以中间部分就没有了

#右中图
SELECT employee_id,department_name
FROM employees e RIGHT OUTER JOIN departments d
ON e.department_id = d.department_id
WHERE e.department_id IS NULL;

#左下图：满外连接
#方式一：左上图 UNION ALL  右中图
SELECT employee_id,department_name
FROM employees e RIGHT OUTER JOIN departments d
ON e.department_id = d.department_id
UNION ALL
SELECT employee_id,department_name
FROM employees e RIGHT OUTER JOIN departments d
ON e.department_id = d.department_id
WHERE e.department_id IS NULL;

#方式二：左中图 UNION ALL  右上图
SELECT employee_id,department_name
FROM employees e LEFT OUTER JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL
UNION ALL
SELECT employee_id,department_name
FROM employees e RIGHT OUTER JOIN departments d
ON e.department_id = d.department_id;

#右下图：左中图 UNION ALL 右中图
SELECT employee_id,department_name
FROM employees e LEFT OUTER JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL
UNION ALL
SELECT employee_id,department_name
FROM employees e RIGHT OUTER JOIN departments d
ON e.department_id = d.department_id
WHERE e.department_id IS NULL;
```

### 5、SQL99语法新特性

#### 5.1自然连接

SQL99 在 SQL92 的基础上提供了一些特殊语法，比如 NATURAL JOIN 用来表示自然连接。我们可以把自然连接理解为SQL92 中的等值连接。**它会帮你自动查询两张连接表中所有相同的字段**，然后进行等值连接。

在SQL92标准中：

```sql
SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
ON e.`department_id` = d.`department_id`
AND e.`manager_id` = d.`manager_id`;
```

在 SQL99 中你可以写成：

```sql
SELECT employee_id,last_name,department_name
FROM employees e NATURAL JOIN departments d;
```

#### 5.2 USING连接

当我们进行连接的时候，SQL99还支持使用 USING 指定数据表里的同名字段进行等值连接。但是只能配合JOIN一起使用。比如：

```sql
SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
USING (department_id);
```



## 八、函数

### 1、函数的分类

MySQL提供的内置函数从实现的功能角度可以分为**数值函数、字符串函数、日期和时间函数、流程控制函数、加密与解密函数、获取MySQL信息函数、聚合函数**等。这里，我将这些丰富的内置函数再分为两类： 单行函数、聚合函数（或分组函数） 。单行函数接受参数返回一个结果，只对一行进行变换，每行返回一个结果，参数可以是一列或一个值。

### 2、数值函数

#### 2.1 基本函数

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/8.png)

#### 2.2 角度与弧度互换函数

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/9.png)

#### 2.3 三角函数

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/11.png)

#### 2.4 指数与对数

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/12.png)

#### 2.5 进制间转换

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/13.png)

### 3、字符串函数

注意：在MySQL种，字符串的位置是从1开始

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/14.png)

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/15.png)

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/16.png)

### 4、日期与时间函数

#### 4.1获取日期、时间

![](https://picture2-1310712259.cos.ap-nanjing.myqcloud.com/17.png)







































