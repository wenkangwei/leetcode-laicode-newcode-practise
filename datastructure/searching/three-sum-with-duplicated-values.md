# Three Sum with duplicated values

### 1. Link

{% embed url="https://www.nowcoder.com/practice/345e2ed5f81d4017bbb8cc6055b0b711?tpId=190&&tqId=35196&rp=1&ru=/activity/oj&qru=/ta/job-code-high-rd/question-ranking" %}

### 2. 描述

给出一个有n个元素的数组S，S中是否有元素a,b,c满足a+b+c=0？找出数组S中所有满足条件的三元组。注意：  


1. 三元组（a、b、c）中的元素必须按非降序排列。（即a≤b≤c）
2. 解集中不能包含重复的三元组。

```text
例如，给定的数组 S = {-10 0 10 20 -10 -40},解集为(-10, -10, 20),(-10, 0, 10) 
0 <= S.length <= 1000
```

### 示例1

输入：

```text
[0]
```

复制返回值：

```text
[]
```

复制

### 示例2

输入：

```text
[-2,0,1,1,2]
```

复制返回值：

```text
[[-2,0,2],[-2,1,1]]
```



### 3. 思路

1. sort array
2. iterate every element num\[i\]
3. binary search for the remaining element to check if two sum to target = 0- num\[i\] exist
4. skip all duplicated values once the first element found to avoid duplicated results
5. Time: O\(nlogn\) for sorting  O\(n \) for the first loop,  O\(n\) for binary search in worst case, so O\(n^2 + nlogn\) = O\(n^2\) 
6. Space O\(1\)

### 4. Coding

```text
#
# 
# @param num int整型一维数组 
# @return int整型二维数组
#
class Solution:
    def threeSum(self , num ):
        # write code here
        if not num or len(num) <3:
            return []
        res = []
        num.sort()
        i = 0
        while i < len(num):
            target = 0 - num[i]
            sol = [num[i]]
            # two sum with binary search
            left, right = i+1, len(num)-1
            while left < right:
                tmp = num[left] + num[right]
                if tmp < target:
                    left += 1
                elif tmp > target:
                    right -= 1
                else:
                    sol.append(num[left])
                    sol.append(num[right])
                    sol.sort()
                    res.append(sol[:])
                    sol =[num[i]]
                    left_idx = left+1
                    # skip all duplicated value to avoid duplicated result
                    while left_idx <right and num[left] == num[left_idx]:
                        left_idx += 1
                    left = left_idx
                    right_idx = right-1
                    while right_idx >left and num[right] == num[right_idx]:
                        right_idx -= 1
                    right = right_idx
                    
                
            # skip all duplicated element 
            idx = i + 1
            while idx<len(num) and num[idx] == num[i]:
                idx += 1
            i = idx

        return res
                    
                    
            
```





