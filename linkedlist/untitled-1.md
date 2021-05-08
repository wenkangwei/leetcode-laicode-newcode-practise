---
description: Easy; 剑指 Offer 22; Linked List
---

# 链表中倒数第k个节点

**1.** **Link**

{% embed url="https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/" %}



**2. 剑指 Offer 22. 链表中倒数第k个节点**

难度简单188收藏分享切换为英文接收动态反馈

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 `6` 个节点，从头节点开始，它们的值依次是 `1、2、3、4、5、6`。这个链表的倒数第 `3` 个节点是值为 `4` 的节点。

**示例：**

```text
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```



**3. 思路**

1. **方法1：**
   1. **先把list 过一遍找到list length**
   2. **计算正着数的node的position**
   3. **再遍历一遍找到position的node，并返回**
2. **方法2**
   1. **把list直接先reverse**
   2. **正着数第k个node**
   3. **把head 达到这个node的list再reverse一遍并返回**
3. **两种方法都是Time complexity:  O\(n\),  Space complexity:  O\(1\)因为只是单纯把linked list 线性遍历**

**4. Coding**

Method **1**: 

```text
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getKthFromEnd(self, head: ListNode, k: int) -> ListNode:
        #
        #1. method 1:
        #   loop through whole list and get the length of list 
        #   then find the position of the kth node from tail
        #
        #2. method2:
        #  first reverse list
        #  then find the kth element from head
        #  then reverse the sublist to get the Kth From end
        #
        # Two methods have time O(n) and space O(1)
        #base case:
        #   
        if not head:
            return None
        cur = head
        length = 0
        while cur:
            cur = cur.next
            length +=1
        if k > length:
            return None
        pos = length - k
        cur_pos = 0
        cur = head
        while cur_pos!=pos:
            cur = cur.next
            cur_pos += 1
        return cur 

```



Method 2: reverse list

```text
class Solution:
    def getKthFromEnd(self, head: ListNode, k: int) -> ListNode:
        # method 2
        #
        if  not head:
            return None
        cur = new_head = self.reverse(head)
        pos = 1
        while pos != k:
            cur = cur.next
            pos += 1
        cur.next = None
        res = self.reverse(new_head)
        return res


    def reverse(self, node):
        prev, cur = None, node
        while cur:
            next_node = cur.next
            cur.next = prev
            prev = cur
            cur = next_node
        return prev

```

