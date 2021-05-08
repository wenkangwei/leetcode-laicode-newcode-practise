---
description: Medium; BST
---

# The Kth smallest in Binary Search Tree

1**. Link**

{% embed url="https://leetcode.com/problems/kth-smallest-element-in-a-bst/" %}



**2. 题目**

230. Kth Smallest Element in a BST

Given the `root` of a binary search tree, and an integer `k`, return _the_ `kth` \(**1-indexed**\) _smallest element in the tree_.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

```text
Input: root = [3,1,4,null,2], k = 1
Output: 1
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)

```text
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```



**3. 思路**

1. **Idea: Recursion  of the Binary search tree**
2. **Input of recursion:  parent node, k, the number of node smaller than parent**
3. **Output of recursion : either the found kth smallest node and it ranking k,  or the maximum node value and the corresponding ranking \(ascending ranking\)**
4. **Idea**
   1. Iterate every node
   2. first go to its left child to find the smallest node and its ranking
   3. then find how many node is smaller than current node/left child and check if the returned result from left node or current node is the K^th smallest. If so, return result.
   4. Then go to right child of current node with updated amount of node smaller than the right node.
   5. Note that the main idea of this method is to label every node with its ranking  based on BST property. ****

**4. Coding**

```text
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        #1. input: tree node root node  output: value of the kth smallest node in BST
        #2. idea: recursion
        #   1. input: node, the number of node smaller than input node, k.  
        #      output: Either the found K^th smallest node or  the maximum node value in that branch with its index/ ranking 
        #      if node value index = k, then no change
        #   2. First go to left tree with the same input.
        #      then check if the maximum node value in the left subtree rank k^th smallest
        #      and check if current node value rank K^th smallest. If so return left node value or current node value
        #      Otherwise, pass the number of node smaller than the right node to the right branch of current node
        #      and return the node found from the right branch
        #
        #   3. Time: O(n) for recursion in whole tree worst case,  Space: O(logn) if tree is balanced
        #
        if not root:
            return None
        node, num = self.findnode(root, 0,k)
        if num ==k:
            return node.val
        
        return None
        
    def findnode(self, node, num_node, k):
        if not node:
            return None, num_node

        l_node, l_num = self.findnode(node.left, num_node,k)
        if l_num ==k:
            return l_node, k
        num = l_num + 1
        if num ==k:
            return node, k
        r_node, r_num = self.findnode(node.right, num,k)
        return r_node, r_num
```















