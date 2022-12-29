题目描述：  
![image](/basical/matrix/image/image6.png)  
解决过程：  
用一个数组记录值为0的行下标和列下标，然后重新遍历矩阵将对应的应该置零的行和列的元素全部置0，缺点是重复把很多元素给置0了。  
题解方法1：用一个大小为m（矩阵的行数）的数组记录应该置0的行，用一个大小为n（矩阵的列数）的数组记录应该置0的列  
题解方法2和方法3：分别用两个常数和一个常数去实现算法，缺点是遍历了很多次数组  
代码：  
```cpp
class Solution {
public:
    void setBit(vector<vector<int>> &matrix,int row,int col) {
        int m = matrix.size(),n = matrix[0].size();
        for (int i = 0; i < n; i++) {
            matrix[row][i] = 0;
        }
        for (int i = 0; i < m; i++) {
            matrix[i][col] = 0;
        }
    }

    void setZeroes(vector<vector<int>>& matrix) {
        vector<pair<int,int>> bit;
        int m = matrix.size(),n = matrix[0].size();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    bit.push_back(make_pair(i,j));
                }
            }
        }
        for (auto &[row,col] : bit) {
            setBit(matrix,row,col);
        }
    }
};
```
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        vector<int> row(m), col(n);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (!matrix[i][j]) {
                    row[i] = col[j] = true;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (row[i] || col[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
};
```
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        int flag_col0 = false, flag_row0 = false;
        for (int i = 0; i < m; i++) {
            if (!matrix[i][0]) {
                flag_col0 = true;
            }
        }
        for (int j = 0; j < n; j++) {
            if (!matrix[0][j]) {
                flag_row0 = true;
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (!matrix[i][j]) {
                    matrix[i][0] = matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (!matrix[i][0] || !matrix[0][j]) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (flag_col0) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
        if (flag_row0) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
    }
};
```
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        int flag_col0 = false;
        for (int i = 0; i < m; i++) {
            if (!matrix[i][0]) {
                flag_col0 = true;
            }
            for (int j = 1; j < n; j++) {
                if (!matrix[i][j]) {
                    matrix[i][0] = matrix[0][j] = 0;
                }
            }
        }
        for (int i = m - 1; i >= 0; i--) {
            for (int j = 1; j < n; j++) {
                if (!matrix[i][0] || !matrix[0][j]) {
                    matrix[i][j] = 0;
                }
            }
            if (flag_col0) {
                matrix[i][0] = 0;
            }
        }
    }
};
```