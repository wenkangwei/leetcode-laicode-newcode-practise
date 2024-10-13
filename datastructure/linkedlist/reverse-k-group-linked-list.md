---
description: 难度：Medium; 类型： linkedlist; 公司：字节研发，华为等
---

# Reverse K-group linked list

### 1. Links

牛客： [https://www.nowcoder.com/practice/b49c3dc907814e9bbfa8437c251b028e?tpId=188\&tqId=38025\&rp=1\&ru=%2Factivity%2Foj\&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking\&tab=answerKey](https://www.nowcoder.com/practice/b49c3dc907814e9bbfa8437c251b028e?tpId=188\&tqId=38025\&rp=1\&ru=%2Factivity%2Foj\&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking\&tab=answerKey)



{% embed url="https://www.nowcoder.com/practice/886370fe658f41b498d40fb34ae76ff9?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

### 2. 题目描述

将给出的链表中的节点每 k k 个一组翻转，返回翻转后的链表 如果链表中的节点数不是 k k 的倍数，将最后剩下的节点保持原样 你不能更改节点中的值，只能更改节点本身。 要求空间复杂度  O(1) O(1) 例如： 给定的链表是1→2→3→4→5 对于  k = 2 k=2, 你应该返回  2→1→4→3→5 对于  k = 3 k=3, 你应该返回3→2→1→4→5\
示例1

### 输入

复制复制

```
{1,2,3,4,5},2
```

### 返回值

复制复制

```
{2,1,4,3,5}
```



### 3.思路： 分两步找每个group，以及把group翻转

1. 找linkedlist的每个group的head和end
   1. 由于reverse后的linkedlist的head会变动，需要先fakehead使fakehead的next始终是list的head
   2. 之后用two pointer的方法，slowpointer指向group的head的前一个node，而fast 指向group的end在把fast不断向前移动时用counter记录它的位置来如果counter %k=0那么就是group的end
   3. 把group先cut出来进行reverse，然后返回新的group head 和end，再把它接上去原来的list
   4. 把 slow更新到fast
2. 把每个group的head 和end的部分进行reverse
   1. 因为要对每一个node的next进行翻转，需要cur pointer 遍历每个node
   2. 需要prev保存cur的上一个node翻转，同时也要next保持cur的下一个node用于先前移动
   3. new tail就是原来的list的head， new head就是最后的prev

### 4. Code

```
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

#
# 
# @param head ListNode类 
# @param k int整型 
# @return ListNode类
#
class Solution:
    def reverseKGroup(self , head , k ):
        # write code here
        #input: head of linkedlist,  k of group size.  output: new head of reverse linkedlist
        #idea: 用two pointer method
        # 物理意义： slow = head, fast = end of sub list
        # 由于head会变动，不利于换行更新后的head，所以要加一个fakehead使得fakehead后面的next是新的head
        # fast从左往右数计算当前的position% k来看 fast是不是一个size of k的group的end
        # if so, reverse slow , fast 之间的sub link list
        #  then slow = fast, 更新下一个group的head
        #  base case: head none, return head no reverse
        # Time: O(n) for iterate every node, O(k) for reverse group
        #  so O(kn)
        # Space: O(1)
        if not head or k <=1:
            return head
        fake_head = ListNode(None)
        fake_head.next = head
        slow = fake_head
        fast = slow
        cnt = 0
        while fast:
            if cnt!= 0 and cnt%k==0:
                # cut sublist
                next_head = fast.next 
                subls_head = slow.next
                fast.next = None
                new_head, new_tail = self.reverse(slow.next)
                # connect
                new_tail.next = next_head
                slow.next = new_head
                # update
                fast = new_tail
                slow = fast
            # update position
            fast = fast.next
            cnt += 1
        return fake_head.next
    def reverse(self, node):
        prev = None
        cur = node
        tail = node
        while cur:
            # backup and reverse
            next_node = cur.next
            cur.next = prev
            # move forward
            prev = cur
            cur = next_node
        new_head= prev
        return new_head, tail
```
