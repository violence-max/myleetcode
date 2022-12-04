题目描述： 
![image](/algorithmn/dynamic_programming/image/image5.png) 
解题过程：  
秒杀，简单dp，但是忽略了初始情况，只有一个格子的时候，不应该啊。  
代码：  
```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m,vector<int>(n));
        dp[0][0] = 1;
        for (int i = 1; i < n; i++) {
            dp[0][i] = 1;
        }
        for (int i = 1; i < m; i++) {
            dp[i][0] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i][j-1] + dp[i-1][j];
            }
        }
        return dp[m-1][n-1];
    }
};
```