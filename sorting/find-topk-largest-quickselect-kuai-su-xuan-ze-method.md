---
description: Medium; quickselect; 腾讯；
---

# Find TopK largest- QuickSelect快速选择 method

### 1. Link

[https://www.nowcoder.com/practice/e016ad9b7f0b45048c58a9f27ba618bf?tpId=117&tqId=37791&companyId=138&rp=1&ru=%2Fcompany%2Fhome%2Fcode%2F138&qru=%2Fta%2Fjob-code-high%2Fquestion-ranking&tab=answerKey](https://www.nowcoder.com/practice/e016ad9b7f0b45048c58a9f27ba618bf?tpId=117&tqId=37791&companyId=138&rp=1&ru=%2Fcompany%2Fhome%2Fcode%2F138&qru=%2Fta%2Fjob-code-high%2Fquestion-ranking&tab=answerKey)

### 2. 题目描述

有一个整数数组，请你根据快速排序的思路，找出数组中第K大的数。

给定一个整数数组a,同时给定它的大小n和要找的K\(K在1到n之间\)，请返回第K大的数，保证答案存在。示例1

### 输入

[复制](javascript:void%280%29;)

```text
[1,3,5,2,2],5,3
```

### 返回值

[复制](javascript:void%280%29;)

```text
2
```



### 3. 思路

1. 像quicksort 一样先把pivot选出来，然后partition使pivot左边的数字大于pivot，右边的小于pivot （这里是从大到小排）
2. 把pivot的position和 K-1对比（因为index是从0开始的），如果pivot index &gt; K-1, 那么end = pivot -1. 如果pivot index &lt; K-1,  那么start = pivot +1.如果一样，那就返回pivot对应的element
3. Time Complexity:   worst case O\(n^2\), average case O\(nlogn\). Space: \(1\) 没有用recursion

### 4.Code

```text
# -*- coding:utf-8 -*-

class Solution:
    def findKth(self, a, n, K):
        # write code here
        # quick select idea:
        # quick sort:
        # 1. pick pivot and move element  pivot to left to pivot, element > pivot, right ot pivot
        # 2. recursion step 1 to go throught the left subarray, right subarray
        # 3. if no longer partition, return 
        #
        # quicksort:
        # In partition part, check if K position in left or right subarray
        # then only consider that subarray include K and recursion on this subarray
        # until subarray contains one element, then that is K largest element
        #
        #base case: if K >n: return None, a == None, reutrn None
        # Time: O(n^2) if array is almost sorted and recursion becomes list. O(nlogn) if not sorted
        # SPace  O（1） work in-place without recursion
        #
        #
        if not a or n< K:
            return None
        return self.quickselect(a, n, 0, n-1,K)
    
    def quickselect(self, a, n, start, end, K):
        
        while start <= end-1:
            pivot =  self.partition(a, start, end)
            if K-1  <pivot:
                end = pivot-1
            elif K-1 >pivot:
                start = pivot+1
            else:
                return a[pivot]
        return a[end]
            
    def partition(self, a, start, end):
        if start >= end:
            return end
        pivot_idx = end
        fast_pt, stored_idx = start, start
        while fast_pt < end:
            if a[fast_pt] >= a[pivot_idx]:
                a[fast_pt], a[stored_idx] = a[stored_idx], a[fast_pt]
                stored_idx += 1
            fast_pt += 1
        a[pivot_idx] , a[stored_idx] = a[stored_idx], a[pivot_idx] 
        l_s = start
        l_e = stored_idx -1
        r_s = stored_idx +1
        r_e = end
        return stored_idx
```

