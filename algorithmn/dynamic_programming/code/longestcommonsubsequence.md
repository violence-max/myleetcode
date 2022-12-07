题目描述：  
![image](/algorithmn/dynamic_programming/image/image28.png)  
解题过程：  
终于，ac了一道动态规划！！还是最长公共子序列！！强无敌！！虽然推导出动态规划方程出现了那么一点点的小困难，但是不愧是我  
代码：  
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size(),n = text2.size();
        int ret = 0;
        vector<vector<int>> dp(m+1,vector<int>(n+1));
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                dp[i][j] = text1[i] == text2[j] ? dp[i+1][j+1] + 1 : max(dp[i][j+1],dp[i+1][j]);
                ret = max(dp[i][j],ret);
            }
        }
        return ret;
    }
};
```