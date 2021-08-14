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



```sql
select s.emp_no, s.salary, sum(s.salary) over (order by s.emp_no) as running_total
from salaries as s
where s.to_date = "9999-01-01"
;
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









