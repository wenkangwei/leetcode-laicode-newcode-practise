---
description: 字节，研发
---

# Reverse LinkedList I

**Links:**

牛客：

{% embed url="https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?tpId=188&tqId=38029&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey" %}

{% embed url="https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/" %}



### 1. 题目描述

输入一个链表，反转链表后，输出新链表的表头。示例1

### 输入

[复制](javascript:void\(0\);)

```
{1,2,3}
```

### 返回值

[复制](javascript:void\(0\);)

```
{3,2,1}
```

### 2.思路

1. &#x20;输入： linkedlist head,  输出：  reversed linkedlist head
2.  idea:

    1. 既然是要反转链表的顺序，那么就要遍历每个node并把每个node的next指向prev
    2. 所以需要prev, cur, next 三个pointer
    3. 把cur.next 保存， 把cur的next 指向prev进行reverse
    4. prev 移向cur， cur 移向保存的next进行遍历
    5. 直到cur =none不用返回，就prev 就是new head，返回prev就可以了


3. Time :O(n)   Space O(n)&#x20;

```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回ListNode
    def ReverseList(self, pHead):
        # write code here
        if not pHead:
            return pHead
        prev = None
        cur = pHead
        while cur:
            next_node = cur.next
            cur.next = prev
            prev = cur
            cur = next_node
        return prev
```

