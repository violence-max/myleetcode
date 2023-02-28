题目描述：  
![image](/algorithmn/dynamic_programming/image/image45.png)  
解决过程：  
测试用例里面s的长度太长了，回溯会超时。  
看的题解，有两个两点，第一个是可以提前处理字符串s知晓i到j的子串是否是回文串，这个是一个基础动态规划；第二个是用f(i)来表示0到i的子串的最少分割次数以及状态转移方程，语意非常清晰  
代码：  
```cpp
class Solution {
public:
    int minCut(string s) {
        int n = s.length();
        vector<vector<bool>> g (n, vector<bool> (n,true));
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                g[i][j] = s[i] == s[j] && g[i+1][j-1];
            }
        }
        vector<int> f (n,INT_MAX);
        for (int i = 0; i < n; i++) {
            if (g[0][i]) {
                f[i] = 0;
            } else {
                for (int j = 0; j < i; j++) {
                    if (g[j+1][i]) {
                        f[i] = min(f[i], f[j]+1);
                    }
                }
            }
        }
        return f[n-1];
    }
};
```