# Tree-PreOrder-InOrder-PostOrder

### 1. Link

{% embed url="https://www.nowcoder.com/practice/a9fec6c46a684ad5a3abd4e365a9d362?tpId=188&&tqId=38560&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking" %}





### 2. 描述

分别按照二叉树先序，中序和后序打印所有的节点。

### 示例1

输入：

```
{1,2,3}
```

复制返回值：

```
[[1,2,3],[2,1,3],[2,3,1]]
```

复制

### 备注：

```
n \leq 10^6n≤106
```



### 3. 思路

1. Recursion method
2. Iteration with Stack method



### 4. Coding# class TreeNode:

{% code title="Tree_Traversal" %}
```python
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

#
# 
# @param root TreeNode类 the root of binary tree
# @return int整型二维数组
#
# class Solution:
#     def threeOrders(self , root ):
#         # write code here
#         preorder_res, inorder_res, postorder_res= [], [],[]
#         self.traverse(root, preorder_res, inorder_res,postorder_res)
#         return [preorder_res, inorder_res, postorder_res]
    
#     def traverse(self,node, pre_res, in_res,postres):
#         if not node:
#             return
#         pre_res.append(node.val)
#         self.traverse(node.left, pre_res, in_res, postres)
#         in_res.append(node.val)
#         self.traverse(node.right, pre_res, in_res, postres)
#         postres.append(node.val)



class Solution:
    def threeOrders(self , root ):
        # method 1: recursion tree
        # input: root tree node
        #  output: list of traversal list  [pre-order, in-order, post-order]
        # base case: not root: return [[],[],[]]
        #
        if not root:
            return [[],[],[]]
        pre_order, in_order, post_order = [], [], []
        self.traverse(root, pre_order, in_order, post_order)
        return [pre_order, in_order, post_order]
        
    def traverse(self, node,pre_order, in_order, post_order):
        if not node:
            return 
        #pre
        pre_order.append(node.val)
        self.traverse(node.left, pre_order, in_order, post_order)
        # in
        in_order.append(node.val)
        self.traverse(node.right, pre_order, in_order, post_order)
        # post
        post_order.append(node.val)
        
        
        
        
class Solution:
    def threeOrders(self , root ):
        # method 2:  iteration with stack
        #1. as each node is visited three times
        #    the first one: pre-order  -> pre-order
        #    the second one : after iterating the left subtree  and go back to parent -> in-order
        #    the third one: after iterating the right substree and go back to parent node -> post order
        #
        pre_order, in_order, post_order = [], [], []
        if not root:
            return [pre_order, in_order, post_order]
        stack = [(root, 0 )]
        while stack:
            node, cnt = stack.pop(-1)
            if cnt == 0:
                # preorder
                pre_order.append(node.val)
                stack.append((node, 1))
                if node.left:
                    stack.append((node.left, 0))
            if cnt == 1:
                in_order.append(node.val)
                stack.append((node, 2))
                if node.right:
                    stack.append((node.right, 0))
                    
            if cnt == 2:
                post_order.append(node.val)
                stack.append((node, 3))
        return [pre_order, in_order, post_order]
        
        






```
{% endcode %}















