# 56. Merge Intervals

## Description

Given a collection of intervals, merge all overlapping intervals.

**Example 1:**

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

**Example 2:**

```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

## Solution

这是一个区间问题，可以有一些套路，就是先把所有区间**按照一定的规则进行排序**。

可以按照每个区间的开头按从小到大的顺序，可以按照每个区间尾从小到大的顺序。

```c++
//按照区间头从小到大的顺序排序
static bool cmp(vector<int> &a, vector<int> &b){
        return a[0] < b[0];
}
```

``` c++
//按照区间尾从小到大的顺序排列
static bool cmp(vector<int> &a, vector<int> &b){
        return a[1] < b[1];
}
```

- 在解这种区间问题的时候要注意，先画个图，会对区间的理解有帮助，这里用**区间头从小到大排序**，并用一个临时变量maxVal作为当前区间的结尾。

```c++
class Solution {
public:
    static bool cmp(vector<int> &a, vector<int> &b){
        return a[0] < b[0];
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if(!intervals.size())return {};
        sort(intervals.begin(), intervals.end(), cmp);
        int i = 0, j = 1;
        int maxVal = intervals[0][1];
        vector<vector<int>> result;
        for(; j < intervals.size(); j++){
            if(intervals[j][0] <= maxVal){
                maxVal = max(maxVal, intervals[j][1]);

            }
            else{
                result.push_back({intervals[i][0], maxVal});
                i = j;
                maxVal = intervals[j][1];
            }
        }
        if(i != j)
            result.push_back({intervals[i][0], maxVal});
        return result;
    }
};
```





