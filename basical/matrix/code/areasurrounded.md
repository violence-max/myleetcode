题目描述：  
![image](/basical/matrix/image/image8.png)  
解决过程：  
我本来以为可以单独判断每个大O字母是不是被X包围—只要上下左右都可以找到有效的X就可以了，后面发现不是这样的，因为从边界衍生出来的大O字母就算上下左右都可以找到X也不是被X包围的。  
看了题解，发现只要从边界开始深度优先搜索然后就可以把没有被X包围的O全部找出来，剩下的就是被X包围的了。细节见代码  
代码：  
```cpp
class Solution {
public:
    int m , n;
    void dfs(vector<vector<char>>& board, int x, int y) {
        if (x < 0 || x >= m || y < 0 || y >= n) return;
        if (board[x][y] == 'X' || board[x][y] == 'A') return;
        board[x][y] = 'A';
        dfs(board,x,y-1);
        dfs(board,x,y+1);
        dfs(board,x-1,y);
        dfs(board,x+1,y);
    }

    void solve(vector<vector<char>>& board) {
        m = board.size();
        n = board[0].size();
        for (int i = 0; i < m; i++) {
            dfs(board,i,0);
            dfs(board,i,n-1);
        }
        for (int i = 1; i < n - 1; i++) {
            dfs(board,0,i);
            dfs(board,m-1,i);
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'A') {
                    board[i][j] = 'O';
                } else if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                }
            }
        }
    }
 };
```