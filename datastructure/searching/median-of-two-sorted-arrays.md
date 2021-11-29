---
description: Binary Search; Hard;
---

# Median of Two Sorted Arrays

### 1. Link

{% embed url="https://www.nowcoder.com/practice/6fbe70f3a51d44fa9395cfc49694404f?tpId=188&&tqId=38639&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking" %}

### 2. 描述: NC36 在两个长度相等的排序数组中找到上中位数

给定两个有序数组arr1和arr2，已知两个数组的长度都为N，求两个数组中所有数的上中位数。上中位数：假设递增序列长度为n，若n为奇数，则上中位数为第n/2+1个数；否则为第n/2个数\[要求]时间复杂度为O(logN)O(logN)，额外空间复杂度为O(1)O(1)

### 示例1

输入：

```
[1,2,3,4],[3,4,5,6]
```

复制返回值：

```
3
```

复制说明：

```
总共有8个数，上中位数是第4小的数，所以返回3。 
```

### 示例2

输入：

```
[0,1,2],[3,4,5]
```

复制返回值：

```
2
```

复制说明：

```
总共有6个数，那么上中位数是第3小的数，所以返回2 
```



### 3. 思路

1. method 1:  merge and sort
   1. &#x20;First merge and sort two array
   2. find the middle element as median of two array
   3. Time: O(n), Space: O(n)
2. method 2:   binary search
   1. &#x20;Define left, right pointers of arr1 = l1, r1 and  left, right pointers of arr2 = l2, r2
   2. &#x20;Since two **sorted arrays** have same length, so first find the middle elements of two arrays (two medians of two array)
   3. if arr1\[mid1] == arr2\[mid2]  ->  two arrays have the same median and this is the global median&#x20;
   4. **if  arr1\[mid1]  > arr2\[mid2], **then arr1\[mid1] is on the right hand side of arr2\[mid2]  in the global array.   That is,  **arr1\[mid1]  >= global median >= arr2\[mid2]**.    So we need to search the left space of arr1\[mid1]  and  the right space of arr2\[mid2].  So let ** l1 = mid1,  r2 = mid2**
   5. **if arr1\[mid1] <  arr2\[mid2],**  similary,  **arr1\[mid1] <= global median <= arr2\[mid2].  So let  r1 = mid1, l2 = mid2.**
   6. repeat step 1\~5  while l1 < r1
   7. Note: &#x20;
      1. when l1=r1-1,  compute middle between  l1, r1 could lead to  dead loop, since  l1 = mid1 always. **When  subarray arr1\[l1:r1+1] has even size, we need l1 = mid1 + 1  and l2 = mid1 + 1**
   8. **Time: O(logn), Space: O(1)**



### 4. Coding

method 1:

```python
class Solution:
    def findMedianinTwoSortedAray(self , arr1 , arr2 ):
        # write code here
        # method 1:  merge and sort to find median
        # Time: O(n ), Sppace: O(n)
        #
        # method 2: binarysearch
        #1. for each array use left, right pointers
        # 2. find median of two arrays
        # 3. compare two median
        #    1. if left median == right median: overall median = left median = right median
        #        since number of element < left median = number of element < right median
        #    2. if left median > right median
        #        it can be left median >= overal median >= right median -> search the 
        #        left space of left median and the right space of right median
        #    3. if left median < right median: reverse case 2
        #    4. we can also see num of element on the left of left median and the left of right median
        #        = num of element on the right of left median and the right of right median
        
        arr = [None]*(len(arr1) + len(arr2))
        pt1, pt2 = 0, 0
        cur = 0
        repeat = 0
        while pt1 < len(arr1) and pt2 < len(arr2):
            if arr1[pt1] < arr2[pt2]:
                arr[cur] = arr1[pt1]
                pt1 += 1
            elif arr1[pt1] >= arr2[pt2]:
                arr[cur] = arr2[pt2]
                pt2 += 1
            cur += 1
            
        while  pt1 < len(arr1):
            arr[cur] = arr1[pt1]
            pt1 += 1
            cur += 1
        while  pt2 < len(arr2):
            arr[cur] = arr2[pt2]
            pt2 += 1
            cur += 1
            
        return arr[(cur-1)//2]
```

method 2:

```python
class Solution:
    def findMedianinTwoSortedAray(self , arr1 , arr2 ):
        if not arr1 or not arr2:
            return None
        if len(arr1) == 1:
            return min(arr1[0], arr2[0])
        
        n = len(arr1)
        l1, r1 = 0, n-1
        l2, r2 = 0, n-1
        while l1 < r1:
            m1 = l1 + ((r1 - l1)>>1)
            m2 = l2 + ((r2 - l2)>>1)
            flag = (r1 - l1 + 1)&1
            if arr1[m1] == arr2[m2]:
                return arr1[m1]
            elif arr1[m1] > arr2[m2]:
                #search left space of m1 and right space of m2
                r1 = m1 
                l2 = m2 if flag else m2 + 1
            else:
                l1 = m1 if flag else m1 +1
                r2 = m2 
        # return the smallest element as median
        return min(arr1[l1], arr2[l2])
    
```





