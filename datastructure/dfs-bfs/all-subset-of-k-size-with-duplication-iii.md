# All Subset of K size III (with duplication without considering order)

1**.Link**

**2.题目**

**Assumptions**

There could be duplicate characters in the original set.\


**Examples**

Set = "abc", K = 2, all the subsets are \[“ab”, “ac”, “bc”].

Set = "abb", K = 2, all the subsets are \[“ab”, “bb”].

Set = "abab", K = 2, all the subsets are \[“aa”, “ab”, “bb”].

Set = "", K = 0, all the subsets are \[""].

Set = "", K = 1, all the subsets are \[].

****

**3. 思路**

1. **在原来的 all subset II without duplication 的基础里面，在不选当前的char， action\[pos] 时把所有的相同的char都不考虑进去** &#x20;
2.  **example**

    1. **action = \[a b  e e e  c  d]**
    2. 如果我考虑第一个e，那么就考虑 剩下的e并且每个e都只考虑一次，那么就会有 “e...”, "ee...", "eee..."  这3中情况并且1个e, 2个e, 3个e的情况都是只出现一次
    3. 如果我不考虑第一个e，那么就不考虑剩下的所有e了。因为如果在不选第一个e的情况下考虑其他e，就会出现“ee..”, "eee.."的情况和 case2 重复
    4. 所以代码变成



    ```
    idx = pos +1
        while idx <len(action) and action[idx] == action[pos]:
          idx += 1
        # skip all duplicated char to move the next char
        self.bt(sols, sol, action,idx, k)

    ```

**4. Coding**

```
class Solution(object):
  def subSetsIIOfSizeK(self, set, k):
    """
    input: string set, int k
    return: string[]
    """
    #
    #idea: DFS
    # each level in recursion tree = an action of char from action set. Then the next level should exclude that action
    # then branch in one level = picking one action from the remaining action set
    # sol: solution, sols: list of solutions, pos: position of the action, used to select action
    # 1. check if solution length == k, if so add solution to result list
    # 2.  select one action from the remaining actions, that is, index >= pos and index  <len(action)
    # 3. add the action to the solution and use bt recursion to search action "after the position selected action"
    #    so that we will not pick the same solution again
    #   For example  [a, b, c, d]
    #  after we pick b in the for loop, then we need to pick  c, d, rather than a,c,d
    #  because when we pick b and then a, to get [b,a], it is as same as we pick a, then pick b to get [a,b]
    #
    #  if we pick char before and after current char, then order matters. If we pick char only after current char,
    #  order not matter
    #
    #base case: k =0, return [""]
    action = list(set)
    action.sort()
    sol = []
    sols = []
    self.bt(sols, sol, action,0, k)
    return sols
  def bt(self, sols, sol, action, pos, k):
    if len(sol) == k:
      sols.append("".join(sol[:]))
      return
    if pos == len(action):
      return
    # each char is considered once
    # and append once
    # we consider the duplicated char
    sol.append(action[pos])
    self.bt(sols, sol, action,pos+1, k)
    sol.pop(-1)

    # if not append current char
    # move index to the next char if the there are multiple repeated current chars
    # so that we will not consider duplicated cases
    idx = pos +1
    while idx <len(action) and action[idx] == action[pos]:
      idx += 1
    # skip all duplicated char to move the next char
    self.bt(sols, sol, action,idx, k)


```

