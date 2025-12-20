# All Permutations of Subsets without duplication

1. **Links**&#x20;

{% embed url="https://app.laicode.io/app/problem/643" %}



1. **题目**
2.  Given a string with no duplicate characters, return a list with all permutations of the string and all its subsets.

    Examples

    Set = “abc”, all permutations are \[“”, “a”, “ab”, “abc”, “ac”, “acb”, “b”, “ba”, “bac”, “bc”, “bca”, “c”, “cb”, “cba”, “ca”, “cab”].

    Set = “”, all permutations are \[“”].

    Set = null, all permutations are \[].<br>
3. **思想**
   1. DFS
   2. 有n个action就有n个position要遍历
   3. 在每个position选择一个action到该位置，剩下的action放到后面，即把选择后的action和前面pos位置的action交换，然后DFS进入下一个位置时pos+=1 就可以选择剩下的没选的action
   4. 在用“”来不选择action时有可能会出现重复的情况，比如\["", b,""] 和\["", "", b] 虽然在不同位置选择b，但是最后join的string是一样的，所以要用set进行对result list除重
   5. Time: O(B\*H), B= tree branch, H= height of tree = number of actions to use
   6. Space: O(H) for recursion, O(n) for storing result so O(H + n)
4. **Coding**

```
class Solution(object):
  def allPermutationsOfSubsets(self, s):
    """
    input: string set
    return: string[]
    """
    # write your solution here
    # tree level = the position we need to fill
    #           [, , , ]
    #    [a, , ,]                     [b, , ,]         [c, , ,]  [c, , ,]
    # [a,b,,], [a,c,,] [a,,,]    [b, a, ,], [b,c, ,]...
    #
    #
    action = list(s)
    sol = []
    sols = set()
    self.bt(sol, sols, action, 0)
    return sols
  def bt(self,sol, sols, action, pos):
    if pos == len(action):
      res = "".join(sol[:])
      # if res not in sols:
      # sols.append(res)
      sols.add(res)
      return
    # add one action
    for i in range(pos, len(action)):
      sol.append(action[i])
      # swap the selected action to the begin of the selectable array
      # so that we will not select the action that already picked
      action[pos], action[i] = action[i], action[pos]
      self.bt(sol, sols, action, pos+1)
      # swap it back 
      action[pos], action[i] = action[i], action[pos]
      sol.pop(-1)
    # don't add anything
    self.bt(sol, sols, action, pos+1)

```

