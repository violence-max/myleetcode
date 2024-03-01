题目描述：

![image](/algorithmn/tracebak/image/image32.png)

解决过程：

应该是写了40分钟

代码：

```cpp
class Solution {
public:
    const int d[4][2] = {
        {1, 0},
        {-1, 0},
        {0, -1},
        {0, 1},
    };

    int uniquePathsIII(vector<vector<int>>& grid) {
        int ans = 0;
        int m = grid.size(), n = grid[0].size();
        int cnt = 0;
        int sx = -1, sy = -1;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) 
                if (grid[i][j] == 0) cnt++;
                else if (grid[i][j] == 1) {
                    sx = i;
                    sy = j;
                }
        }

        int v[m][n];
        memset(v, 0, sizeof v);
        function<int(int,int,int)> dfs = [&](int x, int y, int c) -> int {
            if (grid[x][y] == 2) {
                if (c == cnt) return 1;
                else return 0;
            }
// cout << x << ' ' << y << ' ' << c << ' ' << cnt << endl;
            v[x][y] = 1;
            int res = 0;
            for (int i = 0; i < 4; ++i) {
                int nx = x + d[i][0], ny = y + d[i][1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                    if (grid[nx][ny] == -1) continue;
                    if (v[nx][ny] == 1) continue;

                    int nc = c + (grid[nx][ny] == 0);
                    // cout << nx << ' ' << ny << ' ' << c << ' ' << nc << endl;
                    res += dfs(nx, ny, nc);
                }
            }
            v[x][y] = 0;
            return res;
        };
        return dfs(sx,sy,0);
    }
};
```