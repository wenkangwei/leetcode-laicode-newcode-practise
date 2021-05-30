# Longest Common Substring-II

**1. Link**



**2. 题目**

给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。 两个字符串的 公共子序列 是这两个字符串所共同拥有的子序列。

示例 1：

输入：text1 = "abcde", text2 = "ace" 输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。 示例 2：

输入：text1 = "abc", text2 = "abc" 输出：3 解释：最长公共子序列是 "abc" ，它的长度为 3 。

来源：力扣（LeetCode） 链接：[https://leetcode-cn.com/problems/longest-common-subsequence](https://leetcode-cn.com/problems/longest-common-subsequence) 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



**3. 思路**

1. 思路： 2D- DP
2. row = str1, col = str2, dp\[i\]\[j\] 为str1\[:i\] 和str2\[:j\] substring的最大公共子序列的长度， 这是一个子问题
3. 遍历str1 和str2的每一个char 进行对比，如果 str1\[i\] == str2\[j\] , 那么dp\[i\]\[j\] = dp\[i-1\]\[j-1\] + 1等于不考虑当前相同的char之前的substring的最长公共子序列的长度 + 当前相同的char 1
4. 如果str1\[i\] != str2\[j\], 就直接简单地把dp\[i-1\]\[j-1\]，   dp\[i-1\]\[j\]， dp\[i\]\[j-1\]三种情况逐一对比找最优的情况传到下一个情况
5. Time: O\(n^2\), Space: O\(n^2\)

\*\*\*\*

**4. Coding**

```text
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        #
        #
        #idea: 2D- DP
        # col = str1, row = str2
        # dp[i][j] = str1[:i] 和str2[:j] 的最长公共子序列的长度
        #   str2\str1   ""  s1[0]   s1[1] ....s1[n]
        #   ""
        #   s2[0]
        #   s2[1]
        #   ..
        #   s2[m]
        if not text1  or not text2:
            return 0
        max_len =0
        dp = [[0]*(len(text1)+1) for i in range(len(text2)+1)]
        for i in range(1, len(text2)+1):
            for j in range(1, len(text1)+1):
                
                if text1[j-1] == text2[i-1] :
                    # 把前面连续的substring的长度加上现在新的char的长度
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    # 把前面遇到最大的common substring长度传到后面去
                    dp[i][j] = max(dp[i-1][j-1], dp[i-1][j],dp[i][j-1])
                max_len = max(max_len, dp[i][j])
        return dp[i][j]

        

```



返回最长公共字符串的写法: 从dp矩阵结尾反向寻找str1\[i\] == str2\[j\]的字符，每次往最大的dp\[i\]\[j\]方向进行缩小



```text
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
        #
        #s1 = "1AB3CD4BD5"  s2 = "3B45CE"
        # dp table
        #   1   A   B   3  C   D  4  B  D   5
        # 3 0   0   0   1  0   0  0  0  0   0
        # B 0   0   1   0  0   0  0  1  0   0
        # 4 0   0    0  0   0  0  1  0  0   0
        # 5 0  0    0   0  0   0  0  0  0   1
        # C 0  0   0    0   1  0  0   0 0   0
        # E
        #
        #
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
        
        if not str1 or not str2:
            return "-1"
        dp = [[0]*(len(str2)+1) for i in range(len(str1)+1)]
        max_len = 0
        idx = 0
        for i in range(1,len(str1)+1):
            for j in range(1,len(str2)+1):
                if str1[i-1] == str2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i][j-1], dp[i-1][j])
        i = len(str1)
        j = len(str2)
        res = []
        while i >=0 and j >=0:
            if str1[i-1] == str2[j-1]:
                res.insert(0, str2[j-1])
                i -= 1 
                j -= 1
            else:
                if dp[i-1][j] > dp[i][j-1]:
                    i -=1
                else:
                    j -= 1
        res = "".join(res)
        if not res:
            return "-1"
        return res
                      
```





