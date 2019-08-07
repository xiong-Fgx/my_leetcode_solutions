# Leetcode 209. Minimum Size Subarray Sum

## Description
Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.

Example: 
```
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
```
- Explanation: the subarray [4,3] has the minimal length under the problem constraint.
- Follow up:
    - If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log n). 

----

## Solution
### Step1
- 这相当于一个求窗口大小的问题，肯定是需要用到双指针的，至于这个双指针是从前往后还是从后往前，稍加思考可以得出是**俩指针都从前往后更新**