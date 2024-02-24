题目描述：

![image](/algorithmn/dynamic_programming/image/image105.png)

解题过程：

第369场周赛t4，没做出来

经典树形dp，一个节点两种方案，使用一个变量记录每个节点应该右移几次、右移的概念和提前计算好最多右移几次是精髓

代码：（自顶向下→自底向上）

```cpp
class Solution {
public:
    int dfs(int i, int j, int fa, vector<array<int,14>>& memo, vector<int>& coins, int k, vector<vector<int>>& g) {
        auto &res = memo[i][j];
        if (res != -1) return res;
        int res1 = (coins[i] >> j) - k, res2 = coins[i] >> (j + 1);
        for (auto nx : g[i]) {
            if (nx == fa) continue;
            res1 += dfs(nx, j, i, memo, coins, k, g);
            if (j < 13) {
                res2 += dfs(nx, j + 1, i, memo, coins, k, g);
            }
        }
        return res = max(res1, res2);
    }
    int maximumPoints(vector<vector<int>>& edges, vector<int>& coins, int k) {
        int n = coins.size();
        vector<vector<int>> g (n);
        for (auto edge : edges) {
            int u = edge[0], v = edge[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        array<int, 14> init;
        for (int i = 0; i < 14; ++i) init[i] = -1;
        vector<array<int, 14>> memo (n, init);
        return dfs(0, 0, -1, memo, coins, k, g);
    }
};
```

```cpp
class Solution {
public:
    int maximumPoints(vector<vector<int>> &edges, vector<int> &coins, int k) {
        vector<vector<int>> g(coins.size());
        for (auto &e : edges) {
            int x = e[0], y = e[1];
            g[x].push_back(y);
            g[y].push_back(x);
        }

        function<array<int, 14>(int, int)> dfs = [&](int x, int fa) -> array<int, 14> {
            array<int, 14> res1{}, res2{};
            for (int y : g[x]) {
                if (y == fa) continue;
                auto r = dfs(y, x);
                for (int j = 0; j < 14; j++) {
                    res1[j] += r[j];
                    if (j < 13) {
                        res2[j] += r[j + 1];
                    }
                }
            }
            for (int j = 0; j < 14; j++) {
                res1[j] = max(res1[j] + (coins[x] >> j) - k, res2[j] + (coins[x] >> (j + 1)));
            }
            return res1;
        };
        return dfs(0, -1)[0];
    }
};
```