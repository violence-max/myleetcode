题目描述：

![image](/algorithmn/dynamic_programming/image/image87.png)

解决过程：

第359场周赛t3，没做出来

动态规划定义没有想明白

代码：

```cpp
class Solution {
public:
    int dp[100020]; // dp[i+1]表示出售编号不超过i的房屋的情况下所获得的最大利益
    int maximizeTheProfit(int n, vector<vector<int>>& offers) {
        vector<vector<array<int, 2>>> a (n+1); // a[i+1]表示所有以编号为i的房屋结尾的销售需求
        for (auto& v : offers) {
            int l = v[0], r = v[1] + 1, c = v[2];
            a[r].push_back({l, c});
        }
        memset(dp, 0, sizeof 0);
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            dp[i] = dp[i-1]; // 编号为i-1的房屋不出售的情况
            for (auto& [l, c]: a[i]) {
                dp[i] = max(dp[i], dp[l] + c);
            }
            ans = max(ans, dp[i]);
        }
        return ans;
    }
};
```