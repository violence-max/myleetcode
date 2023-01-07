题目描述：  
![image](/algorithmn/tracebak/image/image18.png)  
解题过程：  
不知道为什么自己的回溯超时了，感觉应该是一个不错的写法才对。但是自己撸出回溯的过程稍微有点久，主要在于逻辑这一块应该时没有盘清楚，什么时候往前走，怎么往前走，找到了一个点以后怎么继续向四周进行寻找，四周找不到下一个点怎么办，把这些想清楚了应该没有问题。  
题解里面的写法跟我差不多，但是它是在主函数里面对每个点进行寻找，我是一口气从开始的地方直接寻找（现在把自己的代码改得跟官方题解几乎一模一样了，发现还是不可以，额。。）  
代码：（自己（超时）→（官方代码超时了）于是我用了一个code友的代码，太离谱了这题）  
```cpp
class Solution {
private:
    vector<vector<bool>> visit;
    bool found = false;
    vector<pair<int,int>> directions = {{0,1},{0,-1},{1,0},{-1,0}};
public:
    void dfs(vector<vector<char>> board,int row,int col,string word,int index) {
        int m = board.size(),n = board[0].size(),lw = (int)word.size();
        if (board[row][col] == word[index]) {
            visit[row][col] = true;
            if (index == lw - 1) {
                found = true;
                return;
            } else {
                int newRow,newCol;
                for (auto p : directions) {
                    newRow = row + p.first;
                    newCol = col + p.second;
                    if (0 <= newRow && newRow < m && 0 <= newCol && newCol < n) {
                        if (!visit[newRow][newCol]) {
                            dfs(board,newRow,newCol,word,index+1);
                            if (found) return;
                        }
                    }
                }
                visit[row][col] = false;
            }
        }
    }

    bool exist(vector<vector<char>>& board, string word) {
        int m = board.size(),n = board[0].size();
        visit = vector<vector<bool>> (m,vector<bool> (n,false));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                dfs(board,i,j,word,0);
                if (found) return true;
            }
        }
        return false;
    }
};
```
```cpp
class Solution {
public:

    bool backtrack(vector<vector<char>>& board, vector<vector<int>>& map ,string word, int i, int j, int index){
        
        if(i<0 || i==board.size() || j < 0 || j==board[0].size() || map[i][j] || board[i][j] != word[index]) return false;
        
        if(index == word.size()-1 && word[index] == board[i][j])    return true;
        

        map[i][j] = 1;
        if (backtrack(board, map, word, i, j-1, index+1) || 
            backtrack(board, map, word, i-1, j, index+1) || 
            backtrack(board, map, word, i, j+1, index+1) || 
            backtrack(board, map, word, i+1, j, index+1)) return true;
        map[i][j] = 0;
        return false;
    }

    bool exist(vector<vector<char>>& board, string word) {
        vector<vector<int>> map(board.size(), vector<int>(board[0].size(), 0));
        for(int i = 0 ; i < board.size(); i++ ){
            for(int j = 0; j < board[0].size(); j++){
                if(board[i][j] != word[0])  continue;
                if(backtrack(board, map, word, i, j, 0))   return true;
            }
        }
        return false;
    }
};
```