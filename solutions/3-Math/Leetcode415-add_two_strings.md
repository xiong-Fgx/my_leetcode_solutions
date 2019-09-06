# Leetcode415. Add Strings

Given two non-negative integers `num1` and `num2` represented as string, return the sum of `num1` and `num2`.

**Note:**

1. The length of both `num1` and `num2` is < 5100.
2. Both `num1` and `num2` contains only digits `0-9`.
3. Both `num1` and `num2` does not contain any leading zero.
4. You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.

---

这道题就是大数加法，其中的坑还是挺多的，不过都是一些需要考虑的细节问题，整体来说的思路还是很简单的，下面来分析：

- 一开始的思路是这样的：
  - 如果两个数的长度是不一样的，那么需要考虑，两个共有的部分长度进行计算完之后，把剩余的部分加在后面。
  - 使用一个carry作为进位符，计算的每一位都把这个carry加上，并产生一个新的carry
  - 最后的结果需要把前面的0全部删掉

根据上面的思路，写出的代码如下：

```c++
class Solution {
public:
    string addStrings(string num1, string num2) {
        int m = num1.size(), n = num2.size(), carry = '0';
        reverse(num1.begin(), num1.end());
        reverse(num2.begin(), num2.end());
        string result(max(m,n) + 1, '0');
        int i = 0;
        while(i < m && i < n) {
            result[i] = (carry + num1[i] + num2[i] - '0'*3) % 10 + '0';
            carry = (carry + num1[i] + num2[i] - '0'*3) / 10 + '0';
            i++;
        }
        while(i < m) {
            result[i] = (carry + num1[i] - 2 * '0') % 10 + '0';
            carry = (carry + num1[i] - 2 * '0') / 10 + '0';
            i++;
        }
        while(i < n) {
            result[i] = (carry + num2[i] - 2 * '0') % 10 + '0';
            carry = (carry + num2[i] - 2 * '0') / 10 + '0';
            i++;
        }
        if(carry != '0') result[i] = carry;
        reverse(result.begin(), result.end());

        int j = 0;
        while(result[j]=='0')j++;
        //case 1 0+0 error
        if(j == result.size())return "0";
        result = result.substr(j,result.size());

        return result;
    }
};
```

- 经过多次提交发现了很多问题，下面是出错的case
  1. `'0' + '0'`由于结果只有一个0，如果不进行case 1修复，则会出现返回值为空的错误。
  2. `'9' + '99'`如果在计算完相同长度的之后，不考虑carry，则会导致这个错误。
  3. `'584' + '18'`与上面类似，都是在考虑进位的时候出错的。

- 而且有很多可以优化的地方：
  - 不管是否需要考虑是不是重叠部分，都需要考虑进位，可以进行合并。
  - 不对num进行逆序，直接用正序来算，这里只是索引的时候需要注意一些细节。
  - result使用一个string类型的变量，因为计算的时候从低位开始，只需要用计算出来的新结果+result即可实现把新计算出来的结果放在前面的效果。
  - 进行临时计算的时候将每一位上的数转换为int型处理会比较方便。

按上面的方法修改之后为：

```c++
class Solution {
public:
    string addStrings(string num1, string num2) {
        if(num1.size() < num2.size()) swap(num1, num2);
        int m = num1.size() - 1, n = num2.size() - 1;
        int digit1 = 0, digit2 = 0, carry = 0;
        while(m >= 0) {
            digit1 = num1[m] - '0';
            if(n >= 0)
                digit2 = num2[n--] - '0';
            else
                digit2 = 0;
            int sum = digit1 + digit2 + carry;
            num1[m] = sum % 10 + '0';
            m--;
            carry = sum / 10;
        }
        if(carry) return '1' + num1;
        else return num1;
    }
};
```

总结一下，这种处理字符串计算的题，最好还是将字符串中的每一位转化成数字来计算，这样不仅容易理解，而且还不容易出错。

