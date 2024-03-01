题目描述：

![image](/algorithmn/dynamic_programming/image/image103.png)

解决过程：
没做出来，考察了整体性思维，从局部推到向整体

代码：

```cpp
class Solution {
public:
    long long dp[1020];
    const int mod = 1e9 + 7;
    int numberOfWays(int n) {
        memset(dp, 0, sizeof 0);
        dp[0] = 1;
        dp[1] = 1;
        for (int i = 2; i <= n; i += 2) {
            for (int j = 1; j < i; j += 2) {
                dp[i] = (dp[i] + dp[j-1] * dp[i-j-1]) % mod;
            }
        }
        return dp[n];
    }
};
```