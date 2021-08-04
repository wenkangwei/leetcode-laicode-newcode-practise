# 删除链表里面重复的元素-1

### 1. Link

{% embed url="https://www.nowcoder.com/practice/71cef9f8b5564579bf7ed93fbe0b2024?tpId=188&tqId=38353&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey" %}





### 2. 题目描述

给出一个升序排序的链表，删除链表中的所有重复出现的元素，只保留原链表中只出现一次的元素。  
例如：  
给出的链表为1 \to 2\to 3\to 3\to 4\to 4\to51→2→3→3→4→4→5, 返回1\to 2\to51→2→5.  
给出的链表为1\to1 \to 1\to 2 \to 31→1→1→2→3, 返回2\to 32→3.  
  
  
示例1

### 输入

[复制](javascript:void%280%29;)

```text
{1,2,2}
```

### 返回值

[复制](javascript:void%280%29;)

```text
{1}
```



### 3. 思路

1. method:  two pointer 快慢指针
2. 先创建一个fake head 它的next = head。因为我们可能会删除list的head
3. 用快慢指针方法，slow = fake\_head, fast= head然后检测fast和fast.next是否存在如果存在，再看看它们的值是否一样。
4. 如果fast.val = fast.next.val 那么就以fast为起始点，用一个temp的指针开始往后遍历直到找到重复的sub-list的最后一个node的next node，并把fast = temp， slow.next = temp 从而跳过duplicate的node
5. 如果fast.val != fast.next.val, 直接把slow， fast都往前移动一位
6.  Time: O\(n\), Space:O\(1\)

### 4. Coding

```text
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

#
# 
# @param head ListNode类 
# @return ListNode类
#
class Solution:
    def deleteDuplicates(self , head ):
        # write code here
        #input: head of linked list
        # output: deduplicated linkedlist
        # idea: two pointer
        # 1. slow: store previous node of the first duplicated node
        # 2. fast store the last duplicated node
        # 3.so we need fake_node and start from fake node so that we can
        #   easily remove the head node if necessary
        # 4. iterate the whole list using fast
        # 5. when we find fast.val == fast.next.val
        #    use temp = fast.next and find the end of the duplicated string
        #    then let slow.next = temp, fast = temp to skip duplicated elements
        
        #bsae case:
        if not head:
            return head
        slow = fake_head = ListNode(None)
        fake_head.next = head
        fast = head
        while fast and fast.next:
            if fast.val == fast.next.val:
                cur = fast.next
                while cur and cur.val == fast.val:
                    cur = cur.next
                slow.next = cur
                fast = cur
            else:
                slow =  slow.next
                fast = fast.next
        return fake_head.next
                
        
```













