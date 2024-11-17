---
description: Easy; String;
---

# Reverse String

### 1. Link

{% embed url="https://www.nowcoder.com/practice/c3a6afee325e472386a1c4eb1ef987f3?tpId=188&tqId=38280&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey" %}

{% embed url="https://leetcode.cn/problems/reverse-words-in-a-string/" %}

### 2. 题目描述

写出一个程序，接受一个字符串，然后输出该字符串反转后的字符串。（字符串长度不超过1000）示例1

### 输入

复制

```
"abcd"
```

### 返回值

复制

```
"dcba"
```

### 3. 思路

1. &#x20;因为String在python是immutable 不可变，每次操作都增加新字符串，所以要先把它变成list
2. 在list中，用two pointers/左右pointers 方法从左右两边进行char的交换，并把 两个pointer向中间移动
3. 重复步骤2 直到 left >= right 时停止
4. 返回reverse之后的字符串
5. Time Complexity: O(n)  Space Complexity: O(n) 因为创建了一个list和返回新的字符串

### 4. Coding

```
#
# 反转字符串
# @param str string字符串 
# @return string字符串
#
class Solution:
    def solve(self , str ):
        # write code here
        ls = list(str)
        left, right = 0, len(ls)-1
        while left < right:
            ls[left], ls[right] = ls[right], ls[left]
            left += 1
            right -= 1
        return "".join(ls)
```

