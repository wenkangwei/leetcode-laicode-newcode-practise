---
description: 2D-DP; Medium;
---

# minEditCost



### 1.Link

{% embed url="https://www.nowcoder.com/practice/05fed41805ae4394ab6607d0d745c8e4?tpId=190&tqId=35213&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-rd%2Fquestion-ranking&tab=answerKey" %}



### 2.题目描述

给定两个字符串str1和str2，再给定三个整数ic，dc和rc，分别代表插入、删除和替换一个字符的代价，请输出将str1编辑成str2的最小代价。示例1

### 输入

[复制](javascript:void%280%29;)

```text
"abc","adc",5,3,2
```

### 返回值

[复制](javascript:void%280%29;)

```text
2
```

示例2

### 输入

[复制](javascript:void%280%29;)

```text
"abc","adc",5,3,100
```

### 返回值

[复制](javascript:void%280%29;)

```text
8
```



### 3. 思路

1. 2D dp
2. construct a 2D dp table, columns represents str1 and rows represent str2
3. table 大小为  \(len\(str2\)+1\) \* \(len\(str1\)+1\)  记上两个string都是空的情况
4. 由于要把str1变成str2,所以在每一行往右操作都是 + deletion cost, 每一列往下都是 + insertion cost
5. 初始第一行和第一列后，从dp\[1\]\[1\]位置,即str1, str2的第一个char开始找最小的cost
6. 把dp\[i-1\]\[j-1\], dp\[i-1\]\[j\], dp\[i\]\[j-1\] 的更新后的结果的最小值作为dp\[i\]\[j\]的结果
7. Time: O\(n\*m\) for iterating and updating table, n = len\(str1\), m= len\(str2\)
8. Space: O\(n\*m\)

### 4. Coding

```text
#
# min edit cost
# @param str1 string字符串 the string
# @param str2 string字符串 the string
# @param ic int整型 insert cost
# @param dc int整型 delete cost
# @param rc int整型 replace cost
# @return int整型
#
class Solution:
    def minEditCost(self , str1 , str2 , ic , dc , rc ):
        # write code here
        # idea: 2D DP
        #
        #
        # 1. 因为题目是找“”最小“”代价，所以 可以用DP把每次最小的子问题用到下一个问题里面
        # 2. state: 因为输入的是两个str，要找它们 之间的最小操作就要2D dp
        #  2D dp table 的行代表str1, 列代表str2, 第i行j列的cell的值代表
        #  str2[:i]  str1[:j] 两个字符串的最小编辑数目
        # 3. base case 找transition fucntion： str1= "apple"  str2 = "banana"
        #  如果是  str1=“”  str2=“”  -> dp[0][0] = 0
        #  如果是  str1=“a”  str2=“”  -> dp[0][1] = min(ic, dc) 
        #  如果是  str1=“”  str2=“b”  -> dp[1][0] = min(ic, dc)
        #  如果是  str1=“a”  str2=“b”  -> dp[1][1] = rc 替换 cost
        # = min(dp[i-1][j], dp[i][j-1], dp[i][j])+1= 0+1
        #  如果是  str1=“a”  str2=“ba”  -> dp[0][0] = 2 = 1+ 1
        #  如果是  str1=“ap”  str2=“b”  -> dp[0][0] = 1 + 1 =2
        #  "a"   "a"  -> 0
        #  "ab", "a"  -> 1 deletion or 
        #  前面3种情况的最小的修改数 + 1 ， 这个1 可以是删除或添加，如果两个str长度一样
        #  那么就修改最后一个char
        # str2    str1
        #    "" a  p   p  l  e
        # "" 0  dc dc  dc  dc dc
        # b ic
        # a ic
        # n ic
        # a ic
        # n ic
        # a ic
        #    ""   a   b   c
        # "" 0    3  6   9
        # a  5    0  3   6
        # d  10   5  2   7
        # c  15   10 2+ 5=7   2+0 = 2
        if not str1 and not str2:
            return 0
        # +1 是从str1, str2 = "", ""的情况开始
        dp = [ [0]*(len(str1)+1) for _ in range(len(str2)+1) ]
        # initialize cost when either str1="" or str2 = ""
        # we can either delete or insert char to string 
        for i in range(1, len(str1)+1):
            # delete cost to delete char to str1="..." to str2=""
            dp[0][i] = dp[0][i-1] + dc
        for j in range(1, len(str2)+1):
            # insertion cost str1= ""  str2 = "..."
            dp[j][0] = dp[j-1][0] + ic
            
        for i in range(1, len(str2)+1):
            for j in range(1, len(str1)+1):
                if str1[j-1] == str2[i-1]:
                    # no need to modify or insert or delete
                    dp[i][j] = dp[i-1][j-1]
                else:
                    replace = dp[i-1][j-1] +rc
                    deletion = cost = dp[i][j-1] + dc
                    insert = cost = dp[i-1][j]+ic
                    dp[i][j] = min(deletion, replace, insert)
        return dp[-1][-1]

                
                
                
        
        
        
        
        
```







