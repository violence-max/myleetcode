题目描述：  
![image](/basical/matrix/image/image3.png)
解题过程：  
在做过这个题之前其实我已经做过顺时针打印矩阵了，没想到做这个题目还是不会做，笑死。第一个方法是改变打印方向，我觉得这个方向的思路是最easy的，但是一开始根本想不到，可能是要依靠另外一个方向矩阵的原因吧，也算是用了另外一个数据结构了。第二个方法是按层模拟，这个方法有四个指针，轮番打印，也还行，但是比起方法一稍微有点抽象。  
c++的方法特性还是有些不太了解的，比如矩阵的初始化，一开始全是0这个特性，还有两个容器嵌套时的索引，其实跟c是一样的。  
代码：  
```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        int maxNum = n * n;
        int cntNum = 1;
        vector<vector<int>> matrix(n,vector<int>(n));
        vector<vector<int>> directions{{0,1},{1,0},{0,-1},{-1,0}};
        int directionIndex = 0;
        int row = 0,col = 0;
        while (cntNum <= maxNum) {
            matrix[row][col] = cntNum++;
            int nextRow = row + directions[directionIndex][0],nextCol = col + directions[directionIndex][1];
            if (nextRow < 0 || nextRow >= n || nextCol < 0 || nextCol >= n  || matrix[nextRow][nextCol] != 0) {
                directionIndex = (directionIndex + 1) % 4;
            }
            row += directions[directionIndex][0];
            col += directions[directionIndex][1];
        }
        return matrix;
    }
};
```