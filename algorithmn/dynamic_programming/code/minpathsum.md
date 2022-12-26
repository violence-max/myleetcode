题目描述：  
![image](/algorithmn/dynamic_programming/image/image40.png)  
解题过程：  
很简单的一个动态规划，一眼就会，但是一直不是很会动态规划的滚动数组版本所以这里补充一下吧。  
二维数组版本：  
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size(),n = grid[0].size();
        vector<vector<int>> fn(m,vector<int> (n));
        for (int i = 0; i < n; i++) 
            fn[0][i] = (i > 0) ? fn[0][i-1] + grid[0][i] : grid[0][i];
        for (int i = 0; i < m; i++) 
            fn[i][0] = (i > 0) ? fn[i-1][0] + grid[i][0] : fn[i][0];
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                fn[i][j] = (fn[i-1][j] >= fn[i][j-1]) ? fn[i][j-1] + grid[i][j] : fn[i-1][j] + grid[i][j];                
            }
        }
        return fn[m-1][n-1];
    }
};
```  
滚动数组版本：  
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size(),n = grid[0].size();
        vector<int> fn(n);
        for (int i = 0; i < n; i++) 
            fn[i] = (i > 0) ? fn[i-1] + grid[0][i] : grid[0][i];
        for (int i = 1; i < m; i++) {
            fn[0] += grid[i][0];
            for (int j = 1; j < n; j++) {
                fn[j] = (fn[j] >= fn[j-1]) ? fn[j-1] + grid[i][j] : fn[j] + grid[i][j];                
            }
        }
        return fn[n-1];
    }
};
```