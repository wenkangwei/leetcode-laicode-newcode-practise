---
description: 'Laicode：  https://app.laicode.io/app/problem/138      二叉树任意两个leaf node的最大路径和'
---

# Maximum Path Sum Binary Tree

**Link**

Laicode： [https://app.laicode.io/app/problem/138](https://app.laicode.io/app/problem/138)

Given a binary tree in which each node contains an integer number. Find the maximum possible sum **from one leaf node to another leaf node.** If there is no such path available, return Integer.MIN\_VALUE\(Java\)/INT\_MIN \(C++\).

**Examples**

  -15

  /    \

2      11

     /    \

    6     14

The maximum path sum is 6 + 11 + 14 = 31.

**How is the binary tree represented?**

We use the level order traversal sequence with a special symbol "\#" denoting the null node.

**For Example:**

The sequence \[1, 2, 3, \#, \#, 4\] represents the following binary tree:

    1

  /   \

 2     3

      /

    4

想法: top-down \(更新global max\) + bottom up（返回从上往下的path的最大的sum）

1. 把每个node遍历一遍
2. 在每个node里面，找到left 的path sum和right path sum\(如果存在\)， 并把left path, right path node.val相加得到最远的两个leaf node的path的sum
3. 对比这个path sum和global max，更新global max
4. 返回加上当前的node.val的一条在root node到leaf node的path上面的最大的path，即max\(left,right\)+node.val,就返回当前最大的single path



## Source Code

```text
class Solution(object):
  def __init__(self):
    self.res = 0
  def maxPathSum(self, root):
    """
    input: TreeNode root
    return: int
    """
    # write your solution here
    #1.  question: input: tree node root, output : max path sum from
    #  one leaf node to another leaf node. The max diameter of tree
    # 1. idea : Top-down 
    # 把每个node遍历一遍， 在每个node 里面，找左边和右边最大的 path sum， 再 加上现在的node的value
    # 把更新的path 和之前的对比和更新
    # start from the top and go down, for each node, consider it as the root node of the path
    #  we need to find the max path sum from right, left branch and then add them with the cur
    #  node value
    #  then compare this value with global max
    #   recursion: 
    #      input: node ,  max_path sum
    #      output: max path sum found in current node, cur node height
    #  base case:
    #
    # base: if root none:  MIN
    # Time: O(n) , n = tree node number  Space O(logn) logn = tree height if tree is balance
    #
    
    self.ans = -2147483648
    self._maxPathSum(root)
    return self.ans

  def _maxPathSum(self, root):
        if not root: return -2147483648
        l =  self._maxPathSum(root.left)
        r = self._maxPathSum(root.right)
        if l != -2147483648 and r !=-2147483648:
          self.ans = max(self.ans, root.val + l + r)
          return root.val + max(l, r)
        elif l!= -2147483648 :
          return root.val + l
        elif r!= -2147483648 :
          return root.val + r
        return root.val 

```





