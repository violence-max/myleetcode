题目描述：

![image](/algorithmn/dynamic_programming/image/image100.png)

解决过程：

没做出来，动态规划的状态定义没有想明白

代码：

```
class Solution {
public:
    int f[100][100][100];
    int cherryPickup(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        memset(f, -1, sizeof f);
        f[0][0][n-1] = grid[0][0] + grid[0][n-1];
        for (int i = 1; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < n; ++k) {
                    int v = j == k ? grid[i][j] : grid[i][j] + grid[i][k];
                    for (int jj = j - 1; jj <= j + 1; ++jj) {
                        for (int kk = k - 1; kk <= k + 1; ++kk) {
                            if (jj < 0 || jj >= n || kk < 0 || kk >= n || f[i-1][jj][kk] == -1) continue;
                            f[i][j][k] = max(f[i-1][jj][kk] + v, f[i][j][k]);
                        }
                    }
                    // cout << i << ' ' << j << ' ' << k << ' ' << f[i][j][k] << endl;
                }
            }
        }
        int ans = INT_MIN;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                ans = max(f[m-1][i][j], ans);
            }
        }
        return ans;
    }
};
```