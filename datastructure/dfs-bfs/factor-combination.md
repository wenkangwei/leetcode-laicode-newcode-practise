# Factor Combination-质数分解

1**. Link**

{% embed url="https://app.laicode.io/app/problem/404" %}

****

**2. 题目**

Given an integer number, return all possible combinations of the factors that can multiply to the target number.

Example

Give A = 24

since 24 = 2 x 2 x 2 x 3

&#x20;             \= 2 x 2 x 6

&#x20;             \= 2 x 3 x 4

&#x20;             \= 2 x 12

&#x20;             \= 3 x 8

&#x20;             \= 4 x 6

your solution should return

{ { 2, 2, 2, 3 }, { 2, 2, 6 }, { 2, 3, 4 }, { 2, 12 }, { 3, 8 }, { 4, 6 } }

note: duplicate combination is not allowed.



**3. 思路**

```
1. bt input: action set: [2, 3,4 ...  target], can not be 1
sols: final results,  sol: current result, target, start: action start from 2
2. terminal state: when target ==1 no need to factorize. Append sol when sol len >1
3.for current target, action is between start to target-1, check if target can be divided by action
if so append action and use dfs to next target/i, start = i (same factor may be used many times)
    
```

**4. Coding**

```
# class Solution(object):
#   def combinations(self, target):
#     """
#     input: int target
#     return: int[][]
#     """
#     # write your solution here
#     sol = []
#     sols = []
#     pos =2
#     self.bt(sols, sol,target, pos)
#     #sols.sort()
#     return sols
#   def bt(self, sols, sol,target,pos):
#     if target ==1:
#       if len(sol)>1:
#         sols.append(sol[:])
#         return

#     for i in range(pos, target+1):
#       if target%i ==0:
#         sol.append(i)
#         self.bt(sols, sol,target//i, i)
#         sol.pop(-1)
        
    
      
class Solution(object):
  def combinations(self, target):
    """
    input: int target
    return: int[][]
    """
    #idea: DFS 
    # 1. bt input: action set: [2, 3,4 ...  target], can not be 1
    #   sols: final results,  sol: current result, target, start: action start from 2
    # 2. terminal state: when target ==1 no need to factorize. Append sol when sol len >1
    # 3.for current target, action is between start to target-1, check if target can be divided by action
    #   if so append action and use dfs to next target/i, start = i (same factor may be used many times)
    #
    #base case: target =1
    if target ==1:
      return [[1]]
    sols = []
    sol = []
    start = 2
    self.bt(sols, sol, start, target)
    return sols
  def bt(self, sols, sol, start, target):
    if target == 1:
      if len(sol) >1:
        sols.append(sol[:])
      return 
    for i in range(start, target+1):
      if target%i == 0:
        sol.append(i)
        self.bt(sols, sol, i, target//i)
        sol.pop(-1)
```



如果只考虑所有质数，那就不用for loop直接看target能不能被start整除(之后start的倍数都会被直接跳过，因为target都已经不能被start整除那就更不可能被它的倍数整除)。如果可以就加上start，如果不行就跳过当前的start，进入下一个factor: start+1

```
class Solution(object):
  def combinations(self, target):
    """
    input: int target
    return: int[][]
    """
    #idea: DFS 
    # 1. bt input: action set: [2, 3,4 ...  target], can not be 1
    #   sols: final results,  sol: current result, target, start: action start from 2
    # 2. terminal state: when target ==1 no need to factorize. Append sol when sol len >1
    # 3.for current target, action is between start to target-1, check if target can be divided by action
    #   if so append action and use dfs to next target/i, start = i (same factor may be used many times)
    #
    #base case: target =1
    if target ==1:
      return [[1]]
    sols = []
    sol = []
    start = 2
    self.bt(sols, sol, start, target)
    return sols
  def bt(self, sols, sol, start, target):
    if target == 1:
      if len(sol) >1:
        sols.append(sol[:])
      return 
    
    if target%start == 0:
        sol.append(start)
        self.bt(sols, sol, start, target//start)
        sol.pop(-1)
    else:
        self.bt(sols, sol, start+1, target)
```

