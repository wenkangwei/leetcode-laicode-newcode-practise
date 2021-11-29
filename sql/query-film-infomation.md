# Query Film Infomation

1. Link:

{% embed url="https://www.nowcoder.com/practice/a158fa6e79274ac497832697b4b83658?tpId=82&&tqId=29781&rp=1&ru=/activity/oj&qru=/ta/sql/question-ranking" %}

2\. Tables

```
drop table if exists  film ;
drop table if exists  category  ; 
drop table if exists  film_category  ; 
CREATE TABLE IF NOT EXISTS film (
  film_id smallint(5)  NOT NULL DEFAULT '0',
  title varchar(255) NOT NULL,
  description text,
  PRIMARY KEY (film_id));
CREATE TABLE category  (
   category_id  tinyint(3)  NOT NULL ,
   name  varchar(25) NOT NULL, `last_update` timestamp,
  PRIMARY KEY ( category_id ));
CREATE TABLE film_category  (
   film_id  smallint(5)  NOT NULL,
   category_id  tinyint(3)  NOT NULL, `last_update` timestamp);
INSERT INTO film VALUES(1,'ACADEMY DINOSAUR','A Epic Drama of a Feminist And a Mad Scientist who must Battle a Teacher in The Canadian Rockies');
INSERT INTO film VALUES(2,'ACE GOLDFINGER','A Astounding Epistle of a Database Administrator And a Explorer who must Find a Car in Ancient China');
INSERT INTO film VALUES(3,'ADAPTATION HOLES','A Astounding Reflection of a Lumberjack And a Car who must Sink a Lumberjack in A Baloon Factory');

INSERT INTO category VALUES(1,'Action','2006-02-14 20:46:27');
INSERT INTO category VALUES(2,'Animation','2006-02-14 20:46:27');
INSERT INTO category VALUES(3,'Children','2006-02-14 20:46:27');
INSERT INTO category VALUES(4,'Classics','2006-02-14 20:46:27');
INSERT INTO category VALUES(5,'Comedy','2006-02-14 20:46:27');
INSERT INTO category VALUES(6,'Documentary','2006-02-14 20:46:27');
INSERT INTO category VALUES(7,'Drama','2006-02-14 20:46:27');
INSERT INTO category VALUES(8,'Family','2006-02-14 20:46:27');
INSERT INTO category VALUES(9,'Foreign','2006-02-14 20:46:27');
INSERT INTO category VALUES(10,'Games','2006-02-14 20:46:27');
INSERT INTO category VALUES(11,'Horror','2006-02-14 20:46:27');

INSERT INTO film_category VALUES(1,6,'2006-02-14 21:07:09');
INSERT INTO film_category VALUES(2,11,'2006-02-14 21:07:09');

```

## Questions:

1\. SQL29 使用join查询方式找出没有分类的电影id以及名称

1. Method 1: use   is NULL

```
select f.film_id, f.title
from film  f left join film_category fc on f.film_id = fc.film_id
where fc.category_id is NULL
;
```

2\. Method 1: use   is NULL

```
select film_id ,title
from film
where film_id not in(select f.film_id
from film f inner join film_category fc on f.film_id=fc.film_id)
```



2\. 你能使用子查询的方式找出属于Action分类的所有电影对应的title,description吗

```
select f.title, f.description
from  (
     select fc.film_id
     from film_category fc left join category c on fc.category_id = c.category_id
     where c.name ='Action'
) fid left join film f  on f.film_id = fid.film_id;

# 或者

select f.title, f.description
from  film f  
where f.film_id in (
    select fc.film_id
    from film_category fc left join category c on fc.category_id = c.category_id
    where c.name ='Action'
) ;
```

