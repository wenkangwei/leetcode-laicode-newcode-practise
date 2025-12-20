# MergeSort Linkedlist

&#x31;**. link**

{% embed url="https://app.laicode.io/app/problem/29" %}



**2. 题目**



**3. 思路**

1. **这里的mergesort linkedlist要考虑partition时只有左边的list只有2个node，右边的list为None，导致死循环的情况。 因为只有2个node时，通过fast slow pointer 找中点时，partition的list还是left= 2node的list， right= None**
2. **Merge sort： Time:O(nlogn),  Space: O(logn) for recursion, O(1) for inplace operation in each node**



**4. Coding**

```
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None
class Solution(object):
  def mergeSort(self, head):
    """
    input: ListNode head
    return: ListNode
    """
    #
    # 1. divide list by finding middle 
    # 2. repeat dividing until we can not divide it
    # 3. merge left and right linklist in place and return 
    #
    # test case:
    #
    if not head or not head.next:
      return head

    mid = self.findmiddle(head)
    #
    #consider the case:  1->2
    # then fast pt =None , mid  =2 , then head2 = mid.next = None, head = 1
    # In this case, left linkedlist always = 1->2 and right linkedlist always = None when recursion
    # so there is a deadloop when input has two nodes only
    

    if mid.next != None:
      head2 = mid.next
    else:
      head2 = head.next
      head.next = None


    # cut the middle of list
    mid.next = None

    l_ls = self.mergeSort(head)
    r_ls =self.mergeSort(head2)
    return self.merge(l_ls, r_ls)

  def findmiddle(self, head):
    if not head:
      return head
    slow = fast = head
    while fast and fast.next:
      fast = fast.next.next
      slow = slow.next
    return slow

  def merge(self, l_list, r_list):
    if not l_list:
      return r_list
    if not r_list:
      return l_list
    fake_head = ListNode(None)
    cur =fake_head
    
    while l_list and r_list:
      if l_list.val < r_list.val:
        cur.next = l_list
        l_list = l_list.next
      else:
        cur.next = r_list
        r_list = r_list.next
      cur = cur.next
    if l_list:
      cur.next = l_list
    if r_list:
      cur.next = r_list
    return fake_head.next
```
