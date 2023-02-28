题目描述：  
![image](/algorithmn/dynamic_programming/image/image42.png)  
解决过程：  
想到了回溯，但是肯定超时所以没写。看了题解，一看到动态规划的定义直接滚去coding，瞬间出来。然后自己优化出了一维的dp数组，发现滚动数组的时候需要从后面开始，因为从前面开始值会被覆盖  
代码：（二维dp数组→一维dp数组）  
```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int m = triangle.size();
        vector<vector<int>> dp(m,vector<int> (m,0));
        int ans = INT_MAX;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < i + 1; j++) {
                if (j == 0 || j == i) {
                    int prev = (i > 0) ? (j == 0) ? dp[i-1][j] : dp[i-1][j-1] : 0;
                    dp[i][j] = triangle[i][j] + prev;
                } else {
                    dp[i][j] = triangle[i][j] + min(dp[i-1][j-1],dp[i-1][j]);
                }
                if (i == m - 1) ans = min(ans,dp[i][j]);
            }
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int m = triangle.size();
        vector<int> dp(m);
        int ans = INT_MAX;
        for (int i = 0; i < m; i++) {
            for (int j = i; j >= 0; j--) {
                if (j == 0 || j == i) {
                    int prev = (i > 0) ? (j == 0) ? dp[j] : dp[j-1] : 0;
                    dp[j] = triangle[i][j] + prev;
                } else {
                    dp[j] = triangle[i][j] + min(dp[j-1],dp[j]);
                }
                if (i == m - 1) ans = min(ans,dp[j]);
            }
        } 
        return ans;
    }
};
```