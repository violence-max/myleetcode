题目描述：  
![image](/basical/string/image/image55.png)  
解决过程：  
5分钟ac，读错题目了，忽略了空格不变，wa了一次  
代码：  
```cpp
class Solution {
public:
    string decodeMessage(string key, string message) {
        unordered_map<char,char> map;
        int index = 0;
        for (char c : key) {
            if (c == ' ' || map.count(c)) continue;
            map[c] = 'a' + index++;
        }   
        string ans;
        for (auto& c : message) {
            if (c == ' ') {
                ans += ' ';
                continue;
            }
            ans += map[c];
        }
        return ans;
    }
};
```