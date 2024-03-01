题目描述：

![image](/basical/gragh/image/image19.png)

解决过程：
第361场周赛t4，没做出来

- 倍增lca+计数

代码：

```cpp
const int LOG = 15;
int deep[10020], p[10020][15], sum[10020][15][27];
class Solution {
public:
    vector<int> minOperationsQueries(int n, vector<vector<int>>& edges, vector<vector<int>>& queries) {
        vector<vector<pair<int,int>>> gragh(n, vector<pair<int,int>>());
        for (auto& edge : edges) {
            int s = edge[0], e = edge[1], w = edge[2];
            gragh[s].push_back({e,w});
            gragh[e].push_back({s,w});
        }       
        function<void(int,int)> dfs = [&](int u, int fa) {
            p[u][0] = fa;
            for (auto& [next, w] : gragh[u]) {
                if (next != fa) {
                    sum[next][0][w] = 1;
                    deep[next] = deep[u] + 1;
                    dfs(next, u);
                }
            }
        };
        memset(deep, 0, sizeof deep);
        memset(p, -1, sizeof p);
        memset(sum, 0, sizeof sum);
        dfs(0, -1);
        for (int i = 1; i < LOG; ++i) {
            for (int j = 0; j < n; ++j) {
                if (p[j][i-1] != -1) {
                    p[j][i] = p[p[j][i-1]][i-1];
                    for (int k = 1; k <= 26; ++k) {
                        sum[j][i][k] = sum[j][i-1][k] + sum[p[j][i-1]][i-1][k];
                    }
                }
            }
        }
        vector<int> ans;
        for (auto& v : queries) {
            int x = v[0], y = v[1];
            int cnt[27]{};
            if (deep[x] > deep[y]) {
                swap(x,y);
            }
            int path_length = deep[x] + deep[y];
            int diff = deep[y] - deep[x];
            for (int i = 0; i < LOG; ++i) {
                if (diff & (1 << i)) {
                    for (int j = 1; j <= 26; ++j) {
                        cnt[j] += sum[y][i][j];
                    }
                    y = p[y][i];
                }
            }
            if (x != y) {
                for (int i = LOG - 1; i >= 0; --i) {
                    int px = p[x][i], py = p[y][i];
                    if (px != py) {
                        for (int j = 1; j <= 26; ++j) {
                            cnt[j] += sum[x][i][j] + sum[y][i][j];
                        }
                        x = px;
                        y = py;
                    }
                }
                for (int j = 1; j <= 26; ++j) {
                    cnt[j] += sum[x][0][j] + sum[y][0][j];
                }
                x = p[x][0];
            }
            int lca = x;
            ans.push_back(path_length - 2 * deep[lca] - *max_element(cnt, cnt + 27));
        }
        return ans;
    }
};
```