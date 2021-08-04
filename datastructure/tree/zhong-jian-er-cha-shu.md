# 重建二叉树

### 1. Link

{% embed url="https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6?tpId=191&&tqId=35922&rp=1&ru=/activity/oj&qru=/ta/job-code-high-algorithm/question-ranking" %}

### 2. 描述

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

### 示例1

输入：

```text
[1,2,3,4,5,6,7],[3,2,4,1,6,5,7]
```

复制返回值：

```text
{1,2,5,3,4,6,7}
```



### 3. 思路

1. 确认 input:  pre-order traversal,  in-order traversal
2. idea: recursion tree method
   1. input to recursion: pre, in-order, start and end of searching range in in-order
   2. pop the top of pre-order in current level
   3. search the node val in in-order sequence to find the corresponding position
   4. create tree node for this node val and partition the searching range to left and right ranges based on this position
   5. Go to the left and right searching range respectively to find the root of left subtree  and root of right subtree
   6. Connect the subtrees to current node and return current node
   7. **Time**: O\(n\) for searching node in in-order,  O\(n\) for iterating every node in pre-order. So O\(n^2\) in total
   8. **Space:** O\(height of tree\)

### 4. Coding

```text
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回构造的TreeNode根节点
    def reConstructBinaryTree(self, pre, tin):
        # write code here
        #1. input: pre: pre-order, tin: in-order
        # in-order: flatten tree into list
        # pre-order: sequence of node to iterate tree
        # 2. idea:
        # 1. use recursion tree method
        #   recursion: 
        #    input: pre, in, start and end of searching region in in-order
        #    output: node of tree in pre-order 
        #   1. pop the first element in pre-order
        #   2. search this node value and pos in in-order sequence
        #   3. construct tree node for this node
        #   4. separate the in-order sequence into left, right array
        #      so that we can goto left, right array to reconstruct the left, right
        #      subtree
        #    5. return current tree node
        #
        # Time: O(n) for searching node in in-order,  O(n) for iterating all node in tree
        # so O(n^2)
        # Space: O(logn) or O(height) for recursion tree  
        #
        #Test:  [1,2,3,4,5,6,7],[3,2,4,1,6,5,7]
        #        1
        #     2      5
        #   3   4   6   7
        #
        #
        if not pre or not tin or len(pre)!= len(tin):
            return None
        root_node = self.construct_node( pre, tin, 0, len(tin)-1)
        return root_node
    def construct_node(self, pre, tin, start, end):
        if len(pre)==0:
            return
        if start >end:
            # return a None if range is invalid
            return None
        node_val = pre.pop(0)
        pos = None
        for i in range(start, end+1):
            if tin[i] == node_val:
                pos = i
                break
        if pos == None:
            return None
        node = TreeNode(node_val)
        # goto left tree
        left_node = self.construct_node( pre, tin, start, pos-1)
        # goto right tree
        right_node = self.construct_node( pre, tin, pos+1, end)
        node.left = left_node
        node.right = right_node
        return node
        
        
        
        
```











