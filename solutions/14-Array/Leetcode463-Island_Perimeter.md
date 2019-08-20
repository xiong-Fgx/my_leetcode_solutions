# 463. Island Perimeter

## Description
You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water.

Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

 

Example:

```
Input:
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

Output: 16
```

Explanation: The perimeter is the 16 yellow stripes in the image below:

![图片](https://assets.leetcode.com/uploads/2018/10/12/island.png)

## Solution
### Step1：
- 这个是最先想到的方法，首先肯定要遍历所有的格子，每个格子都判断它的四周是否有元素存在，是否是边界，因此，核心部分的代码是：
    ```
    int score(vector<vector<int>>& grid, int r, int c){
        int result = 0;
        if(r == 0)result++;
        if(r == grid.size()-1)result++;
        if(c == 0)result++;
        if(c == grid[0].size()-1)result++;
        if(r > 0 && grid[r-1][c] == 0)result++;
        if(c > 0 && grid[r][c-1] == 0)result++;
        if(r < grid.size()-1 && grid[r+1][c] == 0)result++;
        if(c < grid[0].size()-1 && grid[r][c+1] == 0)result++;

        return result;
    }
    ```
### Step2：
- 上面的方法是复杂的，因此需要简化。 
- 首先，如果没有重叠的话，每个位置如果值为1，都有四条边，因此用count来计算总个数。
- 然后，考虑重叠，每个节点都考虑它的左和上的位置是否为1，如果为1，那么说明这两条边都不能考虑在内，因此需要减2，还需要累计，因此用repeat+1来表示。
    ```
    class Solution {
    public:

        int islandPerimeter(vector<vector<int>>& grid) {
            int count=0, repeat=0;
            
            for( int i=0; i<grid.size(); ++i )
            {
                for( int j=0; j<grid[0].size(); ++j )
                {
                    if( grid[i][j]==1 )
                    {
                        ++count;
                        if( i!=0 && grid[i-1][j]==1 ) ++repeat;
                        if( j!=0 && grid[i][j-1]==1 ) ++repeat;
                    }
                }
            }
            
            return 4*count -2*repeat;
        }
    };
    ```