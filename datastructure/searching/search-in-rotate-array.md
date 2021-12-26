---
description: Medium; Binary Search;
---

# Search in Rotate Array

**1. Link**

{% embed url="https://leetcode-cn.com/problems/search-in-rotated-sorted-array/" %}

**2. 题目**

**搜索旋转排序数组**

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 \[nums\[k], nums\[k+1], ..., nums\[n-1], nums\[0], nums\[1], ..., nums\[k-1]]（下标 从 0 开始 计数）。例如， \[0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 \[4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

示例 1：

输入：nums = \[4,5,6,7,0,1,2], target = 0 输出：4 示例 2：

输入：nums = \[4,5,6,7,0,1,2], target = 3 输出：-1 示例 3：

输入：nums = \[1], target = 0 输出：-1

来源：力扣（LeetCode） 链接：[https://leetcode-cn.com/problems/search-in-rotated-sorted-array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array) 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



**3. 思路**

1. **binary search**
2.  &#x20;**move right pointer to middle, right = mid only when**

    1. **target < arr\[mid] and target >  arr\[0]**
    2. **arr\[mid] > target  and  arr\[0] > arr\[mid]**
    3. **target >arr\[0]  and arr\[0] > arr\[mid]**

    ****
3. **otherwise, left = mid**
4. **Time: O(logn), Space: O(1)**

****

**4. Coding**

```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        #
        #
        #binary search
        # move right to mid
        # case1:  target > a[mid] >= a[0]
        # case2: a[0] > target > a[mid]
        # case3: target>= a[0] > a[mid]
        #
        #
        #

        if  not nums:
            return -1
        left, right = 0 , len(nums)-1
        while left < right-1:
            mid = (left + right)//2
            if (target < nums[mid] and target >=nums[0]) or (nums[mid]>target and nums[mid]<nums[0]) or (target>=nums[0] and nums[0]>nums[mid]):
                right = mid
            else:
                left = mid
        if nums[left] == target:
            return left
        if nums[right] == target:
            return right
        return -1
```

