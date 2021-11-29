---
description: Medium; DP; Two Pointer; String
---

# 最长不含重复字符的子字符串

1**. Link**

{% embed url="https://leetcode-cn.com/problems/two-sum/submissions/" %}

****

**2. 题目**

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

示例 1:

输入: "abcabcbb" 输出: 3 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。 示例 2:

输入: "bbbbb" 输出: 1 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。 示例 3:

输入: "pwwkew" 输出: 3 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。 请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

来源：力扣（LeetCode） 链接：[https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof) 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



****

**3. 思路**

1. ** 用 set 存放 window/substring里面visited 的char**
2. **用 slow， fast pointer对sustring boundary存放**
3. **用fast point遍历string，当fast point对应的char在set里面就把slow point存放的char 从set中remove 并把slow pointer往前移动， length -=1，直到fast pointer的char不在 set里面，最后把fast point的char加到set并 length +=1. 这样保证了substring的连续性**
4. **更新 longest length of substring**
5. **Time: O(n^2), Space: O(k), K = window size**

**4. Coding**

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        #input: stirng
        # output: len of substring
        #method : sliding window
        #
        #1. use a set to store the element in the window
        #2. use two pointer left, right to be the boundary to the substring
        #3. while right not = len(string):
        #   if right  char not in set: -> append right char and substring len += 1
        #   else pop left char in set and move left forward until right char not in set then append right char to set
        #   update max len of substring
        #
        #
        #

        if not s or len(s)<=1:
            return len(s)
        
        se = set()
        slow, fast = 0, 0 
        cnt = 0
        res = 0
        while fast < len(s):
            if s[fast] in se:
                se.remove(s[slow])
                slow += 1
                cnt -=1
            else:
                se.add(s[fast])
                cnt += 1
                fast += 1
                res = max(res, cnt)
        return res
        
```

