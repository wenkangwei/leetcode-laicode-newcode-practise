# 1800. Maximum Ascending Subarray Sum









Given an array of positive integers `nums`, return the _maximum possible sum of an **ascending** subarray in _`nums`.

```
```

A subarray is defined as a contiguous sequence of numbers in an array.

A subarray `[numsl, numsl+1, ..., numsr-1, numsr]` is **ascending** if for all `i` where `l <= i < r`, `numsi < numsi+1`. Note that a subarray of size `1` is **ascending**.

**Example 1:**

```
Input: nums = [10,20,30,5,10,50]
Output: 65
Explanation: [5,10,50] is the ascending subarray with the maximum sum of 65.
```

**Example 2:**

```
Input: nums = [10,20,30,40,50]
Output: 150
Explanation: [10,20,30,40,50] is the ascending subarray with the maximum sum of 150.
```



思路： DP

1. 由于是连续的subarray每次只要和前面的arr\[i-1]对比看符不符合条件，所以只要一个loop
2. state: 当前第i个element的符合条件arr\[i]>arr\[i-1]的cumulative sum,&#x20;
3. 转移方程是 if arr\[i]>arr\[i-1],  cum-sum += arr\[i]  otherwise,  cum-sum = arr\[i]
4. optimal solution 是当前的最大值

```
class Solution:
    def maxAscendingSum(self, nums: List[int]) -> int:
        #
        #
        #test case:   [10, 20, 30, 5, 10, 50]
        #
        #
        if not nums:
            return None
        cum_sum = max_sum = nums[0]
        for i in range(1, len(nums)):
            if nums[i] > nums[i-1]:
                cum_sum += nums[i]
            else:
                cum_sum = nums[i]
            max_sum = max(max_sum, cum_sum)
        return max_sum
            
        
```
