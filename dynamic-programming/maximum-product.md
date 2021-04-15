---
description: DP; Medium
---

# Maximum Product





### 1. Link

{% embed url="https://www.nowcoder.com/practice/9c158345c867466293fc413cff570356?tpId=190&tqId=35206&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-rd%2Fquestion-ranking&tab=answerKey" %}



### 2. 题目描述

给定一个double类型的数组arr，其中的元素可正可负可0，返回子数组累乘的最大乘积。示例1

### 输入

[复制](javascript:void%280%29;)

```text
[-2.5,4,0,3,0.5,8,-1]
```

### 返回值

[复制](javascript:void%280%29;)

```text
12.00000
```

3. 思路

1. DP
2. state: max\_dp\[i\] ,min\_dp\[i\] 以当前**第i个位置为结尾的连续的乘积的**的最大和最小值， 这个i一般是指上一次的结果不包含这次的arr\[i\]
3. 因为有可能负负得正得到最大值，所以要保留负数最小值
4. 每次从max\_dp _arr\[i\], min\_dp_arr\[i\], arr\[i\] 里面取最大和最小更新
5. 当前最大值和最小值， 那么其中最大值或最小值肯定有一个是乘上arr\[i\]或者reset成为arr\[i\], 这样下次乘上下一个arr\[i+1\]时乘积始终是连续的

```text
class Solution:
    def maxProduct(self , arr ):
        # state: max_dp[i] ,min_dp[i]上次状态的最大和最小值
        # transition function: 
        #max_dp_new = max(max_dp* arr[i] , min_dp *arr[i], arr[i])
        #min_dp_new = min(max_dp* arr[i] , min_dp *arr[i], arr[i])
        # 1. 因为有可能负负得正得到最大值，所以要保留负数最小值
        #2. 每次从max_dp *arr[i], min_dp*arr[i], arr[i] 里面取最大和最小更新
        #当前最大值和最小值， 那么其中最大值或最小值肯定有一个是乘上arr[i]或者reset成为
        # arr[i], 这样下次乘上下一个arr[i+1]时乘积始终是连续的
        #
        #
        if not arr:
            return None
        res = max_dp = min_dp = arr[0]
        for i in range(1, len(arr)):
            max_dp_new = max(max_dp* arr[i] , min_dp *arr[i], arr[i])
            min_dp_new = min(max_dp* arr[i] , min_dp *arr[i], arr[i])
            max_dp = max_dp_new
            min_dp = min_dp_new
            res = max(res, max_dp)
        return res
```







