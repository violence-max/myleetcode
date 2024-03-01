题目描述：

![image](/algorithmn/dynamic_programming/image/image81.png)

解决过程：

第109场双周赛t4，没做出来

- 0-1背包

代码：

```cpp
int dp[320];
int ans[10][320];
const int mod = 1e9+7;
int init = []() {
    for (int x = 1; x <= 5; ++x) {
        for (int i = 1;  i <= 300 ;++i) {
            memset(dp, 0, sizeof dp);
            dp[0] = 1;
            for (int  j = 1; j <= i; ++j) {
                long long num = 1;
                for (int k = 0; k < x; ++k) num *= j;
                if (num > i) break;
                for (int l = i; l >= num; --l) dp[l] = (dp[l] + dp[l-num]) % mod;
            }
            ans[x][i] = dp[i];
        }
    }
    return 0;
}();

class Solution {
public:
    int numberOfWays(int n, int x) {
        return ans[x][n];
    }
};
```