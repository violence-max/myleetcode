题目描述：  
![image](/algorithmn/dynamic_programming/image/image6.png)  
解题过程：  
感觉自己的智商遭到了非常大的羞辱。包括一开始的循环超出时间限制，以为只要只含有一个节点就一定能到达却忽略了起点处有障碍物的情况，包括起点有障碍物其余点就不能到达的情况。  
看了题解，这下才意识到滚动数组，我去，强无敌，这么好的空间优化。  
代码：　
```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<vector<int>> dp(m,vector<int> (n));
        dp[0][0] = (obstacleGrid[0][0]) ? -1 : 1;
        if (dp[0][0] == -1) return 0;
        for (int i = 1; i < n; i++) {
            if (obstacleGrid[0][i]) {
                int temp = i;
                while (temp < n) 
                    dp[0][temp++] = -1;
                break;
            } else dp[0][i] = 1;
        }
        for (int i = 1; i < m; i++) {
            if (obstacleGrid[i][0]) {
                int temp = i;
                while (temp < m)
                    dp[temp++][0] = -1;
                break;
            } else dp[i][0] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] || (dp[i-1][j] == -1 && dp[i][j-1] == -1)) {
                    dp[i][j] = -1;
                } else if (dp[i-1][j] == -1) {
                    dp[i][j] = dp[i][j-1];
                } else if (dp[i][j-1] == -1){
                    dp[i][j] = dp[i-1][j];
                } else {
                    dp[i][j] = dp[i][j-1] + dp[i-1][j];
                }
            }
        }
        return dp[m-1][n-1] == -1 ? 0 : dp[m-1][n-1];
    }
};
```