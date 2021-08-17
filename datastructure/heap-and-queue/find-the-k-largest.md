# Find the K-Largest

### 1. Link

{% embed url="https://www.nowcoder.com/practice/e016ad9b7f0b45048c58a9f27ba618bf?tpId=188&&tqId=38572&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking" %}

### 2. 描述

有一个整数数组，请你根据快速排序的思路，找出数组中第 ![](https://www.nowcoder.com/equation?tex=K%5C)大的数。给定一个整数数组 ![](https://www.nowcoder.com/equation?tex=a%5C),同时给定它的大小n和要找的 ![](https://www.nowcoder.com/equation?tex=K%281%5Cleq%20K%5Cleq%20n%29%5C)，请返回第 ![](https://www.nowcoder.com/equation?tex=K%5C)大的数\(包括重复的元素，不用去重\)，保证答案存在。要求时间复杂度 ![](https://www.nowcoder.com/equation?tex=O%28n%29%5C)

### 示例1

输入：

```text
[1,3,5,2,2],5,3
```

复制返回值：

```text
2
```

复制

### 示例2

输入：

```text
[10,10,9,9,8,7,5,6,4,3,4,2],12,3
```

复制返回值：

```text
9
```

复制说明：

```text
去重后的第3大是8，但本题要求包含重复的元素，不用去重，所以输出9
```



### 3. 思路

1. method 1: heapsort

   1. use a min heap to store K elements
   2. continue appending elements to heap and then pop the minimum one and keep the largest K elements
   3. when we store the last K elements in heap, just pop the minimum one and return then it is the top K largest element
   4. Note: we need to first append next element into heap to have k+1 element , then pop the smallest one. **Otherwise, it is possible that we pop the K^th largest element from heap, but insert the next element smaller than the top K^th largest element to heap. This is wrong, as we miss the K^th largest element**

   \*\*\*\*

2. method 2: quick select 
   1. randomly pick pivot
   2. partition array by pivot and then return  pivot position
   3. compare pivot position with K
      1. if pos &gt;k: search left space of pivot recursively
      2. if pos &lt; k: search right space of pivot recursively
      3. otherwise: return pos and a\[pos\]





### 4. Coding

method 1: Heap Sort

```text
class Solution:
    def findKth(self, a, n, K):
        #
        # method 1. heap sort 
        # 1. use a min heap to store K elements
        #    continue appending elements to heap and pop the minimum element
        #    and keep the largst k-1 element
        # 2. when we append the last elemnt in list to the heap, then the minimum elment in heap
        #    is hte top K largest element  in list
        # 3. base case: when list has length < K, return None, doesn't exist top K largest element
        # Time: O(n+k + (n-k)log(k))
        import heapq
        
        if not a or  n< K :
            return None
        i = 0
        heap = []
        
        # time : O(k)
        while i < K:
            heap.append(a[i])
            i += 1
        # time: O(n)
        heapq.heapify(heap)
        # time: O((n-k)log(k) )
        res = None
        # Note: here we need to push the next element before poping the smallest one
        # since we want to keep the largest K elements
        # if we first pop then push, it is possible that we pop the K largest element and 
        # then push the smaller one into heap, so that we miss the the K largest
        
        while i < n:
            heapq.heappush(heap, a[i])
            heapq.heappop(heap)
            
            i += 1
        
        return heapq.heappop(heap)

```

method 2: QuickSelect, based on Quick Sort

```text

```











