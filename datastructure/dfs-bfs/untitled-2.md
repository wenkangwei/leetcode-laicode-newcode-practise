# Combinations Of Coins

**1.Link**

**2. 题目**

Given a number of different denominations of coins \(e.g., 1 cent, 5 cents, 10 cents, 25 cents\), get all the possible ways to pay a target number of cents.

**Arguments**

* coins - an array of positive integers representing the different denominations of coins, there are no duplicate numbers and the numbers are sorted by descending order, eg. {25, 10, 5, 2, 1}
* target - a non-negative integer representing the target number of cents, eg. 99

**Assumptions**

* coins is not null and is not empty, all the numbers in coins are positive
* target &gt;= 0
* You have infinite number of coins for each of the denominations, you can pick any number of the coins.

**Return**

* a list of ways of combinations of coins to sum up to be target.
* each way of combinations is represented by list of integer, the number at each index means the number of coins used for the denomination at corresponding index.

**Examples**

coins = {2, 1}, target = 4, the return should be

\[

  \[0, 4\],   \(4 cents can be conducted by 0 \* 2 cents + 4 \* 1 cents\)

  \[1, 2\],   \(4 cents can be conducted by 1 \* 2 cents + 2 \* 1 cents\)

  \[2, 0\]    \(4 cents can be conducted by 2 \* 2 cents + 0 \* 1 cents\)

\]



**3. 思路**

1. **Idea: DFS**
2. **For recursion tree, each level represents a coin and each branch in this level represent the count of this coin, so action = coin \* count**
3. **if all coins have been checked, pos == len\(action\), then check if target ==0. if so, append solution to result list sols**
4. Otherwise, in the coin at position pos of coin list, find the maximum count it can be target//coin.  
5. For all count &lt;=  target//coin, add action coin\*count to solution and do DFS, then pop that action and check the next action
6. Time: O\(B\*H\), B = branch of tree,  H = number of coins
7. Space: O\(logH\) for recursion O\(n\) for storing results

**4. Coding**

```text

class Solution(object):
  def combinations(self, target, coins):
    """
    input: int target, int[] coins
    return: int[][]
    """
    # idea:
    #  pos: position of action, used to select action in each level of tree
    #  in backtracking:
    # each level represents a coin
    # each branch represents count of that coin, so action = coin *  count
    #  1. check if pos == len(action): all actions have been check, then if target == 0 after checking all action, append sol
    #  2. if there is still some actions without checking
    #     find the action/ coins, and use target//coin to find the maximum count of that coin
    #     then the branch in this level =  amount of different cnt of this coin
    #  3. append action with this cnt and update target in the next level
    #  4. pop the cnt action of current coin and choose another cnt action
    sol = []
    sols = []
    self.bt(sols, sol, 0, coins, target)
    return sols
  def bt(self,sols, sol, pos, actions, target):
    
    if pos == len(actions):
      if target == 0:
        sols.append(sol[:])
      return
    a = actions[pos]
    for cnt in range(target//a, -1, -1):
      sol.append(cnt)
      self.bt(sols, sol, pos+1, actions, target - cnt*a)
      sol.pop(-1)
    return 
```









