---
description: 这道题相对于binary tree diameter I + maximum path sum binary tree II +图的DFS
---

# Tree diameter 树的直径II

牛客： [https://www.nowcoder.com/practice/a77b4f3d84bf4a7891519ffee9376df3?tpId=196&tqId=37158&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey](https://www.nowcoder.com/practice/a77b4f3d84bf4a7891519ffee9376df3?tpId=196&tqId=37158&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey)

### 题目描述

给定一棵树，求出这棵树的直径，即树上最远两点的距离。包含n个结点，n-1条边的连通图称为树。  
示例1的树如下图所示。其中4到5之间的路径最长，是树的直径，距离为5+2+4=11![](https://uploadfiles.nowcoder.com/images/20201202/999991351_1606896095422/54D43FCA3EABC9D96189FA8EA98A510C)  
示例1

### 输入

[复制](javascript:void%280%29;)

```text
6,[[0,1],[1,5],[1,2],[2,3],[2,4]],[3,4,2,1,5]
```

### 返回值

[复制](javascript:void%280%29;)

```text
11
```

2.思路

maximum path sum binary tree II的思路  + dfs的多个branch的tree的思路

1通过dictionary建树，key为parent，value= list of \(child node, edge weight\) pair

2.遍历tree的每一个node，找到每个branch的从上往下加起来的max path sum，找到max path sum最大的两个branch，并把两个branch的max path sum相加得到跨越两个branch的path的pathsum， 然后和global max path sum对比更新

3返回找到的最大的从上往下的max path sum + current node的值得到目前最大的path sum





```text
    
class Solution:
    def solve(self , n , Tree_edge , Edge_value ):
        # write code here
        # 1. input: Graph: n= node values, Tree_edge  = list of node to node link/edge, edge_value:weight
        # 2. find max path sum of any two node in tree (maximum path sum binary tree)
        # idea: 
        #  1. build tree using dictionary
        #     key = node value,  value = children node of key
        #  2. find max path sum of any two node in tree, recursion tree, but not binary tree
        #     idea: max path sum  of a node =  max( max path sum from left tree,
        #                                        max path sum from right tree, 
        #                                        max path sum from left + right + node value tree, )
        #   if max path sum from left/ right top-down consecutive path  < 0, set it to 0 
        #    otherwise  add it to the path sum
        #   compare the path sum found in current node with global max
        #   return max top-down path sum including cur node value
        #
        #
        # test case
        if n <=1:
            return None
        tree = {}
        self.global_max = -float('inf')
        # build
        for i in range(len(Tree_edge)):
            if Tree_edge[i].start not in tree.keys():
                tree[Tree_edge[i].start] = []
            if Tree_edge[i].end not in tree.keys():
                tree[Tree_edge[i].end] = []
            # append children and weight to parent node's list 
            node, w = Tree_edge[i].end, Edge_value[i]
            tree[Tree_edge[i].start].append([node,w])
            # 搭建无向图，所以把 end 为key也加上去
            # 加了之后为了防止有loop， 在dfs里面要家 child != parent 的条件
            # 如果不加end的key到dictionary，可以不加child ！= parent的条件
            tree[Tree_edge[i].end].append([Tree_edge[i].start,w])
        # find max path sum
        root = Tree_edge[0].start
        self.findmaxsum(tree, root,-1)
        return self.global_max
    
    def findmaxsum(self, tree, node,parent):
        # tree: dictionary
        # node: key value in dicionary
        # return max top-down path sum 
        if node not in tree.keys():
            return 0
        path_sum = 0
        ret_path_sum = 0
        # iterate children
        path_ls = [0,0]
        for i in range(len(tree[node])):
            
            node_val = tree[node][i][0]
            node_weight = tree[node][i][1]
            if node_val == parent:
                continue
            p_sum = max(self.findmaxsum(tree, node_val,node)+node_weight, 0)
            ret_path_sum = max(ret_path_sum, p_sum)
            # compute path sum across two branches
            if p_sum > path_ls[0]:
                if p_sum > path_ls[1]:
                    path_ls[0] = path_ls[1]
                    path_ls[1] = p_sum
                else:
                    path_ls[0] = p_sum
        path_sum = sum(path_ls)
        # update global max
        self.global_max = max(self.global_max, path_sum)
        return path_ls[1]
        
         
            
```

