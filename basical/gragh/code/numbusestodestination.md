题目描述：

![image](/basical/gragh/image/image21.png)

解决过程：

没做出来

- 广度优先搜索or带边权的迪杰斯特拉or双向bfs+并查集判断可行性

代码：

```
class Solution {
public:
    bool isc[500][500];
    int numBusesToDestination(vector<vector<int>>& routes, int source, int target) {
        if (source == target) return 0;

        unordered_map<int, vector<int>> map;
        unordered_map<int, unordered_set<int>> s;
        memset(isc, 0, sizeof isc);
        int n = routes.size();
        for (int i = 0; i < n; ++i) {
            for (auto& x : routes[i]) {
                for (auto& oths : map[x]) {
                    isc[i][oths] = isc[oths][i] = true;
                }
                map[x].push_back(i);
                s[i].insert(x);
            }
        }
        queue<int> q;
        int dis[n];
        memset(dis, -1, sizeof dis);
        for (auto& x : map[source]) {
            dis[x] = 1;
            q.push(x);
        }
        while (!q.empty()) {
            auto x = q.front();
            q.pop();
            //cout << x << endl;
            if (s[x].count(target)) return dis[x];
            for (int i = 0; i < n; ++i) {
                if (isc[x][i] && dis[i] == -1) {
                    dis[i] = dis[x] + 1;
                    q.push(i);
                }
            }
        }
        return -1;
    }
};
```