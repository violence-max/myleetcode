题目描述：

![image](/basical/matrix/image/image15.png)

题目描述：

5分钟ac

- 题解的哈希表方法可以达到O(n^2)复杂度

代码：（模拟→哈希表）

```cpp
class Solution {
public:
    int equalPairs(vector<vector<int>>& grid) {
        int ret = 0;
        int n = grid.size();
        for (int i = 0; i < n; i++) {
            // 行遍历
            for (int j = 0; j < n; j++) {
                // 列遍历
                int cnt = 0;
                for (int k = 0; k < n; k++) {
                    // 比较第i行和第j列
                    if (grid[i][k] != grid[k][j]) {
                        break;
                    }
                    cnt++;
                }
                ret += cnt == n;
            }
        }
        return ret;
    }
};
```