# Binary Tree Path Sum To Target III

1**. Link **

{% embed url="https://app.laicode.io/app/problem/141" %}

****

**2. 题目**

Given a binary tree in which each node contains an integer number. Determine if there exists a path** (the path can only be from one node to itself or to any of its descendants), **the sum of the numbers on the path is the given target number.

**Examples**

```
    5

  /    \

2      11

     /    \

    6     14

  /

 3
```

If target = 17, There exists a path 11 + 6, the sum of the path is target.

If target = 20, There exists a path 11 + 6 + 3, the sum of the path is target.

If target = 10, There does not exist any paths sum of which is target.

If target = 11, There exists a path only containing the node 11.

**How is the binary tree represented?**

We use the level order traversal sequence with a special symbol "#" denoting the null node.

**For Example:**

The sequence \[1, 2, 3, #, #, 4] represents the following binary tree:

&#x20;   1

&#x20; /   \\

&#x20;2     3

&#x20;     /

&#x20;   4



**3. 思路**

1. 遍历 tree的每个node
2. 以当前的node为起始点用另外一个recursion function查找 path sum to target
3. 如果当前的node找不到path， 那么就recursion的方式查找它的left， right subtree看有没有找到path sum to target
4. Time: O(n) for iterating all nodes in tree, O(n) for finding path sum , so O(n^2) 注意这里的 finding path sum因为每个node有可能是+ 或者 - 没加到最后一个node都不知道是不是可行的path，所以要遍历整棵树，就O(n)
5. Space: O(logn) for recursion if tree is balance. if not balance O(n). 同理O(logn) for finding path if balance，otherwise， O(n), so In total O(n)

**4. Coding**

```
class Solution(object):
  def exist(self, root, target):
    """
    input: TreeNode root, int target
    return: boolean
    """
    # input: tree root and target
    # output: flag
    # idea: 
    # 1. iterate every node in tree
    # 2. for each node, start from that node, find the path sum equal to the target using recursion function
    #in recursion function:
    # input: root node of subtree, target after substracting the node value
    # output: True/False indicating if path found
    # 1. base case: when node is none, return False
    # 2. if cur node.val == target, return True
    # 3. otherwise, go to left, right branch recursively and then return left flag or right flag
    #
    # Time: O(n) for iterating every node, O(n) for finding path, so O(n^2)
    #
    #
    if not root:
      return False
    
    res = self.findpath(root, target)
    if not res:
      res = self.exist(root.left, target) or self.exist(root.right, target)
    return res

  def findpath(self, node, target):
    if not node:
      return False
    if node.val == target:
      return True
    l_res = self.findpath(node.left, target - node.val)
    r_res = self.findpath(node.right, target - node.val)
    return l_res or r_res



```
