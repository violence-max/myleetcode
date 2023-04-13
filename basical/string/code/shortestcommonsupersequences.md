题目描述：  
![image](/basical/string/image/image65.png)  
没做出来  
- 动态规划：
1. dp[i][j]表示str1[i:]和str2[j:]作为子序列的最短公共超序列字符串的长度
2. 状态转移方程  
$$
dp[i][j] = 
\begin{cases}
dp[i+1][j+1]+1, & str1[i] == str2[j] \\
min(dp[i+1][j],dp[i][j+1])+1, & str1[i] != str2[j] \\
\end{cases}
$$
3. 在计算dp[i][j]时，记得处理`i == n`和`j == m`这两种特殊情况；根据状态转移方程，其实很容易知道应该从后往前进行两次便利来进行对动态数组的赋值操作
4. 计算出最短公共超序列字符串的长度之后从前往后开始构造字符串：使用两个指针从0开始分别遍历两个字符串，就像是归并排序的合并操作一样，如果遇到相等的字符则直接加入到答案字符串中并且同时向右移动两个指针，如果遇到不相等的字符通过判断`dp[i][j] - 1 == dp[i+1][j] or dp[i][j+1]`来决定是加入哪个字符串的字符并且移动对应指针
5. 如果`dp[i][j] - 1 == dp[i+1][j]`，从实际意义上来理解，str1[i:]和str2[j:]作为子序列的最短公共超序列字符串的长度减了1后，得到的长度刚好等于dp[i+1][j]，说明取走了str1[i]才是正解
代码：  
```cpp
class Solution {
public:
    string shortestCommonSupersequence(string str1, string str2) {
        int n = str1.size(), m = str2.size();
        vector<vector<int>> dp(n + 1, vector<int>(m + 1));
        for (int i = 0; i < n; ++i) {
            dp[i][m] = n - i;
        }
        for (int i = 0; i < m; ++i) {
            dp[n][i] = m - i;
        }
        for (int i = n - 1; i >= 0; --i) {
            for (int j = m - 1; j >= 0; --j) {
                if (str1[i] == str2[j]) {
                    dp[i][j] = dp[i + 1][j + 1] + 1;
                } else {
                    dp[i][j] = min(dp[i + 1][j], dp[i][j + 1]) + 1;
                }
            }
        }
        string res;
        int t1 = 0, t2 = 0;
        while (t1 < n && t2 < m) {
            if (str1[t1] == str2[t2]) {
                res += str1[t1];
                ++t1;
                ++t2;
            } else if (dp[t1 + 1][t2] == dp[t1][t2] - 1) {
                res += str1[t1];
                ++t1;
            } else if (dp[t1][t2 + 1] == dp[t1][t2] - 1) {
                res += str2[t2];
                ++t2;
            }
        }
        if (t1 < n) {
            res += str1.substr(t1);
        }
        else if (t2 < m) {
            res += str2.substr(t2);
        }
        return res;
    }
};
```