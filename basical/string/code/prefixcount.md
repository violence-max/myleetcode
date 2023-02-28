题目描述：  
![image](/basical/string/image/image37.png)  
解决过程：  
一分钟结束战斗  
代码：  
```cpp
class Solution {
public:
    bool check(string str,string pattern) {
        int ls = str.length(),lp = pattern.length();
        if (ls < lp) return false;
        else {
            for (int i = 0; i < lp; i++) {
                if (str[i] != pattern[i]) {
                    return false;
                }
            }
            return true;
        }
    }

    int prefixCount(vector<string>& words, string pref) {
        int cnt = 0;
        for (auto s : words) {
            if (check(s,pref)) {
                cnt += 1;
            }
        }    
        return cnt;
    }
};
```