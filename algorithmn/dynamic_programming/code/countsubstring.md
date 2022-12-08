题目描述：  
![image](/algorithmn/dynamic_programming/image/image34.png)  
解决过程：  
不会，根本想不出来动态规划，看了题解，一个高级的线性算法，一个中心拓展，都没有看，看了代码随想录的动态规划解法，思想无敌，包括要从哪里开始遍历，有几种条件，动态规划方程给你整得明明白白  
代码：  
```cpp
class Solution {
public:
    int countSubstrings(string s) {
        int n = s.size();
        int result = 0;
        vector<vector<bool>> dp(n,vector<bool> (n));
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (s[i] == s[j]) {
                    if (j - i < 2) {
                        result++;
                        dp[i][j] = true;
                    } else if (dp[i+1][j-1]) {
                        result++;
                        dp[i][j] = true;
                    }
                }
            }
        }
        return result;
    }
};
```