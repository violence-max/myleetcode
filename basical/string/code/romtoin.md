题目描述：  
![image](/basical/string/image/image19.png)  
解决过程：  
还写了一点时间，主要是哈希map设置成const的了，很难顶，我用的还是取子字符串的方法，更难顶了，用取字符的方法应该会快很多，看了题解才说话的  
代码：  
```cpp
class Solution {
const unordered_set<string> set = {"CM","CD","XC","XL","IX","IV"};
unordered_map<string,int> map = {
    {"M",1000},
    {"CM",900},
    {"D",500},
    {"CD",400},
    {"C",100},
    {"XC",90},
    {"L",50},
    {"XL",40},
    {"X",10},
    {"IX",9},
    {"V",5},
    {"IV",4},
    {"I",1},
};

public:
    int romanToInt(string s) {
        int n = s.size();
        int ans = 0;
        for (int i = 0; i < n; i++) {
            if (i < n - 1 && set.find(s.substr(i,2)) != set.end()) {
                ans += map[s.substr(i,2)];
                i++;
            } else {
                ans += map[s.substr(i,1)];
            }
        }
        return ans;
    }
};
```