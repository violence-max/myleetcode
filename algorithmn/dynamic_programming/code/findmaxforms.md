题目描述：  
![image](/algorithmn/dynamic_programming/image/image12.png)  
解题过程： 
没坚持多久，因为自己想不到动态规划去求解这道问题，主要是想不到动态规划的定义和方程。  
看了题解，三维的动态规划，强无敌。  
代码：  
```cpp
class Solution {
public:
    vector<int> zerosones(const string& str) {
        vector<int> ans(2);
        for (char c : str) {
            ans[c - '0']++;
        }
        return ans;
    }

    int findMaxForm(vector<string>& strs, int m, int n) {
        int len = strs.size();
        vector<vector<int>> dp(m+1,vector<int> (n+1));
        for (int i = 1; i <= len; i++) {
            vector<int> zo = zerosones(strs[i-1]);
            int zeros = zo[0],ones = zo[1];
            for (int j = m; j >= zeros; j--) {
                for (int k = n; k >= ones; k--) {
                    dp[j][k] = max(dp[j][k],dp[j-zeros][k-ones]+1);
                }
            }
        }
        return dp[m][n];
    }
};
```