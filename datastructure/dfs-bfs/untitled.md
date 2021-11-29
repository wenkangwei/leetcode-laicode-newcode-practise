---
description: Medium； graph; DFS
---

# Number of Provinces

1\.  Link

&#x20;[https://leetcode.com/problems/number-of-provinces/](https://leetcode.com/problems/number-of-provinces/)

2\. 题目

547\. Number of Provinces

There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `ith` city and the `jth` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return _the total number of **provinces**_.

3\. 思路

1. input: adjacent matrix, output: number of province
2. 在graph 里面用DFS，需要用set除重
3. iterate 每个node，如果node不在set里面就说明这个node没有被visited过，是一个新的province的node，所以cnt+=1
4. 然后在这个node里面做DFS，如果node不在set里面，先把node加到set，然后对它的neighbor做DFS，直到没有neighbor或者neighbor都是被visited 过的为止
5. Time: 最坏情况是O(n^2) 每个node都是独立的province然后把整个表遍历一遍就是n^2, n= number of node
6. Space:  worest case O(n)如果province是linkedlist状的话

```
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        # idea: DFS
        # 
        # 1. iterate every node if node not in set, then we find a new province so  cnt += 1 else go to next step
        # 2. in each node, find its neighbor and add neighbor to set if neighbor not found
        # 3. use DFS to find neighbor of neighbor and add them to set until neighbor already in set or
        #    not neighbor found
        # 4. 
        #
        if not isConnected or not isConnected[0]:
            return 0
        visited = set()
        cnt = 0
        for i in range(len(isConnected)):
            if i not in visited:
                self.findProvinces(visited, isConnected, i)
                cnt += 1
        return cnt
                
            
    def findProvinces(self, visited, g, node):
        if node in visited:
            return
        visited.add(node)
        for n in range(len(g[node])):
            if g[node][n] == 1:
                self.findProvinces(visited, g, n)
                
```











