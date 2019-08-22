# 435. Non-overlapping Intervals

## Description

Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.



**Example 1:**

```
Input: [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of intervals are non-overlapping.
```

**Example 2:**

```
Input: [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of intervals non-overlapping.
```

**Example 3:**

```
Input: [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```

 

**Note:**

1. You may assume the interval's end point is always bigger than its start point.
2. Intervals like [1,2] and [2,3] have borders "touching" but they don't overlap each other.



## Solution

- 这是一个典型的区间重叠问题，可以使用贪心法来解

  1. 首先按照**区间的结尾**来将区间组进行排序

  2. 从排序后的第一个区间开始，向后查找，如果有哪个区间的起始值小于这个区间的结束值，则将这个区间删除
  3. 一直到所有的区间都遍历完，就可以求出结果

- 最开始的思路是这样的（下面的程序中）：设置一个删除的元素表：check，如果有区间和结束最小的区间重合，那么就把这个区间的ID放进去，并将result++。不过这样会有一个问题，就是每次搜查一个区间的时候，都会对所有的deleted列表进行检索，这样的效率是很低的。

  ```C++
  class Solution {
  public:
      static bool cmp(vector<int> a, vector<int> b){
          return a[1] < b[1];
      }
      int eraseOverlapIntervals(vector<vector<int>>& intervals) {
          if(intervals.size() <= 1)return 0;
          sort(intervals.begin(), intervals.end(), cmp);
          int l = intervals.size();
          int result = 0;
          vector<int> deleted;
          for(int i = 0; i < l; i++){
  			if(find(deleted.begin(), deleted.end(), i) != deleted.end())
                  continue;
              for(int j = i+1; j < l; j++){
                  if(find(deleted.begin(), deleted.end(), j) != deleted.end())
                      continue;
                  if(intervals[i][1] > intervals[j][0]){
                      result++;
                      deleted.push_back(j);
                  }
              }
          }
          return result;
      }
  };
  ```

  

- 下面一种解的精妙之处在于，它没有使用双重循环，而是说只用了一层循环。**让后面那个指针移动，前面的只有在不存在不满足情况的时候才进行更新，并且每次更新都更新到j的位置。**

```
|------|

    |----|  deleted

					|------|  下次把更新到这里就行了
```


```c++
class Solution {
public:
    static bool cmp(vector<int> &a, vector<int> &b){
        return a[1] < b[1];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int result = 0;
        int i = 0, j = 0;
        sort(intervals.begin(), intervals.end(), cmp);
        vector<int> deleted;
        for(j = 1; j < intervals.size(); j++){
            if(intervals[i][1] > intervals[j][0]){
                result++;
                deleted.push_back(j);
            }
            else{
                i++;
                if(!deleted.empty() && deleted.back() >= i)
                    i = j;
            }
        }
        return result;
    }
};
```



