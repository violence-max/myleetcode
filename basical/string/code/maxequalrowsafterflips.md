题目描述：  
![image](/basical/string/image/image71.png)  
解决过程：  
没做出来，没有注意到**若某几行通过翻转固定的列编号之后若行相等，说明这几行按位与为0的事实**  
代码：  
```cpp
class Solution {
public:
    int maxEqualRowsAfterFlips(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        unordered_map<string, int> mp;
        for (int i = 0; i < m; i++) {
            string str (n, '0');
            for (int j = 0; j < n; j++) {
                // 将每一行以1开头的字符串每位进行翻转
                str[j] = '0' + (matrix[i][j] ^ matrix[i][0]);
            }
            mp[str]++; // 翻转固定的列后若某几行字符串相同，说明按位与为0
        }
        int res = 0;
        for (auto &[k, v] : mp) {
            res = max(res, v);
        }
        return res;
    }
};
```