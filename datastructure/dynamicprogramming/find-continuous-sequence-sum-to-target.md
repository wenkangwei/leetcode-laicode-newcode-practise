---
description: DP, BruteForce
---

# Find Continuous Sequence Sum to Target

### 1. Link

{% embed url="https://www.nowcoder.com/practice/c451a3fd84b64cb19485dad758a55ebe?tpId=13&tqId=11194&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking&tab=answerKey" %}





### 2. 题目描述

小明很喜欢数学,有一天他在做数学作业时,要求计算出9\~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

### 返回值描述:

```
输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序
```

示例1

### 输入

复制

```
9
```

### 返回值

复制

```
[[2,3,4],[4,5]]
```





### 3. 思路

1. sliding window:
   1. use a sliding window and move fast point forward when sum of window < target
   2. move slow pointer forward when sum of window > target
   3. append element between slow and fast pointers when sum of window == target
   4. stop when fast >= len(arr)&#x20;
   5. Time: O(n\*k), k = step move the slow pointer, in worse case k = window size. n= size of array

### 4. Coding

**穷举法**

```
class Solution:
    def FindContinuousSequence(self, tsum):
        #
        #method2: 穷举
        #Time O(n^2)  Space: O(1) if not consider res
        # 1. iterate every value i in [1, tsum]
        # 2. find the continuous sum ended at i in the second loop
        #    if sum < tsum, then continue adding new value j-1
        #
        res = []
        for i in range(1, tsum):
            sum_val = 0
            sol = []
            for j in range(i,0, -1):
                sum_val += j
                sol.append(j)
                if sum_val == tsum:
                    sol.reverse()
                    res.append(sol[:])
                elif sum_val > tsum:
                    break
        return res
            
                    
            
```



Sliding window method

```
# -*- coding:utf-8 -*-
class Solution:
    def FindContinuousSequence(self, tsum):
        # write code here
        #method 1: sliding window
        # Time:O(n*k), n = array size, k= average window size
        #method2: backtracking
        #
        #
        #
        #
        if tsum == 0:
            return 1
        left, right =1,2
        sum_val = left+right
        res = []
        sol = [left,right]
        
        while right <tsum:
            if sum_val < tsum:
                right += 1
                sum_val += right
                sol.append(right)
            elif sum_val > tsum:
                sol.pop(0)
                sum_val -= left
                left += 1
            else:
                res.append(sol[:])
                right += 1
                sum_val += right
                sol.append(right)
        return res
```





