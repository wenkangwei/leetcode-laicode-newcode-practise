---
description: sorting, quicksort, mergesort,
---

# 排序

### 1. 题目

给定一个数组，请你编写一个函数，返回该数组排序后的形式。示例1

### 输入

[复制](javascript:void\(0\);)

```
[5,2,3,1,4]
```

### 返回值

[复制](javascript:void\(0\);)

```
[1,2,3,4,5]
```

示例2

### 输入

[复制](javascript:void\(0\);)

```
[5,1,6,2,5]
```

### 返回值

[复制](javascript:void\(0\);)

```
[1,2,5,5,6]
```

### 2. 思路

1. merge sort
2. Time O(nlogn) , O(n) for merging in each level, O(logn) for iterating the whole tree. so O(nlogn)
3. &#x20;Space O(n),  我们要在每一个层把sorted 好的element存起来，然后再去另外一个分支进行divide，最坏情况就是把所有的node的value存起来 O(n)，然后再加上recursion的层数的space O(logn),所有O(n) +O(logn)  = O(n)

```
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
# 将给定数组排序
# @param arr int整型一维数组 待排序的数组
# @return int整型一维数组
#
class Solution:
    def MySort(self , arr ):
        # write code here
        # use merge sort
        # 1. input: arr unsorted,  output: sorted array
        # 2. idea: merge sort
        #     1. divide array into left, right subarray
        #     2. if can not divide, return list of the only one element
        #     3. merge left, right sublist and sort
        #
        # Time: O(nlogn),  O(n) for merging, logn for dividing to recurision
        #Space :O(n)  O(n) for storing merge sorted array
        if not arr:
            return None
        #divide 
        return self.mergesort(arr, 0 , len(arr)-1)
    def mergesort(self, arr, start, end):
        if start >=end:
            return [arr[end]]
        mid = (start +end)//2
        l_arr = self.mergesort(arr, start, mid)
        r_arr =  self.mergesort(arr, mid+1, end)
        #merge 
        return self.merge(l_arr, r_arr)
    def merge(self, l_arr, r_arr):
        if not l_arr:
            return r_arr
        if not r_arr:
            return l_arr
        res = []
        l_pt, r_pt = 0,0
        while l_pt <len(l_arr) and r_pt < len(r_arr):
            if l_arr[l_pt] < r_arr[r_pt]:
                res.append(l_arr[l_pt])
                l_pt += 1
            else:
                res.append(r_arr[r_pt])
                r_pt += 1
        if l_pt < len(l_arr):
            res.extend(l_arr[l_pt:])
        if r_pt < len(r_arr):
            res.extend(r_arr[r_pt:])
        return res
            
```

