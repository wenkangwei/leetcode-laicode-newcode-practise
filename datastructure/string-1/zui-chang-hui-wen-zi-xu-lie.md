---
description: Medium; DP; String
---

# 最长回文子序列-DP

&#x31;**. Link**

{% embed url="https://leetcode-cn.com/problems/longest-palindromic-subsequence/" %}

**这题考虑 substring的顺序，不能改变顺序来得到回文数**

**2. 题目**

给定一个字符串 s ，找到其中最长的回文子序列，并返回该序列的长度。可以假设 s 的最大长度为 1000 。

示例 1: 输入:

"bbbab" 输出:

4 一个可能的最长回文子序列为 "bbbb"。

示例 2: 输入:

"cbbd" 输出:

2

来源：力扣（LeetCode） 链接：[https://leetcode-cn.com/problems/longest-palindromic-subsequence](https://leetcode-cn.com/problems/longest-palindromic-subsequence) 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



**3. 题目**

1. &#x20;idea: DP
2. &#x20;dp\[i]\[j]:    从string char s\[i]到s\[j]的最长回文数
3. 当i == j ,  dp\[i]\[j] =0 只有一个char
4. 当 i!=j,   s\[i] == s\[j],  dp\[i]\[j] = dp\[i+1]\[j-1] + 2 这里的2是考虑char s\[i]和s\[j]
5. 当 s\[i] ！= s\[j],     dp\[i]\[j] = max(dp\[i+1]\[j],  dp\[i]\[j-1]) 分别把substring的头尾加到中间的substring然后看两者最大的长度
6. 遍历时候，把string   pointer  i 从后面遍历到前面, 查找 substring  i 到 len(s)-1 之间的substring
7. Time : O(n^2), Space:O(n^2)



**4. Coding**

```
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        #
        #idea: DP
        # state: dp[i] = max length of palindrome between char i and the last char in string
        # transition function:
        # if we have a substring   s[i] s[i+1]...s[j]
        # 如果 s[i] == s[j], 那么dp[i] = max length of palindrom from s[i+1] to s[j-1] + 2 
        # 这里的2 代表 s[i], s[j]
        # 否则dp[i] 不变，依旧是之前找到的考虑s[i]的回文数的长度
        #dp[0] = 1
        # dp[1] = max(dp[1],)
        #
        # dp= [0]*len(s)
        
        # for i in range(len(s)):
        #     dp[i] = 1
        #     max_len = 0
        #     for j in range(i-1, -1, -1):
        #         tmp = dp[j]
        #         if s[i] == s[j]:
        #             dp[j] = max_len + 2
        #         max_len = max(tmp, max_len)
        # return max(dp)
            
        #
        #idea2: 2D-DP
        # state: dp[i][j] = 第i个char到第j个char的最长回文数长度
        # 而 如果第s[i] == s[j] 那么  dp[i][j] = dp[i+1][j-1] + 2 （加上 s[i], s[j]两个）
        # 如果s[i]!=s[j], 那么 dp[i][j]= max(dp[i+1][j],dp[i][j-1]),把s[i], s[j]分别加入回文数看哪个更长
        # 所以 dp[i][i] =1
        # row, col = string
        # 1. iterate s in i 
        #       iterate s in j
        #            when i != j and s[i] == s[j]-> 找到回文数的相同char
        #

        dp = [[0]*len(s) for i in range(len(s))]
        for i in range(len(s)):
            dp[i][i] = 1
        max_len = 0
        for i in range(len(s)-1, -1,-1):
            for j in range(i+1, len(s)):
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1]+2
                else:
                    dp[i][j] = max(dp[i+1][j],dp[i][j-1])
                max_len = max(max_len, dp[i][j])
        return dp[0][-1]

                    

```





