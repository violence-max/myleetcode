题目描述：

![image](/basical/gragh/image/image15.png)

没做出来

- 换根dp

代码：

```cpp
class Solution {
public:
    int dp[30020];
    int sz[30020];
    vector<int> sumOfDistancesInTree(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g (n);
        vector<int> ans (n);
        // memset(dp, -1, 0, sizeof dp);
        for (const auto& v : edges) {
            g[v[0]].emplace_back(v[1]);
            g[v[1]].emplace_back(v[0]);
        }
        function<void(int, int)> dfs = [&](int i, int j) {
            dp[i] = 0;
            sz[i] = 1;
            for (const auto& x : g[i]) {
                if (x == j) continue;
                dfs(x, i);
                dp[i] += dp[x] + sz[x];
                sz[i] += sz[x];
            }
        };
        dfs(0,-1);
        function<void(int,int)> dfs2 = [&](int i, int j) {
            ans[i] = dp[i];
            for (const auto& x : g[i]) {
                if (x == j) continue;
                int pi = dp[i], px = dp[x];
                int si = sz[i], sx = sz[x];
                dp[i] -= dp[x] + sz[x];
                sz[i] -= sz[x];
                dp[x] += dp[i] + sz[i];
                sz[x] += sz[i];
                dfs2(x,i);
                dp[i]=pi;sz[i]=si;dp[x]=px;sz[x]=sx;
            }
        };
        dfs2(0,-1);
        return ans;
    }
};
```