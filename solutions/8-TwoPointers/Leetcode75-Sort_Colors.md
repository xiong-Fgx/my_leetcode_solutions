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
- 棣栧厛鑲�瀹氭槸鍙�鑳借繘琛屼竴娆￠亶鍘嗙殑锛屽洜涓哄垰濂介渶瑕佸��3绉嶆暟鎹�杩涜�屾搷浣滐紝鍥犳�ゅ彲浠ヨ�剧敤鍓嶅悗鍙屾寚閽堬紝褰撻亶鍘嗛亣鍒�0鐨勬椂鍊欙紝灏嗗畠鏀惧埌宸﹁竟锛岄亶鍘嗛亣鍒�2鐨勬椂鍊欏皢瀹冩斁鍒板彸杈广€�
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
- 鍑虹幇浜嗛棶棰橈紝鑰冭檻浠ヤ笅褰撹緭鍏ユ暟缁勪负1,2,0鐨勬椂鍊欎細鍙戠敓浠€涔堬細
    ```
    nums[0] = 1锛屼笉鎿嶄綔鐩存帴璺宠繃锛宨ndex1++
    nums[1] = 2锛屾弧瓒崇��浜屼釜鏉′欢锛屽皢瀹冩斁鍒板悗闈�锛宨ndex2--
    寰�鐜�鍒版�ょ粨鏉�
    ```
    - 寰堝�规槗鐪嬪嚭锛� 鎴戜滑鍦ㄨ窇瀹�2涔嬪悗锛屾崲鍥炴潵鐨勬槸涓�0锛岃€屾垜浠�鐨勯€昏緫鐩存帴灏嗚繖涓�0璺宠繃銆�
    - 鍥犳�わ紝闇€瑕佽€冭檻pointer鐨勭Щ鍔ㄦ潯浠躲€�

### Step2
- 涓庡墠闈�涓嶅悓鐨勫湴鏂规湁锛�
    - 寰�鐜�缁撴潫鐨勬潯浠惰�ヤ负 i <= index2,
    - 褰撲氦鎹㈢殑鍊间负2鐨勬椂鍊欎笉澧炲姞褰撳墠i
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