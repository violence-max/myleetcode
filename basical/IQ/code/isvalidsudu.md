题目描述：  
![image](/basical/IQ/image/image6.png)  
解题过程：  
在处理9个方块的时候稍微花了一点时间才想到要用一个二维数组做处理。还有在写代码的时候又手残了，还有就是忽略了句点号  
代码：  
```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        vector<unordered_set<char>> row (9);
        vector<unordered_set<char>> col (9);
        vector<vector<unordered_set<char>>> block (3,vector<unordered_set<char>> (3));
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char c = board[i][j];
                if (c == '.') continue;
                if (row[i].find(c) != row[i].end() || col[j].find(c) != col[j].end() || block[i/3][j/3].find(c) != block[i/3][j/3].end()) {
                    return false;
                } else {
                    row[i].insert(c);
                    col[j].insert(c);
                    block[i/3][j/3].insert(c);
                }
            }
        }
        return true;
    }
};
```