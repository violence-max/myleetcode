题目描述：  
![image](/algorithmn/tracebak/image/image14.png)  
解题过程:  
感觉N皇后问题应该是枚举的天花板了，也可能是我能力提高了，这个问题真的是一看题解就恍然大悟，有点高中看导数答案恍然大悟的感觉了。  
题解的思路也太好了，用一个数组来存放每一行应该放置的皇后的列的位置，由这个玩意生成答案字符串之一，啧啧啧。  
深度遍历的思路也是强无敌，三个集合存放已经使用过的列和斜线位置，回溯插入和清除集合元素，无敌！  
代码：  
```cpp
class Solution {
private:
    vector<vector<string>> ret;
public:
    void dfs(int n,unordered_set<int> columns,unordered_set<int> dig1,unordered_set<int> dig2,int row,vector<int> queens) {
        if (row == n) {
            vector<string> ans =generateBorad(queens,n);
            ret.push_back(ans);
            return;
        }
        for (int i = 0; i < n; i++) {
            if (columns.find(i) != columns.end()) continue;
            int digg1 = row - i;
            if (dig1.find(digg1) != dig1.end()) continue;
            int digg2 = row + i;
            if (dig2.find(digg2) != dig2.end()) continue;
            queens[row] = i;
            columns.insert(i);
            dig1.insert(digg1);
            dig2.insert(digg2);
            dfs(n,columns,dig1,dig2,row+1,queens);
            queens[row] = -1;
            columns.erase(i);
            dig1.erase(digg1);
            dig2.erase(digg2);
        }
    }

    vector<string> generateBorad(vector<int> queens,int n) {
        vector<string> ans;
        for (int i = 0; i < n; i++) {
            string s (n,'.');
            s[queens[i]] = 'Q';
            ans.push_back(move(s));
        }
        return ans;
    }

    vector<vector<string>> solveNQueens(int n) {
        unordered_set<int> columns;
        unordered_set<int> dig1;
        unordered_set<int> dig2;
        vector<int> queens(n,-1);
        dfs(n,columns,dig1,dig2,0,queens);
        return ret;
    }
};
```