题目描述：  
![image](/algorithmn/dynamic_programming/image/image32.png)  
解题过程：  
最长公共子序列问题而已，简单一批，难的是单纯从删除次数最少这一点去考虑的动态规划解法  
代码：  
```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(),n = word2.size();
        vector<vector<int>> dp(m+1,vector<int>(n+1));
        int MAX = 0;
        for (int i = m - 1; i >= 0; i--) {
            char w1 = word1[i];
            for (int j = n - 1; j >= 0; j--) {
                char w2 = word2[j];
                dp[i][j] = w1 == w2 ? dp[i+1][j+1] + 1 : max(dp[i+1][j],dp[i][j+1]);
                MAX = max(MAX,dp[i][j]);
            }
        } 
        return m + n - 2 * MAX;
    }
};
```