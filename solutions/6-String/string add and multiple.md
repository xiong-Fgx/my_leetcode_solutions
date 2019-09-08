字符串运算中包含了大数加法、大数乘法、数组加数字、二进制计算等一系列题目，这种题目其实是有固定套路的。

# 67. Add Binary

Given two binary strings, return their sum (also a binary string).

The input strings are both **non-empty** and contains only characters `1` or `0`.

**Example 1:**

```
Input: a = "11", b = "1"
Output: "100"
```

**Example 2:**

```
Input: a = "1010", b = "1011"
Output: "10101"
```

---

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        int m = a.size()-1, n = b.size()-1;
        int digit1, digit2, carry = 0;
        string result;
        while(m >= 0 || n >= 0) {
            digit1 = (m >= 0) ? a[m--] - '0' : 0;
            digit2 = (n >= 0) ? b[n--] - '0' : 0;
            int tmp = digit1 + digit2 + carry;
            result.push_back(tmp % 2 + '0');
            carry = tmp / 2;
        }
        if(carry) result.push_back(carry + '0');
        reverse(result.begin(), result.end());
        return result;
    }
};
```

其实可以总结出来这类问题的套路：不管是正常的计算、数组计算还是字符串计算，都可以按照统一的思考方式，就是说考虑进位carry，并且按位操作，就算两者的长度不一样，也可以有一些方法来做，比如上面这道题的解法就是一个很清晰的模板。

```c++
class Solution {
public:
    string add(string a, string b) {
        int m = a.size()-1, n = b.size()-1;
        int digit1, digit2, carry = 0;
        string result;
        while(m >= 0 || n >= 0) {
            //按位考虑，由于长度可能不同，只需要当超过长度的时候的值设置为0即可，并不需要做太多的处理
            digit1 = (m >= 0) ? a[m--] - '0' : 0;
            digit2 = (n >= 0) ? b[n--] - '0' : 0;
            int tmp = digit1 + digit2 + carry;
            //模运算得到值，商运算得到进位值。
            result.push_back(tmp % 2 + '0');
            carry = tmp / 2;
        }
        //尤其要注意计算完成之后有一个大的进位这种情况
        if(carry) result.push_back(carry + '0');
        reverse(result.begin(), result.end());
        return result;
    }
};
```

