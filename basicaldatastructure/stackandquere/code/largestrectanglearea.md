题目描述：  
![image](/basicaldatastructure/stackandquere/image/image15.png) 
解决过程：  
想了一个O(n^2)时间复杂度的代码，每次从后往前遍历寻找最大值，超时了，嘻嘻  
代码：  
```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        int areaMax = 0;
        for (int i = 0; i < n; i++) {
            int minHeight = heights[i],width = 0;
            for (int j = i; j >= 0; j--) {
                minHeight = (heights[j] < minHeight) ? heights[j] : minHeight;
                width++;
                areaMax = max(areaMax,width*minHeight);
            }
        }
        return areaMax;
    }
};
```  
单调栈：这就是单调栈！找到一个值左右两边第一个比它小的值，然后除去这两个值的下标（减去2）之间的数据都是比这个值要大的，就可以直接用宽度乘以高！  
```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> left(n,-1),right(n,n);
        stack<int> s;
        for (int i = 0; i < n; i++) {
            while (!s.empty() && heights[s.top()] > heights[i]) {
                right[s.top()] = i;
                s.pop();
            }
            left[i] = (s.empty()) ? -1 : s.top();
            s.push(i); 
        }
        int areaMax = 0;
        for (int i = 0; i < n; i++) {
            areaMax = max(areaMax,(right[i]-left[i]-1)*heights[i]);
        }
        return areaMax;
    }
};
```