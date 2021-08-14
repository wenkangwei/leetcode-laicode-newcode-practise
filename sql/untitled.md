---
description: practice window function and case when.
---

# Manage Employment Data -2

## Link

{% embed url="https://www.nowcoder.com/practice/c39cbfbd111a4d92b221acec1c7c1484?tpId=82&&tqId=29825&rp=1&ru=/activity/oj&qru=/ta/sql/question-ranking" %}

## Create Table

```sql
# question 1
CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));
CREATE TABLE `dept_emp` (
`emp_no` int(11) NOT NULL,
`dept_no` char(4) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`dept_no`));



# Question 2
CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));
CREATE TABLE `dept_emp` (
`emp_no` int(11) NOT NULL,
`dept_no` char(4) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`dept_no`));
create table emp_bonus(
emp_no int not null,
received datetime not null,
btype smallint not null);
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL, PRIMARY KEY (`emp_no`,`from_date`));


# Question 3
CREATE TABLE `salaries` ( `emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));

#Question 4

CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));
如，输入为：
INSERT INTO employees VALUES(10001,'1953-09-02','Georgi','Facello','M','1986-06-26');
INSERT INTO employees VALUES(10002,'1964-06-02','Bezalel','Simmel','F','1985-11-21');
INSERT INTO employees VALUES(10005,'1955-01-21','Kyoichi','Maliniak','M','1989-09-12');
INSERT INTO employees VALUES(10006,'1953-04-20','Anneke','Preusig','F','1989-06-02');
```



## Questions

1. 使用含有关键字exists查找未分配具体部门的员工的所有信息。

```sql
select *
from employees as e
where not exists (
select d.emp_no
    from dept_emp as d 
    where d.emp_no = e.emp_no

);

# without using exists keyword
select * 
from employees as e
where e.emp_no not in (
select d.emp_no
     from dept_emp as d
);

```



2. SQL59 获取有奖金的员工相关信息

给出emp\_no、first\_name、last\_name、奖金类型btype、对应的当前薪水情况salary以及奖金金额bonus。 bonus类型btype为1其奖金为薪水salary的10%，btype为2其奖金为薪水的20%，其他类型均为薪水的30%。 当前薪水表示to\_date='9999-01-01'

```sql

select e.emp_no, e.first_name,
        e.last_name, b.btype, 
        s.salary, case when b.btype=1 then s.salary*0.1
                   when b.btype=2 then s.salary*0.2
                  else s.salary * 0.3  end  bonus
from emp_bonus as b 
left join employees as e on b.emp_no = e.emp_no
left join salaries as s on b.emp_no = s.emp_no
where s.to_date = "9999-01-01";

```



3. 按照salary的累计和running\_total，其中running\_total为前N个当前\( to\_date = '9999-01-01'\)员工的salary累计和，其他以此类推。 具体结果如下Demo展示。

Note:  在 Window function 里面如果  func\(column\) over  \(...\),  里面 **over \(\) 括号里面没有partition by 对row进行分区操作， 那么 window func在某一行row的操作是把当前这个row（包括当前的row）的前面所有row的值进行操作。 比如 window function是sum，那么在第10行的window function的输出就把1~10行的值给加起来**

```sql

select s.emp_no, s.salary, sum(s.salary) over (order by s.emp_no) as running_total
from salaries as s
where s.to_date = "9999-01-01"
;
```



4. 对于employees表中，输出first\_name排名\(按first\_name升序排序\)为奇数的first\_name

```sql
select a.first_name
from  employees e left join
(
select e.first_name, ROW_NUMBER() over (order by e.first_name ) as r_number
from employees as e
) as a
on e.first_name = a.first_name
where a.r_number % 2 = 1
;

```



## Conclusion

1. 这里回顾了Window Function用法
   1. 在window function里面  func\(..\) over \(..\)  如果over\(\)里面没有指定partition by进行分组， 那么window function 会对当前的row之前的所有row\(包括当前的row\) 进行window function的操作。 比如下面 sum salary 里面没有用partition by， 那么它就会第 i 行的running——total的值等于前面 1~ i行的salary的sum

      ```sql
      select s.emp_no, s.salary, sum(s.salary) over (order by s.emp_no) as running_total
      from salaries as s
      where s.to_date = "9999-01-01"
      ;
      ```
2. Case when的用法回顾

   ```sql
   select  case when condition_a then do_a
   case when condition_b then do_b
   ...
   else default_value end  column_alias_name

   from ...;
   ```



```sql

```









