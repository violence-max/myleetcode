题目描述：

![image](/algorithmn/dynamic_programming/image/image76.png)

解决过程：

5分钟解决

代码：

```cpp
class Solution {
public:
    int f[120][120];
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int ans = INT_MAX;
        int n = matrix.size();
        for (int i = 0; i < n; ++i) for (int j = 0; j < n; ++j) f[i][j] = INT_MAX;
        for (int i = 0; i < n; ++i) f[0][i] = matrix[0][i];
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                int lt = j > 0 ? f[i-1][j-1] : INT_MAX / 2;
                int rt = j < n - 1 ? f[i-1][j + 1] : INT_MAX / 2;
                f[i][j] = min(f[i][j], min(f[i-1][j], min(lt, rt))) + matrix[i][j];
            }
        }
        for (int i = 0; i < n; ++i) ans = min(ans, f[n-1][i]);
        return ans;
    }
};
```