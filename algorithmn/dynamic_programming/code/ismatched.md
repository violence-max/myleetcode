题目描述：  
![image](/algorithmn/dynamic_programming/image/image37.png)  
解决过程：  
一开始想用ifelse语句解决，后来发现‘*’的情况实在是太多了，直接想不开看题解了，然后发现题解用的是动态规划。  
题解本来可以把动态规划讲得很简单，但是他选择讲得很复杂。  
应该这么讲： 
用f[i][j]表示s字符串前i个字符和p字符串前j个字符是否匹配，true代表匹配，false代表不匹配。  
情况一：若第j个字符不是’*‘，则检查s得第i个字符和p的第j个字符是否匹配 
情况二：若第j个字符是’*‘，则可以选择匹配零个或者多个字符，若匹配零个字符则选择s的前i个字符和p的前j-1个字符进行匹配，若匹配多个字符，则继续匹配s的前i-1个字符和p的前j个字符  
总体来讲就是这样，easy 
代码：  
```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(),n = p.size();

        auto matches = [&](int i, int j) {
            if (i == 0) return false;
            else if (p[j-1] == '.') return true;
            else return s[i - 1] == p[j - 1];
        };

        vector<vector<bool>> f(m+1,vector<bool> (n+1));
        f[0][0] = true;
        for (int i = 0; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (p[j-1] == '*') {
                    f[i][j] = f[i][j] | f[i][j-2];
                    if (matches(i,j-1)) {
                        f[i][j] = f[i][j] | f[i-1][j];
                    } 
                } else {
                    if (matches(i,j)) {
                        f[i][j] = f[i][j] | f[i-1][j-1];
                    }
                }
            }
        }
        return f[m][n];
    }
};
```