题目描述：  
![image](/algorithmn/dynamic_programming/image/image58.png)  
解题过程：  
没做出来  
- 动态规划：
1. dp[i]表示以i结尾的分隔数组最大整数
2. $dp[i] = max(dp[i], dp[j] + maxvalue * (i - j)),i - k + 1 <= j <= i$ da  
代码：  
```cpp
class Solution {
public:
    int maxSumAfterPartitioning(vector<int>& arr, int k) {
        int n = arr.size();
        vector<int> dp (n + 1);
        for (int i = 1; i <= n; i++) {
            int maxvalue = arr[i-1];
            for (int j = i - 1; j >= 0 && j >= i - k; j--) {
                dp[i] = max(dp[i], dp[j] + maxvalue * (i - j));
                if (j > 0) {
                    maxvalue = max(maxvalue, arr[j-1]);
                }
            }
        }
        return dp[n];
    }
};
```