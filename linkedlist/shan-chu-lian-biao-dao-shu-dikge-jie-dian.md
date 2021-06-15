# 删除链表倒数第k个节点

### 1. Link

{% embed url="https://www.nowcoder.com/practice/f95dcdafbde44b22a6d741baf71653f6?tpId=188&&tqId=38587&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking" %}



### 2. 描述

给定一个链表，删除链表的倒数第 nn 个节点并返回链表的头指针  
例如，给出的链表为: 1\to 2\to 3\to 4\to 51→2→3→4→5, n= 2n=2.  
删除了链表的倒数第 nn 个节点之后,链表变为1\to 2\to 3\to 51→2→3→5.  
备注：题目保证 nn 一定是有效的  
请给出请给出时间复杂度为\ O\(n\) O\(n\) 的算法

### 示例1

输入：

```text
{1,2},2    
```

复制返回值：

```text
{2}
```





### 3. 描述

1. 找linkedlist的长度和 计算数第k个node的正向数的位置p
2. 找到p-1位置的node，并把p后面的next接到p-1的node。如果p node不存在，就跳过

### 4. Coding

```text
class Solution:
    def removeNthFromEnd(self , head , n ):
        # write code here
        # input: head of list , n: the last n node
        # idea:
        # 1. find the length of linkedlist 
        # 2. find the position of the last n node
        # 
        #
        #
        #
        if not head or not n:
            return head
        # find the position of the last n node
        cnt = 0
        cur = fake_head = ListNode(None)
        fake_head.next = head
        while cur:
            cur = cur.next
            cnt += 1
        pos = cnt - n
        if pos <0:
            return head
        # find the n node
        cnt = 0
        cur = fake_head
        while cur:
            if cnt  == pos -1 and cur.next:
                cur.next = cur.next.next
                break
            cur = cur.next
            cnt += 1
        return fake_head.next
            
            
        
          
```

