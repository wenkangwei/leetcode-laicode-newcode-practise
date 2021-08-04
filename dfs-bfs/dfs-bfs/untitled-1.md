# All Subset of K size without duplication II

1. **Link**

{% embed url="https://app.laicode.io/app/problem/640" %}



**2. 题目**

Given a set of characters represented by a String, return a list containing all subsets of the characters whose size is K.

**Assumptions**

There are no duplicate characters in the original set.

​**Examples**

Set = "abc", K = 2, all the subsets are \[“ab”, “ac”, “bc”\].

Set = "", K = 0, all the subsets are \[""\].

Set = "", K = 1, all the subsets are \[\].



**3. 思路**

1. **DFS**
2. **action set = char list, pos = action的位置， sol: solution， sols: results**
3. **recursion tree 还是 all subset那一题的recursion tree， 每一个level对应一个char，并且每个branch都要选择或不选择的2分叉**
4. **和all subset 那题不同的事，只有len\(sol\) == k 时才添加到sols里面**
5. **如果sol 的长度= k,就直接加到result里面**
6. **如果sol的长度不为k，在action没有重复值的情况下， 每个action/char 只考虑一次，因此对每个char  都要选择和不选两个选项，并且每一层的pos都对应一个char从而确保char是只考虑一次的，防止因为char的选择先后顺序而导致subset重复**
7. **只有当sol 长度=2时才加到sols并退出，或者当所有char都check了之后就退出\(pos = len\(action\)\)**
8. **Note:**
   1. 如果subset的**字符的顺序也考虑进去**就不能用 pick or not pick action\[pos\]的写法，而应该**替换成一个for loop把后面已选的第i个char和当前的char对调**，从而保证在后面recursion时反过来的顺序的subset也加上去
   2. 每个level只考虑 action\[pos\] 选或者不选，可以除重
   3. 每个level里面用for loop把选择后的element和当前的element对调可以考虑subset的顺序性

\*\*\*\*

**4. Coding**

**写法1**

```text
class Solution(object):
  def subSetsOfSizeK(self, set, k):
    """
    input: string set, int k
    return: string[]
    """
    # write your solution here
    sol = []
    sols = []
    action = list(set)
    pos = 0
    self.bt2(sols, sol , pos, action, k)
    return sols

  def bt(self, sols,sol, pos, action, k):
    if len(sol) ==k:
      sols.append("".join(sol[:]))
      return
    if pos == len(action):
      return 
    
    sol.append(action[pos])
    self.bt(sols,sol, pos+1, action, k)
    sol.pop(-1)

    self.bt(sols,sol, pos+1, action, k)

    
```



**写法2**

```text
def bt2(self, sols,sol, pos, action, k):
    if len(sol) ==k:
      sols.append("".join(sol[:]))
      return
    if pos == len(action):
      return 
    for i in range(pos, len(action)):
      sol.append(action[i])
      self.bt(sols,sol, i+1, action, k)
      sol.pop(-1)
```



