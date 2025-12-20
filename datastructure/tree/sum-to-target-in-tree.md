# Path Sum to Target in Binary Tree

### 1. Link

{% embed url="https://www.nowcoder.com/practice/508378c0823c423baa723ce448cbfd0c?tpId=188&&tqId=38614&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking" %}



### 2. 描述

给定一个二叉树和一个值\ sum sum，判断是否有从根节点到叶子节点的节点值之和等于\ sum sum 的路径，\
例如：\
给出如下的二叉树，\ sum=22 sum=22，\
![](https://uploadfiles.nowcoder.com/images/20200807/999991351_1596786493913_8BFB3E9513755565DC67D86744BB6159)\
返回true，因为存在一条路径 5\to 4\to 11\to 25→4→11→2的节点值之和为 22<br>

### 示例1

输入：

```
{1,2},0
```

复制返回值：

```
false
```

复制

### 示例2

输入：

```
{1,2},3
```

复制返回值：

```
true
```



### 3. 思路

1. idea: recursion
2. check if node is none, return False,&#x20;
3. &#x20;if node is leaf node and target== node.val,return True, otherwise False
4. search the left and right tree path and then use OR to combine their results to check if the path exist
5. Time: O(n) to iterate every tree node
6. Space: O(height of tree)

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
# @param sum int整型 
# @return bool布尔型
#
class Solution:
    def hasPathSum(self , root , sum ):
        # write code here
        #input: root, target
        # idea: recursion
        # 1. input: node, current target
        # 2. if node is leaf node and target is 0, -> return True (found a path from root to leaf), otherwise, False
        # 3. update target by target - node.val
        # 4. go to left, right subtree to find the path
        #
        #
        if not root or sum ==None:
            return False
        return self.findpath(root, sum)
    def findpath(self, node, target):
        # if node is invalid
        if not node:
            return False
        # if node is leaf node
        if node and not node.left and not node.right:
            if target == node.val:
                return True
            return False
        # exist a path
        return self.findpath(node.left, target-node.val) or self.findpath(node.right, target-node.val)
    
    
    
        
```





