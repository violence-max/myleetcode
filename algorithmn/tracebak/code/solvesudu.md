题目描述：  
![image](/algorithmn/tracebak/image/image15.png)  
解题过程：  
这道题不难啊，怎么会做了三个小时呢，我都无语了，就是调代码太久了。写这道题，思路花了十分钟不到，写代码花了五十分钟，调代码花了两个小时这样可能不到两个小时。思路一开始我想得很简单，就是设置三个哈希表，分别是行哈希表、列哈希表和块哈希表，维护好了之后深度递归遍历解决。当然，在写代码的时候这个深度递归遍历还是写了很久，其实写代码花了不止五十分钟，可能花了有一个小时四十分钟，主要就是深度遍历这一块出了很大问题，然后debug花了一个小时十分钟左右。  
深度递归遍历：  
正确的思路：每次判断一个位置，先判断列是否遍历成功，若没有则判断当前位置是否已经填入数值，若有则遍历同一行的下一列的位置，若没有则在当前位置填入一个合适的值。对于判断遍历是否成功，直接判断当前位置是否列数已经达到最大，若是，则判断行数是否达到最大，若是，则设置成功全局变量为true并返回，若不是则遍历下一行。  
印象里踩的坑：  
1. 没有设置成功全局变量
2. 在如果从遍历完一行到进入下一行想了很久
3. 在如果当前位置已经填入数字的情况想了很久  
这个题目我踩了太多坑了，罗列一下吧，如果不是踩那么多坑，我早做完了，关键是我也不清楚为什么可以踩这些坑，都很智障啊。  
1. 在生成块索引值的时候循环变量没写明白，count的值没有加明白
2. 在深度遍历的时候插入一个节点之后忘记设置有效数组为true了
3. 在深度遍历的时候删除一个值的时候忘记是要删除对应哈希表的值了，直接吧哈希映射的int型值整个给干掉了
4. 遍历从0到9的字符时用的循环从’0’加到’9’，后面改用了哈希表
5. 在生成块哈希表的时候循环值没写明白  
看了题解，居然可以写得那么简洁，真的无敌。包括一次循环初始化四个表的思想，如果处理整数字符值的思想，还有使用pair<int,int>的有序对的思想，真的猛。  
代码：  
```cpp
class Solution {
private:
    bool rowFlag = false;
    bool colFlag = true;
    unordered_map<int,unordered_set<char>> rowRecord;
    unordered_map<int,unordered_set<char>> colRecord;
    unordered_map<int,unordered_set<char>> xRecord;
    vector<vector<bool>> used;
    bool findFlag = false;
    unordered_set<char> base = {'1','2','3','4','5','6','7','8','9'};
    
public:
    unordered_map<int,unordered_set<char>> scanRowColumn(const vector<vector<char>> board,int row,int column,bool flag) {
        unordered_map<int,unordered_set<char>> ans;
        for (int i = 0; i < row; i++) {
            unordered_set<char> record;
            for (int j = 0; j < column; j++) {
                char c = (flag) ? board[j][i] : board[i][j];
                if (c != '.') {
                    record.insert(c);
                }
            }
            ans[i] = record;
        }
        return ans;
    }

    unordered_map<int,unordered_set<char>> scan3x3(const vector<vector<char>> board,int row,int column) {
        int l1 = 3,l2 = 3;
        int count = 0;
        unordered_map<int,unordered_set<char>> ans;
        for (int i = l1; i <= row; i += 3) {
            for (int j = l2; j <= column; j += 3) {
                unordered_set<char> record;
                for (int k = i - 3; k < i; k++) {
                    for (int l = j - 3; l < j; l++) {
                        if (board[k][l] != '.') {
                            record.insert(board[k][l]);
                        }
                    }
                }
                ans[count++] = record;
            }
        }
        return ans;
    }

    vector<vector<bool>> usedSet(const vector<vector<char>> board,int row,int column) {
        vector<vector<bool>> used (row,vector<bool> (column,false));
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < column; j++) {
                if (board[i][j] != '.') used[i][j] = true;
            }
        }
        return used;
    }

    void dfs(vector<vector<char>>& board,int row,int column) {
        if (column == board[0].size()) {
            if (row == board.size()) {
                findFlag = true;
                return;
            } else{
                int col = (row == board.size()-1) ? column : 0;
                dfs(board,row+1,col);
            }
        } else {
            if (used[row][column]) {
                dfs(board,row,column+1);
            } else {
                int x = generatexIndex(row,column,board.size(),board[0].size());
                for (auto i : base) {
                    if (rowRecord[row].find(i) != rowRecord[row].end() || colRecord[column].find(i) != colRecord[column].end()) continue;
                    if (xRecord[x].find(i) != xRecord[x].end()) continue;
                    rowRecord[row].insert(i);
                    colRecord[column].insert(i);
                    xRecord[x].insert(i);
                    board[row][column] = i;
                    used[row][column] = true;
                    dfs(board,row,column+1);
                    if (findFlag) return;
                    board[row][column] = '.';
                    rowRecord[row].erase(i);
                    colRecord[column].erase(i);
                    xRecord[x].erase(i);
                    used[row][column] = false;
                }
            }
        }
    }

    int generatexIndex(int row,int column,int rowRestrict,int colRestrict) {
        int count = 0;
        int l1 = 3,l2 = 3;
        for (int i = l1; i <= rowRestrict; i += 3) {
            for (int j = l2; j <= colRestrict; j += 3) {
                if ((row < i && row >= i - 3) && (column < j && column >= j - 3)) {
                    return count;
                } else count++;
            }
        }
        return -1;
    }

    void solveSudoku(vector<vector<char>>& board) {
        int row = board.size();
        int column = board[0].size();
        rowRecord = scanRowColumn(board,row,column,rowFlag);
        colRecord = scanRowColumn(board,row,column,colFlag);
        xRecord = scan3x3(board,row,column);
        used = usedSet(board,row,column);
        dfs(board,0,0);
        return;
    }
};
```