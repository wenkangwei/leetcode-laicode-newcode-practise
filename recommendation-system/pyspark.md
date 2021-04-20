# PySpark

**1.** **Download data set MovieLen data set and PySpark**



```text
! wget http://files.grouplens.org/datasets/movielens/ml-latest-small.zip
! unzip ml-latest-small.zip
! ls
!apt-get install openjdk-8-jdk-headless -qq > /dev/null
!wget -q http://apache.osuosl.org/spark/spark-3.1.1/spark-3.1.1-bin-hadoop3.2.tgz
!tar xf spark-3.1.1-bin-hadoop3.2.tgz
!pip install -q findspark
```

**2.  Start a session for spark**

```python
import findspark
findspark.init()
from pyspark.sql import SparkSession
# SparkSession: 创建一个spark的实例
# builder: 构造器，用于添加其他设定
# appName("..."): 实例应用名称
# master("local[*]"): 连接spark 到cluster， local指本地，[*]指任意core 数目
# 如果是local[4]: 连接到本地4个cores
# getOrCreate(): obtain existing instance or create new instance if not exist
spark = SparkSession \
    .builder \
    .master("local[*]") \
    .appName("moive analysis") \
    .config("spark.some.config.option", "some-value") \
    .getOrCreate()
# Test the spark
df = spark.createDataFrame([{"hello": "world"} for x in range(1000)])
df.show(3, False)
```



**3. Load data**

```python
root = "ml-latest-small/"
# if using databrick
databrick_root = "/FileStore/tables/movielen_small/"
movies_df = spark.read.load(root+"movies.csv", format='csv', header = True)
ratings_df = spark.read.load(root+ "ratings.csv", format='csv', header = True)
links_df = spark.read.load(root+"links.csv", format='csv', header = True)
tags_df = spark.read.load(root+"tags.csv", format='csv', header = True)


# Or 
movies_df = spark.read.csv(root+"movies.csv", header = True)
# spark.read.options(...).json(...) or .jdbc(...) 
# options: settings for loading, it can be timezone format
```



**4. Change data type:**

```python
from pyspark.sql.types import IntegerType, FloatType
t_df = tags_df.withColumn('movieId-new', tags_df.movieId.astype('int'))
t_df= t_df.withColumn('userId',t_df['userId'].astype('int'))
t_df= t_df.withColumn('movieId',t_df['movieId'].astype('int'))
t_df= t_df.withColumn('timestamp',t_df['timestamp'].astype('int'))
t_df.collect()[1]

# Or
t_df= t_df.withColumn('movieId',t_df['movieId'].astype(FloatType))
#...
```

**5. Query:**

5.1 DataFrame method

```python
from pyspark.sql.functions import col, explode, split
# find the amount of unique movieId
movies_df.select('movieId').distinct().count()

# use agg() aggregation function to find mean rating of each
# movie.  agg({column_name:aggregation function name})
# 选择topK 个平均评分最高的element, .limit(k)
ratings_df.groupBy("movieId").agg({'rating':"mean"}) \
    .sort("avg(rating)",ascending = False) \
    .limit(10) \
    .show()
    
    
    
```

5.2 SQL method

First register SQL tables from Spark data frame

```python
# Register SQL tables in SQL
movies_df.registerTempTable("movies")
ratings_df.registerTempTable("ratings")
links_df.registerTempTable("links")
tags_df.registerTempTable("tags")

# Or 使用global temporary view。但是 用global temporary view
# 时 在SQL里面需要 select * from global_temp.movies 要加多global_temp.
# 这一句
movies_df.createOrReplaceGlobalTempView("movies")
```

Then use spark.sql to run SQL directly

```python
# 使用Spark SQL
# spark.sql(...): spark, the spark session created
user_count = spark.sql("select count(distinct userId) as user_count from ratings")
```



```python
# %sql 
# Show movies that are not rated
movie_not_rated = spark.sql("select distinct movieId, title from movies where movieId not in (select distinct movieId from ratings)")
movie_not_rated.show()

# select distinct movie Id
# distinct 用于对rows 除重,如果有多个column，就考虑每一个row的组合作为一个
# distinct value
movies_df.select(['moviesId']).distinct().show()


# 搜索
from pyspark.sql.functions import col, explode, split
movie_missing_value= movies_df.where(col('title').isNull() | col('genres').isNull())
movie_missing_value.show()
```









