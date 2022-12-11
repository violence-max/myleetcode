题目描述：  
![image](/basical/string/image/image14.png)  
解决过程：  
一道简答题罢了，根本不算是中等，就是自己太sb了才做了二十分钟。  
1. 没读清除题目：题目要求跳跃空格，我以为连非数字也要跳跃  
2. 越界处理：本来想用新办法—大于最大整数除十意味着溢出来处理，没想到还是溢出了，就用了老办法，直接用long型定义  
逻辑还是要多练，做这道题明显感觉得到自己思维不断在变化  
代码：  
```cpp
class Solution {
private:
    unordered_set<char> set = {'1','2','3','4','5','6','7','8','9','0'};
public:
    int myAtoi(string s) {
        int n = s.size();
        if (n == 0) return 0;
        bool as = true;
        int i = 0;
        while (s[i] == ' ' ) {
            i++;
        }
        if (s[i] == '-' || s[i] == '+') {
            as = (s[i] == '-') ? false : as; 
            i++;
        } else if (set.find(s[i]) == set.end()) {
            return 0;
        }
        long ans = 0;
        for (; i < n; i++) {
            if (set.find(s[i]) == set.end()) {
                break;
            } else {
                if (s[i] == '0' && ans == 0) {
                    continue;
                } else {
                    ans = (long)ans*10 + s[i] - '0';
                    if (ans > INT_MAX) {
                        return (as) ? INT_MAX : INT_MIN;
                    }
                }
            }
        }
        return (as) ? ans : (-1) * ans;
    }
};
```