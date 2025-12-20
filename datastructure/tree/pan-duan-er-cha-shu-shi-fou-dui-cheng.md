# 判断二叉树是否对称

### 1. Link

{% embed url="https://www.nowcoder.com/practice/1b0b7f371eae4204bc4a7570c84c2de1?tpId=188&&tqId=38623&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking" %}

### 2. 描述

给定一棵二叉树，判断其是否是自身的镜像（即：是否对称）\
例如：下面这棵二叉树是对称的\
1\
/  \\\
2    2\
/ \    / \\\
3 4  4  3\
下面这棵二叉树不对称。\
1\
/ \\\
2   2\
\    \\\
3    3\
备注：\
希望你可以用递归和迭代两种方法解决这个问题<br>

### 示例1

输入：

```
{1,2,2}
```

复制返回值：

```
true
```

### 3.思路

see notes in Coding

Time： O(n) since we need to go through every pair of nodes to check if they have same values. Then go back to parent node to search another pair of nodes

Space: O(height of tree)

### 4. Coding

```
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

#
# 
# @param root TreeNode类 
# @return bool布尔型
#
class Solution:
    def isSymmetric(self , root ):
        # write code here
        # idea
        # 1. recursion
        #  input: two nodes
        #  output: boolean
        # if two nodes' values are equal, use recursion to
        # compare node1.left, node2.right,   node1.right and node2.left
        # if not equal or one of two nodes is None but the other is not->return False
        #
        if not root:
            return True
        return self.checknodes(root.left, root.right)
    def checknodes(self, n1, n2):
        if not n1 and not n2:
            return True
        if (n1 and not n2) or (n2 and not n1):
            return False
        if n1.val != n2.val:
            return False
        return self.checknodes(n1.left, n2.right) and self.checknodes(n2.left, n1.right)
        
        
```

