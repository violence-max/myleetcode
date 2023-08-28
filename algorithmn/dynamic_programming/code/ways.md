题目描述：

![image](/algorithmn/dynamic_programming/image/image86.png)

解决过程：

没做出来

dp[i][j][k]表示在以(i,j)为左上顶点的矩形中切k份披萨的总方案数

代码：

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;
    int a[100][100];
    int dp[100][100][20];
    int ways(vector<string>& pizza, int k) {
        int m = pizza.size(), n = pizza[0].size();
        memset(a, 0, sizeof a);
        memset(dp, 0 ,sizeof dp);
        for (int i = m - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                a[i][j] = a[i+1][j] + a[i][j+1] - a[i+1][j+1] + (pizza[i][j] == 'A');
                dp[i][j][1] = a[i][j] > 0 ? 1 : 0; 
            }
        }
        for (int x = 2; x <= k; ++x) {
            for (int i = m - 1; i >= 0; --i) {
                for (int j = n - 1; j >= 0; --j) {
                    for (int ii = i + 1; ii < m; ++ii) { // 横切
                        if (a[i][j] > a[ii][j]) {
                            dp[i][j][x] = (dp[i][j][x] + dp[ii][j][x-1]) % mod;
                        }
                    }
                    for (int jj = j; jj < n; ++jj) {// 竖切
                        if (a[i][j] > a[i][jj]) {
                            dp[i][j][x] = (dp[i][j][x] + dp[i][jj][x-1]) % mod;
                        }
                    }
                }
            }
        }
        return dp[0][0][k];
    }
};
```