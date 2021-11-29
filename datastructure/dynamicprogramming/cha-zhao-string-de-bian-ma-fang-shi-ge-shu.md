---
description: DP;  Medium; 字节
---

# 查找string的编码方式个数

### 1. Link

{% embed url="https://www.nowcoder.com/practice/046a55e6cd274cffb88fc32dba695668?tpId=117&tqId=37840&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high%2Fquestion-ranking&tab=answerKey" %}



### 题目描述

有一种将字母编码成数字的方式：'a'->1, 'b->2', ... , 'z->26'。现在给一串数字，返回有多少种可能的译码结果示例1

### 输入

[复制](javascript:void\(0\);)

```
"12"
```

### 返回值

[复制](javascript:void\(0\);)

```
2
```

### 说明

```
2种可能的译码结果（”ab” 或”l”）
```

示例2

### 输入

[复制](javascript:void\(0\);)

```
"31717126241541717"
```



### 返回值

[复制](javascript:void\(0\);)

```
192
```

思想: DP

1. &#x20;确定state : dp\[i] = 在字符串0到i位置这个子字符里面的decoding ways个数
2. 通过base case 找到transition 方程
   1. base case: when str = "n", n=0,  -> dp\[0] = 0 没有decoding way，因为不在1-26范围里面。 when str ="n", n>0,  then  dp\[0] = 1
   2.  下一个状态 str = "nm", 它的decoding的方式有

       1. str = "n/m", 把新进来的char不管怎么样都总是当成一个digit，那么如果”m” >0 那么dp\[1] += dp\[0],  就是加上前面“n” 里面的decoding的方式个数
       2. str = "/nm" 把前面一个digit加上现在新进来的digit看成一个整体， 如果“nm”里面的n !=0 并且 int(nm) <26 那么它就是valid的字符，这样我们就在只考虑 "nm"之前的digit的编码个数。 “nm”之前没有东西，所以这里就默认成1。 所以 如果 i-2 ==-1,  dp\[i] += 1 否则 i-2>-1,  dp\[i] += dp\[i-2]
       3. 由于编码的数字最多2个digit，所以到这里就不用再切分了。这样就根据限制条件找到转换方程了


3. Time: O(n) Space: O(n)

###

```
#
# 解码
# @param nums string字符串 数字串
# @return int整型
#
class Solution:
    def solve(self , nums ):
        # write code here
        #
        # state dp[i] = number of decoding ways
        # base case "n", if n ==0, return 0, if "n" >0 and <9 return 1
        # "nm": if int(nm) =0 return 0 and  if 0<int(nm) < 11, return 1 "m"
        #  if int(nm)<=26: return 2  "n/m" and "nm"
        #
        # "nmk": consider one way separate k with previous char
        #: "nm/k" then this is equal to dp[i-1] decoding ways
        # consider "n/mk", combine previous char with k and think mk as one char
        # then we always let n, mk separate, then our solution is 
        # dp[i-2] = solve("n")
        #
        # because we can only combine two digiit at max, so no need to think
        # the previous cases
        #
        # so if "n/mk", int("mk")<26, then dp[i] = dp[i-1] + dp[i-2]
        # if int("mk") >26 or int("mk")==0, dp[i] = dp[i-1]
        # if "nm/k"  int("k") ==0, then this way is invalid, otherwise,
        #    dp[i] += dp[i-1]
    
            # if "n/mk", int("mk") ==0 or int("mk") >26, then this way is invalid
        #     otherwise: dp[i] += dp[i-2]
        # if two cases exist, then dp[i] = 0
        #
        # test case: "31" -> "3/1", "/31"
        #
        #
        #
        
        if nums == "0":
            return 0
        if len(nums) ==1:
            return 1
        dp =[0]*len(nums)
        
        for i in range( len(nums)):
            if i ==0:
                if nums[i] != "0":
                    dp[i] = 1
            else:
                # consider the case that we separate the last digit 
                # in this way "..../k"
                # if "k" >0 and <=9 then we add previous solution dp[i-1]
                if i-1 >=-1 and int(nums[i])>0:
                    dp[i] += dp[i-1]
                # consider the case we separate  the digit in this way
                #  ".../mk"
                # if "m" ==0, then "mk" is invalid so no need to add this case
                # if "m" != 0, then "mk" is valid so add the solution
                # "../mk" from dp[i-2]
                if i-2 >=-1 and int(nums[i-1]) !=0 and int(nums[i-1:i+1]) >0 and int(nums[i-1:i+1]) <=26:
                    if i-2 ==-1:
                        dp[i] += 1
                    else:
                        dp[i] += dp[i-2]
        return dp[-1]
        
        
```

### 说明

```
192种可能的译码结果
```

[ 关联企业](javascript:void\(0\);)
