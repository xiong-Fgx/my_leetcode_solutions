# 122. Best Time to Buy and Sell Stock II
## Description
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

Example 1:
```
Input: [7,1,5,3,6,4]
Output: 7
```
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
             
Example 2:
```
Input: [1,2,3,4,5]
Output: 4
```
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
Example 3:
```
Input: [7,6,4,3,1]
Output: 0
```
Explanation: In this case, no transaction is done, i.e. max profit = 0.

---
## Solution
- 这个可以模仿之前的那个尖峰的题目
![leetcode中题解的图](https://leetcode.com/media/original_images/122_maxprofit_1.PNG)
- 也就是说，当一直涨的时候不卖，只要有跌的，就在跌之前全卖了。如果一直涨到最后也没有停下，那么就在最后判断一下，如果最后一个也是涨的，就和前面的最低值（由涨到跌的转折点）求一个差并加到结果里面。
    ```
    class Solution {
    public:
        int maxProfit(vector<int>& prices) {
            if(prices.size() < 2)return 0;
            int result = 0;
            int i = 0, j = 1;
            while(j < prices.size()){
                if(prices[j] > prices[j-1]){
                    j++;
                }
                else{
                    if(i != j-1){
                        result += prices[j-1] - prices[i];
                    }
                    i = j++;
                }

            }
            if(i < j)result += prices[j-1] - prices[i];
            return result;
        }
    };
    ```