题目描述：  
![image](/algorithmn/dynamic_programming/image/image51.png)  
解决过程：  
一道做过2遍的动态规划，5分钟ac  
代码：  
```cpp
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        int m = grid.size();
        if (m == 0) return  0;
        int n = grid[0].size();
        if (n == 0) return 0;

        vector<int> dp (n + 1);
        for (int i = m - 1; i >= 0; i--) {
            dp[n] = i == m - 1 ? 0 : INT_MIN;
            for (int j = n - 1; j >= 0; j--) {
                dp[j] = grid[i][j] + max(dp[j],dp[j+1]);
            }
        }
        return dp[0];
    }
};
```