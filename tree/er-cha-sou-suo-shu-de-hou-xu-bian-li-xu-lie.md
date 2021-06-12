---
description: Hard;
---

# 二叉搜索树的后序遍历序列

### 1. Link

{% embed url="https://www.nowcoder.com/practice/a861533d45854474ac791d90e447bafd?tpId=13&&tqId=11176&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking" %}

### 2. 描述

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则返回true,否则返回false。假设输入的数组的任意两个数字都互不相同。（ps：我们约定空树不是二叉搜素树）

### 示例1

输入：

```text
[4,8,6,12,16,14,10]
```

复制返回值：

```text
true
```

### 3. 思路

1. Post-order traversal 特性：post-order traveral 的result 从右到左的打印相对于是pre-order traversal的打印的结果往相反顺序打印，即先打印右边再打印左边
2. 举个例子
   1. ```text
                      5
                     /  \
                   3     7               
                  / \   / \    
                 1   2  6  8
           
      pre-order:  5, 3, 1, 2, 7, 6, 8
      post-order: 1,2,3,6,8,7,5
      inverse post-order:  5, 7, 8, 6, 3, 2,1
      ```
3. 所以用post-order检查BST时，从右往左遍历array，并且用个stack记录当前最大值
4. 当发现array\[i\] 的值&lt; stack top的最大值，就不断把stack的最大值pop出来直到找到小于array\[i\]的值为止， 这个过程相对于遍历完右子树，返回上一层找左子树。
5. 当往左遍历时如果发现node的值&gt; 当前最大值，即左子树的值&gt; parent 或 右子树的值，那么就return False
6. Time: 遍历每个array的值 O\(n\) ， 因为push的个数= pop的个数，并非每次pop的都是O\(n\), 所以实际上是O\(n\)  for popping +O\(n\) for iterating array = O\(n\)
7. Space: O\(n\)
8. Note:   pre-order traversal 和 反转过来的post-order traversal 在树的遍历方向上是对称/相反的，**pre-order traversal 从左往右， post-order traversal 反转顺序后是从右往左** 而**in-order traversal 相对于把tree 当成list一样平铺开来再打印**

### 4. Coding

```text
# -*- coding:utf-8 -*-
class Solution:
    def VerifySquenceOfBST(self, sequence):
        # write code here
        #idea: 
        # in post-order BST, the last node in sequence = root node
        #1. find the root node at the end of seq and iterate  node before root node to find the
        # start from right to left, find the first node with val < root val
        #
        #then partition subarray by this boundary to get left subtree and right subtree
        # and pass the root val to the recursion
        # For left subtree, find the root of left subtree and find the boundary
        # of element < root of left and <parent root, if find any element > parent root
        # return False
        # similar to left subtree, right subtree do the same thing
        #
        # post-order traversal 的特点是
        #    从右往左iterate array相对于 current-node-> right node->left node的方向的遍历 
        #    相当于和pre-order的对称
        #
        #
        if not sequence:
            return False
        max_val = float('inf')
        min_val = -float('inf')
        stack = [min_val]
        for i in range(len(sequence)-1, -1, -1):
            if sequence[i] > max_val:
                return False
            # pop the right subtree values
            # and find the node of left subtree
            while sequence[i] < stack[-1]:
                max_val = stack.pop(-1)
            stack.append(sequence[i])
        return True
                
            
            
            
            
```





