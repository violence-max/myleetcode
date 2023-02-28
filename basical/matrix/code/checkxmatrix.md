题目描述：  
![image](/basical/matrix/image/image9.png)
解决过程：   
5分钟ac。看了灵神的解法，像这种有两个都为真或者两个都为假的题目，可以用一个爽等号来解决    
代码：  
```cpp
class Solution {
public:
    bool checkXMatrix(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j || n - i - 1 == j) {
                    if (grid[i][j] == 0) return false;
                } else if (grid[i][j] != 0) return false;
            }
        }
        return true;
    }
};
```