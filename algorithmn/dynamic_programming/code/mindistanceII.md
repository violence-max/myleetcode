题目描述：  
![image](/algorithmn/dynamic_programming/image/image33.png)  
解题过程：  
这就是动态规划！简单一批，考虑在计算当前值时其余三个状态最小值或者前一个值相等就可以了  
代码：  
```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(),n = word2.size();
        vector<vector<int>> dp(m+1,vector<int> (n+1));
        for (int i = 1; i <= m; i++) {
            dp[i][0] = i;
        }
        for (int i = 1; i <= n; i++) {
            dp[0][i] = i;
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] = word1[i-1] == word2[j-1] ? dp[i-1][j-1] : min(dp[i-1][j-1],min(dp[i-1][j],dp[i][j-1])) + 1; 
            }
        }
        return dp[m][n];
    }
};