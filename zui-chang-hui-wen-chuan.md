---
description: Medium; String;  Palindrome;
---

# 最长回文串

1. **Link**

{% embed url="https://leetcode-cn.com/problems/longest-palindrome/" %}

\*\*\*\*

**2. 题目**

给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 "Aa" 不能当做一个回文字符串。

注意: 假设字符串的长度不会超过 1010。

示例 1:

输入: "abccccdd"

输出: 7

解释: 我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。

来源：力扣（LeetCode） 链接：[https://leetcode-cn.com/problems/longest-palindrome](https://leetcode-cn.com/problems/longest-palindrome) 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**3. 思路**

1. 遍历所有字符，统计出现频率
2. 对于出现频率为偶数或奇数，把count += freq//2
3. 遇到第一个奇数时，加上1 palindrome的中间点并记录, 之后遇到奇数时要把多余的1个char去掉
4. Time: O\(n + k \), n= array length,  k= unique char in dictionary

**4. Coding**

```text
class Solution:
    def longestPalindrome(self, s: str) -> int:
        if len(s)<=1:
            return len(s)
        dic = {}
        for i in range(len(s)):
            if s[i] not in dic:
                dic[s[i]] =0
            dic[s[i]] += 1
        cnt = 0

        for i in dic.keys():            
            cnt += (dic[i]//2)*2
            # 多出来的 1 个奇数加进去 
            if dic[i] %2 ==1 and cnt %2 == 0:
                cnt += 1
        return cnt
```





