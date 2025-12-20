# 2 Sum All Pair II

&#x31;**. Link**

**2. 题目**

Find all pairs of elements in a given array that sum to the pair the given target number. Return all the **distinct** pairs of values.

**Assumptions**

* The given array is not null and has length of at least 2
* The order of the values in the pair does not matter

**Examples**

* A = {2, 1, 3, 2, 4, 3, 4, 2}, target = 6, return \[\[2, 4], \[3, 3]]

**3. 思路**

**Time: O(nlogn + n), sorting + binary search iteration**

**4. Coding**

```
class Solution(object):
  def allPairs(self, array, target):
    """
    input: int[] array, int target
    return: int[][]
    """
    # write your solution here
    # method 1: sort + Binary search
    # method2: DFS
    #
    if not array:
      return []
    array.sort()
    # dic = {}
    # for i in range(len(array)):
    #   if array[i] not in dic:
    #     dic[array[i]] =0
    #   dic[array[i]] +=1
    # array = list(set(array))
    left, right = 0, len(array)-1
    res = []
    prev_val = [None, None]
    while left < right-1:
      sum_val = array[left] + array[right]
      if sum_val< target:
      #   if dic[array[left]] >1 and 2*array[left] == target:
      #     res.append([array[left], array[right]])  
        left += 1
      elif sum_val > target:
        right -= 1
      else:
        # avoid duplication of result
        if not(prev_val[0] == array[left] and prev_val[1] == array[right]):
          res.append([array[left], array[right]])
          prev_val[0] = array[left]
          prev_val[1] = array[right]
        right -= 1

    if not(prev_val[0] == array[left] and prev_val[1] == array[right]) and array[left] + array[right] == target:
          res.append([array[left], array[right]])
          prev_val[0] = array[left]
          prev_val[1] = array[right]
        
    return res


```



