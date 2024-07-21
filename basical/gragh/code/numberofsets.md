题目描述：

![image](/basical/gragh/image/image29.png)

解决过程：

没做出来，经典枚举思想+弗洛伊德算法

代码：

```cpp
class Solution {
public:
    int numberOfSets(int n, int maxDistance, vector<vector<int>>& roads) {
        vector<int> opened (n); // open[i]为1表示第i个节点存在
        vector<vector<int>> dij (n, vector<int> (n, INT_MAX / 2));
        int ret = 0;
        for (int mask = 0; mask < (1 << n); ++mask) {
            // 对所有节点存在的可能组合循环
            for (int i = 0; i < n; ++i) {
                if (mask & (1 << i)) {
                    opened[i] = 1;
                } else {
                    opened[i] = 0;
                }
            }
            fill(dij.begin(), dij.end(), vector<int> (n, INT_MAX / 2));
            for (const auto& road : roads) {
                int u = road[0], v = road[1], w = road[2];
                if (opened[u] == 0 || opened[v] < 0) continue;
                dij[u][v] = dij[v][u] = min(dij[u][v], w);
            }
            for (int k = 0; k < n; ++k) {
                if (opened[k] > 0) {
                    for (int i = 0; i < n; ++i) {
                        if (opened[i] > 0) {
                            for (int j = i + 1; j < n; ++j) {
                                if (opened[j] > 0) {
                                    dij[i][j] = dij[j][i] = min(dij[i][j], dij[i][k] + dij[k][j]);
                                }
                            }
                        }
                    }
                }
            }
            int good = 1;
            for (int i = 0; i < n; ++i) {
                if (opened[i] > 0) {
                    for (int j = i + 1; j < n; ++j) {
                        if (opened[j] > 0) {
                            if (dij[i][j] > maxDistance) {
                                good = 0;
                                break;
                            }
                        }
                    }
                    if (!good) {
                        break;
                    }
                }
            }
            ret += good;
        }
        return ret;
    }
};
```