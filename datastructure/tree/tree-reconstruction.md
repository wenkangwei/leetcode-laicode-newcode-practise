# Tree ReConstruction

### 1. Link

{% embed url="https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6?tpId=13&tqId=11157&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking&tab=answerKey" %}



### 2. 题目描述

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。示例1

### 输入

复制

```
[1,2,3,4,5,6,7],[3,2,4,1,6,5,7]
```

### 返回值

复制

```
{1,2,5,3,4,6,7}
```



### 3.思路

1. &#x20;Recursion: pre-order 用于遍历每一个node，并得到当前的subtree的rootnode，（pre-order从左往右数起，第一个node是root node） in-order 用于搜索root node位置来重建树
2. Recursion: 输入是 剩下没有重建的pre-order的node和in-order 的信息，以及搜索的左右边界缩小搜索范围
3. 如果在left， right边界里面找到root后，以它为中心分开左右两个subarray查找左右subtree的rootnode，并接到当前的rootnode的left， right
4. Time: O(n) for searching root in subarray, O(n) for iterating every node for reconstruction, so O(n^2)
5.  Space: O(logn) for recursion



### 4. Coding

```
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
        # 1. for pre-order, the first node = root, for in-order
        #    it can be used for binary search to reconstruct node
        # 2. idea: recursion
        #    input: in-order, pre-order array, position of node in pre-order left, right boundary to
        #    output: root node of subtree
        #    base case: if left boundary >= right boundary and right != node
        #     return None
        #    1. find the node of pre-order in in-order 
        #    2. split in-order array into left, right arrays based on 
        #        root node
        #    3. search the second node of pre-order in left and right array of in-order
        #    4. connect root node with root of left-subtree and root of right subtree
        #    5. return root node
        #
        #
        # test case:[1,2,3,4,5,6,7],[3,2,4,1,6,5,7]
        # Time: O(n) for finding root in in-order, O(n) for iterate every node
        #  in tree, so O(n^2)
        # Space: O(logn) for recursion
        #
        if not pre or not tin or len(pre)!= len(tin):
            return None
        
        root = self.findroot(pre, tin,  0, len(tin)-1)
        return root
    
    def findroot(self,pre, tin, left, right ):
        if left >=right :
            if pre and pre[0] == tin[right]:
                return TreeNode(pre.pop(0))
            return None
        # find root node
        idx = None
        for i in range(left, right+1):
            if tin[i] == pre[0]:
                idx = i
                break
        if idx == None:
            return None
        p_node = TreeNode(pre.pop(0))
        # search left array
        l_node = self.findroot(pre, tin, left, idx-1 )
        r_node = self.findroot(pre, tin,  idx+1, right )
        
        p_node .left = l_node
        p_node.right = r_node
        return p_node
        
```

###







