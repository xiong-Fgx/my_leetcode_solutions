# Leetcode 283 Move Zeros
## Description
- Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

- Example:

    ```
    Input: [0,1,0,3,12]
    Output: [1,3,12,0,0]
    ```

## Solution
### Step1. 尝试使用前后双指针。
- 这个很容易看出来是需要使用双指针操作的。第一眼看到的时候，认为应当前后各一个指针，分别向前和向后（向前指针设置为pointer1， 向后指针设置为pointer2）,pointer1找到第一个为零的元素后停下，pointer2找到第一个非0的元素后停下，然后将这两个pointer指向的值交换位置。
- **注意：直接通过两个指针对nums进行访问时注意可能会越界，需要控制边界**
    ```
    void moveZeroes(vector<int>& nums) {
        int pointer1 = 0, pointer2 = nums.size()-1;
        while(pointer1 < pointer2){
            while(pointer1 < nums.size() && nums[pointer1]!=0)pointer1++;
            while(pointer2 >= 0 && nums[pointer2]==0)pointer2--;
            if(pointer1 < pointer2)
                swap(nums[pointer1], nums[pointer2]);
        }
    }
    ```
- 这个提交并没有通过，原因有下：
    - **输出需要有序**：通过上面那种解法的操作，得到的结果应当是[12,1,3,0,0]，与结果中的表达不同，所以考虑其他的方法。

### Step2. 尝试前向双指针。
- 使用双指针并不代表只能够从前后两个方向夹击，也可以同时从一个方向出发。
    ```
    void moveZeroes(vector<int>& nums) {
        int pointer1 = 0, pointer2 = 0;
        int numsLen = nums.size();
        while(pointer1 < numsLen && pointer2 < numsLen){
            while(pointer1 < numsLen && nums[pointer1] == 0)pointer1++;
            while(pointer2 < numsLen && nums[pointer2] != 0)pointer2++;
            if(pointer1 < numsLen && pointer2 < numsLen)
                swap(nums[pointer1], nums[pointer2]);
        }
    }
    ```
- 依然不对，问题出在了下面这个case
    ```
    [1,0]
    ```
- 反思之后发现我们这里的逻辑是：只要遇到一个0，一个非0就交换，一旦开始的时候就是正确的顺序呢？这样岂不是就把1换到后面去了。所以，逻辑需要更改，就是要**找到比当前的0更后面的非0数的时候，才进行交换**。
    ```
    void moveZeroes(vector<int>& nums) {
        int pointer1 = 0, pointer2 = 0;
        int numsLen = nums.size();
        while(pointer1 < numsLen && pointer2 < numsLen){
            while(pointer1 < numsLen && nums[pointer1] == 0)pointer1++;
            while(pointer2 < numsLen && nums[pointer2] != 0)pointer2++;
            if(pointer1 < numsLen && pointer2 < numsLen)
                if(pointer1 > pointer2)
                    swap(nums[pointer1], nums[pointer2]);
                else{
                    pointer1++;
                }
        }
    }
    ```
### Step3. 缩短时间
- 前面这个提交虽然过了，但是用的时间太长了，仅beat20%的人。
- 看了别的大佬的代码，总结了一下：
    1. 并不需要经过这么多的判断，因为pointer1是用来找pointer2后面的第一个非零元素的，因此**它肯定比pointer2先到达尽头**。判断条件可以修改
    2. 不是交换元素，而是将后面的非0元素覆盖前面的位置，然后再在后面没有非0数的时候补0即可。
    3. 将3个while合并为1个：合并之后不需要那么多次的判断，当pointer1在往后面走的时候让pointer2保持不动就可以了。

    ```
    class Solution {
    public:
        void moveZeroes(vector<int>& nums) {
            int a = 0, b = 0;
            int len = nums.size();
            while(a < len){
                if(nums[a] == 0)a++;
                else{
                    nums[b++] = nums[a++];
                }
            }
            while(b < len){
                nums[b++] = 0;
            }
        }
    };
    ```