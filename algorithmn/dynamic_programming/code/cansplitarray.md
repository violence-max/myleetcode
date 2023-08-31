题目描述：

![image](/algorithmn/dynamic_programming/image/image91.png)

解决过程：

第357场周赛t2，ac

一直在想区间dp的解法， 虽然最后是写出来了但是由于对区间dp不是很熟，写了很久，中间还wa了好几发。看了灵神题解，没有想到这竟然是脑筋急转弯的问题

代码：（区间dp→脑筋急转弯）

```cpp
class Solution {
public:
    int pre[101];
    bool dp[101][101];
    bool canSplitArray(vector<int>& nums, int m) {
        pre[0] = 0;
        int n = nums.size();
        for (int i = 1; i <= n; ++i) {
            pre[i] = pre[i-1] + nums[i-1];
        }
        memset(dp, 0, sizeof dp);
        for (int span = 0; span < n; ++span) {
            for (int i = 0; i < n - span; ++i) {
                int j = i + span;

                if (i == j) dp[i][j] = true;
                else {
                    for (int p = i; p < j; ++p) {
                        if (((i == p || pre[p+1] - pre[i] >= m) && dp[i][p]) && 
                            ((j == p + 1 || pre[j+1] - pre[p+1] >= m) && dp[p+1][j])) {
                            dp[i][j] = true;
                        }
                        if (dp[i][j]) break;
                    }
                }
                // cout << i << ' ' << j << ' ' << dp[i][j] << endl;
            }
        }
        return dp[0][n-1];
    }
};
```

```cpp
class Solution {
public:
    bool canSplitArray(vector<int> &nums, int m) {
        int n = nums.size();
        if (n <= 2) return true;
        for (int i = 1; i < n; i++)
            if (nums[i - 1] + nums[i] >= m)
                return true;
        return false;
    }
};
```