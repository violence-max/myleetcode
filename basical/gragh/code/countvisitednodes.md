题目描述：

![image](/basical/gragh/image/image26.png)

题目描述：

第365场周赛t4，没做出来

- 有向基环树

代码：

```
class Solution {
public:
    int d[100020];
    bool vis[100020];
    vector<int> countVisitedNodes(vector<int>& edges) {
        int n = edges.size();
        vector<vector<int>> gg (n);
        memset(d, 0, sizeof d);
        memset(vis, 0, sizeof vis);
        for (int i = 0; i < n; ++i) {
            gg[edges[i]].push_back(i);
            d[edges[i]]++;
        }
        queue<int> q;
        for (int i = 0; i < n; ++i) {
            if (d[i] == 0) q.push(i);
        }
        while (!q.empty()) {
            int x = q.front();
            q.pop();
            if (--d[edges[x]] == 0) q.push(edges[x]);
        }
        vector<int> ans (n);
        function<void(int,int)> dfs = [&](int x, int h) {
            ans[x] = h + 1;
            for (auto nx : gg[x]) {
                dfs(nx, h + 1);
            }
        };
        for (int i = 0; i < n; ++i) {
            if (d[i] > 0 && !vis[i]) {
                int cnt = 0, x = i;
                vector<int> tmp;
                while (!vis[x]) {
                    cnt++;
                    vis[x] = true;
                    tmp.push_back(x);
                    x = edges[x];
                }
                for (auto xx : tmp) {
                    ans[xx] = cnt;
                    for (auto ii : gg[xx]) {
                        if (vis[ii]) continue;
                        dfs(ii, ans[xx]);
                    }
                }
            }
        }
        return ans;
    }
};
```