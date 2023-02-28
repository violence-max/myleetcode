题目描述：  
![image](/basical/matrix/image/image7.png)  
解决过程：  
我用的是类似二叉平衡树的方法进行搜索，从第0行最后一个数字开始  
题解有两种方法：  
1. 两次二分查找（对upper_boun()函数的使用不太了解，知道了c++的binary_search()api）
2. 一次二分查找（把每一行的第一个数拼接到上一行的末尾可以得到一个升序数组然后使用二分查找），用mid表示矩阵中的数字的方法稍微有点tricky，值得注意以下  
代码：  
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(),n = matrix[0].size();
        int i = 0,j = n - 1;
        while (i < m && j >= 0) {
            if (target == matrix[i][j])
                return true;
            else if (target > matrix[i][j]) {
                i += 1;
            } else {
                j -= 1;
            }
        }
        return false;
    }
};
```  
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>> matrix, int target) {
        auto row = upper_bound(matrix.begin(), matrix.end(), target, [](const int b, const vector<int> &a) {
            return b < a[0];
        });
        if (row == matrix.begin()) {
            return false;
        }
        --row;
        return binary_search(row->begin(), row->end(), target);
    }
};
```  
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        int low = 0, high = m * n - 1;
        while (low <= high) {
            int mid = (high - low) / 2 + low;
            int x = matrix[mid / n][mid % n];
            if (x < target) {
                low = mid + 1;
            } else if (x > target) {
                high = mid - 1;
            } else {
                return true;
            }
        }
        return false;
    }
};
```