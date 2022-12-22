题目描述：  
![image](/basical/matrix/image/image5.png) 
解决过程：  
这绝对是矩阵问题里比较经典的一个题目，因此必须要记住答案。  
题解提供了三种解决方案：  
1. 借助辅助数组，第i行的数直接赋值到另外一个大小一样的数组的倒数第i列，然后等赋值结束之后再复制回来  
2. 原地旋转：（我想要实现的方法）通过数学的方法，主要是观察规律，找到了一个i行j列的数据如果发生旋转应该被赋值到什么位置上：j行n-i-1列，视作是被赋值到了倒数第i列的第j个字符上，依次旋转赋值会发现会回到数据本身，因此可以使用一个temp变量来进行逆时针赋值，一轮赋值四个数字。这个方法的两个精髓：规律的寻找和循环范围的限定。一开始我想的是从最外层开始，每次都旋转最外层的第0行的前n-1个数字，然后逐步往里，但是题解通过等价类的划分，直接划分到了一个个矩形上，并且同意了矩阵的边长为奇数或者偶数的情况  
3. 先水平旋转再通过对角线划分：这个方法最为巧妙，通过数学计算发现先水平对称再通过主对角线对称与第2种方法得到的结果是一样的。  
代码： 
```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        // C++ 这里的 = 拷贝是值拷贝，会得到一个新的数组
        auto matrix_new = matrix;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                matrix_new[j][n - i - 1] = matrix[i][j];
            }
        }
        // 这里也是值拷贝
        matrix = matrix_new;
    }
};
```  
```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for (int i = 0; i < n / 2; i++) {
            for (int j = 0; j < (n + 1) / 2; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n-j-1][i];
                matrix[n-j-1][i] = matrix[n-i-1][n-j-1];
                matrix[n-i-1][n-j-1] = matrix[j][n-i-1];
                matrix[j][n-i-1] = temp;
            }
        }
    }
};
```  
```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        // 水平翻转
        for (int i = 0; i < n / 2; ++i) {
            for (int j = 0; j < n; ++j) {
                swap(matrix[i][j], matrix[n - i - 1][j]);
            }
        }
        // 主对角线翻转
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                swap(matrix[i][j], matrix[j][i]);
            }
        }
    }
};
```