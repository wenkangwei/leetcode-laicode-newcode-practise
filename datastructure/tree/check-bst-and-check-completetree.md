# Check BST and Check CompleteTree

### 1. Link

{% embed url="https://www.nowcoder.com/practice/f31fc6d3caf24e7f8b4deb5cd9b5fa97?tpId=188&tqId=38309&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey" %}



### 2. 题目描述

给定一棵二叉树，已经其中没有重复值的节点，请判断该二叉树是否为搜索二叉树和完全二叉树。示例1

### 输入

[复制](javascript:void\(0\);)

```
{2,1,3}
```

### 返回值

[复制](javascript:void\(0\);)

```
[true,true]
```





### 3. 思路

1. Check Valid Binary Search Tree: Time O(n)
2. Check Complete Tree: Time O(n)



### 4. Coding

```
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

#
# 
# @param root TreeNode类 the root
# @return bool布尔型一维数组
#
class Solution:
    def judgeIt(self , root ):
        # write code here
        # input: root of tree, output
        #
        #
        #
        if not root:
            return True, True
        
        return  self.isBST(root),self.isCompleteTree(root)
    
    def isBST(self,root):
        return self.checknode(root, -float('inf'), float('inf'))
        
    
    def checknode(self, node, left, right):
        if not node:
            return True
        if node.val <left or node.val > right:
            return False
        # go to left, right subtree
        # update right boundary
        l_res = self.checknode(node.left,left, node.val)
        # update left boundary
        r_res = self.checknode(node.right,node.val, right)
        return l_res and r_res
        
    def isCompleteTree(self, root):
        #
        #check if tree is complete tree
        # level order traversal to check
        # 
        #
        if not root:
            return True
        queue = [root]
        level = [] # store node in the next level
        while queue:
            node = queue.pop(0)
            
#             if (node.left and level and level[-1] == None) or (node.right and not node.left):
#                 return False
            if node != None:
                # return False
                # if left child is none but right child is not none
                # or previous node in level is None and one of left/right is not none
                if (not node.left and node.right) or (level and level[-1]==None and (node.left or node.right)):
                    return False
                else:
                    level.append(node.left)
                    level.append(node.right)
                
            if not queue:
                queue = level
                level = []
        return True
    
```





