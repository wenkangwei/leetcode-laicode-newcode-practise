# Merge Two Array in-place

### 1. Link

{% embed url="https://www.nowcoder.com/practice/89865d4375634fc484f3a24b7fe65665?tpId=191&tqId=36145&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-algorithm%2Fquestion-ranking&tab=answerKey" %}



### 2. 题目描述

给出两个有序的整数数组 ![](https://www.nowcoder.com/equation?tex=A%20%5C)和 ![](https://www.nowcoder.com/equation?tex=B%5C)，请将数组 ![](https://www.nowcoder.com/equation?tex=B%5C)合并到数组 ![](https://www.nowcoder.com/equation?tex=A%5C)中，变成一个有序的数组  
注意：  
可以假设 ![](https://www.nowcoder.com/equation?tex=A%5C)数组有足够的空间存放 ![](https://www.nowcoder.com/equation?tex=B%5C)数组的元素， ![](https://www.nowcoder.com/equation?tex=A%5C)和 ![](https://www.nowcoder.com/equation?tex=B%5C)中初始的元素数目分别为 ![](https://www.nowcoder.com/equation?tex=m%5C)和 ![](https://www.nowcoder.com/equation?tex=n%5C)



### 3. 思路

1.  倒过来从尾部的元素开始对比A和B的最后一个元素
2. 把A和B的尾部的更大的元素添加到A array的尾部，因为A的array最后的n个位置是空的前m个位置才是A的元素
3. 不断重复知道A或B的元素被遍历完为止， 之后把A或B没有遍历的剩下的元素加到A array前面

### 4. Coding

```text
#
# 
# @param A int整型一维数组 
# @param B int整型一维数组 
# @return void
#
class Solution:
    def merge(self , A, m, B, n):
        # write code here
        if not A and not B: 
            return []
        elif not A: 
            return B
        elif not B: 
            return A
        # start from the end of array
        while m > 0 and n > 0:
            if A[m-1] >= B[n-1]:
                A[m+n-1] = A[m-1]
                m-=1
            else:
                A[m+n-1] = B[n-1]
                n-=1
        if n > 0: 
            A[:n] = B[:n]
        return A
```

