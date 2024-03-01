题目描述：

![image](/basical/gragh/image/image23.png)

解决过程：

没有做出来

- 拓扑排序

代码：

```
class Solution {
public:
    int deg[30020];
    int collectTheCoins(vector<int>& coins, vector<vector<int>>& edges) {
        int n = coins.size();
        vector<int> g[n];
        memset(deg, 0, sizeof deg);
        for (auto& edge : edges) {
            int x = edge[0], y = edge[1];
            g[x].push_back(y);
            g[y].push_back(x);
            ++deg[x];
            ++deg[y];
        }
        int le = n - 1;
        vector<int> q;
        for (int i = 0; i < n; ++i) {
            if (deg[i] == 1 && !coins[i]) {
                q.push_back(i);
            }
        }
        while (!q.empty()) {
            --le;
            auto x = q.back(); q.pop_back();
            for (auto nx : g[x]) {
                if (--deg[nx] == 1 && !coins[nx]) {
                    q.push_back(nx);
                }
            }
        }
        for (int i = 0; i < n; ++i) {
            if (deg[i] == 1 && coins[i]) {
                q.push_back(i);
            }
        }
        le -= q.size();
        for (auto x : q) {
            for (auto y : g[x]) {
                if (--deg[y] == 1) {
                    --le;
                }
            }
        }
        return max(le * 2, 0);
    }
};
```