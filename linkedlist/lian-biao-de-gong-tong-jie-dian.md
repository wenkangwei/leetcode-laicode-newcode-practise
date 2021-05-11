# 链表的共同节点

### 1.Link

### 

### 2. 题目描述

输入两个链表，找出它们的第一个公共结点。（注意因为传入数据是链表，所以错误测试数据的提示是用其他方式显示的，保证传入数据是正确的）



### 3. 思路

### 4. Coding

```text
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def FindFirstCommonNode(self, pHead1, pHead2):
        # write code here
        #  3-4-5-1-4-6
        #  2-1-3-5-7
        #idea:
        # 而同一个地址值意味着存储的内容一致，以此往下推，说明从第一个公共结点之后，两链表的所有结点都是一样的。
        # 找到公共节点后，因为节点地址一样，后面连的内容都是一样，剩下的所有节点都是公共点
        # 也就是
        #例子     1-2-3   6-7-8-9-0
        #        10-12  -6-7-8-9-0
        #
        # 把他们拼接情况有
        #1-2-3   6-7-8-9-0 10-12  -6-7-8-9-0
        #10-12  -6-7-8-9-0 1-2-3   6-7-8-9-0
        #
        #相交的节点6 很明显前面的长度是3+ 5 + 2   或者 2+ 5 + 3 长度一样，只不过不同的片段换了位置
        # 第一个节点始终能在同一位置相交
        pt1 = pHead1
        pt2 = pHead2
        while pt1 != pt2:
            if not pt1:
                pt1 = pHead2
            else:
                pt1= pt1.next
            if not pt2:
                pt2 = pHead1
            else:
                pt2 = pt2.next
        return pt1
        
        
```



