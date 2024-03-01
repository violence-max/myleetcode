题目描述：

![image](/basical/gragh/image/image17.png)

解决过程：

十分钟搞定，但是题解提出了一种可以减小边数的将无向图转变为有向图的构造方式：

- 构造规则：（当在原始无向图中i和j有一条边时）
1. 若degree[i] < degree[j] 或者 degree[i] == degree[j] && i < j，则i到j有一条边
2. 否则j到i有一条边

这样的构造可以保证节点i的出度不会超过sqrt(2*m)，可以用反证法证明。经过这样的构造之后，可以在边数m达到1e5数量级时仍能保证良好的时间复杂度m*sqrt(m)

代码：

```cpp
class Solution {
public:
    vector<int> adj[401];
    bool connect[401][401];
    int minTrioDegree(int n, vector<vector<int>>& edges) {
        for (int i = 1; i <= n; ++i) adj[i].clear();
        memset(connect, 0, sizeof connect);
        for (auto& edge : edges) {
            int u = edge[0], v = edge[1];
            adj[u].push_back(v);
            adj[v].push_back(u);
            connect[u][v] = connect[v][u] = true;
        }

        int ans = INT_MAX;
        for (int i = 1; i <= n; ++i) {
            if (adj[i].size() < 2) continue;

            int m = adj[i].size();
            for (int j = 0; j < m; ++j) {
                for (int k = j + 1; k < m; ++k) {
                    if (connect[adj[i][j]][adj[i][k]]) {
                        ans = min(ans, (int)adj[i].size() + (int)adj[adj[i][j]].size() + (int)adj[adj[i][k]].size() - 6);
                    }
                }
            }
        }
        return ans == INT_MAX ? -1 : ans;
    }
};
```