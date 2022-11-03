题目描述：  
![image](/basical/matrix/image/image4.png)  
解题过程：  
没有想到做了两个螺旋矩阵的题目，我还是不能直接把这个题目给做出来，我想用改变方向的方法直接把这道题撕出来，没有想到，我居然忽略了改变方向方法中最重要的一点：是否访问。螺旋矩阵II这个题目可以用初始化的数组是否为0而不使用访问数组，但是这里不行，这里的初始数组每个位置都有一个值，所以必须要一个访问数组。  
知道了c++如何获取向量二维矩阵的行数和列数  
注意一开始的空集检查！！！  
```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return {};
        }

        vector<vector<int>> directions{{0,1},{1,0},{0,-1},{-1,0}};
        int directionIndex = 0;
        int row = 0,col = 0;
        int cnt = 0,m = matrix.size(),n = matrix[0].size();
        vector<vector<bool>> visited(m,vector<bool>(n));
        vector<int> result(m * n);
        while (cnt < m * n) {
            result[cnt++] = matrix[row][col];
            visited[row][col] = true;
            int nextRow = row + directions[directionIndex][0],nextCol = col + directions[directionIndex][1];
            if (nextRow < 0 || nextRow >= m || nextCol < 0 || nextCol >= n || visited[nextRow][nextCol]) {
                directionIndex = (directionIndex + 1) % 4;
            }
            row += directions[directionIndex][0];
            col += directions[directionIndex][1];
        }
        return result;
    }
};
```