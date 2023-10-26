题目描述：

![image](/algorithmn/dynamic_programming/image/image102.png)

解决过程：
ac，但是折腾了很久，主要是二维矩阵的坐标化以及初始值上

代码：

```cpp
class Solution {
public:
    int f[100020][50];
    const int mod = 1e9 + 7;
    int numberOfPaths(vector<vector<int>>& grid, int k) {
        memset(f, 0, sizeof f);
        int m = grid.size(), n = grid[0].size();
        // cout << m << ' ' << n << endl;
        f[n+2][grid[0][0] % k] = 1;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (i == 1 && j == 1) continue;
                int v = grid[i-1][j-1];
                // cout << i << ' ' << j << ' ' << v << ':' << endl;
                for (int kk = 0; kk < k; ++kk) {
                    int nk = (v + kk) % k;
                    f[i * (n+1) + j][nk] = (f[i * (n+1) + j][nk] + f[(i-1)*(n+1)+j][kk]) % mod;
                    f[i * (n+1) + j][nk] = (f[i * (n+1) + j][nk] + f[i*(n+1)+j-1][kk]) % mod;
                    // cout << kk << ' ' << nk << ' ' << f[i * (n+1) + j][nk] << endl;
                }
            }
        }
        return f[m * (n+1) + n][0];
    }
};
```