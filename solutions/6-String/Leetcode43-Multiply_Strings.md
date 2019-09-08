# 43. Multiply Strings

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Example 1:**

```
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**

```
Input: num1 = "123", num2 = "456"
Output: "56088"
```

**Note:**

1. The length of both `num1` and `num2` is < 110.
2. Both `num1` and `num2` contain only digits `0-9`.
3. Both `num1` and `num2` do not contain any leading zero, except the number 0 itself.
4. You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.

---

这个就相当于是求一个大数乘法，这个思路是很巧妙的。

- 平时列竖式解题是这样解的：
  - 用下面那个数的每一位分别于上面那个数相乘，然后将乘出来的结果相加。
  - 有一个规律是这样的：第一个数的第`i`位和第二个数的第`j`位相乘的积影响第`i+j+1`和`i+j`位

|              |              | 1            | 2            | 3            |
| ------------ | ------------ | ------------ | ------------ | ------------ |
|              |              |              | 4            | 5            |
| ------------ | ------------ | ------------ | ------------ | ------------ |
|              |              |              | 1            | 5            |
|              |              | 1            | 0            |              |
|              | 0            | 5            |              |              |
| ------------ | ------------ | ------------ | ------------ | ------------ |
|              |              | 1            | 2            |              |
|              | 0            | 8            |              |              |
| 0            | 4            |              |              |              |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| index = 0    | index = 1    | index = 2    | index = 3    | index = 4    |

因此代码如下：

```c++
class Solution {
public:
    string multiply(string num1, string num2) {
        int m1 = num1.size(), n1 = num2.size();
        string result(m1 + n1, '0');
        for(int m = m1-1; m >=0; m--) {
            for(int n = n1-1; n >= 0; n--) {
                int digit1 = num1[m] - '0', digit2 = num2[n] - '0';
                int tmp = result[m+n+1] - '0' + digit1 * digit2;
                result[m+n+1] = tmp % 10 + '0';
                result[m+n] += tmp / 10;
            }
        }
        for(int i = 0; i < m1 + n1; i++) {
            if(result[i]!='0') {
                return result.substr(i);
            }
        }
        return "0";
    }
};
```

