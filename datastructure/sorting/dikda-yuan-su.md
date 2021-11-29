# 第K大元素

### 1. Link

[https://www.nowcoder.com/practice/e016ad9b7f0b45048c58a9f27ba618bf?tpId=188&\&tqId=38572\&rp=1\&ru=/ta/job-code-high-week\&qru=/ta/job-code-high-week/question-ranking](https://www.nowcoder.com/practice/e016ad9b7f0b45048c58a9f27ba618bf?tpId=188&\&tqId=38572\&rp=1\&ru=/ta/job-code-high-week\&qru=/ta/job-code-high-week/question-ranking)

### 2. 描述

有一个整数数组，请你根据快速排序的思路，找出数组中第K大的数。

给定一个整数数组a,同时给定它的大小n和要找的K(1<=K<=n)，请返回第K大的数(包括重复的元素，不用去重)，保证答案存在。

### 示例1

输入：

```
[1,3,5,2,2],5,3
```

复制返回值：

```
2
```

复制

### 示例2

输入：

```
[10,10,9,9,8,7,5,6,4,3,4,2],12,3
```

复制返回值：

```
9
```

复制说明：

```
去重后的第3大是8，但本题要求包含重复的元素，不用去重，所以输出9  
```



### 3. 思路

1. method 1: quickselect
   1. Time: O(nlogn) if array is not almost sorted,  Otherwise, O(n^2)
   2. Space: O(1), inplace
2. method 2: heapsort
   1. Time: O(Nlogk)
   2. Space: O(k) for heap storage

### 4. Coding

1. QuickSelect （quicksort ） method

```

class Solution:
    def findKth(self, a, n, K):
        #
        # method 1: quickselect
        # method 2: heapsort
        # input: a, n, k
        #
        #
        if K >n or not a:
            return None
        return self.quickselect(a, 0, len(a)-1, K)
    def quickselect(self, arr, start, end, k):
        #
        #1. partition arr into two part
        # 2. get the pos of pivot
        # 3. goto left or right  array to find the k^th element
        #
        
        #binary search
        while start <=end-1:
            pivot = self.partition(arr, start,end)
            if pivot <k-1:
                start = pivot+1
            elif pivot>k-1:
                end = pivot -1
            else:
                return arr[pivot]
        return arr[end]
        
        
        
    def partition(self, arr, start, end):
        if start >= end:
            return end
        pivot = end
        stored_idx = start # store the first element that is greater than pivot
        pt = start
        while pt <end:
            # since it is Kth largest, so sort it in descending order
            if arr[pt] >= arr[pivot]:
                arr[pt], arr[stored_idx] = arr[stored_idx], arr[pt] 
                stored_idx += 1
            pt += 1
        arr[stored_idx], arr[pivot] =arr[pivot], arr[stored_idx]
        return stored_idx
        
        

```

2\. Heap Sort method

```
class Solution:
    def findKth(self, a, n, K):
        # method2: heap sort
        # 1. build a min-heap using first K element in array
        # 2. use min-heap to pop the minimum element repeatedly until
        #   the heap contain only K leement. Then the last element as top k largest
        #
        #
        # Time: O(klogk + (n-k)logk + logk) = O(nlogk)
        # Build heap: O(klogk) for build heap, while loop O((n-k)logk), O(logk) for return result
        # 
        import heapq
        if K >n and not a:
            return None
        heap = [a[i] for i in range(K)]
        heapq.heapify(heap)
        pt = K
        while pt < n:
            
            heapq.heappush(heap, a[pt])
            heapq.heappop(heap)
            pt += 1
        
        # return the minimum value in the K largest values of array
        # that is TopK largest 
        return heapq.heappop(heap)

```





