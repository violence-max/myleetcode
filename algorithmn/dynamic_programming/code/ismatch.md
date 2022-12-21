题目描述：  
![image](/algorithmn/dynamic_programming/image/image38.png)  
解决过程：  
跟正则表达式的匹配完全一样的好吧，就是星号的那个地方不太一样而已。用一个二维数组来表示字符串s和字符串p的前i个字符和前j个字符是否匹配，如果p的第j个字符是星号那么可以选择星号匹配空串或者匹配一个字符或者匹配i个字符，如果是问号或者小写字母那只能选择匹配s的第i个字符  
代码： 
```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = (int)s.size(),n = (int)p.size();
        vector<vector<bool>> fn(m+1,vector<bool> (n+1));

        auto matches = [&](int i,int j) {
            if (i == 0) return false;
            else {
                if (p[j - 1] == '?') return true;
                else return s[i-1] == p[j-1];
            }
        };

        fn[0][0] = true;
        for (int i = 0; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (p[j-1] == '*') {
                    for (int k = i,l = j - 1; k >= 0; k--) {
                        fn[i][j] = fn[i][j] | fn[k][l];
                        if (fn[i][j]) break;
                    }
                } else {
                    if (matches(i,j)) {
                        fn[i][j] = fn[i][j] | fn[i-1][j-1];
                    }
                }
            }
        }
        return fn[m][n];
    }
};
```