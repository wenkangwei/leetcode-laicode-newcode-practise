---
description: Easy; LinkedList
---

# 链表的共同节点

### 1.Link

{% embed url="https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/submissions/" %}

{% embed url="https://www.nowcoder.com/practice/6ab1d9a29e88450685099d45c9e31e46?tpId=196&tqId=37095&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey" %}

### 2. 题目描述

输入两个链表，找出它们的第一个公共节点。

如下面的两个链&#x8868;**：**

[![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

![](<../../.gitbook/assets/image (8).png>)

在节点 c1 开始相交。

**示例 1**

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```



### 3. 思路

而同一个地址值意味着存储的内容一致，以此往下推，说明从第一个公共结点之后，两链表的所有结点都是一样的。 找到公共节点后，因为节点地址一样，后面连的内容都是一样，剩下的所有节点都是公共点

例子  list1:  1-2-3 6-7-8-9-0   list2:  10-12 -6-7-8-9-0

把他们拼接情况有:

1-2-3 6-7-8-9-0 **+** 10-12 -**6**-7-8-9-0

10-12 -6-7-8-9-0  **+** 1-2-3 **6**-7-8-9-0

其中加粗的6的数字就是第一个公共点。 第一个公共的节点6 很明显前面片段的长度是3+ 5 + 2 或者 2+ 5 + 3 长度一样，只不过不同的片段换了位置。也就是说只要把两个list前后拼接起来就会时如果用两个指针分别从两个list 遍历， 当两个指针相遇时，那个相遇点就是第一个公共点。

Time Complexity: O(n).  Space Complexity: O(1)&#x20;



### 4. Coding

```
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

