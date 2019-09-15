# 10. Regular Expression Matching

题目的要求是进行正则表达式匹配

其中

` . 可以匹配任意单个字符`

`* 等于0个或多个前面的字符`

Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```

The matching should cover the **entire** input string (not partial).

**Note:**

- `s` could be empty and contains only lowercase letters `a-z`.
- `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

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
p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```
Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

**Example 4:**

```
Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".
```

**Example 5:**

```
Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
```

---

## Solution

这道题的思路框架很简单，就是使用动态规划的方法

下面就是动态规划的递推公式，其中`T[i][j]`表示`s[0:i]`和`p[0:j]`是否能匹配。

![img递推公式](https://assets.leetcode.com/users/ckc721/image_1559480744.png)

```c++
class Solution {
public:
    bool isMatch(string s, string p) {
        vector<vector<bool>> result(s.size()+1, vector<bool>(p.size()+1, false));
        result[0][0] = true;
        // 当p出现a*b*c*或者a*b*之类的形式时，s是空串也是正确的，因此先把这种情况考虑进来。
        for(int j = 1; j < p.size()+1; j++)
            if(p[j-1] == '*')
                result[0][j] = result[0][j-2];

        // 下面就来更新递推表
        for(int i = 1; i < s.size() + 1; i++) {
            for(int j = 1; j < p.size() + 1; j++) {
                // case 1:
                if(s[i-1] == p[j-1] || p[j-1] == '.') 
                    result[i][j] = result[i-1][j-1];
                else if(p[j-1] == '*') {
                    // case 2:
                    result[i][j] = result[i][j-2];
                    if(s[i-1] == p[j-2] || p[j-2] == '.') {
                        // case 3:
                        // 
                        result[i][j] = result[i][j] | result[i-1][j];
                    }
                }
                else result[i][j] = false;
            }
        }
        return result[s.size()][p.size()];

    }
};
```

至于这里case3里面为什么这么写，可以列个表看看

比如对于下面这个输入

```
"aaa" 
"ab*a*c*a"
```

|      | 0     | a     | b     | *     | a     | *                | c     | *           | a     |
| ---- | ----- | ----- | ----- | ----- | ----- | ---------------- | ----- | ----------- | ----- |
| 0    | true  | false | false | false | false | false            | false | false       | false |
| a    | false | true  | false | true  | false | true\|false=true | false | true\|false | false |
| a    | false | false | false | false | true  | false\|true=true | false | true\|false | true  |
| a    | false | false | false | false | false | false\|true=true | false | true\|false | true  |

- 从上面的这个表里可以看出，当前的字符为\*的时候，可以表示为重复0次，重复1次，重复多次，

  - 如果是重复0次的话，表示当前的第j个p和j-1个p都跳过，那么有`dp[i][j] = dp[i][j-2]`

  - 如果是重复1次的话，那么有`dp[i][j] = dp[i-1][j]`

  因此，case3里面写的 `result[i][j] = result[i][j] | result[i-1][j];`

  实际上指的是当第`i`个`s`和第`j`个`p`的值相等时，这个\*号可以选择跳过或者不跳过也就是0次或多次都是有可能的，也就是` result[i][j] = result[i][j-2] | result[i-1][j];`

- 如果想要避免歧义，这里可以这样写：

  ```c++
  class Solution {
  public:
      bool isMatch(string s, string p) {
          vector<vector<bool>> result(s.size()+1, vector<bool>(p.size()+1, false));
          result[0][0] = true;
          // 当p出现a*b*c*或者a*b*之类的形式时，s是空串也是正确的，因此先把这种情况考虑进来。
          for(int j = 1; j < p.size()+1; j++)
              if(p[j-1] == '*')
                  result[0][j] = result[0][j-2];
  
          // 下面就来更新递推表
          for(int i = 1; i < s.size() + 1; i++) {
              for(int j = 1; j < p.size() + 1; j++) {
                  // case 1:
                  if(s[i-1] == p[j-1] || p[j-1] == '.') 
                      result[i][j] = result[i-1][j-1];
                  else if(p[j-1] == '*') {
                      if(s[i-1] == p[j-2] || p[j-2] == '.') {
                          // 虽然*前面的那个字符和s中正在判断的那个字符是相同的，
                          // 这时可以选择跳过或者再多保留1次（和j-2的相同就表示跳过）
                          // 如果j-2是false，那就选择保留1次，二者只要一个成立即可
                          result[i][j] = result[i][j-2] | result[i-1][j];
                      }
                      else
                          // *前面那个字符和s中正在判断的字符不同，这时是跳过
                          result[i][j] = result[i][j-2];
                  }
                  else result[i][j] = false;
              }
          }
          return result[s.size()][p.size()];
  
      }
  };
  ```

  