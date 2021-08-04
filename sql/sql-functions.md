# SQL  and PySpark functions

1. Aggregation function
2. Window functions
3. Create /  Drop tables
4. Create / Drop database
5. Insert and modify 
6. Other Useful PySpark functions

   1. **lit**\(value\).alias\(column\_name\): 用于添加新的column到table而column的值=value 或者 value 是原来的table的某列的值， 之后再用alias重新命名
   2. **pivot**\(column\_name\) .count\(\) 用于把某一列的类别值展开变成multi-onehot的形式

```text
tmp.groupBy("movieId").pivot("splitted_genres").count().fillna(0).show()
```



