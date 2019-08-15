# 121. Best Time to Buy and Sell Stock

## Description

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

Example 1:
```
Input: [7,1,5,3,6,4]
Output: 5
```
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.

Example 2:
```
Input: [7,6,4,3,1]
Output: 0
```
Explanation: In this case, no transaction is done, i.e. max profit = 0.

---

## Solution
### Step 1.
- 首先，如果考虑暴力破解的话，就是从左向右，对每一个元素都执行一次这样的操作：找到该元素后面最大的元素，并将求出最大元素-该元素的值，对比最大的结果值，如果比当前的结果还要大，就将结果更新。这种方法的**时间复杂度是O(n^2)**。

### Step 2.

- 考虑一种一次循环就可以解决的方法，那就是将前面最小的值存起来，当前的值如果比这个最小值大，就用当前值-最小值求出来一个利润，然后将利润与结果作比较，如果比结果还要大，就更新结果。如果当前的值比最小值小，那么就需要将原来的最小值更新。
    ```
    class Solution {
    public:
        int maxProfit(vector<int>& prices) {
            if(prices.size() < 2)return 0;
            int minVal = prices[0];
            int result = 0;
            for(int i = 1; i < prices.size(); i++){
                if(prices[i] < minVal){
                    minVal = prices[i];
                }
                else{
                    result = max(result, (prices[i] - minVal));
                }
            }
            return result;
        }
    };
    ```