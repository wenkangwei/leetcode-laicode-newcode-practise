---
description: practice update, alter, replace, insert keywords.
---

# Manage Employment Data

## Link&#x20;

{% embed url="https://www.nowcoder.com/practice/2bec4d94f525458ca3d0ebf3bc8cd240?tpId=82&&tqId=29812&rp=1&ru=/activity/oj&qru=/ta/sql/question-ranking" %}

## Create Table and insert Data



```sql
CREATE TABLE titles_test (
   id int(11) not null primary key,
   emp_no  int(11) NOT NULL,
   title  varchar(50) NOT NULL,
   from_date  date NOT NULL,
   to_date  date DEFAULT NULL);

insert into titles_test values
('1', '10001', 'Senior Engineer', '1986-06-26', '9999-01-01'),
('2', '10002', 'Staff', '1996-08-03', '9999-01-01'),
('3', '10003', 'Senior Engineer', '1995-12-03', '9999-01-01'),
('4', '10004', 'Senior Engineer', '1995-12-03', '9999-01-01'),
('5', '10001', 'Senior Engineer', '1986-06-26', '9999-01-01'),
('6', '10002', 'Staff', '1996-08-03', '9999-01-01'),
('7', '10003', 'Senior Engineer', '1995-12-03', '9999-01-01');


CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));
```

## Questions&#x20;

1.  &#x20;将id=5以及emp\_no=10001的行数据替换成id=5以及emp\_no=10005,其他数据保持不变，**使用replace实现，直接使用update会报错**。（Using Update）

    ```sql
    update titles_test set emp_no = REPLACE(emp_no, 10001, 10005) where id=5
    ```
2. 将titles\_test表名修改为titles\_2017
   1.  alter  table table-name operation-name to ....

       ```sql
       alter table titles_test rename to titles_2017;
       ```
3.  将所有获取奖金的员工当前的薪水增加10%

    ```sql
    update salaries
    set salary = 1.1 * salary
    where to_date = '9999-01-01' and emp_no in (select emp_no
    from bouns
    );

    ```
4.  获取Employees中的first\_name，**查询按照first\_name最后两个字母**，**按照升序进**行排列

    ```sql
    select first_name 
    from employees
    order by right(first_name,2);
    ```
5.  按照dept\_no进行汇总，属于同一个部门的emp\_no按照逗号进行连接，结果给出dept\_no以及连接出的结果, 预期结果:

    ```sql
    CREATE TABLE `salaries` ( `emp_no` int(11) NOT NULL,
    `salary` int(11) NOT NULL,
    `from_date` date NOT NULL,
    `to_date` date NOT NULL,
    PRIMARY KEY (`emp_no`,`from_date`));
    如：
    INSERT INTO salaries VALUES(10001,85097,'2001-06-22','2002-06-22');
    INSERT INTO salaries VALUES(10001,88958,'2002-06-22','9999-01-01');
    INSERT INTO salaries VALUES(10002,72527,'2001-08-02','9999-01-01');
    INSERT INTO salaries VALUES(10003,43699,'2000-12-01','2001-12-01');
    INSERT INTO salaries VALUES(10003,43311,'2001-12-01','9999-01-01');
    INSERT INTO salaries VALUES(10004,70698,'2000-11-27','2001-11-27');
    INSERT INTO salaries VALUES(10004,74057,'2001-11-27','9999-01-01');



    select avg(s1.salary) as avg_salary
    from salaries s1
    where s1.to_date ='9999-01-01' 
    and s1.salary not in (select max(s.salary)
        from salaries s
        where s.to_date ='9999-01-01'
    )
    and s1.salary not in (
    select min(s.salary)
        from salaries s
        where s.to_date ='9999-01-01'
    )
    ;
    ;
    ```
6.
7.  按照dept\_no进行汇总，属于同一个部门的emp\_no按照逗号进行连接，结果给出dept\_no以及连接出的结果, 预期结果:

    | dept\_no | employees         |
    | -------- | ----------------- |
    | d001     | 10001,10002       |
    | d002     | 10006             |
    | d003     | 10005             |
    | d004     | 10003,10004       |
    | d005     | 10007,10008,10010 |
    | d006     | 10009,10010       |

    ```sql
    select d.dept_no, group_concat(d.emp_no )  as employees
    from dept_emp d
    group by d.dept_no;
    ```

## Conclusion

1.  更新table row的数据： update

    ```sql
    update table_name set  column_name = ... where col_name = ...;
    ```
2.  更新table属性: alter

    ```sql
    # add column
    alter table  table_name add col_name, col_type;
    # drop column
    ALTER TABLE table_name DROP COLUMN column_name;
    # change column type
    ALTER TABLE table_name ALTER COLUMN column_name TYPE datatype;
    ```
3.  SQL里面的group-concat() 函数相对于Spark里面的collect-list() 函数，把groupby分组后的列表里面的column的list聚集到一行里面

    ```sql
    select d.dept_no, group_concat(d.emp_no )  as employees
    from dept_emp d
    group by d.dept_no;
    ```

```sql
```
