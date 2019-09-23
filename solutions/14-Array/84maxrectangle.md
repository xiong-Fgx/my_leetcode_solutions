# 求柱状图中的最大面积

使用栈的方法来解

从左到右遍历数组，如果遍历到的值大于栈顶值，则将该值的index放入栈中，如果当前的值小于栈顶index所对应的值，则将该值弹栈并计算出临时的最大值。

当整个流程走完之后，栈底的值就是最小值。

认真画图并分析，把代码和运行结果贴上来：

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int result = 0;
        stack<int> stk;
        heights.push_back(0);
        for(int i = 0; i < heights.size(); i++) {
            cout<<"i= "<<i<<endl;
            while(!stk.empty() && heights[stk.top()] > heights[i]) {
                int top = stk.top();
                stk.pop();
                int tmpHeight = heights[top], tmpWidth = stk.empty() ? i : i - top;
                result = max(result, tmpHeight * tmpWidth);
                cout<<"out: "<<top<<" height: "<<tmpHeight<<" Width: "<<tmpWidth<<endl;
            }
            stk.push(i);
            cout<<"top: "<<stk.top()<<endl<<endl;
        }
        return result;
    }
};
```

运行结果为：

```
i= 0
top: 0

i= 1
out: 0 height: 2 Width: 1
top: 1

i= 2
top: 2

i= 3
top: 3

i= 4
out: 3 height: 6 Width: 1
out: 2 height: 5 Width: 2
top: 4

i= 5
top: 5

i= 6
out: 5 height: 3 Width: 1
out: 4 height: 2 Width: 2
out: 1 height: 1 Width: 6
top: 6

10
```

提交后发现有一个样例过不了

```
[4,2,0,3,2,5]
output:5
expect:6
```

很明显就是在0,3,2,5这一段，2进栈的时候直接把3弹出去了

与此很类似的还可以是这种样例

```
[0,2,3,4,5,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1]
output:19
expect:23
```

下面将一个可以通过的代码放上来，进行对比

```c++
class Solution {
    public:
    int largestRectangleArea(vector<int>& heights) {
        heights.push_back(0);
        stack<int> stk;
        int maxArea = 0;
        for(int i = 0;i<heights.size();i++)
        {
            while(!stk.empty() && heights[i]<heights[stk.top()])
            {
                int top= stk.top();
                stk.pop();
                maxArea = max(maxArea,heights[top]*(stk.empty()? i:(i - stk.top() -1)));
            }
            stk.push(i);
        }
        return maxArea;
    }
};
```

这两个代码的唯一一点不同之处是`tmpWidth`的求法

```c++
//前者
int tmpWidth = stk.empty() ? i : i - top;
//后者
int tmpWidth = stk.empty() ? i : i - stk.top() - 1;
```

为什么要这么写呢，如果只弹出1个值，二者是等价的，然而如果要弹出很多个值的话，就是要考虑一下这里的特征。

比如，在`[2,1,5,6,3,7]`这个直方图中，运行到最后，`index=6`（算上额外附加的那一个），这时的栈里面存的数据是这样的：`[1,4,5]`，弹出5的计算就不在考虑了，这里只讨论边界。

也就是说heights值为5,6的那两个点index=2,3并没有存在这个栈中，在循环中的while语句里，最后一次循环的时候，弹出的栈顶元素是4（也就是栈不空且满足height[4]=3>0）。执行一下，发现，当4弹出之后，栈中只剩下一个1了，然而，index从1到4（不包括1）的heights值都是满足>height[4]的，也就是说可以用`(index=6) - (index=1) - 1` 来表示满足`heights>=heights[4]`条件的宽度，因为1后面的2,3肯定是大于4的（遇到4将2,3弹出）。因此这个地方也不是特别容易想清楚。

