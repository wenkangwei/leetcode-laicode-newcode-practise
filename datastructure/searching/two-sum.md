---
description: Easy; Two pointer;
---

# Two Sum

1**.  Link**

{% embed url="https://leetcode-cn.com/problems/two-sum/" %}

****

**2. 题目**

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

示例 1：

输入：nums = \[2,7,11,15], target = 9 输出：\[0,1] 解释：因为 nums\[0] + nums\[1] == 9 ，返回 \[0, 1] 。 示例 2：

输入：nums = \[3,2,4], target = 6 输出：\[1,2] 示例 3：

输入：nums = \[3,3], target = 6 输出：\[0,1]

来源：力扣（LeetCode） 链接：[https://leetcode-cn.com/problems/two-sum](https://leetcode-cn.com/problems/two-sum) 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



**3.  思路**

**method 1:  Sorting + binary search**

1. 先把 (nums\[i], i) 的list进行按照element大小排序
2. 然后用binary search方法把left， right pointers从左右两边往中间搜索 target
3. Time : O(n + nlogn), Space: O(n)

**method 2: dictionary **

1. 用dictionary把visited的element进行存放， key= nums\[i], value = i index
2. 把nums array的 element遍历一遍，看看target - nums\[i]有没有visited，如果有就已经找到two sum result， 返回indices
3. Time: O(n) , Space: O(n)

**4.  Coding**

```
# class Solution:
#     def twoSum(self, nums: List[int], target: int) -> List[int]:
#         #
#         #method 1: sorting + binary search
#         #
#         #method 2: dictionary store the visited num as key
#         #
#         #
#         if not nums:
#             return []
#         arr = [ (nums[i], i) for i in range(len(nums))]
#         arr.sort()
#         left, right =0, len(arr)-1
#         while left < right-1:
#             x = arr[left][0] + arr[right][0]
#             if x < target:
#                 left += 1
#             elif x >target:
#                 right -=1
#             else:
#                 return [arr[left][1] , arr[right][1]]

#         if arr[left][0] + arr[right][0] == target:
#             return [arr[left][1] , arr[right][1]]
#         return []

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        if not nums:
            return []
        dic = {}
        for i in range(len(nums)):
            if target - nums[i] not in dic:
                dic[nums[i]] = i
            else:
                return [dic[ target - nums[i]], i]

        return []

```





