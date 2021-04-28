# Array Hopper I

1. **Link**

{% embed url="https://app.laicode.io/app/problem/88" %}



**2. 题目**

Given an array A of non-negative integers, you are initially positioned at index 0 of the array. **A\[i\] means the maximum jump distance from that position \(you can only jump towards the end of the array\).** Determine if you are able to reach the last index.

**Assumptions**

* The given array is not null and has length of at least 1.

**Examples**

* {1, 3, 2, 0, 3}, we are able to reach the end of array\(jump to index 1 then reach the end of the array\)
* {2, 1, 1, 0, 2}, we are not able to reach the end of array



**3. 思路**

1. dp\[i\]  state = maximum index can reach in array\[0:i+1\]
2. transition function
   1. when dp\[i-1\] &lt; i, can not reach index i, then dp\[i\] = dp\[i-1\]
   2. when dp\[i-1\] &gt; i, can reach index i, then dp\[i\] = max\(dp\[i-1\], max\_i\)
   3. max\_i = maximum index can reach when starting from current index i

**4. Coding**

```text
class Solution(object):
  def canJump(self, array):
    """
    input: int[] array
    return: boolean
    """
    # write your solution here
    #
    #idea: dp
    #dp[i] state = the maximum distance, starting from index =0, can reach in array[0:i+1]
    # base case: dp[0] = array[0]
    #dp[1]  = max(max distance starting from 1, dp[0]) if index =1 can be reach, that is, if dp[i-1]>=i, else dp[1] = dp[0], dp[i]= dp[i-1] 
    #so
    #
    #
    if not array:
      return False
    if len(array) ==1:
      return True
    dp = [0] *len(array)
    dp[0] = array[0]
    for i in range(1, len(array)):
      if dp[i-1] >= i:
        #find max distance starting from current idx
        j = i
        while j < len(array):
          if array[j] == 0:
            break
          j += array[j]
        # compare previous max distance and max distance starting from current idx
        dp[i] = max(dp[i-1], j)
      else:
        dp[i] = dp[i-1]
    return dp[len(array)-1] >= len(array)-1

    

```



