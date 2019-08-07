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
- 确定使用双同向指针之后，制定策略：当当前的sum小于target时，需要扩大范围，因此fast指针增加，当当前的sum大于target的时候，可以尝试将window缩小，也就是增加slow指针。
    ```
    class Solution {
    public:
        int minSubArrayLen(int s, vector<int>& nums) {
            if(nums.size()==0)return 0;
            int slow = 0, fast = 0;
            int minLen = nums.size()+1, sum = nums[0];
            while(fast < nums.size() && slow < nums.size()){
                if(sum < s){
                    fast++;
                    if(fast < nums.size())
                        sum+=nums[fast];
                }
                else if(sum >= s){
                    minLen = min(minLen, fast-slow+1);
                    sum-=nums[slow];
                    slow++;
                }
            }
            if(sum < s && minLen > nums.size())return 0;
            else return minLen;
        }
    };
    ```

### Step2 优化
- 与上面的不同之处在于，slow向前推进的速度不是每次循环只有一个，而是使用while一步到位。
    ```
    class Solution {
    public:
        int minSubArrayLen(int s, vector<int>& nums) {
            if(nums.size()==0)return 0;
            int slow = 0, fast = 0;
            int minLen = nums.size()+1, sum = 0;
            while(fast < nums.size()){
                sum+=nums[fast];
                fast++;
                while(sum >= s){
                    minLen = min(minLen, fast - slow);
                    sum-=nums[slow];
                    slow++;
                }
            }
            if(minLen == nums.size()+1)return 0;
            else return minLen;

        }
    };
    ```