---
description: '难度: Medium, 公司: 字节 研发 facebook'
---

# Merge intervals

### 1. Links

牛客： [https://www.nowcoder.com/practice/69f4e5b7ad284a478777cb2a17fb5e6a?tpId=188&tqId=38073&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey](https://www.nowcoder.com/practice/69f4e5b7ad284a478777cb2a17fb5e6a?tpId=188&tqId=38073&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey)



### 

### 2.题目描述

给出一组区间，请合并所有重叠的区间。请保证合并后的区间按区间起点升序排列。示例1

### 输入

[复制](javascript:void%280%29;)

```text
[[10,30],[20,60],[80,100],[150,180]]
```

### 返回值

[复制](javascript:void%280%29;)

```text
[[10,60],[80,100],[150,180]]
```

### 3.思路

1. 先把intervals按照start 进行排序
2. two points method: slow pt 指向要返回的list的最后一个interval， fast pt用于遍历查找下一个interval
3. 对比slowpt interval和fast pt的interval

   1. case 1:  if slow interval.end  &gt; fast.interval. end -&gt;continue
   2. case 2: if slow interval.end &gt; fast. interval .start and slow interval end &lt; fast interval.end -&gt; merge intervals by updating slow.interval end = fast.interval.end
   3. case 3: if slow interval end &lt; fast interval.start -&gt; slow += 1, copy fast interval to slow
   4. move fast forward by 1

4. Time: O\(nlogn\) for sorting  +O\(n\) for merging. Space: O\(1\)

### 4. Code

```text
# class Interval:
#     def __init__(self, a=0, b=0):
#         self.start = a
#         self.end = b

#
# 
# @param intervals Interval类一维数组 
# @return Interval类一维数组
#
class Solution:
    def merge(self , intervals ):
        # write code here
        # question: merge two regions if two regions overlap, input: list of interval
        #  output: list of interval after merge ascending 
        # idea: 
        # 1.  sort list based on interval start
        # 2. case 1: the end of the last interval in res > the start of the first interval in intervals
        #    case 2: the end of the last interval in res > the end of the first interval in intervals
        # Time: O(nlogn) sorting, O(n) merging -> O(nlogn +n), O(n) for popping
        #    s O(n^2)
        #base case:  intervals length <2: return intervals
        # test case: 
        #[[10,30],[20,60],[80,100],[150,180]]
#         if not intervals or len(intervals)<=1:
#             return intervals
        
#         intervals.sort(key = lambda x : x.start)
#         inv = intervals.pop(0)
#         res = [Interval(inv.start, inv.end)]
#         while intervals:
#             if intervals[0].start <= res[-1].end and intervals[0].end <= res[-1].end:
#                 intervals.pop(0)
#             elif intervals[0].start <= res[-1].end and intervals[0].end > res[-1].end:
#                 res[-1].end = intervals[0].end
#                 intervals.pop(0)
#             else:
#                 inv = intervals.pop(0)
#                 res.append(Interval(inv.start, inv.end))
#         return res
    
        #Method 2: use pointer rather than popping of element
        #    
        #
        #
        if not intervals or len(intervals)<=1:
            return intervals
        # O(nlogn)
        intervals.sort(key = lambda x : x.start)
        slow, fast = 0,0
        #O(n)
        while fast< len(intervals):
            if intervals[slow].end > intervals[fast].end:
                fast += 1
            elif intervals[slow].end <= intervals[fast].end and intervals[slow].end >= intervals[fast].start:
                # merge if overlap
                intervals[slow].end  = intervals[fast].end
                fast += 1
            else:
                slow += 1
                intervals[slow] = intervals[fast]
                fast += 1
        return intervals[:slow+1]
                
                
            
        
        
```

```text
    
class Solution:
    # array 二维列表
    def Find(self, target, array):
        #
        #input: target int, 2D array,  output: boolean 
        # idea: binary search
        # search from upper left corner
        # since element < cur on the left, >cur on the bottom, so
        # check current element:
        #  case 1: if current clement < target ->search bottom
        # case 2: if current elemtn > target -> search left side
        if not array or not array[0] or target == None:
            return False
        
        r, c = 0, len(array[0])-1
        while c >=0 and r < len(array):
            if array[r][c] <target:
                r += 1
            elif array[r][c] >target:
                c -= 1
            else:
                return True
        return False
```

