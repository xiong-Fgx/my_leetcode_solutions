# 560. Subarray Sum Equals K

## Description

Given an array of integers and an integer **k**, you need to find the total number of continuous subarrays whose sum equals to **k**.

**Example 1:**

```
Input:nums = [1,1,1], k = 2
Output: 2
```



**Note:**

1. The length of the array is in range [1, 20,000].
2. The range of numbers in the array is [-1000, 1000] and the range of the integer **k** is [-1e7, 1e7].

**next：**

```
523, 713, 724, 974
```



## Solution

### Step1: 暴力搜索

- 可以看出，使用了一个临时变量来存储自己前面的所有元素的和，然后使用一个双重循环，来搜索差与所需值相同的区间，如果找到，则将result+1

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int result = 0;
        unordered_map<int, int> dict;
        vector<int> subSum;
        subSum.push_back(0);
        for(int i = 0; i < nums.size(); i++){
            int tmp = subSum.back() + nums[i];

            subSum.push_back(tmp);
        }
        //核心的二重循环
        for(int i = 1; i <= nums.size(); i++){
            for(int j = 0; j < i; j++){
                if(subSum[i] - subSum[j] == k)result++;
            }
        }
        return result;
    }
};
```

### Step2：使用哈希表

- 前面的双重循环中的第二重表示的含义是：查找有多少个j能够使得sum[i]和sum[j]的差为k，每找到一个这样的值，就把result+1
- 优化的思路是：直接记录下有多少个sum[j]与sum[i-k]相同，直接更新结果，则可以省掉第二个循环。可以使用哈希表，在记录前缀和的同时，记录该前缀和出现的次数

```
比如，有一个数组如下：
[3, 4, 7, 2, -3, 1, 4, 2]
那么他的前缀和为：
[0]
[3]
[7]
[14]
[16]
[13]
[14]
[18]
[20]
把前缀和当做一个哈希表，可以得到：
dict[0]:1
dict[3]:1
dict[7]:1
dict[14]:2
dict[16]:1
dict[13]:1
dict[18]:1
dict[20]:1
```

下面，我们要找k=7的值，来遍历这个数组，可以得到：

| 前缀和（遍历） | 前缀和-7 | 前缀和-7 对应的值作为key的哈希表键值 |
| :------------- | -------- | :----------------------------------: |
| [0]            | ---      |                 ---                  |
| [3]            | -4       |         NULL，并将dict[3]++          |
| [7]            | 0        |       dict[0]:1，并将dict[7]++       |
| [14]           | 7        |      dict[7]:1，并将dict[14]++       |
| [16]           | 9        |         NULL，并将dict[16]++         |
| [13]           | 6        |         NULL，并将dict[13]++         |
| [14]           | 7        |      dict[7]:1，并将dict[14]++       |
| [18]           | 11       |         NULL，并将dict[18]++         |
| [20]           | 13       |      dict[13]:1，并将dict[20]++      |

- 从上面的表可以看出，满足条件的一共有4个

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int result = 0;
        unordered_map<int, int> dict;
        int sub = 0;
        dict.insert(make_pair(0,1));
        for(int i = 0; i < nums.size(); i++){
            sub += nums[i];
            int tmp = sub - k;
            if(dict[tmp])result+=dict[tmp];
            dict[sub]++;
        }
        return result;
    }
};
```

- 这种题的解法有点像动态规划？

