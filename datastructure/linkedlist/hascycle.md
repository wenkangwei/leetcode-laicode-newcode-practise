---
description: Easy; Linkedlist;
---

# HasCycle

**1. Link**

{% embed url="https://leetcode-cn.com/problems/linked-list-cycle/submissions/" %}

{% embed url="https://www.nowcoder.com/practice/650474f313294468a4ded3ce0f7898b9?tpId=188&tqId=38282&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey" %}

**2. 题目： 141. 环形链表**

难度简单1057收藏分享切换为英文接收动态反馈

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 `true` 。 否则，返回 `false` 。

**进阶：**

你能用 _O(1)_（即，常量）内存解决此问题吗？

**示例 1：**

![](<../../.gitbook/assets/image (7).png>)

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

**3. 思路**



&#x20;**证明快慢指针会相遇**: 重点定义路径的头尾，以及中部(环)

* 例子: 1 -> 2 -> 3 -> 4 -> 2 -> 3 ->4 -> 2 -> 3 -> 4 -> ...
* 假设初始时 slow = 1, fast =2, fast = slow \* 2 (不过 slow= 1, fast=1同样成立 fast = slow \* 2 -1）, 始终有 fast //2 = slow
* 定义list的第一个node到环的第一个node（不包括环的第一个node）距离为: x
* 定义环的第一个node 到相遇点（包括环的第一个node和相遇点）距离为: y
* fast pt 循环了环的次数为: m
* slow pt 循环了环的次数为: n
* 环的node个数: L
* slow pt经过的node的个数 = x(路径头部) + nL + y (路径尾部)
* fast pt经过的node的个数 = x(路径头部) + mL + y (路径尾部，并且节点和slow达到的节点是同一个)
* 问题就是要确认是否有解y使得 2(x + nL + y) = x + mL +y -> x+y(路径头尾相加) = L(m-2n)（环的长度循环的次数）
* 由于 x, y , m, 2n 都是整数，那等式肯定有解决，也就是slow fast肯定相遇
* **综上所说， 用快慢指针同时从头往前移动，当他们相遇就是有loop，如果不相遇，快指针就会指向None,就没有loop**

**4. Coding**

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if not head:
            return False
        fast, slow = head.next, head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if slow == fast:
                return True
        return False
```





