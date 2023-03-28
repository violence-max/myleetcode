题目描述：  
![image](/algorithmn/dynamic_programming/image/image28.png)  
解题过程：  
1. dp[i][j]表示的是text1[i:]和text2[j:]的最长公共子序列的长度
2. 不论是反向遍历还是正向遍历都可以求得最终答案
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