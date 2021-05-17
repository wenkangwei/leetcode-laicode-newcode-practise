# 最小的k个数

### 1. Link

{% embed url="https://www.nowcoder.com/practice/6a296eb82cf844ca8539b57c23e6e9bf?tpId=188&tqId=38279&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey" %}



### 2. 题目描述

给定一个数组，找出其中最小的K个数。例如数组元素是4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。如果K&gt;数组的长度，那么返回一个空的数组示例1

### 输入

[复制](javascript:void%280%29;)

```text
[4,5,1,6,2,7,3,8],4 
```

### 返回值

[复制](javascript:void%280%29;)

```text
[1,2,3,4]
```



### 3. 思路

method 1:  sorting + select top K

1. 先把array 从小到大排序一遍然后选择前k个值
2. Time: O\(nlogn\) for sorting on average .   Space: O\(n\) 如果用merge sort。 O\(logn\)如果用quicksort recursion method并且recursion tree是balance。如果不是balance，O\(n\)

method 2: heap sorting using min-heap

1. 先用一个size=k的min-heap对input array前k个element的负值进行heapify\(由于python的heap是min-heap, 我们需要用max-heap来找最小的k个值，所以需要取反\)
2. 把剩下的n-k elements 进行insert 到heap里面并把最小的值pop出来
3. 不断重复step2直到n-k个element全部被遍历
4. 把heap剩下的k个element pop 出来并取反得到排序后的k smallest element
5. Time: O\(k + klogk +  \(n-k\)logk + klogk\) = O\(nlogk + k\). Space: O\(k\)

### 4. Coding

```text
import heapq
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        #
        #method 1: sort and then return top k . O(nlogn) for sorting
        #method 2: heap sort using max heap to pop n-k elements return the last k elements
        # 1. heapify the first K element 
        # 2. loop throught the remaining n-k element and pop the 
        # 3. return the last k element
        
        #base case
        if not tinput or (k > len(tinput) or k<1):
            return []
        
        heap = [0]*k
        # O(k)
        for i in range(k):
            heap[i] = -tinput[i]
            
        heapq.heapify(heap)
        # TIme:  O((n-k)logk)
        for i in range(k, len(tinput)):
            if -tinput[i] > heap[0]:
                heapq.heappop(heap)
                heapq.heappush(heap, -tinput[i])
                
        # reverse list
        # klognk
        res = [0]*k
        for i in range(k-1, -1, -1):
            res[i] =  -heapq.heappop(heap)
        return res
        
```





