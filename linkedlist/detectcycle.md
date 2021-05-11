# Detect Start of Cycle

### 题目描述



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
    def detectCycle(self , head ):
        # write code here
        # fast = slow = head
        # idea: two pointer fast pt move 2 slow move 1 
        # 1. move fast slow pointer until fast == None or fast.next ==NOne,  return false
        # 2. if fast == slow, have loop 
        # 3. when fast == slow, use the slow2 pt from head and slow2 form slow move forward
        #    until they are equal, then it is loop entry
        #  
        #
        #Prove fast == slow -> loop
        #  assume have loop, 相遇的位置到array的start 距离就是n,  到array的end距离了是m
        #  loop的length = k
        #  slow pt 走过的距离就是 x， fast pt走过的距离就是 y
        #  x = n+z*k +m, y = n+j*k +m 
        #  y = 2x = 2(n+z*k+m) = n+j*k +m,    n+m = (j-2z)*k 整数可以解所有就是有loop
        #
        #
        #Prove 2: slowpt1 slow pt2 分别从array begin开始和meeting point开始走直到相遇
        #  相遇点就是cycle的entry
        #
        #把fast pt走过的路展开
        # 前半段是  x+ z*k +m 后半段： a +z*k + b
        # b 就是fast和slow相遇点 到环尾部的距离 = m
        # 那么 由于 x + z*k + m = a + z*k +b 所有a = x
        # 所有只要slow1, slow 2 分别从开始和slow的位置移动 x的distance就会到环的入口
        if not head:
            return None
        slow , fast = head, head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                slow1, slow2 = head, slow
                while slow1 != slow2:
                    slow1 = slow1.next
                    slow2 = slow2.next
                return slow1
        return None
```

