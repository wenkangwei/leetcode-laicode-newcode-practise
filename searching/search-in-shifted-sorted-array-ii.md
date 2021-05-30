# Search In Shifted Sorted Array II



Given a target integer T and an integer array A, A is sorted in ascending order first, then shifted by an arbitrary number of positions.

For Example, A = {3, 4, 5, 1, 2} \(shifted left by 2 positions\). Find the index i such that A\[i\] == T or return -1 if there is no such index.

**Assumptions**

* There could be duplicate elements in the array.
* Return the smallest index if target has multiple occurrence. 

**Examples**

* A = {3, 4, 5, 1, 2}, T = 4, return 1
* A = {3, 3, 3, 1, 3}, T = 1, return 3
* A = {3, 1, 3, 3, 3}, T = 1, return 1

​**Corner Cases**

* What if A is null or A is of zero length? We should return -1 in this case.



思路：

1. 先把所有的 array\[mid\]在target右边的情况全列出来，并更新right
2. 当 array\[mid\] == array\[0\] 时我们不清楚mid是在target的右边还是左边，就默认把left +=1右移逐个检查
3. else默认左移right

```text
class Solution(object):
  def search(self, array, target):
    """
    input: int[] array, int target
    return: int
    """
    # write your solution here
    if not array:
      return -1
    left, right = 0,  len(array)-1
    stored_idx = []
    while left < right-1:
      if array[left] ==target or array[right] ==target:
          break
      mid = (left + right)//2
      # when target == array[0], we need to move right to mid as well
      if (array[mid]>=target and target > array[0] ) or (array[mid]>=target and array[mid]< array[0] )  or ( target >= array[0] and array[mid] < array[0]):
        right = mid
      elif array[mid] == array[0]:
        #将 left, right searching space 往中间缩小
        left += 1
      else:
        left = mid
    if array[left] == target:
      return left
    if array[right] == target:
      return right
    return -1

```



