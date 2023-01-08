# 滑动窗口最大值

### 1. Links

### 2.题目描述

给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {\[2,3,4],2,6,2,5,1}， {2,\[3,4,2],6,2,5,1}， {2,3,\[4,2,6],2,5,1}， {2,3,4,\[2,6,2],5,1}， {2,3,4,2,\[6,2,5],1}， {2,3,4,2,6,\[2,5,1]}。窗口大于数组长度的时候，返回空\
示例1

### 输入

复制

```
[2,3,4,2,6,2,5,1],3
```

### 返回值

复制

```
[4,4,6,6,6,5]
```

3\. Idea&#x20;

1 heap method

Code:  Heap

```
# -*- coding:utf-8 -*-
class Solution:
    def maxInWindows(self, num, size):
        # write code here
        # input: num: array, size= window 
        # base case: num: none-> none
        #  window<=1: reutnr num
        #  window > num length: return [array max ]
        #1. idea
        #   simplest:  one pointer 
        #      iterate array using one pointer until pt + size-1 >=len(num):
        #        use sliding to extract max value in window and append it to res array
        #  Time: O(n) iterate * O(n) -> O(n)  Space: O(m), m= number of window
        #
        # idea 2:
        # max heap = window or min heap with inverse value
        # heap top  = current max
        # heap store (value, index)
        # one pointer : pt
        # move pt until pt+ size-1 >=len(num):
        #   add new element (value, index) to max heap
        #   check top element in heap index inside [pt, pt+ size-1] or not?
        #   if os, add top element to res
        #    else: pop top of heap until element insize this range
        # k =size
        # Time: O(klogk) + O(n) * O(2logk) -> O(klogk + n*logk)
        # space: O(n) for storing element in heap
        #
        # idea 3: monontonic queue 单调队列
        # window = 一个单调队列  i< j  q[i] < q[j] 存放 value, index
        # 每次move by1 时 对比 新进来的元素和queue的最后一个元素(max value)对比
        # if new element > max value, then append it to queue , then check if
        # the first element in queue is still inside range of window, if not pop
        if len(num)==0 or size <=0 or size > len(num):
            return []
        if size ==1 or len(num)==1:
            return num
        
        import heapq
        heap = [ [-num[i], i] for i in range(size)]
        heapq.heapify(heap)
        res = [-heap[0][0]]
        i = 1
        while i+size-1 < len(num):
            heapq.heappush(heap, [ -num[i+size-1], i+size-1])
            while len(heap)>0 and not (heap[0][1] >= i and heap[0][1]<=i+size-1):
                heapq.heappop(heap)
            if len(heap)>0:
                res.append(-heap[0][0])
            # move forwar dwinow
            i += 1
        return res
            
            
        
        
        
       
```

2\. monotonic queue 单调队列用于window
