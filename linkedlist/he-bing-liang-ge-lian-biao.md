# 合并两个链表



**1. Link**

{% embed url="https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/" %}



**2. 剑指 Offer 25. 合并两个排序的链表**

难度简单117收藏分享切换为英文接收动态反馈

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

**示例1：**

```text
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```



**3. 思路: 两个指针**

1.  用两个指针分别用于遍历两个lists
2. 创建一个fake\_head node它的next永远指向于merged list的头部。这样可以方便我们在创建merged list的head的时候，不用特意增加代码去判断头部从哪个list提取。之后用一个pointer， cur 去遍历merged list
3. 在while loop里面不断对比两个pointer的node的value，并把value小的那个node加到cur的next，之后把pointer向前移动一位
4. 当其中一个list被遍历完后，把另外没有遍历完的list的剩下部分加到merged list尾巴
5. Time complexity:   O\(n\) 因为是对linkedlist 线性遍历。 Space: O\(1\)

**4. Coding**

```text
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        #
        # two pointers methods:
        # idea: 
        # 1. two pointers pointing to the head of two list, and create a fake head
        #    used as the previous node of head of merge list
        #    use one pointer to iterate the new list
        # 2. loop until one of two pointers is None, 
        #    each time extract and add the smaller node to the next of the end of new list
        # 3. when one of pointers is none, append the remaining unchecked list to the end of new list   
        #    return new list

        pt1, pt2 = l1, l2
        fake_head = ListNode(None)
        cur = fake_head
        while pt1 and pt2:
            # extract node with smaller value to the end of merged list
            if pt1.val < pt2.val:
                cur.next = pt1
                pt1 = pt1.next
            else:
                cur.next = pt2
                pt2 = pt2.next
            # move forward
            cur = cur.next
        # append the remaining list
        if pt1:
            cur.next = pt1
        if pt2:
            cur.next = pt2
        return fake_head.next

```









