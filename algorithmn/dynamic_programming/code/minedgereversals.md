题目描述：

![image](/algorithmn/dynamic_programming/image/image95.png)

解决过程：

第113场双周赛t4，没做出来

换根dp，没想出来

代码：

```cpp
class Solution {
public:
    vector<int> minEdgeReversals(int n, vector<vector<int>>& edges) {
        vector<pair<int,int>> g[n];
        for (auto &edge : edges) {
            int u = edge[0], v = edge[1];
            g[u].push_back({v, 0});
            g[v].push_back({u, 1});
        }        

        int recor[n];
        function<int(int,int)> dfs = [&](int root, int fa) -> int {
            int ret = 0;
            for (auto nex : g[root]) {
                if (nex.first == fa) continue;
                int nx = nex.first, cost = nex.second;
                ret += dfs(nx, root) + cost;
            }
            return recor[root] = ret;
        };
        dfs(0, -1);
        vector<int> res (n);
        function<void(int,int)> reroot = [&](int root, int fa) {
            res[root] = recor[root];
            for (auto nex : g[root]) {
                if (nex.first == fa) continue;
                int nx = nex.first, cost = nex.second;
                int origin1 = recor[root], origin2 = recor[nx];
                recor[root] -= recor[nx] + cost;
                recor[nx] += recor[root] + (cost == 0);
                reroot(nx, root);
                recor[root] = origin1;
                recor[nx] = origin2;
            }
        };
        reroot(0, -1);
        return res;
    }
};
```