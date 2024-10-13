# 合并k个已排序的链表

{% embed url="https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

```python3

import queue
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
#
# 
# @param lists ListNode类一维数组 
# @return ListNode类
#
class Solution:
    def mergeKLists(self , lists: List[ListNode]) -> ListNode:
        # time complexity: Nlog(K), 遍历所有node 就是N， 每次插入元素优先级队列排序是logk
        # 所以是O(nlogk)
        # write code here
        from queue import PriorityQueue
        qu = PriorityQueue()
        # each list put the first element to queue
        for i in range(len(lists)):
            if lists[i] !=None:
                qu.put([lists[i].val, i])
                lists[i]= lists[i].next
        head = ListNode(None)
        pt = head
        # 每pop一个元素，就插入一个新元素到queue
        while not qu.empty():
            node_val, idx = qu.get()
            node = ListNode(node_val)
            next_node = lists[idx] 
            pt.next = node
            pt = pt.next
            # update queue
            if next_node !=None:
                qu.put([next_node.val, idx])
                lists[idx] = lists[idx].next
        return head.next
            
            



```

