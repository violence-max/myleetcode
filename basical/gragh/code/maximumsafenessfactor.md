题目描述：

![image](/basical/gragh/image/image18.png)

解决过程：

第357场周赛t3，没做出来

- 多源BFS+并查集

代码：

```cpp
class Solution {
public:
    int man[400][400], f[160000];
    const int dir[4][2] = {
        {-1, 0},
        {1, 0},
        {0, -1},
        {0, 1},
    };
    int maximumSafenessFactor(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<pair<int,int>> q;
        memset(man, -1, sizeof man);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    man[i][j] = 0;
                    q.push_back({i,j});
                }
            }
        }
        vector<vector<pair<int,int>>> groups = {q};
        while (!q.empty()) { // 多源bfs
            vector<pair<int,int>> tmp;
            for (auto [x,y]: q) {
                for (int i = 0; i < 4; ++i) {
                    int nx = x + dir[i][0], ny = y + dir[i][1];
                    if (nx >= 0 && nx < n && ny >= 0 && ny < n && man[nx][ny] == -1) {
                        man[nx][ny] = groups.size();
                        tmp.push_back({nx,ny});
                    }
                }
            }
            q = move(tmp);
            if (!q.empty()) groups.push_back(q);
        }
        // for (int i = 0; i < n; ++i) for (int j = 0; j  < n; ++j) cout << i << ' ' << j << ' ' << man[i][j] << endl; // debug
        function<int(int)> get = [&](int x) {
            if (x == f[x]) {
                return f[x];
            }
            return f[x] = get(f[x]);
        };
        iota(f, f + n * n, 0);
        for (int ans = groups.size() - 1; ans > 0; --ans) { 
            // cout << ans << endl;
            for (auto [x,y] : groups[ans]) {
                // cout << x << ' ' << y << ' ' << man[x][y] << endl;
                for (int i = 0; i < 4; ++i) {
                    int nx = x + dir[i][0], ny = y + dir[i][1];
                    if (nx >= 0 && nx < n && ny >= 0 && ny < n && man[x][y] <= man[nx][ny]) {
                        f[get(x * n + y)] = f[get(nx * n + ny)];
                        // cout << nx << ' ' << ny << ' ' << man[nx][ny] << ' ' << f[get(x * n + y)] << ' ' << f[get(nx * n + y)] << endl;
                    }
                }
            }
            if (f[get(0)] == f[get(n*n - 1)]) return ans;
        }
        return 0;
    }
};
```