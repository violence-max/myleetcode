题目描述：  
![image](/algorithmn/dynamic_programming/image/image29.png)  
解题过程：  
就是最长公共子序列  
代码：  
```cpp
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(),n = nums2.size();
        vector<vector<int>> dp(m+1,vector<int>(n+1));
        int ret = 0;
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                dp[i][j] = nums1[i] == nums2[j] ? dp[i+1][j+1] + 1 : max(dp[i][j+1],dp[i+1][j]);
                ret = max(ret,dp[i][j]);
            }
        }
        return ret;
    }
};
```