题目描述：  
![image](/basicaldatastructure/stackandquere/image/image16.png)  
解题过程：  
遇到困难题我直接两手一摊  
题解里面说的特别好，两种方法，一种是优化版的暴力解法，一种是单调栈  
**优化版的暴力解法：**  
1. 创建一个left二维数组，一维长度为row（矩阵martrixt的行数），二维长度为col（矩阵matrixt的列数），left[i][j]表示的是第i行上到第j列为止连续1的个数
2. 开始求解：对矩阵matrixt进行遍历，如果遇到字符0就继续循环否则则可以得到一个可行的矩阵面积。使用width接住当前位置上left数组的值，则width可作为一个长为width，宽为1的矩形，使用area接住width的值作为第一个可行的矩形面积，然后往之前的行进行遍历（例如当前遍历到了第i行，则遍历第i-1行到第0行），每次遍历都对width进行更新，更新规则是取当前位置上width和left数组的最小值，然后以width为长度，宽度即为遍历的次数，求解一个可行矩形的面积，与area取最大值，当遍历行数结束之后当前位置上可以得到的可行矩阵的最大值就是area，与答案ret取最大值即可  
单调栈：  
仍然使用left数组记录每一行某一列上的1的连续个数  
对于每一列使用单调栈进行处理，看作是以列为横轴，左数组数值为纵轴的一个最大矩形题目  
1. 使用up数组记录第i行上方第一个left值小于第i行当前列上left值的行的序号，down数组记录第i行下方第一个left值小于第i行当前列上left值得行得序号，则up和down当前行的值之间的行的当前列的left值都大于或者等于第i行当前列上的left值，则可以直接使用left[i][j]作为矩形的宽度，up和down之间的行数作为高度，计算一个可行矩形的面积  
要点我认为是对于每一列都从0开始遍历行，一反惯性的先遍历行再遍历列的思维  
代码：（优化版的暴力解法→单调栈）  
```cpp
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        int row = matrix.size(),col = matrix[0].size();
        vector<vector<int>> left (row,vector<int> (col));
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (matrix[i][j] == '1') {
                    left[i][j] = (j == 0 ? 0 : left[i][j-1]) + 1;
                }
            }
        }
        int ret = 0;
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (matrix[i][j] == '0') continue;
                int width = left[i][j];
                int area = width;
                for (int k = i - 1; k >= 0; k--) {
                    if (left[k][j] == 0) break;
                    width = min(width,left[k][j]);
                    area = max(area,(i - k + 1) * width);
                }
                ret = max(ret,area);
            }
        }
        return ret;
    }
};
```
```cpp
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        int row = matrix.size(),col = matrix[0].size();
        vector<vector<int>> left (row,vector<int> (col));
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (matrix[i][j] == '1') {
                    left[i][j] = (j == 0 ? 0 : left[i][j-1]) + 1;
                }
            }
        }
        int ret = 0;
        for (int j = 0; j < col; j++) {
            stack<int> stk;
            vector<int> up (row,-1),down (row,row);
            for (int i = 0; i < row; i++) {
                while (!stk.empty() && left[stk.top()][j] >= left[i][j]) {
                    stk.pop();
                }
                up[i] = (stk.empty()) ? -1 : stk.top();
                stk.push(i);
            }
            stk = stack<int> ();
            for (int i = row - 1; i >= 0; i--) {
                while (!stk.empty() && left[stk.top()][j] >= left[i][j]) {
                    stk.pop();
                }
                down[i] = (stk.empty()) ? row : stk.top();
                stk.push(i);
            }
            int area = 0;
            for (int i = 0; i < row; i++) {
                int height = down[i] - up[i] - 1;
                area = max(area,height * left[i][j]);
            }
            ret = max(ret,area);
        }
        return ret;
    }
};
```