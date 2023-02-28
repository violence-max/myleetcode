题目描述：  
![image](/algorithmn/dynamic_programming/image/image48.png)  
解决过程：  
关键是以蛇尾为判断标准和使用三维数组  
代码：（动态规划→广度优先搜索）  
```cpp
class Solution {
public:
    int minimumMoves(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<array<int, 2>>> f(n, vector<array<int, 2>>(n, {INVALID, INVALID}));
        f[0][0][0] = 0;

        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                bool canHorizontal = (j + 1 < n && grid[i][j] == 0 && grid[i][j + 1] == 0);
                bool canVertical = (i + 1 < n && grid[i][j] == 0 && grid[i + 1][j] == 0);

                if (i - 1 >= 0 && canHorizontal) {
                    f[i][j][0] = min(f[i][j][0], f[i - 1][j][0] + 1);
                }
                if (j - 1 >= 0 && canHorizontal) {
                    f[i][j][0] = min(f[i][j][0], f[i][j - 1][0] + 1);
                }
                if (i - 1 >= 0 && canVertical) {
                    f[i][j][1] = min(f[i][j][1], f[i - 1][j][1] + 1);
                }
                if (j - 1 >= 0 && canVertical) {
                    f[i][j][1] = min(f[i][j][1], f[i][j - 1][1] + 1);
                }

                if (canHorizontal && canVertical && grid[i + 1][j + 1] == 0) {
                    f[i][j][0] = min(f[i][j][0], f[i][j][1] + 1);
                    f[i][j][1] = min(f[i][j][1], f[i][j][0] + 1);
                }
            }
        }

        return (f[n - 1][n - 2][0] == INVALID ? -1 : f[n - 1][n - 2][0]);
    }

private:
    static constexpr int INVALID = INT_MAX / 2;
};
```  
```cpp
class Solution {
public:
    int minimumMoves(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<array<int, 2>>> dist(n, vector<array<int, 2>>(n, {-1, -1}));
        dist[0][0][0] = 0;
        queue<tuple<int, int, int>> q;
        q.emplace(0, 0, 0);

        while (!q.empty()) {
            auto [x, y, status] = q.front();
            q.pop();
            if (status == 0) {
                // 向右移动一个单元格
                if (y + 2 < n && dist[x][y + 1][0] == -1 && grid[x][y + 2] == 0) {
                    dist[x][y + 1][0] = dist[x][y][0] + 1;
                    q.emplace(x, y + 1, 0);
                }
                // 向下移动一个单元格
                if (x + 1 < n && dist[x + 1][y][0] == -1 && grid[x + 1][y] == 0 && grid[x + 1][y + 1] == 0) {
                    dist[x + 1][y][0] = dist[x][y][0] + 1;
                    q.emplace(x + 1, y, 0);
                }
                // 顺时针旋转 90 度
                if (x + 1 < n && y + 1 < n && dist[x][y][1] == -1 && grid[x + 1][y] == 0 && grid[x + 1][y + 1] == 0) {
                    dist[x][y][1] = dist[x][y][0] + 1;
                    q.emplace(x, y, 1);
                }
            }
            else {
                // 向右移动一个单元格
                if (y + 1 < n && dist[x][y + 1][1] == -1 && grid[x][y + 1] == 0 && grid[x + 1][y + 1] == 0) {
                    dist[x][y + 1][1] = dist[x][y][1] + 1;
                    q.emplace(x, y + 1, 1);
                }
                // 向下移动一个单元格
                if (x + 2 < n && dist[x + 1][y][1] == -1 && grid[x + 2][y] == 0) {
                    dist[x + 1][y][1] = dist[x][y][1] + 1;
                    q.emplace(x + 1, y, 1);
                }
                // 逆时针旋转 90 度
                if (x + 1 < n && y + 1 < n && dist[x][y][0] == -1 && grid[x][y + 1] == 0 && grid[x + 1][y + 1] == 0) {
                    dist[x][y][0] = dist[x][y][1] + 1;
                    q.emplace(x, y, 0);
                }
            }
        }

        return dist[n - 1][n - 2][0];
    }
};
```