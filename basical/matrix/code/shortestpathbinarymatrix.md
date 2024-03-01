题目描述：

![image](/basical/matrix/image/image13.png)

解题过程：

做了将近一个小时没有做出来

- 广度优先搜索：
1. 适用无权图的最短路径问题

代码：（广度优先搜索）

```cpp
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        if (grid[0][0] == 1) {
            return -1;
        }
        int n = grid.size();
        queue<pair<int,int>> q;
        vector<vector<int>> dist (n, vector<int> (n, INT_MAX));
        dist[0][0] = 1;
        q.push({0,0});
        while (!q.empty()) {
            auto [x,y] = q.front();
            q.pop();
            if (x == n - 1 && y == n - 1) {
                return dist[n-1][n-1];
            }
            for (int dx = -1; dx <= 1; dx++) {
                for (int dy = -1; dy <= 1; dy++) {
                    int new_x = x + dx, new_y = y + dy;
                    if (new_x < 0 || new_x >= n || new_y < 0 || new_y >= n || grid[new_x][new_y] == 1 || dist[new_x][new_y] <= dist[x][y] + 1) {
                        // 防止越界、单元格数字为1和已经访问过三种情况
                        continue;
                    }
                    dist[new_x][new_y] = dist[x][y] + 1;
                    q.push({new_x, new_y});
                }
            }
        }
        return -1;
    }
};
```

遍历8个方向和判断是否访问过的方法值得学习