---
description: LinkedList; Easy
---

# DetectCycle II

**1. Link**

{% embed url="https://leetcode-cn.com/problems/linked-list-cycle-ii/" %}





**2. 题目**

**142. 环形链表 II**

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。

说明：不允许修改给定的链表。

进阶：

你是否可以使用 O(1) 空间解决此题？

示例 1：

![](<../../.gitbook/assets/image (9).png>)

输入：head = \[3,2,0,-4], pos = 1 输出：返回索引为 1 的链表节点 解释：链表中有一个环，其尾部连接到第二个节点。

来源：力扣（LeetCode） 链接：[https://leetcode-cn.com/problems/linked-list-cycle-ii](https://leetcode-cn.com/problems/linked-list-cycle-ii) 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



**3. 思路**

1. &#x20;用快慢指针的方法
2. 先用slow, fast pointer 把linkedlist遍历，fast pointer遍历两步，slow pointer遍历一步。假设 X = head 到 cycle第一个点的距离， Y=slow fast pointer相遇的点到cycle第一个点的距离，  C= 1个cycle展开后的长度。 那么 head到相遇点(slow)的距离 = X + m \* C +Y.   head 到fast所在的相遇点的距离 = X + n \* C + Y = 2\*( X + m \* C +Y)。 当这个等式的解为整数时，那么就是slow和fast会相遇，即有loop
3. 在找到相遇点后，用2个slow pointers， slow1, slow2 分别从head和相遇点出发。当他们再次相遇时，新的相遇点就是cycle的第一个点。
4. 这是因为head到fast和slow的相遇点的距离 = X + m\*C +Y,   而 slow所在的相遇点到fast所在的相遇点距离 = Z + m\*C +Y。 而事实上Z就是 相遇点到cycle第一个点的距离， Z=X。 当 slow1, slow2同时向前移动，他们的相遇点就是经过距离Z后的第一个点，即环的入口点
5. Time Complexity: O(n+k), n=slow和fast相遇点到head的距离， k=head 到环入口的距离。  Space Complexity: O(1)
6. 例子：

带cycle的linked list展开之后的形式：

1->2 -> 3 **->4 (slow) ->**&#x35; ->3->**4** (fast)->5 ->3->4->5....

这里的环是3， 4， 5 所以 X= 2, C=3, fast 和slow的相遇点是4 而环的入口是3，所以Y =1。 那么slow1, slow2分别从1 和4(slow)开始前进， X=Z = 2。 当两个pointer到3时就相遇，即环的入口



**4. Coding**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        # input: head of linked list
        # output: entry of the loop
        # idea:
        #   1. check if there is a loop. If so, find the node that slow pt meet fast pt
        #   X: distance from head to entry of loop, Y distance from entry of loop to meeting point
        #   C = lenght of cycle
        #   then we know that distance from slow to the head = X+ m*C+y
        #   distance from fast to the head = X+ n*C+y
        #   Then we have 2*(X+ m*C+y) =  X+ n*C+y. When it has solution, there is a loop
        #
        #   2. use two slow pointers start from head and the meeting point
        #     distance from head to meeting point = X+ m*C+y
        #     distance from meeting point to fast = Z + m*C+y =X+ n*C+y - ( X+ m*C+y) .  Z is the distance from meeting point to entry and Z = X
        #     when using two slow pointer, it is actually  finding X and Z
        #
        if not head:
            return None
        fast = head
        slow = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            # if loop exist, then find the entry of loop
            if slow == fast:
                slow1 = head
                slow2 = slow
                while slow1!=slow2:
                    slow1 = slow1.next
                    slow2 = slow2.next
                return slow1 
        return None  
            
```





