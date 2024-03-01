题目描述:

![image](/algorithmn/tracebak/image/image30.png)

解决过程：

没做出来

- 广度优先搜索

代码：

```cpp
class Solution {
public:
    const int dir[8][2] = {{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
    int n, m;
    bool check(const vector<string>& chessboard, int x, int y, int dx, int dy) {
        int nextx = x + dx, nexty = y + dy;
        while (nextx >= 0 && nextx < n && nexty >= 0 && nexty < m) {
            if (chessboard[nextx][nexty] == 'X') {
                return true;
            } else if (chessboard[nextx][nexty] == '.') {
                return false;
            }
            nextx += dx;
            nexty += dy;
        }
        return false;
    }

    int bfs(vector<string> chessboard, int row, int col) {
        int cnt = 0;
        queue<pair<int,int>> q;
        q.push({row, col});
        chessboard[row][col] = 'X';
        while(!q.empty()) {
            auto [x, y] = q.front();
            q.pop();
            for (int i = 0; i < 8; ++i) {
                int dx = dir[i][0], dy = dir[i][1];
                if (check(chessboard, x, y, dx, dy)) {
                    // cout << row << ' ' << col <<  ' ' << dx << ' ' << dy << endl;
                    int newx = x + dx, newy = y + dy;
                    while (chessboard[newx][newy] != 'X') {
                        chessboard[newx][newy] = 'X';
                        q.push({newx, newy});
                        newx += dx;
                        newy += dy;
                        cnt++;
                    }
                }
            }
        }
        return cnt;
    }
    int flipChess(vector<string>& chessboard) {
        n = chessboard.size();
        m = chessboard[0].size();
        int res = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (chessboard[i][j] == '.') {
                    res = max(res, bfs(chessboard, i, j));
                }
            }
        }      
        return res;
    }
};
```