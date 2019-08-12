# Leetcode 349. Intersection of Two Arrays

## Description
Given two arrays, write a function to compute their intersection.
- (找两个array的交集，输出array中不能有重复的元素。)

Example 1:
```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```
Example 2:
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```
- Note:
    - Each element in the result must be unique.
    - The result can be in any order.


## 扩展
- 如果两个array是已排序的，是否能用O(N)的时间和O(1)的空间来解这道题。
---

## Solution

### Step1 暴力破解
- 时间复杂度为O(m×n)

### Step2 使用set
- To solve the problem in linear time, let's use the structure **set**, which provides **in/contains operation in O(1) time** in average case.
- 也就是先把两个vector中的数据分别转换为两个set，再遍历其中一个set，查找它中的元素在另一个set中是否存在。
    ```
    class Solution {
    public:
        vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
            vector<int> result;
            unordered_set<int> a1, b1;
            for(auto i : nums1){
                a1.insert(i);
            }
            for(auto i : nums2){
                b1.insert(i);
            }
            for(auto i : a1){
                if(b1.count(i) != 0)result.push_back(i);
            }
            return result;
        }
    };
    ```
