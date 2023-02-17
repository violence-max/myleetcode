题目描述:  
![image](/basical/matrix/image/image11.png)  
解决过程:  
自己用动态规划想了一下,发现好像不能得到什么有效结果,所以看了题解.题解就比较暴力,直接一个三重遍历吧算是.主要是利用了前缀和的知识  
代码:  
```cpp
class Solution {
public:
    int largest1BorderedSquare(vector<vector<int>>& grid) {
       int m = grid.size() , n = grid[0].size();
       int border = 0;
       vector<vector<int>> left (m + 1, vector<int> (n + 1)) , up (m + 1, vector<int> (n + 1));
       for (int i = 0; i < m; i++) {
           for (int j = 0; j < n; j++) {
               if (grid[i][j] == 1) {
                   left[i+1][j+1] = left[i+1][j] + 1;
                   up[i+1][j+1] = up[i][j+1] + 1;
                   int minborder = min(left[i+1][j+1],up[i+1][j+1]);
                   while (left[i - minborder + 2][j + 1] < minborder || up[i + 1][j - minborder + 2] < minborder) {
                       minborder--;
                   }
                   border = max(border,minborder);
               }
           }
       } 
       return border * border;
    }
};
```