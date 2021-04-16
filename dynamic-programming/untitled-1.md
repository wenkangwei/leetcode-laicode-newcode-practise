---
description: 2D-DP
---

# Longest Common Substring



### Link

{% embed url="https://www.nowcoder.com/practice/f33f5adc55f444baa0e0ca87ad8a6aac?tpId=190&tqId=36002&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-rd%2Fquestion-ranking&tab=answerKey" %}





### 题目描述

给定两个字符串str1和str2,输出两个字符串的最长公共子串题目保证str1和str2的最长公共子串存在且唯一。示例1

### 输入

[复制](javascript:void%280%29;)

```text
"1AB2345CD","12345EF"
```

### 返回值

[复制](javascript:void%280%29;)

```text
"2345"
```



思路：2D-DP

1. 用2D dp array 存放str1- vs- str2的table
2. 用两个iterate str1, 和str2， 当str1\[i\] == str2\[j\]时把对应的DP table\[i\]\[j\]个位置存放以当前位置为结尾的连续相等的char的个数， 即 table\[i\]\[j\] = table\[i-1\]\[j-1\] +1. 之所以要i-1, j-1是因为要str1, str2都往前一位的char是相同的
3. 把当前的table\[i\]\[j\] 与global max的length对比并更新max len以及当前的str1或者str2的位置idx
4. 最后用返回存放的位置idx以及对应的max len 长度进行substring查找
5. Time: O\(n\*m\) n = len\(str1\)  m= len\(str2\).  Space  = O\(n\*m\)



```text
# #
# # longest common substring
# # @param str1 string字符串 the string
# # @param str2 string字符串 the string
# # @return string字符串
# #
class Solution:
    def LCS(self , str1 , str2 ):
        # write code here
        # test case:
        # s1 = "ABCDBD"  s2 = "BCE"
        # dp table
        #    A   B   C   D  B  D
        # B  0   1   0   0  1  0
        # C  0   0   1   0  0  0
        # E  0   0   0   0  0  0
        #
        # 可以发现如果是连续的substring那么table里面就有连续的斜线
        # 然后我们要找的就是最长的斜线的位置
        # idea:
        # 1. 搭建table， dp table的cell代表以当前的str1 第i个位置， str2第j个位置 结尾的
        #  common substring的长度
        # 2. 我们只要找到最长的长度，以及结尾的位置就可以找到longest common substring
        # idea
        #  1. iterate str1
        #     iterate str 2 
        #       if str1[i] == str2[j]: label tb[i][j] = tbl[i-1][j-1] +1
        #        update max len and  end of index in str1
        #     return str1[end- max_len+1: end+1]
        
#         if not str1 or not str2:
#             return ""
        dp = [[0]*(len(str2)+1) for i in range(len(str1)+1)]
        max_len = 0
        idx = 0
        idx2 = 0 
        for i in range(1,len(str1)+1):
            for j in range(1,len(str2)+1):
                if str1[i-1] == str2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                    if max_len < dp[i][j]:
                        idx = j
                        idx2 = i
                        max_len = dp[i][j]
        return str2[idx-max_len:idx]
```





