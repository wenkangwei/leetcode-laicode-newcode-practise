# All Permutation II (with duplication and consider order)

1**. Link**

{% embed url="https://app.laicode.io/app/problem/65" %}

****

**2. 题目**

Given a string with possible duplicate characters, return a list with all permutations of the characters.

**Examples**

* Set = “abc”, all permutations are \[“abc”, “acb”, “bac”, “bca”, “cab”, “cba”]
* Set = "aba", all permutations are \["aab", "aba", "baa"]
* Set = "", all permutations are \[""]
* Set = null, all permutations are \[]



**3. 思路**

1. 先把重复的char用**dictionary表示除重，key= char， value= count** action = key
2. DFS method, 因为是考虑order顺序，所以需要**用for loop对不同的char进行位置**对换
3. 对于重复的char，每次pick action时把它的count -= 1 表示剩下可以选的次数，如果没有，count=0，就选下一个action.这样能避免同一个level里面有相同的tree branch 分支，因为**每次level选取的char都保证是不一样**
4.  recursion tree 变成： exmaple

    ```
    For example:   str = "aaab"
    #       root
    #      a:1        b:2          the number represents the count of a that can be selected
    #   aa:0  ab:1      ba     
    #  aab    aba     baa   



    str = 'abbcdd'
    {'a':1, 'b':2, 'c':1, 'd':2}
                  a:1
              ab:1                   ac
         abb:0  abc:1,..      acb:1   acd
     abbc      acbb:0        ....
     
    ```

**4. Coding**

```
class Solution(object):
  def permutations(self, input):
    """
    input: string input
    return: string[]
    """
    # write your solution here
    # main idea:
    # to avoid duplicated tree branch due to duplicate string, we can restrict
    # each branch of tree contain distinct chars only
    # Then for the duplicated char, we can repeat the duplicated char in level of tree
    # For example:   str = "aaab"
    #       root
    #      a:1        b:2          the number represents the count of a that can be selected
    #   aa:0  ab:1      ba     
    #  aab    aba     baa   
    # Since we want to count the amount of duplicated char that can be selected, 
    # So it leads to the idea of using dictionary to store keys and counts
    # Time complexity: O(n) for dictionary construction,  O(B*H) for recursion tree traversal, 
    # O(n) for creating new string solution in tree node. 
    # So time complexity: O(n)+O(B*H)*O(n) = O(B*H)*O(n) in total
    # 
    # Space complexity:  O(n)  for build dictionary, O(H) for recursion, H= tree height, so O(H+n) in total

    sol = []
    sols = []
    ls = list(input)
    ls.sort()
    action = {}
    for c in ls:
      if c not in action.keys():
        action[c] = 0
      action[c] += 1
    pos = 0
    N = len(ls)
    self.bt(sols, sol, pos, N, action)
    return sols

  def bt(self, sols, sol, pos, N, action):
    if len(sol) == N:
      sols.append("".join(sol[:]))
      return

    for act in action.keys():
      if action[act] != 0:
        action[act] -= 1
        sol.append(act)
        self.bt(sols, sol, pos+1, N, action) 
        sol.pop(-1)
        # recover the count of char since we want to reuse it in other branches of recursion tree
        action[act] += 1
```











