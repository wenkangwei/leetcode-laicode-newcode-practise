---
description: 二叉树任意两个节点的最大路径和
---

# Maximum Path Sum Binary Tree II

牛客: [https://www.nowcoder.com/practice/da785ea0f64b442488c125b441a4ba4a?tpId=196&tqId=37050&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey](https://www.nowcoder.com/practice/da785ea0f64b442488c125b441a4ba4a?tpId=196&tqId=37050&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey)

Laicode: [https://app.laicode.io/app/problem/139](https://app.laicode.io/app/problem/139)

Given a binary tree in which each node contains an integer number. Find the maximum possible sum **from any node to any node \(the start node and the end node can be the same\).** 

**Assumptions**

* ​The root of the given binary tree is not null

**Examples**

   -1

  /    \

2      11

     /    \

    6    -14

one example of paths could be -14 -&gt; 11 -&gt; -1 -&gt; 2

another example could be the node 11 itself

The maximum path sum in the above binary tree is 6 + 11 + \(-1\) + 2 = 18

**How is the binary tree represented?**

We use the level order traversal sequence with a special symbol "\#" denoting the null node.

**For Example:**

The sequence \[1, 2, 3, \#, \#, 4\] represents the following binary tree:

    1

  /   \

 2     3

      /

    4

想法：

1. 和之前的任意两个leaf node的最大路径不同的是，这个是任意两个node之间的最大路径，所以在把一个node的left， right subtree的max path相加之前，要先看一下来自left， right 的max path是否大于0。 如果是left/right 的从上往下的max path sum &lt;0, 那么就不用加那个path了
2. 步骤就是
   1. 遍历每一个node
   2. recursion的输入是node， 输出是从那个node开始自上往下最大的path sum
   3. 如果node是none，返回 0
   4. 在每一个node里面找到left, right children的自上往下的max path sum，如果自上往下max path sum &gt;0, 就 left +right +node.val 得到从左往右的max path sum
   5. 把从左往右的max path sum和 global max path sum 对比并更新
   6. 返回加上当前节点的自上往下的max path sum

Source Code

```text
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution(object):
  def maxPathSum(self, root):
    """
    input: TreeNode root
    return: int
    """
    # write your solution here
    # 1. question: input: root, output: maximum path sum from any node to any node
    # 2. idea:
    #   iterate every node in tree:
    #     for every node, find the max sum of its left consecutive path and max sum of its right consecutive path 
    #     compare the value of :  left path max sum, right path max sum, node.val and 
    #                            (left path max sum + right path max sum + node.val) and global max sum of path
    #    update global max sum of path of any two node， 更新任意两个node 之间的pathsum的最大值
    #    return max path sum of single path under this node 返回单条从上往下连续path的sum
    # Time: O(n), Space O(n) for imbalance tree, O(logn) for balance tree
    if not root:
      return None
    self.max_sum = -float('inf')
    self.helper(root)
    return self.max_sum
  def helper(self, node):
    if not node:
      return 0
    left = max(self.helper(node.left),0)
    right = max(self.helper(node.right),0)
    # if left or right <0, then no need to add them to max_path
    #  max_path can be single path or sum of two paths or only node.val
    # then compare it with the global max
    max_path = left + right + node.val
    self.max_sum = max(max_path, self.max_sum)
    # return the single path with max sum
    return max(left,right) +node.val


```

 

