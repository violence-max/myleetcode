题目描述：

![image](/algorithmn/dynamic_programming/image/image80.png)

解决过程：

第109场双周赛t3，ac

代码：

```cpp
class Solution {
public:
    long long dp[100020][2];
    long long maxScore(vector<int>& nums, int x) {
        memset(dp, 0, sizeof dp);
        if (nums[0]& 1) {
            dp[0][1]= nums[0];
            dp[0][0] = -x;
        } else {
            dp[0][0]= nums[0];
            dp[0][1] = -x;
        }
        long long ans = nums[0];
        int n = nums.size();
        for (int i = 1; i < n; ++i) {
            if (nums[i] & 1) {
                // int tmp = max(dp[i-1][1], dp[i-1][0] - x);
                dp[i][1] = max(dp[i-1][1], dp[i-1][0] - x) + nums[i];
                // cout << i << ' ' << dp[i][1] <<' ' << tmp <<' '<<dp[i-1][1]<<' '<<dp[i-1][0]<< endl;
                // cout << i << ' ' << dp[i][1] << endl;
                dp[i][0] = dp[i-1][0];
            } else {
                dp[i][1] = dp[i-1][1];
                dp[i][0] = max(dp[i-1][1] - x, dp[i-1][0]) + nums[i];
            }
            ans = max(ans, max(dp[i][0], dp[i][1]));
            // cout << i << ' ' << dp[i][0] << ' ' << dp[i][1] << ' ' << ans << endl;
        }
        return ans;
    }
};
```