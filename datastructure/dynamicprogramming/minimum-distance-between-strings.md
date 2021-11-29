---
description: DP; Medium;
---

# Minimum distance between strings

### 1. Link

[https://www.nowcoder.com/practice/82bd533cd9c34df29ba15bbf1591bedf?tpId=196&\&tqId=37196\&rp=1\&ru=/activity/oj\&qru=/ta/job-code-total/question-ranking](https://www.nowcoder.com/practice/82bd533cd9c34df29ba15bbf1591bedf?tpId=196&\&tqId=37196\&rp=1\&ru=/activity/oj\&qru=/ta/job-code-total/question-ranking)

### 2. 描述

给定两个长度相等的，由小写字母组成的字符串S1和S2，定义S1和S2的距离为两个字符串有多少个位置上的字母不相等。现在牛牛可以选定两个字母X1和X2，将S1中的所有字母X1均替换成X2。（X1和X2可以相同）牛牛希望知道执行一次替换之后，两个字符串的距离最少为多少。\


### 示例1

输入：

```
"aaa","bbb"
```

复制返回值：

```
0
```

复制说明：

```
牛牛可以将S1中的字符'a'全部替换成字符'b'，这样S1就变成了"bbb"，那么S1和S2的距离就是0
```

### 示例2

输入：

```
"aabb","cdef"
```

复制返回值：

```
3
```

复制说明：

```
一种可行的方案是将S1中的字符'a'全部替换成字符'c'，那么S1变成了"ccbb"，和S2的距离是3
```

### 备注：

```
S1.size() =S2.size()S1.size()=S2.size()1 \leq S1.size() \leq 5 * 10^41≤S1.size()≤5∗104
S1和S2中的字母均为小写字母
```



### 3. 思路： 2D-DP

1. 搭建26\*26的2D table， row和column分别代表26个字母，dp\[i]\[j]= string S1里面的char = 第i个字符时，string S2对应位置里面为第j个字符的个数。 例子: s1= "aab", s2= "bbc",  当s1的字符=‘a’ (i=0)时 s2的字符=‘b’ (j=1)的个数是2， 所以dp\[0]\[1] = 2
2. 遍历S1，把S1和S2的不同字符的个数加起来起来， **distance = 不同字符个数+ S1和S2的字符长度差, 并且把dp table填充**
3. 遍历dp table的每一行， 并计算那一行的里面 最大的count - dp\[i]\[i]的值， 即把这个char换成另外一个char时最大能减少的distance长度，Di
4. 把每一行的最大能减少的distance长度对比，找到S1里面最大Di， 即 D\_max&#x20;
5. 返回 distance -D\_max
6. TIme: O(n + 26\*26) = O(n)
7. Space: O(26\*26) = O(1)

### 4. Coding

```
class Solution:
    def GetMinDistance(self , s1 , s2 ):
        #
        #method1: 2D-DP
        # build a table with column and row = dictionary of 26 char
        # dp[i][j] = count of the case when s1[i]= i^th char and s2[j] = j^th char
        # then 
        #  we iterate every char in row and find the max dp[i][j] on this row
        #  then we compute the different between max dp[i][j] and dp[i][i]
        #  to see if we replace the i^th char in s1,  the maximum distance we can subtract
        #  do this over every char and find the char that has maximum different
        #  
        #  the final distance = the original distance - max different + abs(len(s1) - len(s2)) the length different
        # Time: O(n) + O(26*26) = O(n)
        #
        
        dp= [[0]*26 for i in range(26)]
        dist = 0
        for i in range(len(s1)):
            r= ord(s1[i]) - ord('a')
            if i < len(s2):
                c = ord(s2[i]) -ord('a')
                dp[r][c] += 1
                if s1[i] != s2[i]:
                    dist += 1
                    
        cnt = -float('inf')
        for i in range(26):
            max_ocur = 0
            cnt_sum = 0
            for j in range(26):
                max_ocur = max(max_ocur, dp[i][j])
                cnt_sum  += dp[i][j]
            cnt = max(cnt, max_ocur -  dp[i][i])
        dist = dist - cnt + abs(len(s1) - len(s2))
        return dist
        
        
        
        
```

