---
description: array,; hard;
---

# Trapping water

1\. Link

{% embed url="https://leetcode-cn.com/problems/trapping-rain-water/comments/" %}

{% embed url="https://app.laicode.io/app/problem/522" %}



2\.  题目

522\. Trapping Rain WaterDescriptionNotesHard

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

For example, \
Given `[0,1,0,2,1,0,1,3,2,1,2,1]`, return `6`.

![](https://leetcode.com/static/images/problemset/rainwatertrap.png)

3\. 思路

1. 在array里面 找global max height的index位置&#x20;
2. 定义local max height=A\[0]，从左往global max height的位置走，如果A\[cur] < local max height, 就result加上高度差（水位的深度）。如果A\[cur] >=local max height, 更新local max height,之后继续找高度差
3. 定义 local max height=A\[-1]，从右往global max height的位置走，如果A\[cur] < local max height, 就resut加上高度差（水位的深度）。如果A\[cur] >=local max height, 更新local max height,之后继续找高度差
4. Time Complexity: O(3n) = O(n), Space O(1)

4\. Code

```
class Solution(object):
  def trapWater(self, A):
    """
    input: int[] A
    return: int
    """
    # write your solution here
    # question: input array,  element = height, output: cattle capactiy
    #2. idea
    #  two pointers: from left to right
    #  if the next position height < cur height, set left_pt = cur.
    #  move right_pt to next until height of right_pt >=left_pt
    #  store right pt,  search the capacity between left_pt and right_pt 
    #  update capacity
    #  set left_pt = right_pt
    # case2:  next posti height > cur height, left_pt = right_pt = next position
    # bsae case: len(A) <=2, return 0
    #
    # idea 2： 
    # first find the global max height
    # left pointer from left to global max height
    #   let A[0] = local max height
    #   if pt height < local height
    #      capacity += local height - pt height (depth in one column)
    #   elif pt height > local height:
    #       update local height
    #
    # do the same thing start from right to the global max height position
    #
    if len(A) <=2:
      return 0
    capacity = 0
    global_h_ind = 0
    pt = 1
    while pt < len(A):
      if A[pt] > A[global_h_ind]:
        global_h_ind =pt
      pt += 1
    #from left to global height
    
    local_h = A[0]
    pt = 1
    while pt < global_h_ind:
      if A[pt] < local_h:
        capacity += local_h - A[pt]
      else:
        local_h = A[pt]
      pt += 1
    # from right to gloabl max height
    local_h = A[-1]
    pt = len(A)-2
    while pt > global_h_ind:
      if A[pt] < local_h:
        capacity += local_h - A[pt]
      else:
        local_h = A[pt]
      pt -= 1
    return capacity



    
```

