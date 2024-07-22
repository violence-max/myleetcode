题目描述：

![image](/basical/gragh/image/image30.png)

解决过程：

一开始尝试使用并查集去解决，但是之后发现不行，这是有向图

代码：

```
int vis[120];
class Solution {
public:
    int maximumDetonation(vector<vector<int>>& bombs) {
        std::function<long long(int,int,int,int)> cal = [&](int x, int y, int c, int d) -> long long {
            return (long long)abs(x - c) * abs(x - c) + (long long)abs(y - d) * abs(y - d);
        };
        int n = bombs.size();
        vector<vector<int>> edges (n);
        for (int i = 0; i < n; ++i) {
            int x = bombs[i][0], y = bombs[i][1], r = bombs[i][2];
            for (int j = 0; j < n; ++j) {
                if (i == j) continue;
                int c = bombs[j][0], d = bombs[j][1];
                if (cal(x, y, c, d) <= (long long)r * r) {
                    edges[i].push_back(j);
                    cout << i << ' ' << j << endl;
                }
            }
        }
        int ret = INT_MIN;
        for (int i = 0; i < n; ++i) {
            memset(vis, 0, sizeof vis);
            int cnt = 0;
            queue<int> q;
            q.push(i);
            vis[i] = 1;
            while (!q.empty()) {
                int x = q.front(); q.pop();
                cnt++;
                for (int next : edges[x]) {
                    if (!vis[next]) {
                        vis[next] = 1;
                        q.push(next);
                    }
                }
            }
            ret = max(ret, cnt);
        }
        return ret;
    }
};
```