# MySQL



# 

### 1. 编写一个 SQL 查询，来删除 `Person` 表中所有重复的电子邮箱，重复的邮箱里只保留 **Id** *最小* 的那个。[链接](https://leetcode-cn.com/problems/delete-duplicate-emails/)

| id   | email            |
| ---- | ---------------- |
| 1    | john@example.com |
| 2    | bob@example.com  |
| 3    | john@example.com |

Id 是这个表的主键。

例如，在运行你的查询语句之后，上面的 Person 表应返回以下几行:

| id   | email            |
| ---- | ---------------- |
| 1    | john@example.com |
| 2    | bob@example.com  |

我的解法1：

~~~mysql
 delete from Person where id=
 (select id from 
  (
     select max(id) as id from Person where email in
     (
         select email from Person group by email having count(email)>1
     )
  ) as a
 );
~~~

我的思路是这样的：

- ~~~mysql
   select email from Person group by email having count(email)>1)
  ~~~

  首先按照email进行分组，挑选出重复的email，即通过count(email)>1判断->**这一方法适用于挑选出表中重复的元素**

- 然后在重复的eamil中找到id最大的那一行，进行删除即可

- 注意：**MySQL每一个派生出来的表都必须有一个自己的别名**



但是该方法碰到有三个或三个以上的重复email时就会失效。因为在删除的时候值删除了重复email中最大id的那一行，而中间大的没有被删除。

那我们换一个思路：能不能之间将重复email中最小id的那一行挑选出来，就行保留，其余的删除即可。这就是解法2：



~~~mysql
delete from Person where id not in 
(select id from
 		(
            select min(id) as id  ,email from Person group by email
        ) as t
);
~~~

思路：

- ~~~mysql
  select min(id) as id  ,email from Person group by email
  ~~~

  按照email进行分组，如果email是重复的，则只保留id最小的那一行，如果email是不重复的，即id最小的那一行就是它本身。

- 如果id不在派生出来的表中，则将对应行进行删除。



### 2. 编写一个 SQL 查询，查找 `Person` 表中所有重复的电子邮箱。

~~~mysql
select Email from Person group by Email having count(Email)>1;
~~~



### 3.编写一个 SQL 查询，获取 `Employee` 表中第二高的薪水（Salary） 。

解法1

利用自查询的特性：

子查询可以在任意地方使用。相当于查询的列。在oracle中只写查询列没有查询表需要加上 from dual（伪表）。而mysql不用写

~~~mysql
select (select distinct salary from employee order by salary desc limit 1,1) as second;
~~~



解法2：

~~~mysql
select ifnull((select distinct salary from employee order by salary desc limit 1,1),null) as second;
~~~





IFNULL(expression, alt_value)
**如果第一个参数的表达式 expression 为 NULL，则返回第二个参数的备用值(此题中是返回null)**。
expression是table的时候要加括号

distinct：
去重一样的Salary

limit：限时返回的个数
offset：跳过几个
limit 1 offset 1:返回一个结果，跳过一个
例如返回第三高就是：limit 1 offset 2



### 某网站包含两个表，`Customers` 表和 `Orders` 表。编写一个 SQL 查询，找出所有从不订购任何东西的客户。

`Customers` 表：

~~~
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+

~~~

`Orders` 表：	

~~~mysql
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+

~~~

~~~mysql
select Name as Customers from Customers where Id not in (select CustomerId as Id from Orders);
~~~









### 多表连接：

```mysql
select 字段列表 from 表1 inner | left |right join 表2 on 条件
```

1）内联结（inner join），取两表的公共数据

2）左联结（left join），联结结果保留左表的全部数据 -> 在inner join的基础上，增加左边表有右边没有的内容 -> 

3）右联结（right join），联结结果保留右表的全部数据





![1.jpg](https://pic.leetcode-cn.com/ad3df1c4ecc7d2dbe85f92cdde8ec9a731fdd20dc4c5629ecb372b21de26c682-1.jpg)



[链接](https://leetcode-cn.com/problems/combine-two-tables/)

~~~mysql
select FirstName, LastName, City, State  from Person left join Address on Person.PersonId=Address.PersonId;
~~~





### 收入超过经理的员工

[链接](https://leetcode-cn.com/problems/employees-earning-more-than-their-managers/)

Employee 表包含所有员工，他们的经理也属于员工。每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。

~~~
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
~~~



给定 Employee 表，编写一个 SQL 查询，该查询可以获取收入超过他们经理的员工的姓名。在上面的表格中，Joe 是唯一一个收入超过他的经理的员工。

~~~
+----------+
| Employee |
+----------+
| Joe      |
+----------+
~~~

解法1：

~~~mysql
select name as Employee from 
(
    select * from Employee where managerid is not null #挑选出managerid不为null的行
) as t
    where salary>  #进行比较
(
    select salary from Employee where id=t.managerid
);
~~~

解法2：

~~~mysql
SELECT
    a.Name AS Employee
FROM
    Employee AS a,
    Employee AS b
WHERE
    a.ManagerId = b.Id
        AND a.Salary > b.Salary
;
~~~





### 编写一个 SQL 查询，来查找与之前（昨天的）日期相比温度更高的所有日期的 `id` 。

返回结果 **不要求顺序** 。

我的解法：

~~~mysql
select id from Weather as w1 
where 
Temperature >(select Temperature from Weather w2 where recordDate=(w1.recordDate-1) ) #挑选出前一天日期的温度
;
~~~

但是运行的时候会出错，原因在：比较两个日期的时候不能做差，而要使用datediff函数。datediff函数返回两个日期的时间差

例如：

~~~mysql
#DATEDIFF(date1,date2)
SELECT DATEDIFF('2008-12-30','2008-12-29')
#返回1,data1-data2
~~~



改正后：

~~~mysql
select id from Weather as w1 
where 
Temperature >(select Temperature from Weather w2 where DATEDIFF(w1.recordDate,w2.recordDate)=1 ) #挑选出前一天日期的温度
;
~~~

或

~~~mysql
SELECT w2.Id
FROM Weather w1, Weather w2
WHERE DATEDIFF(w2.RecordDate, w1.RecordDate) = 1
AND w1.Temperature < w2.Temperature
~~~



### [重新格式化部门表](https://leetcode-cn.com/problems/reformat-department-table/)

原表为

~~~
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+
~~~

通过语句

~~~mysql
SELECT id,
IF(`month`='Jan',revenue,NULL) Jan_Revenue, #对于原表中的第一行，其month=jan,所以该位置返回8000
IF(`month`='Feb',revenue,NULL) Feb_Revenue, #因为month!=feb,返回null
IF(`month`='Mar',revenue,NULL) Mar_Revenue,
IF(`month`='Apr',revenue,NULL) Apr_Revenue,
IF(`month`='May',revenue,NULL) May_Revenue,
IF(`month`='Jun',revenue,NULL) Jun_Revenue,
IF(`month`='Jul',revenue,NULL) Jul_Revenue,
IF(`month`='Aug',revenue,NULL) Aug_Revenue,
IF(`month`='Sep',revenue,NULL) Sep_Revenue,
IF(`month`='Oct',revenue,NULL) Oct_Revenue,
IF(`month`='Nov',revenue,NULL) Nov_Revenue,
IF(`month`='Dec',revenue,NULL) Dec_Revenue
FROM Department;
~~~

或

~~~mysql
SELECT id,
CASE `month` WHEN 'Jan' THEN revenue END Jan_Revenue,
CASE `month` WHEN 'Feb' THEN revenue END Feb_Revenue,
CASE `month` WHEN 'Mar' THEN revenue END Mar_Revenue,
CASE `month` WHEN 'Apr' THEN revenue END Apr_Revenue,
CASE `month` WHEN 'May' THEN revenue END May_Revenue,
CASE `month` WHEN 'Jun' THEN revenue END Jun_Revenue,
CASE `month` WHEN 'Jul' THEN revenue END Jul_Revenue,
CASE `month` WHEN 'Aug' THEN revenue END Aug_Revenue,
CASE `month` WHEN 'Sep' THEN revenue END Sep_Revenue,
CASE `month` WHEN 'Oct' THEN revenue END Oct_Revenue,
CASE `month` WHEN 'Nov' THEN revenue END Nov_Revenue,
CASE `month` WHEN 'Dec' THEN revenue END Dec_Revenue
FROM Department;

~~~



得到虚拟表1,如下所示

~~~
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+

| 1    | 8000        | null        | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
| 2    | 9000        | null        | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | null        | null        | 6000        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | null        | 7000        | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+

~~~

通过group by id语句可以得到得到一个虚拟表2，如下所示。

~~~
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
|      | 8000        | null        | null        | ... | null        |
|  1   | null        | null        | 6000        | ... | null        |
|      | null        | 7000        | null        | ... | null        |
-------+-------------+-------------+-------------------+--------------
| 2    | 9000        | null        | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
~~~

该虚拟表按id分组，其中对于id=1的组中，Jan_Revenue，Feb_Revenue，...，Dec_Revenue等列中有多个数值，因此需要一个聚合函数将多个数值进行整合，输出为一个数值。例如sum或max函数。



因此总的语句为：

~~~mysql
SELECT id,
SUM(CASE `month` WHEN 'Jan' THEN revenue END) Jan_Revenue,
SUM(CASE `month` WHEN 'Feb' THEN revenue END) Feb_Revenue,
SUM(CASE `month` WHEN 'Mar' THEN revenue END) Mar_Revenue,
SUM(CASE `month` WHEN 'Apr' THEN revenue END) Apr_Revenue,
SUM(CASE `month` WHEN 'May' THEN revenue END) May_Revenue,
SUM(CASE `month` WHEN 'Jun' THEN revenue END) Jun_Revenue,
SUM(CASE `month` WHEN 'Jul' THEN revenue END) Jul_Revenue,
SUM(CASE `month` WHEN 'Aug' THEN revenue END) Aug_Revenue,
SUM(CASE `month` WHEN 'Sep' THEN revenue END) Sep_Revenue,
SUM(CASE `month` WHEN 'Oct' THEN revenue END) Oct_Revenue,
SUM(CASE `month` WHEN 'Nov' THEN revenue END) Nov_Revenue,
SUM(CASE `month` WHEN 'Dec' THEN revenue END) Dec_Revenue
FROM Department
GROUP BY id;

~~~

最终的结果：

~~~
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
|  1   | 8000        | 7000        | 6000        | ... | null        |
-------+-------------+-------------+-------------------+--------------
|  2   | 9000        | null        | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
|  3   | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
~~~





### if语句

 在mysql中if()函数的用法类似于java中的三目表达式，其用处也比较多，具体语法如下：

IF(expr1,expr2,expr3)，如果expr1的值为true，则返回expr2的值，如果expr1的值为false，

则返回expr3的值。

### group by语句

[参考](https://blog.csdn.net/u014717572/article/details/80687042)
