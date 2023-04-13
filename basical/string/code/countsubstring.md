题目描述：  
![image](/basical/string/image/image64.png)  
解决过程：  
没做出来  
- 枚举：直接循环字符串s和字符串t，每次循环只需要确认开始的子串的起点，不断增加长度并且记录已经得到的子串的不相同的字母的个数，如果大于1则退出循环，等于1则答案加1
- 动态规划：
1. 假设我们已经计算出字符串s和字符串t以(i,j)二元组为终点的向左扩展和向右扩展可以得到的最长的公共连续子串的长度分别为dpl[i][j]和dpr[i][j]
2. 使用二重循环遍历字符串s和t，寻找不同的字符。如果找到不同的字符，则基于该字符二元组(i,j)计算得到dpl[i][j]和dpr[i][j]，那么可以得到以该字符为不同字符得到的符合题意的子串长度为`(dp[i][j] + 1) * (dpr[i][j] + 1)` 
3. 可以从枚举的方法得到启发：在两串只有一个字符不相同的字符串中，该不相同的字符的左侧有l个字符，则可以以（l + 1）个字符为起点，基于该字符右侧有r个字符的前提下得到(r + 1)种可能  
代码：（枚举→动态规划）  
```cpp
class Solution {
public:
    int countSubstrings(string s, string t) {
        int ans = 0;
        for (int i = 0; i < s.size(); i++) {
            for (int j = 0; j < t.size(); j++) {
                int diff = 0;
                for (int k = 0; i + k < s.size() && j + k < t.size(); k++) {
                    diff += s[i+k] == t[j+k] ? 0 : 1;
                    if (diff > 1) {
                        break;
                    } else if (diff == 1) {
                        ans++;
                    }
                }
            }
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    int countSubstrings(string s, string t) {
        int m = s.size(), n = t.size();
        vector<vector<int>> dpl(m + 1, vector<int>(n + 1));
        vector<vector<int>> dpr(m + 1, vector<int>(n + 1));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                dpl[i + 1][j + 1] = s[i] == t[j] ? (dpl[i][j] + 1) : 0;
            }
        }
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                dpr[i][j] = s[i] == t[j] ? (dpr[i + 1][j + 1] + 1) : 0;
            }
        }
        int ans = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (s[i] != t[j]) {
                    ans += (dpl[i][j] + 1) * (dpr[i + 1][j + 1] + 1);
                }
            }
        }
        return ans;
    }
};
```