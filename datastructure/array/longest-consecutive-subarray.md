# Longest Consecutive Subarray

### 1. Link

[https://www.nowcoder.com/practice/eac1c953170243338f941959146ac4bf?tpId=188&&tqId=38566&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week/question-ranking](https://www.nowcoder.com/practice/eac1c953170243338f941959146ac4bf?tpId=188&&tqId=38566&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week/question-ranking)



### 2. 描述

给定无序数组arr，返回其中最长的连续序列的长度\(要求值连续，位置可以不连续,例如 3,4,5,6为连续的自然数）

### 示例1

输入：

```text
[100,4,200,1,3,2]
```

复制返回值：

```text
4
```

复制

### 示例2

输入：

```text
[1,1,1]
```

复制返回值：

```text
1
```



### 3. 思路

1. 先排序
2. 如果arr\[i-1\] == arr\[i\]-1, 就len += 1 ,  如果arr\[i-1\] == arr\[i\], 就len = len   Otherwise, 就len = 1 reset
3. 更新最大的max\_len

### 4. Coding

```text

class Solution:
    def MLS(self , arr ):
        # idea
        # 1. sort array
        #  iterate element in array
        #    if arr[i] == arr[i-1] +1 -> len += 1
        #    otherwise: max_len = max(len, max_len), reset len
        arr.sort()
        max_len = 1
        length =1
        for i in range(1, len(arr)):
            if arr[i-1]+1 ==arr[i]:
                length += 1
            elif arr[i-1] ==arr[i]:
                continue
            else:
                length =1
            max_len = max(max_len, length)
        return max_len
        
```



