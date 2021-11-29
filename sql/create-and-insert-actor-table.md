# Create, Insert and Alter Actor Table

1. Link

{% embed url="https://www.nowcoder.com/activity/oj?tab=1" %}

{% embed url="https://www.runoob.com/mysql/mysql-delete-query.html" %}



1. **Create ， insert,  alter,  delete  table**&#x20;
   1.  创建一个actor表，包含如下列信息

       ```sql
       CREATE TABLE IF NOT EXISTS actor (
       ACTOR_ID  SMALLINT(5)    not Null PRIMARY KEY,  # set primary key
       FIRST_NAME VARCHAR(45)    NOT NULL,
       LAST_NAME VARCHAR(45)     NOT NULL,
       LAST_UPDATE DATETIME          NOT NULL
       );
       ```

       | 列表           | 类型          | 是否为NULL  | 含义   |
       | ------------ | ----------- | -------- | ---- |
       | actor\_id    | smallint(5) | not null | 主键id |
       | first\_name  | varchar(45) | not null | 名字   |
       | last\_name   | varchar(45) | not null | 姓氏   |
       | last\_update | date        | not null | 日期   |
   2.  **批量插入数据**

       ```sql
       INSERT INTO actor (actor_id,
                         first_name,
                         last_name,
                         last_update) 
       VALUES  (1, 'PENELOPE', 'GUINESS', '2006-2-15 12:34:33'),
               (2, 'NICK', 'WAHLBERG', '2006-2-15 12:34:33');
       ```
   3.  请你创建一个actor\_name表，并且**将actor表中的所有first\_name以及last\_name导入该表**. actor\_name表结构如下：

       | 列表          | 类型          | 是否为NULL  | 含义 |
       | ----------- | ----------- | -------- | -- |
       | first\_name | varchar(45) | not null | 名字 |
       | last\_name  | varchar(45) | not null | 姓氏 |

       ```sql
       CREATE TABLE IF NOT EXISTS actor_name (
       first_name varchar(45)    not null,
       last_name  varchar(45)    not null
       );

       insert into actor_name
        (select a.first_name, a.last_name
              from actor a);
       ```

       1. **Note:**
          1. **insert  into 如果 table的key已经有了会报错**
          2. **replace into 把已经有的key的数据进行替换，如果没有重复的key就会和insert into一样**
          3. **insert ignore into: 对于已经有的key，会直接忽略，不会更新**
   4.  **修改 table： 关键词'add', 'drop'**

       ```sql
       # Add new Column
       alter table table_name add new_column_name datetime not null default('0000-00-00 00:00:00');       

       # example:
       alter table actor add create_date datetime not null default('0000-00-00 00:00:00');       


       # Delete Column
       alter table table_name drop column_name;  
       # example:
       alter table actor drop create_date;
       ```
   5. **删除table 记录:   delete from table\_name  where**
      1. 直接  delete from table\_name :  删除所有数据
      2.  选择性删除：  （delete和 query不能同时用，应该先query，再delete）

          ```sql
          delete from table_name where column_name in (
          select * from (
          #sub-query...
          )

          );


          # example:
          # 删除emp_no重复的记录，只保留最小的id对应的记录。
          delete from titles_test
          where id not in (
          select * from(
          select min(id)
              from titles_test
              group by emp_no
          ) a

          );
          ```
   6.
2.  Note

    1. SQL 对table的大小写敏感，但对table里面的column name大小写不敏感
    2. &#x20;Insert 数据时，如果是直接用row 数据输入要加 table的column list以及 values (row1), (row2) ...;   但如果是从已有的table里面导入数据， 直接insert into table\_name  (select \* from table ..);
    3. 关于 insert into 和replace into区别
       1. **insert  into 如果 table的key已经有了会报错**
       2. **replace into 把已经有的key的数据进行替换，如果没有重复的key就会和insert into一样**
       3. **insert ignore into: 对于已经有的key，会直接忽略，不会更新**

    \


