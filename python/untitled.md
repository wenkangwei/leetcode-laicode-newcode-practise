---
description: 常用的函数和细节备忘录
---

# Python 笔记

1.  ** 2D array 初始化的 \[\[0]\*n for i in range(m)]  和 \[ \[0]\*n ]\*m     区别  ：**

    1. 如果用 arr = \[ \[0]\*n for i in range(m)  ], 那么 arr\[0]\[0] = x 就只是对第0行，第0列的element进行更新
    2. **如果用 arr = \[\[0]\*n]\*m 初始化，  那么arr\[y]\[0] = x 不管 y是什么值，都只对第0列所有element赋值. ** 这是因为 \[\[0]\*n]**\*m **表示的是指向 \[0]\*n 这个列表的引用.


2. &#x20;常用函数：
   1. ** ord(char):** convert character to ASCII number
   2. ** chr(num):** convert int number to character
   3. **"".isalpha()** : return True if all char in string is char a-z
   4. **"".isnumeric():** return True if **all chars** in string is number, do not consider negative number and float
   5. **id(a)**:   return address  ID of that variable
3. Deep Copy and Shallow Copy
   1. &#x20;list 里面的 copy:   b=a.copy() 或者 b= a\[:]是deep copy
   2. dictionary 里面的copy:    b=a.copy()是只对key的array进行deep copy，而对应的value的list是shallow copy

```
# 对于 List而言：
a = [1,2,3,4]
b = a.copy()
b.append(5)

#输出： b=[1,2,3,4,5], a= [1,2,3,4]
# 这里的list是deep copy，b=a.copy() 同等于 b=a[:]
# shallow copy:  b=a

#对于dictionary而言

a = {1：[1,2,3,4]}
b = a.copy()
b[1].append(5)

#这里b是对a字典的 key array进行deep copy。但是copy之后a和b的key所对应的
#value的地址是不变，所以对于list而言，b只是对a的list进行shallow copy
#所以这里的输出是 a={1:[1,2,3,4,5]}, b={1:[1,2,3,4,5]}
#

b[2] =[8,9]

#这里是对b的key的array添加新的key-value pair，而没有对a的key的array
#进行操作， 所以这句之后的输出是
# a={1:[1,2,3,4,5]}, b={1:[1,2,3,4,5] , 2:[8,9]}
#



```
