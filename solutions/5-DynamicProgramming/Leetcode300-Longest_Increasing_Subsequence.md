# 300. Longest Increasing Subsequence

## Description

`动态规划` 

Given an unsorted array of integers, find the length of longest increasing subsequence.

**Example:**

```
Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
```

**Note:**

- There may be more than one LIS combination, it is only necessary for you to return the length.
- Your algorithm should run in O(*n2*) complexity.

**Follow up:** Could you improve it to O(*n* log *n*) time complexity?

## next：

```
334. Increasing Triplet Subsequence
354. Russian Doll Envelopes
646. Maximum Length of Pair Chain
673. Number of Longest Increasing Subsequence
712. Minimum ASCII Delete Sum for Two Strings
```

## Solution

### Step1:使用O(N^2)时间复杂度进行DP

- 首先分析待优化的子问题：包含当前元素在内的最大递增长度是多少
- 知道了这个之后，可以继续往下推理，找到优化递推式。

```
对于示例：
nums = [10,9,2,5,3,7,101,18]
dp   = [1, 1,1,2,2,4,5,   ?]
假设我们前面的dp值已经求出来了，如何求最后一个DP值？
```

- 很容易想到遍历呗，遍历当前元素前面的所有元素，通过比较当前值与此元素前面值的大小，如果满足递增，则把这个元素的dp值+1，在这个过程中，实时更新这个dp值即可

```c++
for(int i = 1; i < nums.size(); i++){
    for(int j = 0; j < i; j++){
        if(nums[i] > nums[j]) //如果满足递增条件，则更新当前的dp值
            dp[i] = max(dp[j]+1, dp[i]);
    }
}
```

### Step2：优化

- 题中已经给出了提示，说如何用O(NlogN)求解，很自然的联想到二分，但是如何用二分呢？

- 下面给了一个和蜘蛛纸牌很类似的方法：

  - 就是只能把点数小的牌压到点数大的牌上面，如果当前的牌点数较大没有可以放置的堆，那么就新建一个堆把这个牌放进去。然后**牌的堆数就是所求的最长递增子序列的长度**

  - 当然这种方法也是很匪夷所思的，具体怎么证明呢？？

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        //top数组存了nums个0，用来表示堆顶的元素（记录最大值）
        vector<int> top(nums.size(), 0);
        //初始化的堆个数为0
        int piles = 0;
        for(int i = 0; i < nums.size(); i++){
            //对这些堆进行二分查找，去找那个合适插入的位置
            int left = 0, right = piles;
            while(left < right){
                int mid = (left + right) / 2;
                //
                if(nums[i] > top[mid])left = mid + 1;
                else right = mid;
            }
            if(left == piles) piles++;
            top[left] = nums[i];
        }
        return piles;
    }
};
```

- 这里倒是领悟了一下二分的奥义，就是在写循环的二分时，判断当前值与二分的中值大小作比较，如果有等号，那么边界变化的时候也包含这个中值，如果没有等号，则修改边界的时候也没有等号。因为循环结束的条件就是left=right，如果判断的时候有等于号，则说明需要找的位置可能就是中值，因此在后面进行区间边界变化的时候也要把这个值考虑在内。