# Spark Issues

## 1. Background

由于近期发现spark sql和hive的sql的用法有差异， 这里记录databrick spark sql的一些差异和一些常见问题



## 2. Issue List

1. **Issue 1: Databrick extraneous input ';'expecting \<EOF>**

Example:

```
-- 页面曝光，区分单一混 明细 
select a.dt, d.site_l1_cd, a.trmnl_tp, a.page_nm, a.category_id ,
case when b.sku_cate_nm is null or trim(b.sku_cate_nm)='' then '混品类' else '单一品类' 
end as category_nm -- 单一品类场景名称 ,
sum(pv) page_expose_pv ,
sum(pv_dis) page_view_pv 
from tb


```



在databrick里面注释  `-- 单一品类场景名称 `   需要加上双引号才能生效，比如`--"单一品类场景名称" `，不然会当成EOF 边界错误。&#x20;









