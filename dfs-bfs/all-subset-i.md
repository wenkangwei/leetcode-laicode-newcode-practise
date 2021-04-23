# All Subset I

1.**Link**



**2. 题目**

Given a set of characters represented by a String, return a list containing all subsets of the characters.

**Assumptions**

* There are no duplicate characters in the original set.

​**Examples**

* Set = "abc", all the subsets are \[“”, “a”, “ab”, “abc”, “ac”, “b”, “bc”, “c”\]
* Set = "", all the subsets are \[""\]
* Set = null, all the subsets are \[\]



**3. 思路**

1. DFS
2. In recursion tree,  each level = one char, each branch = choose or no choose that char
3. use pos, position of char to select each different char in action set in each level to avoid duplication of solution
4. Time: O\(2^n\), n = number of action/char in list, since each action can be "pick" or "not pick", it is binary tree
5. Space: O\(logn\)

**4. Coding**

```text
# class Solution(object):
#   def subSets(self, setstr):
#     """
#     input : String setstr
#     return : String[]
#     """
#     # write your solution here
#     # if not setstr:
#     #   return setstr
#     # if len(setstr)==0:
#     #   return [""]
#     decision_set = list(setstr)
#     results = []
#     self.bt(results, [], 0, decision_set)
#     return results

#   def bt(self, results, result, cur_pos, decision_set):
#     if cur_pos == len(decision_set):
#       results.append("".join(result[:]))
#       return

#     # pick current char
#     result.append(decision_set[cur_pos])
#     self.bt(results, result, cur_pos+1, decision_set)
#     result.pop(-1)
#     # Not pick current char
#     self.bt(results, result, cur_pos+1, decision_set)



class Solution(object):
  def subSets(self, setstr):
    """
    input : String setstr
    return : String[]
    """
    # idea: DFS
    # action = char, sol: solution, sols : list of solutions, pos: position of action
    # in recursion tree:
    #  i^th level represent i^th char in action set
    #  branch of the level can be selecting this char, or  not select
    #  then each char is considered once so no char are duplicated, and we can find subset without duplication
    #
    # in recursion tree of dfs:
    # 1. if pos == len(action), all actions are checked, then append solution to sols
    # 2.  select one char in action set at pos
    # 3.  don't not select char in action set at pos, but just move forward to the next char
    #   so no char can be consider twices
    #
    # action = "abc"
    # pos= 0:a          a                     ""
    # pos= 1:b   ab          a""             b         ""
    # pos= 2:c  abc ab    ac     a"""      bc   b    c    ""
    #
    action = list(setstr)
    sol = []
    sols = []
    pos = 0
    self.bt(sols, sol, action, pos)
    return sols
  def bt(self, sols, sol, action, pos):
    if pos == len(action):
      sols.append(sol[:])
      return
    # considering current char
    sol.append(action[pos])
    self.bt(sols, sol, action, pos+1)
    sol.pop(-1)
    # without considering current char
    self.bt(sols, sol, action, pos+1)
```







