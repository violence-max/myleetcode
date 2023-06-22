题目描述：

![image](/algorithmn/tracebak/image/image29.png)

解决过程：

深度优先搜索，15分钟

代码：

```cpp
class Solution {
public:
    int m, n;
    vector<int> ans;
    int dfs(vector<vector<int>>& land, int row, int col) {
        if (row >= m || row < 0 || col < 0 || col >= n || land[row][col] != 0) {
            return 0;
        };
        land[row][col] = -1;
        return 1 + dfs(land, row - 1, col) + dfs(land, row + 1, col) + dfs(land, row, col - 1) + dfs(land, row, col + 1)
                 + dfs(land, row - 1, col - 1) + dfs(land, row - 1, col + 1) + dfs(land, row + 1, col - 1) + dfs(land, row + 1, col + 1);
    }
    vector<int> pondSizes(vector<vector<int>>& land) {
        m = land.size();
        n = land[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (land[i][j] == 0) {
                    ans.push_back(dfs(land, i, j));
                    cout << i << ' ' << j << endl;
                }
            }
        }
        sort(ans.begin(), ans.end());
        return ans;
    }
};
```