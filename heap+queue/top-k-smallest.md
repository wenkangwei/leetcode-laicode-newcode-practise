# Top-K smallest

### 0. Links

牛客： [https://www.nowcoder.com/practice/6a296eb82cf844ca8539b57c23e6e9bf?tpId=188&tqId=38031&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey](https://www.nowcoder.com/practice/6a296eb82cf844ca8539b57c23e6e9bf?tpId=188&tqId=38031&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey)

laicode K-largest: [https://app.laicode.io/app/problem/436](https://app.laicode.io/app/problem/436)

### 1. 题目

### 题目描述

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

### 2. 思路

a.方法1： heap，用max heap \(min heap + 把value 取反reverse\)思路

    1. 先把前k个element build heap

    2. 在剩下的element里面不断把element 取反并加入heap，heap每次把当前最小（取反最大）的数pop出去，剩下的heap里面的k element就是当前最大\(加负号取反后最小的k个数\)

   3. Time: O\(k\) for popping top k elements to list, O\(klogk\) for build heap,  O\(\(n-k\)logk\) for updating heap, O\(k\) for reversing list and return result.  So  O\(k+ nlogk\)

4. Space: O\(k\) for heap

    

方法2：直接先通过min heap把整个list heapify，再pop到剩下k个element

Time:  O\(n +klogn\) in total, O\(n\) for building heap, O\(klogn\) popping 最小的k个element

Space: O\(n\)

方法3： quickselect



### 3.Coding

```text
import heapq
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        # base case: when k > tinput or k<1: return None
        # method 2 : max-heap with k element
        #   pop the top k element to heap and then heapify max heap with K element
        #    iterate the n-k element
        #        if element > the top element in heap / max element in heap, skip
        #           else: pop the max element and append current element
        #  Finally  we find the K smallest element in heap only
        # 
        # Time O(klogk) for heapify  + O((n-k)logk ) for popping and inserting element
        #  SO  O(nlogk) in total
#       #
        # Space:  O(k) for heap
        #
        #
        #
        if not tinput or (k > len(tinput) or k<1):
            return []
        #O(k)
        heap = []
        for i in range(k):
            heap.append(-tinput.pop())
        # O(klogk)
        heapq.heapify(heap)
        # O((n-k)logk)
        for i in range(len(tinput)):
            if -tinput[i] > heap[0]:
                heapq.heappop(heap)
                heapq.heappush(heap,-tinput[i])
        #O(klogk)
        res = []
        while len(heap):
            res.append(-heapq.heappop(heap))
        # O(n)
        res.reverse()
        return res
            
```



