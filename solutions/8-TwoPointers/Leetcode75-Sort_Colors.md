# Leetcode 75. Sort Colors

## Description
Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note: You are not suppose to use the library's sort function for this problem.

Example:
```
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```
Follow up:

- A rather straight forward solution is a two-pass algorithm using counting sort.
- First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
- Could you come up with a **one-pass algorithm** using only constant space?

---

## Solution
### Step1
- 首先肯定是只能进行一次遍历的，因为刚好需要对3种数据进行操作，因此可以设用前后双指针，当遍历遇到0的时候，将它放到左边，遍历遇到2的时候将它放到右边。
    ```
    class Solution {
    public:
        void sortColors(vector<int>& nums) {
            int index0 = 0, index2 = nums.size()-1;
            for(int i = 0; i < index2;i++){
                if(nums[i] == 0)swap(nums[i], nums[index0++]);
                else if(nums[i] == 2)swap(nums[i], nums[index2--]);
            }
        }
    };
    ```
- 出现了问题，考虑以下当输入数组为1,2,0的时候会发生什么：
    ```
    nums[0] = 1，不操作直接跳过，index1++
    nums[1] = 2，满足第二个条件，将它放到后面，index2--
    循环到此结束
    ```
    - 很容易看出， 我们在跑完2之后，换回来的是个0，而我们的逻辑直接将这个0跳过。
    - 因此，需要考虑pointer的移动条件。

### Step2
- 与前面不同的地方有：
    - 循环结束的条件该为 i <= index2,
    - 当交换的值为2的时候不增加当前i
    ```
    class Solution {
    public:
        void sortColors(vector<int>& nums) {
            int index0 = 0, index2 = nums.size()-1;
            for(int i = 0; i <= index2;){
                if(nums[i] == 0)swap(nums[i++], nums[index0++]);
                else if(nums[i] == 2)swap(nums[i], nums[index2--]);
                else i++;
            }
        }
    };
    ```