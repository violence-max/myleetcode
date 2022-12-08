题目描述：  
![image](/algorithmn/dynamic_programming/image/image31.png)    
解决过程：  
看了两眼直接放弃了，毕竟是一道hard，看了题解，感觉强无敌，主要就是跟之前的子序列问题是一样的，从后往前判断，如果当前值相等，则选择从当前位置开始匹配或者从原字符串下一个位置开始匹配。**因为这个问题判断的是子序列的个数，所以可以选取当前位置或者下一个位置**。如果当前值不相等，那么就只能从同时从下一个位置开始匹配个数。  
代码：  
```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        int m = s.size(),n = t.size();
        if (m < n) return 0;
        vector<vector<unsigned long long>> dp(m+1,vector<unsigned long long> (n+1));
        for (int i = 0; i <= m; i++) {
            dp[i][n] = 1;
        }
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (s[i] == t[j]) {
                    dp[i][j] = dp[i+1][j+1] + dp[i+1][j];
                } else {
                    dp[i][j] = dp[i+1][j];
                }
            }
        }
        return dp[0][0];
    }
};
```