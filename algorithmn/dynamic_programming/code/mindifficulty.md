题目描述：  
![image](/algorithmn/dynamic_programming/image/image62.png)  
解题过程：  
看了题解自己码出来了，但是没有优化空间，一开始想的也是动态规划，但是想的是第二维才放置天数，没有想到第一维放置会更合适，看上去第一维适合放影响更大的元素吧  
- 题解的代码写得精炼，没有用到最大元素数组，而是先考虑第一维为0的情况，在后续的二维动态变化过程中，使用一个变量去维护一个最大值，而且，j从i开始，充分体现了d > n则直接返回-1这一点  
代码：（我的→题解）  
```cpp
class Solution {
public:
    int minDifficulty(vector<int>& jobDifficulty, int d) {
        int n = jobDifficulty.size();
        if (n < d) return -1;
        vector<vector<int>> dp (d, vector<int> (n, INT_MAX)); // dp[i][j]表示在前i+1天能够执行j个任务的最低难度，j从0开始计数

        vector<vector<int>> maxe (n, vector<int> (n)); // maxe[i][j] 表示区间[i,j]之间的最大值
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                if (j == i) {
                    maxe[i][j] = jobDifficulty[i];
                    continue;
                } else {
                    maxe[i][j] = max(jobDifficulty[j], maxe[i][j-1]);
                }
            }
        }

        for (int i = 0; i < d; i++) {
            // 对天数进行循环
            for (int j = 0; j < n; j++) {
                // 对任务进行循环
                if (i == 0) {
                    // 第0天，直接取最值
                    dp[i][j] = maxe[i][j];
                } else {
                    for (int k = i - 1; k < j; k++) {
                        // 前i天已经至少完成了i项任务，第j项任务必须在第i+1天完成
                        dp[i][j] = min(dp[i][j], dp[i-1][k] + maxe[k+1][j]);
                    }
                }
            }
        }

        return dp[d-1][n-1];
    }
};
```  
```cpp
class Solution {
public:
    int minDifficulty(vector<int>& jobDifficulty, int d) {
        int n = jobDifficulty.size();
        if (n < d) {
            return -1;
        }
        vector<vector<int>> dp(d + 1, vector<int>(n, 0x3f3f3f3f));
        int ma = 0;
        for (int i = 0; i < n; ++i) {
            ma = max(ma, jobDifficulty[i]);
            dp[0][i] = ma;
        }
        for (int i = 1; i < d; ++i) {
            for (int j = i; j < n; ++j) {
                ma = 0;
                for (int k = j; k >= i; --k) {
                    ma = max(ma, jobDifficulty[k]);
                    dp[i][j] = min(dp[i][j], ma + dp[i - 1][k - 1]);
                }
            }
        }
        return dp[d - 1][n - 1];
    }
};
```