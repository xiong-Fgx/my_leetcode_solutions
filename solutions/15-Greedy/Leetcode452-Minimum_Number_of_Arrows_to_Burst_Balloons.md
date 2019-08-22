# 452. Minimum Number of Arrows to Burst Balloons

## Description

There are a number of spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter and hence the x-coordinates of start and end of the diameter suffice. Start is always smaller than end. There will be at most 104 balloons.

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps travelling up infinitely. The problem is to find the minimum number of arrows that must be shot to burst all balloons.

**Example:**

```
Input:
[[10,16], [2,8], [1,6], [7,12]]

Output:
2

Explanation:
One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).
```



## Solution

- 这道题与435基本上是相同的，不同的地方，这里的[1,2]和[2,3]是算是重复的

```c++
class Solution {
public:
    static bool cmp(vector<int> &a, vector<int> &b){
        return a[1] < b[1];
    }

    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), cmp);
        int i = 0, j = 1;
        int result = 0;
        vector<int> deleted;
        for(; j < points.size(); j++){
            if(points[i][1] >= points[j][0]){
                result++;
                deleted.push_back(j);
            }
            else{
                if(!deleted.empty() && deleted.back() >= i)
                    i = j;
                else i++;
            }
        }
        return points.size() - result;
    }
};
```

- 正确的思路：
  1. 从区间集合中选择一个区间，这个区间是所有区间中结束时间最早的一个
  2. 把所有与选择出来的这个区间相交的区间从集合中删除
  3. 重复该操作，直到所有元素都被检查过

- 也就是说，**按end排序**，不难发现， 所有与选定区间相交的区间必然会与它的end相交，如果一个区间不想与选定区间的end相交，那么它的start就必须大于这个选定区间的end。

### 一些总结

- 对于区间问题的处理，一般来说第一步都是排序，相当于预处理降低后续操作的难度。