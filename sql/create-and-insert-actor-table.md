# Create and Insert Actor Table

1. Link
2. Create table 

   1. 创建一个actor表，包含如下列信息

      ```sql
      CREATE TABLE IF NOT EXISTS actor (
      ACTOR_ID  SMALLINT(5)    not Null PRIMARY KEY,  # set primary key
      FIRST_NAME VARCHAR(45)    NOT NULL,
      LAST_NAME VARCHAR(45)     NOT NULL,
      LAST_UPDATE DATETIME          NOT NULL
      );
      ```

      | 列表 | 类型 | 是否为NULL | 含义 |
      | :--- | :--- | :--- | :--- |
      | actor\_id | smallint\(5\) | not null | 主键id |
      | first\_name | varchar\(45\) | not null | 名字 |
      | last\_name | varchar\(45\) | not null | 姓氏 |
      | last\_update | date | not null | 日期 |

   2. 批量插入数据

      ```sql
      INSERT INTO actor (actor_id,
                        first_name,
                        last_name,
                        last_update) 
      VALUES  (1, 'PENELOPE', 'GUINESS', '2006-2-15 12:34:33'),
              (2, 'NICK', 'WAHLBERG', '2006-2-15 12:34:33');
      ```

   3. 请你创建一个actor\_name表，并且将actor表中的所有first\_name以及last\_name导入该表. actor\_name表结构如下：

      | 列表 | 类型 | 是否为NULL | 含义 |
      | :--- | :--- | :--- | :--- |
      | first\_name | varchar\(45\) | not null | 名字 |
      | last\_name | varchar\(45\) | not null | 姓氏 |

      ```sql
      CREATE TABLE IF NOT EXISTS actor_name (
      first_name varchar(45)    not null,
      last_name  varchar(45)    not null
      );

      insert into actor_name
       (select a.first_name, a.last_name
             from actor a);
      ```



   1. kk

3. Note

   1. SQL 对table的大小写敏感，但对table里面的column name大小写不敏感
   2.  Insert 数据时，如果是直接用row 数据输入要加 table的column list以及 values \(row1\), \(row2\) ...;   但如果是从已有的table里面导入数据， 直接insert into table\_name  \(select \* from table ..\); 

```sql
CREATE TABLE IF NOT EXISTS actor_name (
first_name varchar(45)    not null,
last_name  varchar(45)    not null
);

insert into actor_name
 (select a.first_name, a.last_name
       from actor a);
```



