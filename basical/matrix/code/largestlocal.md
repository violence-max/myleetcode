题目描述：  
![image](/basical/matrix/image/image12.png)  
解题过程：  
5分钟ac  
代码:  
```cpp
class Solution {
public:
    int getMAX(vector<vector<int>>& grid, int x, int y) {
        int ans = INT_MIN;
        for (int i = x - 1; i <= x + 1; i++) {
            for (int j = y - 1; j <= y + 1; j++) {
                ans = max(ans,grid[i][j]);
            }
        }
        return ans;
    }

    vector<vector<int>> largestLocal(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<int>> ans (n - 2, vector<int> (n-2));
        for (int i = 0; i <= n - 3; i++) {
            for (int j = 0; j <= n - 3; j++) {
                ans[i][j] = getMAX(grid,i+1,j+1);
            }
        }
        return ans;
    }
};
```