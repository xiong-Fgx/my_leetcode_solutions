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
- ���ȣ�������Ǳ����ƽ�Ļ������Ǵ������ң���ÿһ��Ԫ�ض�ִ��һ�������Ĳ������ҵ���Ԫ�غ�������Ԫ�أ�����������Ԫ��-��Ԫ�ص�ֵ���Ա����Ľ��ֵ������ȵ�ǰ�Ľ����Ҫ�󣬾ͽ�������¡����ַ�����**ʱ�临�Ӷ���O(n^2)**��

### Step 2.

- ����һ��һ��ѭ���Ϳ��Խ���ķ������Ǿ��ǽ�ǰ����С��ֵ����������ǰ��ֵ����������Сֵ�󣬾��õ�ǰֵ-��Сֵ�����һ������Ȼ�������������Ƚϣ�����Ƚ����Ҫ�󣬾͸��½���������ǰ��ֵ����СֵС����ô����Ҫ��ԭ������Сֵ���¡�
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