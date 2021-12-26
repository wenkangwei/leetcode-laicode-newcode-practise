---
description: 没有weight的二叉树直径, 直接用node的高度计算直径
---

# Binary Tree diameter I

laicode：[https://app.laicode.io/app/problem/142](https://app.laicode.io/app/problem/142)

Leetcode: [https://blog.csdn.net/xuchonghao/article/details/80657610](https://blog.csdn.net/xuchonghao/article/details/80657610)

Given a binary tree in which each node contains an integer number. The diameter is defined as the longest distance from one leaf node to another leaf node**.** The distance is the number of nodes on the path.

If there does not exist any such paths, return 0.

**Examples**

&#x20;   ****    5

&#x20; /    \\

2      11

&#x20;    /    \\

&#x20;   6     14

The diameter of this tree is 4 (2 → 5 → 11 → 14)

**How is the binary tree represented?**

We use the level order traversal sequence with a special symbol "#" denoting the null node.

**For Example:**

The sequence \[1, 2, 3, #, #, 4] represents the following binary tree:

&#x20;   1

&#x20; /   \\

&#x20;2     3

&#x20;     /

&#x20;   4

想法: top-down+ bottom up

1. 遍历tree的每一个node
2.  在recursion里面： 输入： 下一个child node， 输出： 那个node的高度

    1. 在当前的node里面从left， right children得到对应的高度
    2. 把left，right的高度相加再+1 当前的node得到当前subtree 的直径
    3. 把当前的直径和global max的直径对比，更新global max 直径
    4. 返回当前的node的高度，即max(left, right) +1



```
class Solution(object):
  def diameter(self, root):
    """
    input: TreeNode root
    return: int
    """
    # write your solution here
    #
    # 1.confirm question
    #  tree diameter is the longest path from one leaf node to another leaf node in the tree
    #  Assumption/Base case 
    #  
    # 3. method 1: use global max 
    # 4. debug
    #   test case:
    #         1
    #   None    2
    #        None 3
    #           5   4
    # the diameter is 3:   5->3->4
    #
    # 5. TIme complexity: O(n) since we need to go through every node to check the height and update max_path
    #   space compelxity: O(h)
    if not root :
      return 0
    self.max_path = 0
    height = self.get_diameter(root)
    return self.max_path 

  def get_diameter(self, node):
    height =0
    # base case when cur node is none
    if not node:
      return height
    # get height
    left_h = self.get_diameter(node.left)
    right_h = self.get_diameter(node.right)
    # update height of current node
    #
    height = max(left_h,right_h) +1
    if left_h != 0 and right_h != 0:
      self.max_path = max(self.max_path , left_h + right_h + 1)
    return height

    
```

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution(object):
  def diameter(self, root):
    """
    input: TreeNode root
    return: int
    """
    # write your solution here
    # 1.confirm question
    #  tree diameter is the longest path from one leaf node to another leaf node in the tree
    #  Assumption/Base case 
    #   1. when tree has root node only: return 0
    #   2. when root node has only one node and that child node doesn't have children node-> return 0 since there is no path
    #
    # 2. input: root node of tree node, output : the length of tree diameter
    #
    # 3. idea:
    #  1. check if node dosen't exists, return 0
    #  2. use recursion to iterate each node in tree
    #     in each node, use get_height to obtain the max heigh of two children  of current node
    #     sum up two heights + 1 to get the path length and compare it with the max path length from two children node
    #     return the maximum length
    #  3. base case: when node is none, then return height 0, max_path = 0
    #     base case, when node is not none, it contain only one children, then return height of children+1,  max_path from children
    #     base case, when node and its two children nodes are not none,
    #     then compute max_path = heightof left children + height of right children  + 1 and compare it with max_path from 
    #    its children nodes
    #
    # 4. debug
    #   test case:
    #         1
    #   None    2
    #        None 3
    #           5   4
    # the diameter is 3:   5->3->4
    #
    # 5. TIme complexity: O(n) since we need to go through every node to check the height and update max_path
    #   space compelxity: O(h)
    if not root:
      return 0
    height, max_path = self.get_diameter(root)
    return max_path

  def get_diameter(self, node):
    height, max_path =0,0
    # base case when cur node is none
    if not node:
      return height, max_path
    # get height
    left_h, left_path = self.get_diameter(node.left)
    right_h, right_path = self.get_diameter(node.right)
    # update height of current node
    height = max(left_h,right_h) +1
    #when path exist，children node are not none, then update max path 
    if right_h!= 0 and left_h != 0:
      max_path = left_h + right_h +1
    # update diameter by comparing the max_path from left, right and current node
    max_path = max(max_path, left_path,right_path)
        
    return height, max_path

```
