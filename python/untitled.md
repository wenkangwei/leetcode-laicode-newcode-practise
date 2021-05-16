---
description: 常用的函数和细节备忘录
---

# Python 笔记

1.  **2D array 初始化的 \[\[0\]\*n for i in range\(m\)\]  和 \[ \[0\]\*n \]\*m     区别  ：**

   1. 如果用 arr = \[ \[0\]\*n for i in range\(m\)  \], 那么 arr\[0\]\[0\] = x 就只是对第0行，第0列的element进行更新
   2. **如果用 arr = \[\[0\]\*n\]\*m 初始化，  那么arr\[y\]\[0\] = x 不管 y是什么值，都只对第0列所有element赋值.**  这是因为 \[\[0\]\*n\]**\*m** 表示的是指向 \[0\]\*n 这个列表的引用.

2.  常用函数：
   1.  **ord\(char\):** convert character to ASCII number
   2.  **chr\(num\):** convert int number to character
   3. **"".isalpha\(\)** : return True if all char in string is char a-z
   4. **"".isnumeric\(\):** return True if **all chars** in string is number, do not consider negative number and float

