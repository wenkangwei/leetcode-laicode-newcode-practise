---
description: Binary Search; Easy;
---

# 在排序数组中查找元素的第一个和最后一个位置

&#x31;**. Link**

{% embed url="https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/" %}



**2. 题目**

**34. 在排序数组中查找元素的第一个和最后一个位置**

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 \[-1, -1]。

进阶：

你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？

示例 1：

输入：nums = \[5,7,7,8,8,10], target = 8 输出：\[3,4] 示例 2：

输入：nums = \[5,7,7,8,8,10], target = 6 输出：\[-1,-1] 示例 3：

输入：nums = \[], target = 0 输出：\[-1,-1]

来源：力扣（LeetCode） 链接：[https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array)&#x20;

**3. 思路**

1. &#x20;思路： binary search
2. &#x20;先用binary search 找到target并不断把right pointer 移动到target的位置，从而找到 第一次出现的 target的位置，即start
3. &#x20;从start到 len(nums)-1 的subarray里面， 重新用binary search 找到target并不断把left pointer 移动到target的位置，从而找到 最后一次次出现的 target的位置，即end
4. 返回 \[start, end]
5. Time Complexity: O(logn) + O(logn) = O(logn).  Space: O(1)

**4. Coding**

```
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        #
        #input: sorted array  output: indices/positions of start and end of target
        # idea: binary search
        #  1.  start from left and right of array to search target
        #  2. find the left boundary/the first occurance of target
        #  3. find the right boundary/the last occurance of target
        #  4. return start and end

        if not nums:
            return [-1, -1]
        # find first occurance
        left, right = 0, len(nums)-1
        start =-1
        end = -1
        while left < right -1:
            mid = (left + right)//2
            if nums[mid] >= target:
                right = mid
            else:
                left = mid
        if nums[left] == target:
            start = left
        elif nums[right] == target:
            start = right

        left = start
        right = len(nums)-1
        while left < right -1:
            mid = (left + right)//2
            if nums[mid] > target:
                right = mid
            else:
                left = mid
        if nums[right] == target:
            end = right
        elif nums[left] == target:
            end = left
        return [start, end]


```
