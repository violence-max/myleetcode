题目描述：  
![image](/algorithmn/dynamic_programming/image/image35.png)  
解决过程：  
简单，一个动态规划结束，我就是超人  
代码：  
```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        vector<vector<int>> dp(n,vector<int>(n));
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (s[j] == s[i]) {
                    if (j - i < 3) dp[i][j] = j - i + 1;
                    else dp[i][j] = dp[i+1][j-1] + 2;
                } else { 
                    dp[i][j] = max(dp[i][j-1],dp[i+1][j]);
                }
            }
        }
        return dp[0][n-1];
    }
};
```