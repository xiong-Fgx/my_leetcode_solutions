# 724. Find Pivot Index

`前缀和` `后缀和`

## Description

Given an array of integers `nums`, write a method that returns the "pivot" index of this array.

We define the pivot index as the index where the sum of the numbers to the left of the index is equal to the sum of the numbers to the right of the index.

If no such index exists, we should return -1. If there are multiple pivot indexes, you should return the left-most pivot index.

**Example 1:**

```
Input: 
nums = [1, 7, 3, 6, 5, 6]
Output: 3
Explanation: 
The sum of the numbers to the left of index 3 (nums[3] = 6) is equal to the sum of numbers to the right of index 3.
Also, 3 is the first index where this occurs.
```

 

**Example 2:**

```
Input: 
nums = [1, 2, 3]
Output: -1
Explanation: 
There is no index that satisfies the conditions in the problem statement.
```

next：

```
628, 1035, 1052
```



## Solution

### Step1:使用双指针

- 分别设置两个指针`left`和`right`，分别从左向右，从右向左移动，每次都只移动和较小的那边的指针，直到相遇，如果左边与右边的值都相等，那么说明这个值就是中间的值，否则返回-1表示不存在这样的值。

```c++
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        if(nums.size() == 0)return -1;
        int left = 0, right = nums.size()-1;
        int leftSum = 0, rightSum = 0;
        while(left < right){
            if(leftSum > rightSum){
                rightSum += nums[right];
                right--;
            }
            else{
                leftSum += nums[left];
                left++;
            }
        }
        if(leftSum == rightSum)return left;
        else return -1;
    }
};
```

- 但是这个解法是错误的，是因为没有考虑负数的情况！！！如果输入是`[-1，-1，-1，-1,0]`很显然是正确的，但是按照这个解法算出来是-1,。

## Step2：使用前缀后缀和

- 前面的方法是不可行的，因此可以尝试下面这种方法：仿照前缀和的方法，定义后缀和，后缀和的计算方法就是`当前后缀和-当前值`，
- 首先将所有元素的值都相加，得到所有元素的和，将这个值逐渐减去遍历值，就可以得到遍历当前位置的后缀和，同时，将遍历过的值都加起来作为前缀和，当前缀和和后缀和相等的时候，这个点就是所求点。（当然，这个表述是不准确的，还可以进行简化）

```c++
if(sumPre == sumAfter - sumPre - nums[i])return i;
```

- 整个代码是这样的：可以看出非常简洁

```c++
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int sumAfter = 0, sumPre = 0;
        for(int i : nums) sumAfter+=i;
        for(int i = 0; i < nums.size(); i++){
            if(sumPre == sumAfter - sumPre - nums[i])return i;
            else sumPre += nums[i];
        }
        return -1;
    }
};
```

