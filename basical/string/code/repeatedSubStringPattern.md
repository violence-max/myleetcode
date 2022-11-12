题目描述：  
![image](/basical/string/image/image11.png)  
解题过程：  
自己想不出来暴力解法，看道题解还看不会他的其他方法，一个KMP，一个感觉是错的。。。KMP还是找个时间仔细研究一下吧，感觉要看好一会才能学会的东西。  
代码：  
```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int n = s.size();
        for (int i = 1; i * 2 <= n; ++i) {
            if (n % i == 0) {
                bool match = true;
                for (int j = i; j < n; ++j) {
                    if (s[j] != s[j - i]) {
                        match = false;
                        break;
                    }
                }
                if (match) {
                    return true;
                }
            }
        }
        return false;
    }
};
```