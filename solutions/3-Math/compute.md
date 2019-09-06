# 数学运算

| 题目         | 功能                            |
| ------------ | ------------------------------- |
| Leetcode 29  | Divide Two Integers  两数除法   |
| Leetcode 50  | Pow(x, n)  乘方                 |
| Leetcode 69  | Sqrt(x)  开方                   |
| Leetcode 231 | Power of Two  判断是否为2的开方 |

## Basic

| 7    | 6    | 5    | 4    | 3    | 2    | 1    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 64   | 32   | 16   | 8    | 4    | 2    | 1    |

- **关于位运算，有以下性质**：

**性质1：**

​	因为数字可以表示成为二进制，因此可以表示成：Σ1<<2的幂

​	**举个例子：**

​	19 = 16 + 2 + 1

​	可以表示为：

​	19 = (1<<4) + (1<<1) + (1<<0)

**性质2：**

​	对于任意数来说，左移1位都相当于乘2

​	**举个例子：**

​	3 * 19 = 57

​	可以表示为：

​	因为：19 = 1 * 19 = (1<<4) + (1<<1) + (1<<0)

​	因此：57 = 3 * 19 = (3<<4) + (3<<1) + (3<<0)



## Leetcode29 两数相除

思路：

- 假设被除数为a，除数为b，则找到商c = a / b

  可以直接比对上面的性质2

  已知c和a，要求b，那就用位运算的方式来做

  一步一步确定b的每一位上分别是多少。

- 由上面的分析可知，因为要求的值是一个int型，因此这种做法的时间复杂度固定为O(32)

- 这道题里面需要考虑很多的异常情况，比如各种边界

  - 当被除数 `dividend=-INT_MIN` 的时候，是不能直接通过正负号将其转换为正值的，这里直接将他设置成一个long long类型，省掉很多麻烦。
  - 当除数=0的时候，直接返回0

- 这道题的解法是很精妙的，首先，设置一个长度为33的数组（主要是为了防止越界，多设置1个），这个数组的第i个元素的作用是用来存储**结果的第i位上的值与divisor的移位结果（例如数组中的第3位存储3<<4的值）**，一直找到那个3<<i 大于dividend值的地方。

- 第二次遍历的时候，从最高位开始，遍历这个长度为33的数组，用dividend减去数组中的值，如果结果大于0，则表示这一位可以选进来，值为1，因为这个数组中存的是divisor与2的幂相乘的结果，我们要求是商，也就是2的幂，只需将1左移i位即可，把它加进来。

```c++
class Solution {
public:
    int divide(int dividend, int divisor) {
        if(divisor == 1)return dividend;
        if(divisor == -1 && dividend == INT32_MIN) return INT32_MAX;
        if(divisor == -1)return -dividend;
        int sign = ((dividend > 0 && divisor < 0) || (dividend < 0 && divisor > 0)) ? -1 : 1;
        long long i = 0;

        vector<long long> bitNum(33, 0);
        long long dvd = dividend > 0 ? dividend : -(long long)dividend;
        long long dvs = divisor > 0 ? divisor : -(long long)divisor;

        long long tmp = dvs;


        bitNum[0] = tmp;

        while(tmp <= dvd) {
            tmp <<= 1;
            bitNum[++i] = tmp; // 找到最高位
        }
        //因为上面的循环最后一次一定会使得tmp > dvd，因此bitNum的最高一位中存的数一定比
        //dvd更大，在后面做直接将其跳过不做判断。
        i--;
        int result = 0;
        while(dvd >= dvs) {
            if(dvd >= bitNum[i]) {
                dvd -= bitNum[i];
                result += (1 << i);
            }
            else i--;
        }

        if(result > INT32_MAX && sign > 0)return INT32_MAX;
        return (int)sign*result;
    }
};
```



---

## Leetcode50 乘方pow

这道题很容易想到的一种方法是用递归+二分调用，实际上，每个递归调用都会出现重复计算的问题，因此在使用递归的时候，一定要思考如何优化来减少重复计算。

- 思路：将大问题拆成小问题，

  ```c++
  //当m为偶数时：
  pow(n,m) = pow(n,m/2) * pow(n,m/2);
  //当m为奇数时：
  pow(n,m) = pow(n,m/2) * pow(n,m/2) * n;
  ```

- 就根据前面的这个思路，把代码写出来如下：

```c++
class Solution {
public:
    double compute(double x, long long n) {
        if(n == 1) return x;
        if(n == 0) return 1;
        if(n % 2 == 0) return compute(x, n/2) * compute(x, n/2);
        else return compute(x, n/2) * compute(x, n/2) * x;
    }
    
    double myPow(double x, int n) {
		long long N = n > 0 ? n : -(long long)n;
        return compute(x, N);
    }
};
```

- 使用递归调用的一个常见问题就是容易出现重复计算的问题，在上面的问题里也同样出现了重复计算，`if(n % 2 == 0) return compute(x, n/2) * compute(x, n/2);`这行代码中，`compute(x,n/2)`就执行了两次，这样就大大地降低了效率。果然在n很大的时候超时了。
- 既然有重复计算，那么就可以只计算一次，把compute(x,n/2)提取出来单独计算即可，这样就只需要执行一次计算。经过改进后的代码如下：

```c++
class Solution {
public:
    double compute(double x, long long n) {
        if(n == 1) return x;
        if(n == 0) return 1;
        double half = compute(x, n/2);
        if(n % 2 == 0) return half * half;
        else return half * half * x;
    }
    
    double myPow(double x, int n) {
		long long N = n > 0 ? n : -(long long)n;
        return compute(x, N);
    }
};
```

---

## Leetcode69 开方Sqrt

- 数学类的题目其实都可以往二分法的方向靠拢，把问题从遍历的时间复杂度降到二分查找的时间复杂度logn，比如这道题，就是去找一个开方数，完全可以通过遍历的方式来查找这个数，但是这样效率就会很低，通过这样的方法就可以增加效率，降低时间复杂度。

- 于是可以写出第一版代码：

```c++
class Solution {
public:
    int mySqrt(int x) {
        if(x <= 0)return 0;
        if(x == 1)return 1;
        int left = 0, right = x;
        while(left < right) {
            int mid = (right + left) / 2;
            if(mid * mid == x) return mid;
            if(mid * mid > x) right = mid - 1;
            else left = mid+1;
        }
        if(left * left > x)
            return left - 1;
        else return left;
    }
};
```

这个代码有很多的问题

1. 存在溢出的可能

   比如`if(mid * mid == x) return mid;`这一句，因为是从x的一半开始作为二分的中点的，因此这里可能会出现mid值很大的情况。

2. 后面又多了几句判断left的语句

3. `int mid = (right + left) / 2;`一句可能出现溢出。

因此可以做出以下改进：

1. 为了防止数值溢出，可以将乘法换成除法
2. 优化二分的判断条件，知道left > right才跳出循环，这样可以使得在循环结束的时候，right可以直接作为返回值。
3. 改掉这种不好的习惯，在求mid的时候采用一种风险较低的方式，`int mid = left + (right - right) / 2;`，这样就可以避免left + right过大而溢出的风险。

总结一下，就是说，在写二分代码的时候，上面的三条都是非常重要的，而且很容易出错。

因此经过优化后的代码如下：

```c++
class Solution {
public:
    int mySqrt(int x) {
        if(x <= 0)return 0;
        if(x == 1)return 1;
        int left = 0, right = x;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(x / mid < mid) right = mid - 1;
            else left = mid + 1;
        }
        return right;
    }
};
```

---

## Leetcode231 判断2的次方

题目的意思就是给定一个数值，判断这个数是不是2的某个次方。

找规律，2的次方的特征是，使用二进制表示的时候，只有第一位是1，后面全是0，比如：

| 十进制 | 二进制     |
| ------ | ---------- |
| 2      | 10         |
| 4      | 100        |
| 16     | 10000      |
| 1024   | 1000000000 |

因此这道题就非常简单了，只需要搞一个1，让这个1不断左移，看是否会与n相等，如果相等，那么肯定是2的次方了。这里再用long long做保障，防止溢出。

```c++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        long long i = 1;
        while(i <= n) {
            if(i == n)return true;
            i <<= 1;
        }
        return false;
    }
};
```





