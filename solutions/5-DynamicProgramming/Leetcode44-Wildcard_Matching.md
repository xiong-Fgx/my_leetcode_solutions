# 44. Wildcard Matching

这道题也是正则表达式的匹配，与第10题不同的地方在于，它的*号可以任意匹配，而不是复制前面的一个字符0次或多次，因此会相对来说比较简单一些。

Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'`.

```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
```

The matching should cover the **entire** input string (not partial).

**Note:**

- `s` could be empty and contains only lowercase letters `a-z`.
- `p` could be empty and contains only lowercase letters `a-z`, and characters like `?` or `*`.

**Example 1:**

```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```
Input:
s = "aa"
p = "*"
Output: true
Explanation: '*' matches any sequence.
```

**Example 3:**

```
Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
```

**Example 4:**

```
Input:
s = "adceb"
p = "*a*b"
Output: true
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
```

**Example 5:**

```
Input:
s = "acdcb"
p = "a*c?b"
Output: false
```

---

## Solution

首先，可以把前面的方法直接搬过来，只是其中一些细节不需要考虑

`dp[i][j]`表示s到i位置,p到j位置是否匹配!

初始化:

`dp[0][0]`:什么都没有,所以为`true`
第一行`dp[0][j]`,换句话说,`s`为空,与`p`匹配,所以只要`p`开始为`*`才为`true`
第一列`dp[i][0]`,当然全部为`false`

递推方程：

如果`(s[i] == p[j] || p[j] == "?")` ,有`dp[i][j] = dp[i-1][j-1]`

如果`p[j] == "*" ` 有`dp[i][j] = (dp[i-1][j] = true || dp[i][j-1] = true)`

 note:

 `dp[i][j-1]`,表示`*`代表是空字符,例如`ab,ab*`

 `dp[i-1][j]`,表示`*`代表非空任何字符,例如`abcd,ab*`

举个例子

比如对于`abcde`和`a*e`来说:

|      | a     | *                  | e                    |
| ---- | ----- | ------------------ | -------------------- |
| a    | true  | `=dp[i][j-1]`=true | false                |
| b    | false | `=dp[i-1][j]`=true | false                |
| c    | false | `=dp[i-1][j]`=true | false                |
| d    | false | `=dp[i-1][j]`=true | false                |
| e    | false | `=dp[i-1][j]`=true | `=dp[i-1][j-1]`=true |

从这个递推表就可以很清晰的看出，`dp[i][j] = dp[i-1][j]`表示的含义就是跳过字符串s中的字符`s[i]`。

```c++
class Solution {
public:
    bool isMatch(string s, string p) {
        vector<vector<bool>> result(s.size()+1, vector<bool>(p.size()+1, false));
        result[0][0] = true;
        for(int j = 1; j < p.size()+1; j++)
            if(p[j-1] == '*')
                result[0][j] = result[0][j-1];

        for(int i = 1; i < s.size() + 1; i++) {
            for(int j = 1; j < p.size() + 1; j++) {
                // 当s与p的值相等，或者p的值为?时直接通过
                if(s[i-1] == p[j-1] || p[j-1] == '?') result[i][j] = result[i-1][j-1];
                // 当p的值为*号时：以下两种case均可
                // case 1: p中的*表示空：result[i][j] = result[i][j-1]
                // case 2: p中的*匹配s中的一个字符：result[i][j] = result[i-1][j]
                // 至于，为什么这样做可以使得*匹配多个s中的字符，可以这样理解：
                // 因为i从1遍历到s.size()+1，而且对于每一个p中的字符，都与s中的字符进行过比较
                // 所以跳过一个之后可以再跳过一个
                if(p[j-1] == '*') {
                    result[i][j] = result[i][j-1] | result[i-1][j];
                }
            }
        }
        return result[s.size()][p.size()];

    }
};
```

